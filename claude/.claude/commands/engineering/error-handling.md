# ⚠️ Skill: Error Handling

## Description
Estratégias de tratamento de erros por camada da aplicação. Define como criar, propagar, logar e responder a erros de forma que seja útil para debug, segura para o usuário e rastreável em produção.

---

## Hard Rules
1. **NUNCA** silencia erros com `catch(e) {}` vazio
2. **SEMPRE** use erros tipados — nunca `throw new Error('string genérica')`
3. **NUNCA** exponha stack traces ou detalhes internos ao usuário
4. **SEMPRE** logue o erro completo internamente antes de responder
5. **NUNCA** use códigos de erro numéricos sem constante nomeada
6. **SEMPRE** inclua contexto suficiente no erro para debug sem reprodução
7. **NUNCA** trate erros de negócio como erros de sistema (e vice-versa)
8. **SEMPRE** defina o comportamento de retry para erros recuperáveis

---

## Hierarquia de Erros

```ts
// Base
class AppError extends Error {
  constructor(
    message: string,
    public readonly code: string,
    public readonly statusCode: number,
    public readonly isOperational: boolean = true,
    public readonly context?: Record<string, unknown>
  ) {
    super(message)
    this.name = this.constructor.name
    Error.captureStackTrace(this, this.constructor)
  }
}

// Erros de domínio (negócio)
class ValidationError extends AppError {
  constructor(message: string, public readonly fields?: Record<string, string>) {
    super(message, 'VALIDATION_ERROR', 400)
  }
}

class NotFoundError extends AppError {
  constructor(resource: string, id: string) {
    super(`${resource} not found`, 'NOT_FOUND', 404, true, { resource, id })
  }
}

class ConflictError extends AppError {
  constructor(message: string) {
    super(message, 'CONFLICT', 409)
  }
}

class UnauthorizedError extends AppError {
  constructor(message = 'Authentication required') {
    super(message, 'UNAUTHORIZED', 401)
  }
}

class ForbiddenError extends AppError {
  constructor(message = 'Insufficient permissions') {
    super(message, 'FORBIDDEN', 403)
  }
}

// Erros de infraestrutura (não operacionais)
class DatabaseError extends AppError {
  constructor(message: string, cause?: Error) {
    super(message, 'DATABASE_ERROR', 500, false, { cause: cause?.message })
  }
}

class ExternalServiceError extends AppError {
  constructor(service: string, cause?: Error) {
    super(`External service failed: ${service}`, 'EXTERNAL_SERVICE_ERROR', 502, true, {
      service,
      cause: cause?.message
    })
  }
}
```

---

## Error Handler Global (Express)

```ts
// middleware/error-handler.ts
import { Request, Response, NextFunction } from 'express'
import { AppError } from '../errors'
import { logger } from '../logger'

export function errorHandler(
  err: Error,
  req: Request,
  res: Response,
  _next: NextFunction
): void {
  const requestId = req.headers['x-request-id'] as string

  if (err instanceof AppError) {
    logger.warn({
      requestId,
      code: err.code,
      message: err.message,
      context: err.context,
      path: req.path,
      method: req.method,
    })

    res.status(err.statusCode).json({
      error: {
        code: err.code,
        message: err.message,
        ...(err instanceof ValidationError && { details: err.fields }),
        requestId,
      }
    })
    return
  }

  logger.error({
    requestId,
    message: err.message,
    stack: err.stack,
    path: req.path,
    method: req.method,
  })

  res.status(500).json({
    error: {
      code: 'INTERNAL_ERROR',
      message: 'An unexpected error occurred',
      requestId,
    }
  })
}
```

---

## Logging Estruturado

```ts
// logger.ts — JSON estruturado para parsing em produção
import pino from 'pino'

export const logger = pino({
  level: process.env.LOG_LEVEL ?? 'info',
  formatters: {
    level: (label) => ({ level: label }),
  },
  transport: process.env.NODE_ENV === 'development'
    ? { target: 'pino-pretty' }
    : undefined,
  redact: ['password', 'token', 'authorization', 'cookie', '*.password', '*.token'],
})

// Uso correto
logger.info({ userId, action: 'login' }, 'User logged in')
logger.error({ err, userId, orderId }, 'Failed to process order')

// ❌ Nunca
logger.info(`User ${userId} logged in`) // sem estrutura, difícil de parsear
logger.error(err.message) // perde stack trace e contexto
```

---

## Retry com Backoff Exponencial

```ts
async function withRetry<T>(
  fn: () => Promise<T>,
  options: {
    maxAttempts?: number
    baseDelayMs?: number
    maxDelayMs?: number
    retryOn?: (err: Error) => boolean
  } = {}
): Promise<T> {
  const {
    maxAttempts = 3,
    baseDelayMs = 100,
    maxDelayMs = 5000,
    retryOn = (err) => err instanceof ExternalServiceError,
  } = options

  let lastError: Error

  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn()
    } catch (err) {
      lastError = err as Error

      if (attempt === maxAttempts || !retryOn(lastError)) {
        throw lastError
      }

      const delay = Math.min(baseDelayMs * 2 ** (attempt - 1), maxDelayMs)
      const jitter = Math.random() * delay * 0.1
      await sleep(delay + jitter)

      logger.warn({ attempt, maxAttempts, delay }, 'Retrying operation')
    }
  }

  throw lastError!
}
```

---

## Output Format

```markdown
## ⚠️ Error Handling: [módulo/camada]

### Erros Identificados
| Situação | Tipo de Erro | Status | Retry? |
|----------|-------------|--------|--------|
| [situação] | [tipo] | [HTTP] | Sim/Não |

### Implementação
```ts
[código]
```

### O que é logado (interno) vs respondido (usuário)
- Interno: [campos completos para debug]
- Usuário: [mensagem segura sem detalhes internos]
```
