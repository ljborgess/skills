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

type: feat | fix | refactor | test | docs | chore | perf | ci | revert
scope: módulo afetado (opcional)
subject: imperativo, lowercase, sem ponto final, max 72 chars
```

### Exemplos
```bash
feat(auth): add Google OAuth login
fix(orders): prevent duplicate order on network retry
refactor(users): extract email validation to separate function
test(payments): add edge cases for refund calculation
docs(api): update authentication guide with refresh token flow
chore(deps): update express to 4.18.2
perf(queries): add index on orders.user_id
```

---

## Branch Naming

```bash
feature/[issue-id]-short-description
fix/[issue-id]-short-description
refactor/short-description
hotfix/[issue-id]-short-description
```

---

## Fluxo de Trabalho Diário

```bash
# 1. Sempre comece atualizado
git checkout main && git pull origin main

# 2. Crie branch a partir de main
git checkout -b feature/123-my-feature

# 3. Commits atômicos
git add src/users/user.service.ts
git commit -m "feat(users): add email uniqueness validation"

# 4. Rebase antes do PR
git fetch origin && git rebase origin/main

# 5. Push e abra PR
git push -u origin feature/123-my-feature
```

---

## Rebase vs Merge

```
Merge:   A─B─C─────────M   (preserva histórico)
Rebase:  A─B─C─D'─E'─F'   (histórico linear)
```

**Use rebase:** Para atualizar feature branch com main antes do PR
**Use merge:** Para integrar PRs aprovados via GitHub
**Nunca rebase:** Branches compartilhadas ou já publicadas

---

## Checklist de PR

```markdown
- [ ] Testes passando localmente
- [ ] Sem console.log de debug
- [ ] Sem código comentado
- [ ] Sem secrets no código
- [ ] PR tem descrição clara do que muda e por quê
- [ ] Link para a issue relacionada
```

---

## Comandos Úteis

```bash
git diff --staged                    # ver o que vai commitar
git reset --soft HEAD~1              # desfazer último commit
git stash push -m "WIP: feature xyz" # guardar mudanças temporariamente
git log --oneline --graph --all      # histórico visual
git bisect start                     # encontrar commit que introduziu bug
git cherry-pick <commit-hash>        # aplicar commit específico
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
