# 🗄️ Skill: Database Patterns

## Description
Padrões e práticas para banco de dados que garantem performance, integridade e manutenibilidade. Cobre indexação, N+1, migrations seguras, soft delete, auditoria e connection pooling.

---

## Hard Rules
1. **NUNCA** faça migration sem backup verificado
2. **SEMPRE** use migrations versionadas — nunca altere banco manualmente em produção
3. **NUNCA** delete dados sem soft delete em sistemas com auditoria
4. **SEMPRE** adicione índice em colunas usadas em WHERE, JOIN e ORDER BY frequentes
5. **NUNCA** faça query dentro de loop — use batch queries
6. **SEMPRE** use connection pooling — nunca crie conexão por request
7. **NUNCA** armazene dados sensíveis sem criptografia (PII, financeiro)
8. **SEMPRE** defina constraints no banco, não apenas na aplicação

---

## Indexação

### Quando criar índice
```sql
-- ✅ Crie índice em:
-- Colunas em WHERE frequente
CREATE INDEX idx_users_email ON users(email);

-- Colunas em JOIN
CREATE INDEX idx_orders_user_id ON orders(user_id);

-- Colunas em ORDER BY com paginação
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);

-- Índice composto (ordem importa: mais seletivo primeiro)
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Índice parcial (apenas subset dos dados)
CREATE INDEX idx_users_active ON users(email) WHERE deleted_at IS NULL;
```

### EXPLAIN ANALYZE
```sql
-- Sempre analise queries lentas
EXPLAIN ANALYZE
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
WHERE u.status = 'active'
GROUP BY u.id;

-- Procure por: Seq Scan em tabelas grandes (sinal de índice faltando)
-- Bom: Index Scan, Index Only Scan
-- Ruim: Seq Scan em tabela > 10k rows
```

---

## N+1 Query — Detecção e Correção

```ts
// ❌ N+1: 1 query para posts + N queries para authors
const posts = await db.query('SELECT * FROM posts')
for (const post of posts) {
  // N queries adicionais!
  post.author = await db.query('SELECT * FROM users WHERE id = $1', [post.authorId])
}

// ✅ 2 queries com JOIN
const posts = await db.query(`
  SELECT p.*, u.name as author_name, u.email as author_email
  FROM posts p
  JOIN users u ON u.id = p.author_id
`)

// ✅ Ou 2 queries com IN (melhor para relacionamentos N:M)
const posts = await db.query('SELECT * FROM posts')
const authorIds = [...new Set(posts.map(p => p.authorId))]
const authors = await db.query('SELECT * FROM users WHERE id = ANY($1)', [authorIds])
const authorsMap = Object.fromEntries(authors.map(a => [a.id, a]))
posts.forEach(p => { p.author = authorsMap[p.authorId] })
```

---

## Migrations Seguras

### Expand-Contract (zero downtime)
```sql
-- ❌ NUNCA: rename direto (quebra código em produção)
ALTER TABLE users RENAME COLUMN name TO full_name;

-- ✅ Fase 1: Adicionar nova coluna
ALTER TABLE users ADD COLUMN full_name VARCHAR(255);

-- ✅ Fase 2: Preencher dados
UPDATE users SET full_name = name WHERE full_name IS NULL;

-- ✅ Fase 3: Adicionar NOT NULL após preencher
ALTER TABLE users ALTER COLUMN full_name SET NOT NULL;

-- ✅ Fase 4: Deploy do código usando full_name

-- ✅ Fase 5: Remover coluna antiga (próximo deploy)
ALTER TABLE users DROP COLUMN name;
```

### Migration Template
```sql
-- migrations/20240115_add_full_name_to_users.sql
-- UP
ALTER TABLE users ADD COLUMN full_name VARCHAR(255);
CREATE INDEX idx_users_full_name ON users(full_name);

-- DOWN (rollback)
DROP INDEX IF EXISTS idx_users_full_name;
ALTER TABLE users DROP COLUMN IF EXISTS full_name;
```

---

## Soft Delete

```sql
-- Schema
ALTER TABLE users ADD COLUMN deleted_at TIMESTAMP;
CREATE INDEX idx_users_deleted_at ON users(deleted_at) WHERE deleted_at IS NULL;

-- Query padrão (sempre filtra deletados)
SELECT * FROM users WHERE deleted_at IS NULL;

-- Restore
UPDATE users SET deleted_at = NULL WHERE id = $1;

-- Hard delete periódico (LGPD compliance)
DELETE FROM users WHERE deleted_at < NOW() - INTERVAL '90 days';
```

---

## Auditoria de Dados

```sql
-- Tabela de auditoria genérica
CREATE TABLE audit_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  table_name VARCHAR(100) NOT NULL,
  record_id UUID NOT NULL,
  action VARCHAR(10) NOT NULL, -- INSERT, UPDATE, DELETE
  old_values JSONB,
  new_values JSONB,
  changed_by UUID REFERENCES users(id),
  changed_at TIMESTAMP DEFAULT NOW()
);

-- Trigger automático
CREATE OR REPLACE FUNCTION audit_trigger()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO audit_log(table_name, record_id, action, old_values, new_values, changed_by)
  VALUES (
    TG_TABLE_NAME,
    COALESCE(NEW.id, OLD.id),
    TG_OP,
    CASE WHEN TG_OP != 'INSERT' THEN row_to_json(OLD) END,
    CASE WHEN TG_OP != 'DELETE' THEN row_to_json(NEW) END,
    current_setting('app.current_user_id', true)::UUID
  );
  RETURN COALESCE(NEW, OLD);
END;
$$ LANGUAGE plpgsql;
```

---

## Connection Pooling

```ts
// ✅ Pool configurado corretamente (Node.js + pg)
import { Pool } from 'pg'

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20,                    // máximo de conexões
  min: 2,                     // mínimo mantido aberto
  idleTimeoutMillis: 30_000,  // fecha conexão ociosa após 30s
  connectionTimeoutMillis: 2_000, // timeout para obter conexão
  ssl: process.env.NODE_ENV === 'production' ? { rejectUnauthorized: true } : false
})

// Sempre libere a conexão
const client = await pool.connect()
try {
  const result = await client.query('SELECT * FROM users WHERE id = $1', [id])
  return result.rows[0]
} finally {
  client.release() // SEMPRE libere, mesmo em caso de erro
}
```

---

## Output Format

```markdown
## 🗄️ Database: [problema/padrão]

### Diagnóstico
[Problema identificado com evidência]

### Solução
```sql
[query/migration/schema]
```

### Performance Impact
[Antes: X ms | Depois: Y ms | Melhoria: Z%]

### Rollback
```sql
[como reverter]
```
```
