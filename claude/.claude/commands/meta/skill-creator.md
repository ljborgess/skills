# 🏭 Skill: Skill Creator

## Description
Meta-skill para criação de novas skills e agentes. Define o contrato, estrutura e padrões que toda nova skill/agente deve seguir para ser compatível com o sistema. Use este guia sempre que precisar adicionar uma nova capacidade ao sistema.

---

## Hard Rules
1. **NUNCA** crie uma skill sem Description, Hard Rules e Output Format
2. **SEMPRE** siga a estrutura de template definida aqui
3. **NUNCA** duplique uma skill existente — estenda ou componha
4. **SEMPRE** teste a skill com pelo menos 3 casos de uso antes de publicar
5. **NUNCA** crie skill com escopo maior que um domínio específico

---

## Estrutura de Command (Skill)

```markdown
# [emoji] Skill: [Nome]

## Description
[O que faz, quando usar, qual problema resolve — 2-3 frases]

---

## Hard Rules
[Lista numerada de regras invioláveis]

---

## [Seções específicas do domínio]
[Frameworks, checklists, matrizes, exemplos]

---

## Output Format
[Template exato de como a skill deve formatar suas respostas]
```

---

## Estrutura de Agente (.claude/agents/)

```markdown
---
name: nome-do-agente
description: Papel, especialidade, quando acionar. Use quando [situação].
tools: Read, Write, Bash, WebSearch, Grep
---

# [Nome do Agente]

## Disciplina
[Como o agente opera — loop disciplinado, nunca pule fases]

### Fase 1 — [Nome]
[O que fazer nesta fase]

### Fase 2 — [Nome]
[O que fazer nesta fase]

## Regras invioláveis
1. [regra]

## Formato de saída
[template]

## Escalação
- [situação] → **agente-responsável**
```

---

## Categorias

| Categoria | Pasta commands | Pasta agents |
|-----------|---------------|-------------|
| Engenharia | `commands/engineering/` | — |
| Produtividade | `commands/productivity/` | — |
| Pessoal | `commands/personal/` | `agents/personal/` |
| Misc/Setup | `commands/misc/` | — |
| Meta | `commands/meta/` | — |
| Qualidade | — | `agents/quality/` |
| Arquitetura | — | `agents/architecture/` |
| Segurança | — | `agents/security/` |
| Ops | — | `agents/ops/` |
| Planning | — | `agents/planning/` |

---

## Checklist de Nova Skill

```markdown
- [ ] Arquivo na pasta correta
- [ ] Description clara (problema + solução)
- [ ] Hard Rules definidas (mínimo 5)
- [ ] Output Format com template real
- [ ] Não duplica skill existente
- [ ] Testada com 3 casos de uso
- [ ] CLAUDE.md atualizado
```

---

## Output Format

```markdown
## 🏭 Nova Skill Criada: [nome]

**Categoria:** [engineering/productivity/personal/misc/meta]
**Arquivo:** `.claude/commands/[categoria]/[nome].md`

### Resumo
[O que a skill faz em 1 frase]

### Como usar
`/[nome]` ou "aplique a skill [nome] para [tarefa]"

### Próximos passos
- [ ] Atualizar CLAUDE.md
```
