# 📐 Skill: Output Format

## Description
Padrões de formatação de resposta para todos os agentes. Garante consistência, legibilidade e rastreabilidade em todos os outputs do sistema. Define quando usar cada formato e como estruturar informação para máxima clareza.

---

## Hard Rules
1. **NUNCA** use formatação markdown em respostas de uma linha
2. **SEMPRE** use código em blocos com linguagem especificada
3. **NUNCA** misture múltiplos formatos sem separação clara
4. **SEMPRE** inclua contexto suficiente para o output ser entendido isoladamente
5. **NUNCA** use emojis em código ou outputs técnicos
6. **SEMPRE** use emojis como marcadores visuais em documentação (com moderação)

---

## Formatos por Tipo de Resposta

### Resposta Direta (< 3 linhas)
Sem markdown. Texto direto e objetivo.

### Explicação Técnica
```markdown
## [Título]
[Contexto em 1-2 frases]

### Como funciona
[Explicação]

### Exemplo
[código]

### Quando usar
- [caso 1]
```

### Análise Comparativa
```markdown
## Comparativo: [A] vs [B]

| Critério | [A] | [B] |
|----------|-----|-----|

**Recomendação:** [A/B] — [justificativa em 1 linha]
```

### Plano de Ação
```markdown
## Plano: [objetivo]

### Fase 1: [nome] — [estimativa]
- [ ] [tarefa atômica]
```

### Code Review
```markdown
## Review: [arquivo]
**Veredicto:** ✅ / ⚠️ / ❌

### Issues
#### 🔴 [CRITICAL] — [título]
**Linha:** X | **Impacto:** [impacto]
[código problemático → código corrigido]
```

---

## Hierarquia de Títulos

```
# → Título do documento
## → Seção principal
### → Subseção
**bold** → Destaque inline
`code` → Referência a código inline
```

---

## Tamanho de Resposta

| Tipo de Pergunta | Tamanho Ideal |
|-----------------|---------------|
| Pergunta direta | 1-3 linhas |
| Explicação conceitual | 1-2 parágrafos |
| Tutorial/How-to | Completo com exemplos |
| Code review | Todos os issues, sem omitir |
| Debug de erro | Causa + solução + prevenção |
