---
inclusion: auto
---

# 🚫 Anti-Patterns — Proibidos Neste Projeto

> Padrões que foram explicitamente banidos neste projeto com justificativa.  
> O agente NUNCA deve sugerir ou usar estes padrões.

---

## Como Usar Este Arquivo

Preencha com os anti-patterns específicos do seu projeto.  
Inclua o motivo — sem contexto, as regras não são seguidas.

---

## Template de Anti-Pattern

```markdown
### [Nome do Anti-Pattern]
**Proibido:** [o que não fazer]
**Motivo:** [por que foi banido]
**Use em vez disso:** [alternativa aprovada]
**Exemplo do que NÃO fazer:**
[código proibido]
**Exemplo correto:**
[código aprovado]
```

---

## Anti-Patterns Universais (Sempre Ativos)

### Any em TypeScript
**Proibido:** Usar `any` sem justificativa documentada  
**Motivo:** Elimina type safety, esconde bugs em tempo de compilação  
**Use em vez disso:** Tipos específicos, `unknown` com type guard, generics

```ts
// ❌ Proibido
function process(data: any) { ... }

// ✅ Correto
function process(data: unknown) {
  if (!isValidData(data)) throw new ValidationError('Invalid data')
  // data é tipado aqui
}
```

### Console.log em Produção
**Proibido:** `console.log`, `console.error` direto no código  
**Motivo:** Sem estrutura, sem nível, sem contexto, vaza para produção  
**Use em vez disso:** Logger estruturado (pino, winston)

```ts
// ❌ Proibido
console.log('User created:', user)
console.error('Error:', err)

// ✅ Correto
logger.info({ userId: user.id }, 'User created')
logger.error({ err, userId }, 'Failed to create user')
```

### Catch Vazio
**Proibido:** `catch(e) {}` ou `catch(e) { return null }`  
**Motivo:** Silencia erros, impossibilita debug, esconde bugs  
**Use em vez disso:** Tratar o erro ou relançar com contexto

```ts
// ❌ Proibido
try {
  await sendEmail(user)
} catch (e) {}

// ✅ Correto
try {
  await sendEmail(user)
} catch (err) {
  logger.error({ err, userId: user.id }, 'Failed to send welcome email')
  // Decide: relançar, usar fallback, ou aceitar falha silenciosa com log
}
```

### Magic Numbers/Strings
**Proibido:** Números e strings literais sem nome  
**Motivo:** Sem contexto, difícil de manter, fácil de errar

```ts
// ❌ Proibido
if (user.role === 3) { ... }
setTimeout(fn, 86400000)

// ✅ Correto
const UserRole = { ADMIN: 3 } as const
const ONE_DAY_MS = 24 * 60 * 60 * 1000
if (user.role === UserRole.ADMIN) { ... }
```

---

## Anti-Patterns do Projeto (Preencha)

### [Nome]
**Proibido:** [descrição]  
**Motivo:** [justificativa]  
**Use em vez disso:** [alternativa]

---

## Decisões de Tecnologia Banidas

| Tecnologia | Motivo do Ban | Alternativa |
|-----------|--------------|-------------|
| moment.js | Deprecated, bundle pesado | date-fns |
| [lib] | [motivo] | [alternativa] |

---

## Padrões de Código Banidos

| Padrão | Motivo | Alternativa |
|--------|--------|-------------|
| `var` | Escopo confuso, use `const`/`let` | `const` por padrão |
| Callbacks aninhados | Callback hell, difícil de ler | async/await |
| `==` (loose equality) | Coerção implícita perigosa | `===` sempre |
| `Object.assign` para imutabilidade | Não é deep clone | spread operator ou structuredClone |
