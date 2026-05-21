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

[Descreva a situação que motivou esta decisão. Qual problema estamos resolvendo?
Quais são as forças em jogo? Quais constraints existem?
Escreva em presente — descreva o mundo como ele é agora.]

## Decisão

[Descreva a decisão tomada de forma clara e afirmativa.
"Vamos usar X para Y porque Z."
Seja específico — evite linguagem vaga.]

## Alternativas Consideradas

### Opção A: [Nome] ← escolhida
**Descrição:** [o que é]  
**Prós:**
- [vantagem concreta]
- [vantagem concreta]

**Contras:**
- [desvantagem concreta]
- [desvantagem concreta]

### Opção B: [Nome]
**Descrição:** [o que é]  
**Prós:**
- [vantagem]

**Contras:**
- [desvantagem]
- [por que foi descartada]

### Opção C: Não fazer nada
**Por que foi descartada:** [motivo]

## Consequências

### Positivas
- [benefício concreto que esta decisão traz]
- [benefício concreto]

### Negativas / Trade-offs aceitos
- [custo ou limitação que aceitamos conscientemente]
- [o que fica mais difícil com esta decisão]

### Riscos e Mitigações
| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|
| [risco] | Alta/Média/Baixa | Alto/Médio/Baixo | [ação] |

### Impacto em outros sistemas/times
- [quem mais é afetado e como]

## Notas de Implementação

[Orientações específicas para quem vai implementar esta decisão.
Links para documentação, exemplos, ou padrões a seguir.]

## Referências

- [Link para documentação relevante]
- [Link para benchmark ou artigo que embasou a decisão]
- [ADR relacionado: ADR-NNN]
```

---

## Exemplos de ADRs Comuns

### ADR sobre banco de dados
```markdown
# ADR-001: Usar PostgreSQL como banco de dados principal

**Status:** Aceita

## Contexto
Precisamos escolher um banco de dados para o sistema de pedidos.
Requisitos: ACID, suporte a JSON, queries complexas, < 10k req/s inicial.

## Decisão
Vamos usar PostgreSQL 16 como banco de dados principal.

## Alternativas Consideradas
### PostgreSQL ← escolhida
Prós: ACID, JSONB nativo, extensível, open source, excelente suporte
Contras: Escalabilidade horizontal mais complexa que NoSQL

### MongoDB
Prós: Schema flexível, escalabilidade horizontal
Contras: Sem ACID multi-documento até v4, queries relacionais complexas

## Consequências
Positivas: Transações confiáveis, queries SQL expressivas
Negativas: Schema migrations necessárias para mudanças de estrutura
```

---

## Estrutura de Pastas

```
docs/
└── adr/
    ├── README.md          ← índice de todos os ADRs
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
