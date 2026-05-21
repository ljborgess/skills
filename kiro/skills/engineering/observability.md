# 📊 Skill: Observability

## Description
Os três pilares da observabilidade: logs, métricas e traces. Define como instrumentar aplicações para que problemas em produção sejam detectados antes dos usuários e diagnosticados em minutos, não horas.

---

## Hard Rules
1. **NUNCA** logue dados sensíveis (senhas, tokens, PII) — use redaction
2. **SEMPRE** use logs estruturados (JSON) — nunca strings concatenadas
3. **NUNCA** logue em nível ERROR o que é WARNING (ex: 404 é warning)
4. **SEMPRE** inclua requestId/traceId em todos os logs de uma requisição
5. **NUNCA** ignore métricas de erro rate e latência P95/P99
6. **SEMPRE** defina alertas antes de ir para produção
7. **NUNCA** use `console.log` em produção — use logger estruturado
8. **SEMPRE** instrumente os SLIs (Service Level Indicators) do negócio

---

## Os Três Pilares

```
LOGS     → O que aconteceu? (eventos discretos)
METRICS  → Quanto/quão rápido? (agregações numéricas)
TRACES   → Por que demorou? (fluxo de uma requisição)
```

---

## Logging Estruturado

```ts
// logger.ts
import pino from 'pino'

export const logger = pino({
  level: process.env.LOG_LEVEL ?? 'info',
  base: {
    service: process.env.SERVICE_NAME,
    version: process.env.APP_VERSION,
    env: process.env.NODE_ENV,
  },
  // Redact campos sensíveis automaticamente
  redact: {
    paths: ['password', 'token', 'authorization', 'cookie',
            '*.password', '*.token', 'req.headers.authorization'],
    censor: '[REDACTED]'
  },
})

// Middleware de request logging
export function requestLogger(req, res, next) {
  const requestId = req.headers['x-request-id'] ?? crypto.randomUUID()
  req.requestId = requestId
  res.setHeader('x-request-id', requestId)

  const start = Date.now()
  const reqLogger = logger.child({ requestId })
  req.logger = reqLogger

  res.on('finish', () => {
    reqLogger.info({
      method: req.method,
      path: req.path,
      statusCode: res.statusCode,
      durationMs: Date.now() - start,
      userAgent: req.headers['user-agent'],
    }, 'Request completed')
  })

  next()
}
```

### Níveis de Log

| Nível | Quando usar |
|-------|-------------|
| `fatal` | Sistema vai encerrar |
| `error` | Erro inesperado que requer atenção imediata |
| `warn` | Situação anormal mas recuperável (404, rate limit) |
| `info` | Eventos de negócio importantes (login, order created) |
| `debug` | Informação de debug (desabilitado em produção) |
| `trace` | Detalhe extremo (nunca em produção) |

---

## Métricas (Prometheus)

```ts
import { Counter, Histogram, Gauge, register } from 'prom-client'

// Contador de requisições HTTP
export const httpRequestsTotal = new Counter({
  name: 'http_requests_total',
  help: 'Total number of HTTP requests',
  labelNames: ['method', 'path', 'status_code'],
})

// Histograma de latência
export const httpRequestDuration = new Histogram({
  name: 'http_request_duration_seconds',
  help: 'HTTP request duration in seconds',
  labelNames: ['method', 'path', 'status_code'],
  buckets: [0.01, 0.05, 0.1, 0.3, 0.5, 1, 2, 5],
})

// Gauge de conexões ativas
export const activeConnections = new Gauge({
  name: 'active_connections',
  help: 'Number of active connections',
})

// Métricas de negócio
export const ordersCreated = new Counter({
  name: 'orders_created_total',
  help: 'Total orders created',
  labelNames: ['payment_method', 'status'],
})

// Endpoint de métricas
app.get('/metrics', async (req, res) => {
  res.set('Content-Type', register.contentType)
  res.end(await register.metrics())
})
```

---

## Distributed Tracing (OpenTelemetry)

```ts
// tracing.ts — inicializar antes de tudo
import { NodeSDK } from '@opentelemetry/sdk-node'
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http'
import { HttpInstrumentation } from '@opentelemetry/instrumentation-http'
import { PgInstrumentation } from '@opentelemetry/instrumentation-pg'

const sdk = new NodeSDK({
  serviceName: process.env.SERVICE_NAME,
  traceExporter: new OTLPTraceExporter({
    url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT,
  }),
  instrumentations: [
    new HttpInstrumentation(),  // auto-instrumenta HTTP
    new PgInstrumentation(),    // auto-instrumenta queries PostgreSQL
  ],
})

sdk.start()
```

---

## SLIs, SLOs e Alertas

```yaml
# Exemplos de SLIs e SLOs
SLIs (o que medir):
  - Availability: % de requests com status < 500
  - Latency: % de requests completados em < 500ms
  - Error rate: % de requests com erro

SLOs (metas):
  - Availability: 99.9% (8.7h de downtime/ano)
  - Latency P95: < 500ms
  - Error rate: < 0.1%

Alertas (quando acordar alguém):
  - Error rate > 1% por 5 minutos → PagerDuty
  - Latency P95 > 2s por 5 minutos → Slack #alerts
  - Availability < 99% por 1 minuto → PagerDuty
```

---

## Dashboard Mínimo (Grafana)

```
Painel 1: Request Rate (req/s por endpoint)
Painel 2: Error Rate (% de 5xx)
Painel 3: Latência P50/P95/P99
Painel 4: Active Connections
Painel 5: CPU/Memory usage
Painel 6: Database query duration
Painel 7: Business metrics (orders/min, signups/min)
```

---

## Output Format

```markdown
## 📊 Observability: [serviço]

### Instrumentação Adicionada
- [ ] Logs estruturados com requestId
- [ ] Métricas HTTP (rate, latência, errors)
- [ ] Métricas de negócio: [lista]
- [ ] Tracing distribuído

### SLOs Definidos
| SLI | SLO | Alerta em |
|-----|-----|-----------|
| Availability | 99.9% | < 99% por 1min |
| Latência P95 | < 500ms | > 2s por 5min |

### Código
[implementação]
```
