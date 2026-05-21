---
inclusion: manual
---

# 🛠️ Tech Stack

> **Como usar:** Preencha com a stack exata do seu projeto.  
> Ative com `#tech-stack` no chat para que o agente use as tecnologias corretas sem perguntar.

---

## Runtime & Linguagem

**Linguagem:** [TypeScript 5.x / JavaScript / Python 3.x / Java 21 / outro]  
**Runtime:** [Node.js 20.x / Deno / Bun / outro]  
**Gerenciador de pacotes:** [npm / yarn / pnpm / pip / maven]

---

## Frontend

**Framework:** [React 18 / Next.js 14 / Vue 3 / Angular 17 / —]  
**Estilização:** [Tailwind CSS / CSS Modules / Styled Components / —]  
**State management:** [Zustand / Redux Toolkit / Jotai / React Query / —]  
**Testes:** [Vitest / Jest / Playwright / Cypress]  
**Build:** [Vite / Webpack / Turbopack]

---

## Backend

**Framework:** [Express / Fastify / NestJS / FastAPI / Spring Boot / —]  
**ORM/Query builder:** [Prisma / Drizzle / TypeORM / Knex / SQLAlchemy / —]  
**Validação:** [Zod / Joi / class-validator / Pydantic]  
**Autenticação:** [JWT / NextAuth / Passport / —]

---

## Banco de Dados

**Principal:** [PostgreSQL 16 / MySQL 8 / MongoDB 7 / SQLite / —]  
**Cache:** [Redis 7 / Memcached / —]  
**Search:** [Elasticsearch / Typesense / —]  
**Migrations:** [Flyway / Liquibase / Prisma Migrate / Alembic]

---

## Infraestrutura

**Cloud:** [AWS / GCP / Azure / Vercel / Railway / —]  
**Containers:** [Docker / Kubernetes / —]  
**CI/CD:** [GitHub Actions / GitLab CI / CircleCI / —]  
**Monitoramento:** [Datadog / New Relic / Grafana / —]  
**Logs:** [CloudWatch / Loki / Papertrail / —]

---

## Versões Pinadas (Críticas)

| Dependência | Versão | Motivo do pin |
|-------------|--------|---------------|
| [dep] | [versão exata] | [por que não pode atualizar] |

---

## Libs Aprovadas vs Proibidas

### ✅ Aprovadas (use estas)
- HTTP client: [axios / fetch nativo / got]
- Datas: [date-fns / dayjs] — não use moment.js
- Validação: [Zod]
- Testes: [Vitest + Testing Library]

### ❌ Proibidas (não use)
- [lib] — motivo: [por que não]
- moment.js — motivo: deprecated, use date-fns
- lodash — motivo: use métodos nativos ES2022+

---

## Comandos do Projeto

```bash
# Desenvolvimento
[comando para rodar em dev]

# Testes
[comando para rodar testes]

# Build
[comando de build]

# Lint
[comando de lint]

# Migrations
[comando de migration]
```
