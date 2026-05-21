# 🐳 Skill: Docker Best Practices

## Description
Guia de boas práticas para Docker em produção. Cobre multi-stage builds, imagens mínimas, segurança (non-root, secrets), healthchecks e otimização de cache de layers.

---

## Hard Rules
1. **NUNCA** use `latest` como tag de imagem base — sempre versão específica
2. **NUNCA** rode container como root em produção
3. **NUNCA** copie `.env` ou secrets para dentro da imagem
4. **SEMPRE** use multi-stage build para separar build de runtime
5. **SEMPRE** adicione HEALTHCHECK em serviços de produção
6. **SEMPRE** defina resource limits (memory, CPU) em produção
7. **NUNCA** instale ferramentas de debug na imagem de produção
8. **SEMPRE** use `.dockerignore` para excluir arquivos desnecessários

---

## Multi-Stage Build (Node.js)

```dockerfile
# ============================================
# Stage 1: Install dependencies
# ============================================
FROM node:20.11-alpine AS deps
WORKDIR /app

# Copia apenas arquivos de dependência (melhor cache)
COPY package*.json ./
RUN npm ci --only=production --ignore-scripts

# ============================================
# Stage 2: Build application
# ============================================
FROM node:20.11-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm ci --ignore-scripts

COPY . .
RUN npm run build

# ============================================
# Stage 3: Production runtime (imagem mínima)
# ============================================
FROM node:20.11-alpine AS runner
WORKDIR /app

# Security: non-root user
RUN addgroup --system --gid 1001 appgroup && \
    adduser --system --uid 1001 --ingroup appgroup appuser

# Copia apenas o necessário para rodar
COPY --from=deps --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/dist ./dist
COPY --from=builder --chown=appuser:appgroup /app/package.json ./

# Security: non-root
USER appuser

# Documenta a porta (não publica automaticamente)
EXPOSE 3000

# Healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget -qO- http://localhost:3000/health || exit 1

# Não use npm start em produção (processo filho, signals não propagam)
CMD ["node", "dist/main.js"]
```

---

## .dockerignore

```
# Dependencies (rebuilt no container)
node_modules/
.npm/

# Build outputs locais
dist/
build/
.next/

# Secrets e configs locais
.env
.env.*
!.env.example

# Dev tools
.git/
.gitignore
*.md
.eslintrc*
.prettierrc*
jest.config.*
*.test.ts
*.spec.ts

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
```

---

## Docker Compose (Desenvolvimento)

```yaml
# docker-compose.yml
version: '3.9'

services:
  app:
    build:
      context: .
      target: builder  # usa stage de build para dev (com devDeps)
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    env_file:
      - .env  # nunca commite este arquivo
    volumes:
      - .:/app
      - /app/node_modules  # evita sobrescrever node_modules do container
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    ports:
      - "5432:5432"  # apenas em dev

  redis:
    image: redis:7-alpine
    command: redis-server --requirepass ${REDIS_PASSWORD}
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 3

volumes:
  postgres_data:
```

---

## Otimização de Cache de Layers

```dockerfile
# ✅ Ordem correta: menos mutável → mais mutável
# Layer 1: imagem base (raramente muda)
FROM node:20.11-alpine

# Layer 2: dependências do sistema (raramente muda)
RUN apk add --no-cache curl

# Layer 3: dependências da aplicação (muda quando package.json muda)
COPY package*.json ./
RUN npm ci

# Layer 4: código fonte (muda frequentemente)
COPY . .
RUN npm run build

# ❌ Ordem errada: invalida cache de deps a cada mudança de código
COPY . .          # muda sempre
RUN npm ci        # reinstala tudo desnecessariamente
```

---

## Security Scan

```bash
# Scan de vulnerabilidades na imagem
docker scout cves myapp:latest

# Ou com Trivy
trivy image myapp:latest --severity HIGH,CRITICAL

# Verificar usuário do container
docker inspect myapp:latest | grep -i user

# Verificar capabilities
docker run --rm myapp:latest cat /proc/1/status | grep Cap
```

---

## Output Format

```markdown
## 🐳 Docker: [serviço/aplicação]

### Imagem Base Escolhida
`[imagem]:[versão]` — [justificativa]

### Tamanho da Imagem
Antes: X MB | Depois: Y MB

### Security Checklist
- [ ] Non-root user
- [ ] Multi-stage build
- [ ] Sem secrets na imagem
- [ ] Healthcheck configurado
- [ ] .dockerignore presente

### Arquivos Gerados
- `Dockerfile`
- `.dockerignore`
- `docker-compose.yml` (se aplicável)
```
