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

## Estrutura de Skill

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

## Estrutura de Agente

```markdown
# [emoji] Agent: [Nome]

## Description
[Papel, especialidade, quando acionar — 2-3 frases]

## Role
[Uma frase: função + valor entregue]

---

## Capabilities
[Lista de capacidades específicas]

---

## Hard Rules
[Regras numeradas — invioláveis]

---

## Priority Matrix
[Tabela de critérios e pesos]

---

## Output Format
[Template de resposta com exemplo]

---

## Trigger Phrases
[Frases que ativam este agente]

---

## Escalation
[Quando e para qual agente escalar]
```

---

## Categorias de Skills

| Categoria | Pasta | Exemplos |
|-----------|-------|---------|
| Engenharia | `skills/engineering/` | refactor, code-review, architecture |
| Produtividade | `skills/productivity/` | priorizacao, output-format, planning |
| Pessoal | `skills/personal/` | tcc-workflow, learning-path |
| Misc/Setup | `skills/misc/[nome]/` | setup-pre-commit, setup-ci |
| Meta | `skills/skill-creator/` | este arquivo |

---

## Checklist de Nova Skill

```markdown
## ✅ Checklist: Nova Skill "[nome]"

### Estrutura
- [ ] Arquivo em pasta correta (`skills/[categoria]/`)
- [ ] Description clara (problema + solução)
- [ ] Hard Rules definidas (mínimo 5)
- [ ] Output Format com template real

### Qualidade
- [ ] Não duplica skill existente
- [ ] Escopo bem delimitado
- [ ] Exemplos concretos incluídos
- [ ] Testada com 3 casos de uso

### Integração
- [ ] README.md atualizado
- [ ] Referenciada em agentes relevantes (se aplicável)
- [ ] Categoria correta
```

---

## Output Format

```markdown
## 🏭 Nova Skill Criada: [nome]

**Categoria:** [engineering/productivity/personal/misc]
**Arquivo:** `skills/[categoria]/[nome].md`

### Resumo
[O que a skill faz em 1 frase]

### Hard Rules principais
1. [regra mais importante]
2. [segunda mais importante]

### Como usar
[Exemplo de trigger/uso]

### Próximos passos
- [ ] Adicionar ao README.md
- [ ] Referenciar em agentes relevantes
```
