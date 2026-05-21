# 📋 Skill: ADR Template

## Description
Template e guia para Architecture Decision Records (ADRs). ADRs documentam decisões arquiteturais importantes com contexto, alternativas e consequências — para que o time entenda não apenas o que foi decidido, mas por quê.

---

## Hard Rules
1. **NUNCA** tome decisão arquitetural sem criar um ADR
2. **SEMPRE** escreva o ADR antes de implementar, não depois
3. **NUNCA** delete ADRs — marque como Depreciado ou Substituído
4. **SEMPRE** numere sequencialmente: ADR-001, ADR-002...
5. **NUNCA** escreva ADR sem listar alternativas consideradas

---

## Quando Criar um ADR

Crie um ADR quando a decisão:
- Afeta a estrutura do sistema (arquitetura, padrões, tecnologias)
- É difícil de reverter (one-way door)
- Causaria confusão para um novo dev sem contexto
- Envolve trade-offs significativos
- Foi debatida pelo time

**Não precisa de ADR:** Escolha de biblioteca utilitária simples, mudanças de estilo, refatorações internas.

---

## Template Completo

```markdown
# ADR-[NNN]: [Título Descritivo da Decisão]

**Data:** YYYY-MM-DD
**Status:** Proposta | Aceita | Depreciada | Substituída por [ADR-NNN]
**Decisores:** [nomes ou times]
**Consultados:** [quem foi consultado mas não decidiu]

---

## Contexto
[Descreva a situação que motivou esta decisão.]

## Decisão
[Descreva a decisão tomada de forma clara e afirmativa.]

## Alternativas Consideradas

### Opção A: [Nome] ← escolhida
**Prós:** [vantagens]
**Contras:** [desvantagens]

### Opção B: [Nome]
**Prós:** [vantagens]
**Contras:** [desvantagens / por que foi descartada]

### Opção C: Não fazer nada
**Por que foi descartada:** [motivo]

## Consequências

### Positivas
- [benefício concreto]

### Negativas / Trade-offs aceitos
- [custo ou limitação aceita conscientemente]

### Riscos e Mitigações
| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|

## Referências
- [Link para documentação relevante]
- [ADR relacionado: ADR-NNN]
```

---

## Estrutura de Pastas

```
docs/
└── adr/
    ├── README.md
    ├── ADR-001-database.md
    ├── ADR-002-auth-strategy.md
    └── ADR-003-api-versioning.md
```

---

## Output Format

```markdown
## 📋 ADR Criado: ADR-[NNN]

**Arquivo:** `docs/adr/ADR-[NNN]-[titulo].md`

[conteúdo completo do ADR]
```
