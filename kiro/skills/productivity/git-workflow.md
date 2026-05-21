# 🌿 Skill: Git Workflow

## Description
Guia de fluxo de trabalho com Git para times que entregam com velocidade e segurança. Cobre estratégias de branch, commits atômicos, rebase vs merge, resolução de conflitos e boas práticas de PR.

---

## Hard Rules
1. **NUNCA** force push em branches compartilhadas (main, develop)
2. **NUNCA** commite diretamente em main — sempre via PR
3. **SEMPRE** use commits atômicos — uma mudança lógica por commit
4. **NUNCA** commite arquivos de secrets ou .env com valores reais
5. **SEMPRE** escreva mensagens de commit no imperativo presente
6. **NUNCA** use `git add .` sem revisar o que está sendo adicionado
7. **SEMPRE** faça rebase antes de abrir PR (histórico limpo)
8. **NUNCA** resolva conflitos sem entender ambos os lados

---

## Conventional Commits

```
<type>(<scope>): <subject>

[body opcional]

[footer opcional: BREAKING CHANGE, Closes #123]
```

### Tipos
| Tipo | Quando usar |
|------|-------------|
| `feat` | Nova funcionalidade |
| `fix` | Correção de bug |
| `refactor` | Refatoração sem mudança de comportamento |
| `test` | Adição ou correção de testes |
| `docs` | Documentação |
| `chore` | Tarefas de manutenção (deps, configs) |
| `perf` | Melhoria de performance |
| `ci` | Mudanças de CI/CD |
| `revert` | Reverter commit anterior |

### Exemplos
```bash
feat(auth): add Google OAuth login
fix(orders): prevent duplicate order on network retry
refactor(users): extract email validation to separate function
test(payments): add edge cases for refund calculation
docs(api): update authentication guide with refresh token flow
chore(deps): update express to 4.18.2
perf(queries): add index on orders.user_id
ci: add security scan to PR pipeline
revert: feat(auth): add Google OAuth login
```

---

## Branch Naming

```bash
feature/[issue-id]-short-description    # nova feature
fix/[issue-id]-short-description        # correção de bug
refactor/short-description              # refatoração
chore/short-description                 # manutenção
hotfix/[issue-id]-short-description     # fix urgente em produção

# Exemplos
feature/123-user-authentication
fix/456-duplicate-order-on-retry
refactor/extract-payment-service
hotfix/789-critical-auth-bypass
```

---

## Fluxo de Trabalho Diário

```bash
# 1. Sempre comece atualizado
git checkout main
git pull origin main

# 2. Crie branch a partir de main
git checkout -b feature/123-my-feature

# 3. Trabalhe em commits atômicos
git add src/users/user.service.ts  # nunca git add .
git commit -m "feat(users): add email uniqueness validation"

git add src/users/user.service.test.ts
git commit -m "test(users): add tests for email uniqueness"

# 4. Antes de abrir PR, rebase para histórico limpo
git fetch origin
git rebase origin/main

# 5. Push e abra PR
git push -u origin feature/123-my-feature
```

---

## Rebase vs Merge

```
Merge (preserva histórico):
main:    A─B─C─────────M
feature:       D─E─F─┘

Rebase (histórico linear):
main:    A─B─C─D'─E'─F'
feature:         D─E─F (descartado)
```

**Use rebase:** Para atualizar feature branch com main antes do PR  
**Use merge:** Para integrar PRs aprovados (via GitHub/GitLab — squash ou merge commit)  
**Nunca rebase:** Branches compartilhadas ou já publicadas

---

## Checklist de PR

```markdown
## PR Checklist

### Código
- [ ] Testes passando localmente
- [ ] Sem console.log de debug
- [ ] Sem código comentado
- [ ] Sem TODOs sem issue rastreável

### Segurança
- [ ] Sem secrets no código
- [ ] Inputs validados
- [ ] Sem vulnerabilidades óbvias

### Documentação
- [ ] CHANGELOG atualizado (se feature/fix relevante)
- [ ] README atualizado (se mudou comportamento público)
- [ ] JSDoc atualizado (se mudou assinatura pública)

### Review
- [ ] PR tem descrição clara do que muda e por quê
- [ ] Screenshots/vídeo para mudanças de UI
- [ ] Link para a issue relacionada
```

---

## Comandos Úteis

```bash
# Ver o que mudou antes de commitar
git diff --staged

# Desfazer último commit (mantém mudanças)
git reset --soft HEAD~1

# Desfazer mudanças em arquivo específico
git checkout -- src/file.ts

# Stash mudanças temporariamente
git stash push -m "WIP: feature xyz"
git stash pop

# Ver histórico limpo
git log --oneline --graph --all

# Encontrar commit que introduziu bug
git bisect start
git bisect bad HEAD
git bisect good v1.0.0
# git bisect good/bad até encontrar o commit

# Aplicar commit específico de outra branch
git cherry-pick <commit-hash>
```

---

## Output Format

```markdown
## 🌿 Git: [situação/dúvida]

### Solução
```bash
[comandos]
```

### Explicação
[por que esses comandos resolvem o problema]

### Cuidados
[o que pode dar errado e como evitar]
```
