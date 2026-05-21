# 🧪 Skill: Testing Strategy

## Description
Estratégia completa de testes que equilibra confiança, velocidade e manutenibilidade. Define o que testar, como testar e o que NÃO testar — porque testes ruins são piores que nenhum teste.

---

## Hard Rules
1. **NUNCA** teste implementação — teste comportamento observável
2. **SEMPRE** escreva o teste que falha antes de corrigir um bug
3. **NUNCA** mocke o que você está testando
4. **SEMPRE** prefira testes de integração a mocks excessivos
5. **NUNCA** ignore testes flaky — corrija ou delete
6. **SEMPRE** mantenha testes rápidos (unit < 100ms, integration < 1s)
7. **NUNCA** compartilhe estado entre testes
8. **SEMPRE** nomeie testes como especificações de comportamento

---

## O Que Testar (e o Que Não Testar)

### ✅ Teste isso
- Lógica de negócio complexa
- Casos de borda e edge cases
- Caminhos de erro
- Contratos de API (input/output)
- Integrações com banco de dados
- Fluxos críticos de usuário (E2E)

### ❌ Não teste isso
- Getters/setters triviais
- Código de terceiros (libs)
- Configuração de framework
- Código que só delega para outra função
- Implementação interna (detalhes que podem mudar)

---

## Tipos de Test Doubles

```ts
// STUB: retorna valor fixo, não verifica chamadas
const userRepo = { findById: async () => makeUser() }

// MOCK: verifica se foi chamado corretamente
const emailService = { send: jest.fn() }
expect(emailService.send).toHaveBeenCalledWith({ to: 'user@test.com' })

// SPY: observa chamadas sem substituir implementação
const spy = jest.spyOn(console, 'error')
expect(spy).toHaveBeenCalledTimes(1)

// FAKE: implementação simplificada mas funcional
class InMemoryUserRepository implements UserRepository {
  private users: User[] = []
  async findById(id: string) { return this.users.find(u => u.id === id) ?? null }
  async save(user: User) { this.users.push(user); return user }
}
```

**Prefira Fakes a Mocks** — fakes são mais robustos a refatorações.

---

## Testes de Integração com Banco

```ts
// Usando banco real em testes (recomendado sobre mocks de DB)
describe('UserRepository (integration)', () => {
  let db: Database

  beforeAll(async () => {
    db = await createTestDatabase() // banco isolado para testes
  })

  afterAll(async () => {
    await db.close()
  })

  beforeEach(async () => {
    await db.query('TRUNCATE users CASCADE') // limpa entre testes
  })

  it('should find user by email', async () => {
    const user = await db.query(
      'INSERT INTO users (email, name) VALUES ($1, $2) RETURNING *',
      ['test@example.com', 'Test User']
    )

    const repo = new PostgresUserRepository(db)
    const found = await repo.findByEmail('test@example.com')

    expect(found).toMatchObject({ email: 'test@example.com' })
  })
})
```

---

## Property-Based Testing

```ts
import fc from 'fast-check'

// Em vez de testar casos específicos, testa propriedades universais
it('should always return sorted array', () => {
  fc.assert(
    fc.property(fc.array(fc.integer()), (arr) => {
      const sorted = sortArray(arr)
      // Propriedade: cada elemento deve ser <= ao próximo
      for (let i = 0; i < sorted.length - 1; i++) {
        expect(sorted[i]).toBeLessThanOrEqual(sorted[i + 1])
      }
    })
  )
})
```

---

## Coverage Targets

| Tipo de Código | Coverage Mínima |
|----------------|----------------|
| Lógica de negócio (domain) | 90% |
| Application services | 80% |
| Infrastructure (repos, adapters) | 70% (integration tests) |
| Controllers/Routes | 60% (E2E cobre o resto) |
| Utilitários | 85% |
| Configuração | Não aplicável |

---

## Output Format

```markdown
## 🧪 Testing Strategy: [módulo/feature]

### Cobertura Atual
Linhas: X% | Branches: X% | Functions: X%

### Gaps Críticos
- [ ] [caso não coberto com impacto]

### Estratégia Recomendada
- Unit: [o que testar com unit tests]
- Integration: [o que testar com integration tests]
- E2E: [fluxos críticos para E2E]

### Testes Gerados
[código]
```
