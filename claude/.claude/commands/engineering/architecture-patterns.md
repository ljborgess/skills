# 🏗️ Skill: Architecture Patterns

## Description
Catálogo de padrões arquiteturais com critérios objetivos de quando usar, quando evitar e como implementar. Elimina decisões baseadas em hype — cada padrão tem contexto, trade-offs e exemplos reais.

---

## Hard Rules
1. **NUNCA** adote um padrão sem entender o problema que ele resolve
2. **SEMPRE** considere a complexidade operacional, não apenas técnica
3. **NUNCA** use microserviços para times < 5 devs sem justificativa de escala
4. **SEMPRE** documente a decisão em ADR antes de implementar
5. **NUNCA** misture padrões incompatíveis sem camada de adaptação

---

## Catálogo de Padrões

### Repository Pattern
**Problema:** Acoplamento entre lógica de negócio e persistência  
**Solução:** Interface que abstrai o acesso a dados

```ts
// Contrato
interface UserRepository {
  findById(id: string): Promise<User | null>
  findByEmail(email: string): Promise<User | null>
  save(user: User): Promise<User>
  delete(id: string): Promise<void>
}

// Implementação (pode trocar PostgreSQL por MongoDB sem mudar o domínio)
class PostgresUserRepository implements UserRepository {
  async findById(id: string): Promise<User | null> {
    const row = await db.query('SELECT * FROM users WHERE id = $1', [id])
    return row ? mapToUser(row) : null
  }
}
```

**Use quando:** Precisa de testabilidade, múltiplas fontes de dados, ou DDD  
**Evite quando:** CRUD simples sem lógica de negócio complexa

---

### CQRS (Command Query Responsibility Segregation)
**Problema:** Modelo único não serve bem para leitura e escrita simultâneas  
**Solução:** Modelos separados para comandos (escrita) e queries (leitura)

```
Write side:  Command → CommandHandler → Domain → WriteDB
Read side:   Query → QueryHandler → ReadDB (otimizado para leitura)
```

**Use quando:** Cargas de leitura/escrita muito diferentes, relatórios complexos  
**Evite quando:** CRUD simples, time pequeno, baixa complexidade

---

### Event Sourcing
**Problema:** Precisa de histórico completo de mudanças e auditoria  
**Solução:** Armazena eventos, não estado atual

```ts
// Em vez de: UPDATE orders SET status = 'shipped'
// Armazena: { type: 'OrderShipped', orderId, timestamp, by }

// Estado atual = replay de todos os eventos
const order = events
  .filter(e => e.orderId === id)
  .reduce(applyEvent, initialState)
```

**Use quando:** Auditoria obrigatória, CQRS, sistemas financeiros  
**Evite quando:** Dados simples sem necessidade de histórico, queries complexas de estado atual

---

### Saga Pattern
**Problema:** Transações distribuídas entre múltiplos serviços  
**Solução:** Sequência de transações locais com compensação em caso de falha

```
OrderSaga:
  1. ReserveInventory → sucesso → 2
                     → falha   → fim (nada a compensar)
  2. ChargePayment   → sucesso → 3
                     → falha   → ReleaseInventory (compensação)
  3. ShipOrder       → sucesso → fim
                     → falha   → RefundPayment + ReleaseInventory
```

**Use quando:** Microserviços com operações multi-step  
**Evite quando:** Monolito, transações simples com banco único

---

### Circuit Breaker
**Problema:** Falha em cascata quando dependência externa fica lenta/indisponível  
**Solução:** Interrompe chamadas após N falhas, tenta recuperar gradualmente

```
CLOSED → (N falhas) → OPEN → (timeout) → HALF-OPEN → (sucesso) → CLOSED
                                                     → (falha)  → OPEN
```

```ts
const breaker = new CircuitBreaker(externalApiCall, {
  failureThreshold: 5,      // abre após 5 falhas
  successThreshold: 2,      // fecha após 2 sucessos
  timeout: 30_000,          // tenta recuperar após 30s
  fallback: () => cachedData // resposta de fallback
})
```

**Use quando:** Dependências externas não confiáveis  
**Evite quando:** Dependências internas confiáveis, latência não é crítica

---

### Strangler Fig
**Problema:** Migrar sistema legado sem reescrita total e sem downtime  
**Solução:** Novo sistema cresce ao redor do legado, substituindo gradualmente

```
Fase 1: Proxy na frente — roteia tudo para legado
Fase 2: Migra Feature A → proxy roteia A para novo, resto para legado
Fase 3: Migra Feature B → ...
Fase N: Legado desativado
```

**Use quando:** Modernização de legado, migração de stack  
**Evite quando:** Sistema pequeno onde reescrita é viável

---

### Outbox Pattern
**Problema:** Garantir que evento é publicado se e somente se a transação commitou  
**Solução:** Salva evento na mesma transação do banco, worker publica depois

```sql
BEGIN;
  UPDATE orders SET status = 'confirmed' WHERE id = $1;
  INSERT INTO outbox (event_type, payload) VALUES ('OrderConfirmed', $2);
COMMIT;
-- Worker lê outbox e publica no message broker
```

**Use quando:** Consistência entre banco e message broker é crítica  
**Evite quando:** Eventual consistency é aceitável, sem message broker

---

## Decision Matrix

| Padrão | Complexidade | Testabilidade | Escala | Time Mínimo |
|--------|-------------|---------------|--------|-------------|
| Repository | Baixa | ⭐⭐⭐⭐⭐ | Qualquer | 1 dev |
| CQRS | Média | ⭐⭐⭐⭐ | Média-Alta | 3+ devs |
| Event Sourcing | Alta | ⭐⭐⭐ | Alta | 5+ devs |
| Saga | Alta | ⭐⭐⭐ | Microserviços | 5+ devs |
| Circuit Breaker | Baixa | ⭐⭐⭐⭐ | Qualquer | 1 dev |
| Strangler Fig | Média | ⭐⭐⭐ | Qualquer | 2+ devs |
| Outbox | Média | ⭐⭐⭐⭐ | Média-Alta | 2+ devs |
