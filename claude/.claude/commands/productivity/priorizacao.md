# 🎯 Skill: Priorização

## Description
Framework de priorização de tarefas, features e decisões técnicas. Elimina subjetividade do processo de priorização usando matrizes e critérios objetivos. Aplicável a backlogs, sprints e decisões arquiteturais.

---

## Hard Rules
1. **NUNCA** priorize por urgência percebida sem validar impacto real
2. **SEMPRE** use critérios explícitos — nunca "feeling"
3. **NUNCA** deixe mais de 3 itens com prioridade máxima simultaneamente
4. **SEMPRE** reavalie prioridades quando contexto muda
5. **NUNCA** ignore dependências ao priorizar

---

## Frameworks Disponíveis

### 1. RICE Score (Recomendado para Features)
```
RICE = (Reach × Impact × Confidence) / Effort

Reach:      Quantas pessoas afeta? (número/período)
Impact:     0.25=mínimo | 0.5=baixo | 1=médio | 2=alto | 3=massivo
Confidence: 100%=certo | 80%=alto | 50%=médio | 20%=baixo
Effort:     Person-months (1 semana = 0.25)
```

### 2. MoSCoW (Recomendado para Sprints)
```
Must Have    → Sem isso o produto não funciona
Should Have  → Importante mas não crítico agora
Could Have   → Desejável se houver tempo
Won't Have   → Fora do escopo desta iteração
```

### 3. Matriz Impacto × Esforço (Quick Wins)
```
         BAIXO ESFORÇO    ALTO ESFORÇO
ALTO     ┌─────────────┬─────────────┐
IMPACTO  │  QUICK WIN  │  BIG BET    │
         │  Faça agora │  Planeje    │
         ├─────────────┼─────────────┤
BAIXO    │  FILL-IN    │  THANKLESS  │
IMPACTO  │  Se sobrar  │  Evite      │
         └─────────────┴─────────────┘
```

### 4. ICE Score (Experimentos)
```
ICE = Impact × Confidence × Ease  (cada critério: 1-10)
```

---

## Priority Levels

| Nível | Label | SLA | Critério |
|-------|-------|-----|----------|
| P0 | 🔴 CRÍTICO | Imediato | Sistema down / segurança comprometida |
| P1 | 🟠 URGENTE | < 24h | Funcionalidade core quebrada |
| P2 | 🟡 ALTO | < 1 semana | Feature importante / bug significativo |
| P3 | 🔵 MÉDIO | < 1 sprint | Melhoria relevante |
| P4 | ⚪ BAIXO | Backlog | Nice-to-have / tech debt menor |

---

## Output Format

```markdown
## 🎯 Priorização: [Contexto/Sprint/Backlog]

### Framework Utilizado: [RICE/MoSCoW/Matriz]

| # | Item | Score/Categoria | Prioridade | Justificativa |
|---|------|----------------|------------|---------------|

### Top 3 para Esta Iteração
1. **[Item]** — [justificativa em 1 linha]
2. **[Item]** — [justificativa em 1 linha]
3. **[Item]** — [justificativa em 1 linha]

### Itens Adiados e Motivo
- [Item] → [motivo objetivo]
```
