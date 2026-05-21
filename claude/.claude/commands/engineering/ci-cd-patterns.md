# 🚀 Skill: CI/CD Patterns

## Description
Padrões de pipelines CI/CD para entrega contínua segura e rápida. Cobre estratégias de branch, gates de qualidade, deploy strategies e rollback automático.

---

## Hard Rules
1. **NUNCA** faça deploy em produção sem passar por staging
2. **SEMPRE** tenha rollback automático configurado
3. **NUNCA** ignore falhas de teste no pipeline — são bloqueantes
4. **SEMPRE** use ambientes efêmeros para PRs (preview deployments)
5. **NUNCA** armazene secrets no código do pipeline — use secret managers
6. **SEMPRE** assine imagens Docker antes de fazer deploy
7. **NUNCA** faça deploy manual em produção — tudo via pipeline

---

## Pipeline Completo (GitHub Actions)

```yaml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  IMAGE_NAME: ghcr.io/${{ github.repository }}

jobs:
  # ─── QUALITY GATE ───────────────────────────────────────
  lint-and-types:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check

  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: testdb
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm test -- --coverage
        env:
          DATABASE_URL: postgresql://postgres:test@localhost/testdb
      - uses: codecov/codecov-action@v4
        with:
          fail_ci_if_error: true
          minimum_coverage: 80

  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm audit --audit-level=high
      - uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          severity: 'CRITICAL,HIGH'
          exit-code: '1'

  # ─── BUILD ──────────────────────────────────────────────
  build:
    needs: [lint-and-types, test, security]
    runs-on: ubuntu-latest
    outputs:
      image-tag: ${{ steps.meta.outputs.tags }}
      image-digest: ${{ steps.build.outputs.digest }}
    steps:
      - uses: actions/checkout@v4
      - uses: docker/setup-buildx-action@v3
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - uses: docker/metadata-action@v5
        id: meta
        with:
          images: ${{ env.IMAGE_NAME }}
          tags: |
            type=sha,prefix={{branch}}-
            type=ref,event=branch
      - uses: docker/build-push-action@v5
        id: build
        with:
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # ─── DEPLOY STAGING ─────────────────────────────────────
  deploy-staging:
    needs: build
    if: github.ref == 'refs/heads/main'
    environment:
      name: staging
      url: https://staging.myapp.com
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to staging
        run: |
          # kubectl set image deployment/app app=${{ needs.build.outputs.image-tag }}
          echo "Deploy to staging"
      - name: Run smoke tests
        run: |
          # curl -f https://staging.myapp.com/health
          echo "Smoke tests"

  # ─── DEPLOY PRODUCTION ──────────────────────────────────
  deploy-production:
    needs: deploy-staging
    environment:
      name: production
      url: https://myapp.com
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production (canary 10%)
        run: echo "Deploy canary"
      - name: Monitor canary (5 min)
        run: sleep 300
      - name: Promote to 100% or rollback
        run: echo "Promote or rollback based on metrics"
```

---

## Branch Strategy

### Trunk-Based Development (Recomendado)
```
main ──────────────────────────────────────► (produção)
  └── feature/xyz ──► PR ──► merge ──► deploy
  └── fix/abc     ──► PR ──► merge ──► deploy
```
- Branches de vida curta (< 2 dias)
- Feature flags para features incompletas
- Deploy contínuo para produção

### Git Flow (Para releases com ciclo longo)
```
main ────────────────────────────────────────► (produção)
develop ──────────────────────────────────────► (staging)
  └── feature/xyz ──► develop
  └── release/1.2 ──► main + develop
  └── hotfix/abc  ──► main + develop
```

---

## Rollback Automático

```yaml
# Critério de rollback automático
- name: Check deployment health
  run: |
    for i in {1..10}; do
      ERROR_RATE=$(curl -s https://metrics.myapp.com/error-rate)
      if (( $(echo "$ERROR_RATE > 5" | bc -l) )); then
        echo "Error rate too high: $ERROR_RATE% — rolling back"
        kubectl rollout undo deployment/app
        exit 1
      fi
      sleep 30
    done
    echo "Deployment healthy"
```

---

## Output Format

```markdown
## 🚀 CI/CD: [projeto]

### Pipeline Criado
- Stages: lint → test → security → build → staging → production
- Deploy strategy: [rolling/canary/blue-green]
- Rollback: automático se error rate > X%

### Arquivos
- `.github/workflows/ci.yml`

### Secrets necessários
| Secret | Descrição |
|--------|-----------|
| [nome] | [para que serve] |
```
