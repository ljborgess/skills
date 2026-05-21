---
inclusion: auto
---

# 👥 Team Conventions

> Convenções específicas do time. O agente segue estas convenções em todo código gerado.  
> Preencha com as decisões reais do seu time.

---

## Estrutura de Pastas

```
src/
├── domain/              → Entidades, value objects, regras de negócio puras
│   ├── [entity]/
│   │   ├── [entity].ts
│   │   ├── [entity].repository.ts  (interface)
│   │   └── [entity].errors.ts
├── application/         → Casos de uso, services, DTOs
│   ├── [feature]/
│   │   ├── [feature].service.ts
│   │   └── [feature].dto.ts
├── infrastructure/      → Implementações concretas (DB, APIs externas)
│   ├── database/
│   │   └── [entity].repository.pg.ts
│   └── http/
│       └── [service].client.ts
├── presentation/        → Controllers, routes, middlewares
│   └── [resource]/
│       ├── [resource].controller.ts
│       └── [resource].routes.ts
└── shared/              → Utils, tipos globais, constantes
    ├── errors/
    ├── types/
    └── utils/
```

---

## Naming Conventions

### Arquivos
```
user.service.ts          → services
user.repository.ts       → interfaces de repositório
user.repository.pg.ts    → implementação PostgreSQL
user.controller.ts       → controllers
user.routes.ts           → rotas
user.dto.ts              → Data Transfer Objects
user.errors.ts           → erros específicos do domínio
user.service.test.ts     → testes unitários
user.repository.test.ts  → testes de integração
```

### Variáveis e Funções
```ts
// camelCase para variáveis e funções
const userEmail = 'test@example.com'
function calculateOrderTotal(items: OrderItem[]): number { ... }

// PascalCase para classes, interfaces, types, enums
class UserService { ... }
interface UserRepository { ... }
type CreateUserDTO = { ... }
enum OrderStatus { Pending, Confirmed, Shipped }

// UPPER_SNAKE_CASE para constantes
const MAX_LOGIN_ATTEMPTS = 5
const JWT_EXPIRY_SECONDS = 3600
```

### Testes
```ts
describe('[NomeDaClasse]', () => {
  describe('[nomeDoMétodo]', () => {
    it('should [comportamento esperado] when [condição]', () => { ... })
    it('should throw [TipoDeErro] when [condição de erro]', () => { ... })
  })
})
```

---

## Padrões de Código

### Imports (ordem)
```ts
// 1. Node built-ins
import { readFile } from 'fs/promises'

// 2. Dependências externas
import express from 'express'
import { z } from 'zod'

// 3. Imports internos (absolutos)
import { UserService } from '@/application/users/user.service'
import { AppError } from '@/shared/errors'

// 4. Imports relativos
import { makeUser } from './user.factory'
```

### Exports
```ts
// ✅ Named exports (facilita tree-shaking e refatoração)
export class UserService { ... }
export type CreateUserDTO = { ... }

// ❌ Default exports (evitar — dificulta refatoração)
export default class UserService { ... }
```

### Async/Await
```ts
// ✅ Sempre async/await, nunca .then().catch() encadeado
async function getUser(id: string): Promise<User> {
  const user = await userRepo.findById(id)
  if (!user) throw new NotFoundError('User', id)
  return user
}

// ✅ Operações independentes em paralelo
const [user, orders] = await Promise.all([
  userRepo.findById(userId),
  orderRepo.findByUserId(userId)
])
```

---

## Processo de PR

### Tamanho de PR
- Máximo: 400 linhas alteradas (exceto geração automática)
- Ideal: < 200 linhas
- PRs grandes devem ser quebrados em PRs menores

### Descrição de PR
```markdown
## O que muda
[Descrição clara do que foi implementado/corrigido]

## Por que
[Contexto e motivação]

## Como testar
1. [passo]
2. [passo]

## Screenshots (se UI)
[imagem]

## Checklist
- [ ] Testes passando
- [ ] Sem console.log
- [ ] CHANGELOG atualizado (se relevante)
```

### Review
- Mínimo 1 aprovação para merge
- Author não pode aprovar o próprio PR
- CI deve estar verde antes do merge
- Squash merge para features, merge commit para releases

---

## Variáveis de Ambiente

- Sempre documentadas em `.env.example`
- Sempre validadas no startup da aplicação (fail-fast)
- Prefixo por contexto: `DB_`, `REDIS_`, `JWT_`, `AWS_`, `STRIPE_`

```ts
// Validação no startup
const env = z.object({
  DATABASE_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  PORT: z.coerce.number().default(3000),
}).parse(process.env)
```
