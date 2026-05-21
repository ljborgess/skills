# ♻️ Skill: Refactor

## Description
Guia de refatoração segura, incremental e rastreável. Define quando, como e até onde refatorar sem introduzir regressões. Prioriza legibilidade, manutenibilidade e performance — nessa ordem.

---

## Hard Rules
1. **NUNCA** refatore e adicione features no mesmo commit
2. **SEMPRE** garanta cobertura de testes antes de refatorar
3. **NUNCA** mude comportamento externo durante refatoração
4. **SEMPRE** refatore em passos pequenos e verificáveis
5. **NUNCA** refatore código que não entende completamente
6. **SEMPRE** documente o motivo da refatoração no commit message
7. **NUNCA** otimize prematuramente — meça antes de otimizar

---

## Refactor Decision Matrix

| Situação | Ação |
|----------|------|
| Complexidade ciclomática > 10 | Extrair funções |
| Função > 30 linhas | Decompor |
| Duplicação > 3 ocorrências | Extrair abstração |
| Naming confuso | Renomear (semântico) |
| Comentário explicando código | Reescrever o código |
| Classe com > 2 responsabilidades | Separar (SRP) |
| Parâmetros > 3 em função | Usar objeto de configuração |

---

## Refactor Patterns

### Extract Function
```ts
// ❌ Antes
function processOrder(order) {
  // validar
  if (!order.id || !order.items.length) throw new Error('Invalid')
  // calcular total
  const total = order.items.reduce((sum, i) => sum + i.price * i.qty, 0)
  // aplicar desconto
  const discount = total > 100 ? total * 0.1 : 0
  return total - discount
}

// ✅ Depois
function validateOrder(order: Order): void {
  if (!order.id || !order.items.length) throw new Error('Invalid order')
}

function calculateTotal(items: OrderItem[]): number {
  return items.reduce((sum, i) => sum + i.price * i.qty, 0)
}

function applyDiscount(total: number): number {
  return total > 100 ? total * 0.9 : total
}

function processOrder(order: Order): number {
  validateOrder(order)
  return applyDiscount(calculateTotal(order.items))
}
```

### Replace Magic Numbers
```ts
// ❌ Antes
if (status === 3) { ... }
setTimeout(fn, 86400000)

// ✅ Depois
const ORDER_STATUS = { SHIPPED: 3 } as const
const ONE_DAY_MS = 24 * 60 * 60 * 1000
if (status === ORDER_STATUS.SHIPPED) { ... }
setTimeout(fn, ONE_DAY_MS)
```

### Early Return (Guard Clauses)
```ts
// ❌ Antes
function getDiscount(user) {
  if (user) {
    if (user.isPremium) {
      if (user.yearsActive > 2) {
        return 0.2
      }
    }
  }
  return 0
}

// ✅ Depois
function getDiscount(user: User | null): number {
  if (!user) return 0
  if (!user.isPremium) return 0
  if (user.yearsActive <= 2) return 0
  return 0.2
}
```

---

## Output Format

```markdown
## ♻️ Refactor: [arquivo/função]

### Motivação
[Por que refatorar — métrica ou problema concreto]

### Escopo
- Arquivos afetados: [lista]
- Comportamento externo alterado: ❌ Não

### Antes → Depois
[código antes]
[código depois]

### Checklist
- [ ] Testes passando antes
- [ ] Testes passando depois
- [ ] Sem mudança de comportamento
- [ ] Commit isolado criado
```
