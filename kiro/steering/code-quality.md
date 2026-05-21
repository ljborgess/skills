---
inclusion: auto
---

# 📊 Code Quality Standards

Métricas e critérios de qualidade aplicados automaticamente em todas as sessões de código.

## Métricas de Qualidade

| Métrica | Limite | Ação se violado |
|---------|--------|-----------------|
| Complexidade ciclomática | ≤ 10 | Extrair funções |
| Linhas por função | ≤ 30 | Decompor |
| Parâmetros por função | ≤ 3 | Usar objeto de config |
| Profundidade de aninhamento | ≤ 3 | Guard clauses / early return |
| Duplicação de código | 0 ocorrências | Extrair abstração |
| Cobertura de testes | ≥ 80% | Adicionar testes |

## Padrões de Nomenclatura

```
Variáveis/funções:  camelCase       → userSessionToken, calculateTotal
Classes/Types:      PascalCase      → UserService, OrderItem
Constantes:         UPPER_SNAKE     → MAX_RETRY_COUNT, API_BASE_URL
Arquivos:           kebab-case      → user-service.ts, order-item.ts
Testes:             [nome].test.ts  → user-service.test.ts
```

## Estrutura de Projeto (Node.js/TypeScript)

```
src/
├── domain/          → entidades e regras de negócio
├── application/     → casos de uso / services
├── infrastructure/  → DB, APIs externas, frameworks
├── presentation/    → controllers, routes, DTOs
└── shared/          → utils, types, constants
```

## Commit Message Format

```
<type>(<scope>): <subject>

type: feat | fix | refactor | test | docs | chore | perf | ci
scope: módulo afetado (opcional)
subject: imperativo, lowercase, sem ponto final, max 72 chars

Exemplos:
feat(auth): add JWT refresh token rotation
fix(orders): prevent duplicate order creation on retry
refactor(users): extract validation to separate service
```

## Code Smells — Sempre Sinalizar

- **God Object**: classe com mais de 2 responsabilidades
- **Long Method**: função > 30 linhas
- **Magic Numbers**: números sem nome (`if (status === 3)`)
- **Dead Code**: código comentado ou nunca executado
- **Primitive Obsession**: usar string/number onde deveria ser tipo próprio
- **Feature Envy**: método que usa mais dados de outra classe que da própria
- **Shotgun Surgery**: mudança simples requer alterações em muitos arquivos
