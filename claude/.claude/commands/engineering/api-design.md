# 🔌 Skill: API Design

## Description
Guia de design de APIs REST e GraphQL que são intuitivas, consistentes e evoluem sem breaking changes. Cobre naming, versionamento, paginação, error responses, autenticação e documentação.

---

## Hard Rules
1. **NUNCA** retorne 200 com erro no body — use HTTP status codes corretos
2. **SEMPRE** versione a API desde o início (`/v1/`)
3. **NUNCA** exponha IDs internos de banco diretamente — use UUIDs ou slugs
4. **SEMPRE** use substantivos no plural para recursos (`/users`, não `/getUser`)
5. **NUNCA** use verbos na URL — use métodos HTTP (`DELETE /users/1`, não `/deleteUser/1`)
6. **SEMPRE** pagine listagens — nunca retorne arrays ilimitados
7. **NUNCA** quebre contratos sem versionamento ou período de deprecação
8. **SEMPRE** documente todos os endpoints com OpenAPI/Swagger

---

## REST Conventions

### URL Structure
```
GET    /v1/users              → lista usuários (paginado)
POST   /v1/users              → cria usuário
GET    /v1/users/:id          → busca usuário por ID
PATCH  /v1/users/:id          → atualiza parcialmente
PUT    /v1/users/:id          → substitui completamente
DELETE /v1/users/:id          → remove usuário

GET    /v1/users/:id/orders   → lista pedidos do usuário
POST   /v1/users/:id/orders   → cria pedido para o usuário
```

### HTTP Status Codes
```
200 OK              → GET, PATCH, PUT bem-sucedidos
201 Created         → POST bem-sucedido (inclui Location header)
204 No Content      → DELETE bem-sucedido
400 Bad Request     → Dados inválidos (erro do cliente)
401 Unauthorized    → Não autenticado
403 Forbidden       → Autenticado mas sem permissão
404 Not Found       → Recurso não existe
409 Conflict        → Conflito (ex: email duplicado)
422 Unprocessable   → Validação de negócio falhou
429 Too Many Req    → Rate limit atingido
500 Internal Error  → Erro do servidor (nunca exponha detalhes)
```

### Error Response Padrão
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address"
      },
      {
        "field": "age",
        "message": "Must be at least 18"
      }
    ],
    "requestId": "req_abc123",
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

### Paginação Padrão
```json
// Request: GET /v1/users?page=2&limit=20&sort=createdAt&order=desc

// Response:
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 150,
    "totalPages": 8,
    "hasNext": true,
    "hasPrev": true
  }
}
```

### Filtros e Busca
```
GET /v1/users?status=active&role=admin
GET /v1/users?search=joão
GET /v1/users?createdAfter=2024-01-01&createdBefore=2024-12-31
GET /v1/users?fields=id,name,email    ← sparse fieldsets
GET /v1/users?include=orders,profile  ← relacionamentos
```

---

## Versionamento de API

### Estratégias
```
URL path (recomendado):  /v1/users, /v2/users
Header:                  API-Version: 2024-01-01
Query param:             /users?version=2
```

### Ciclo de Vida
```
v1 → CURRENT (suportada ativamente)
v1 → DEPRECATED (aviso no header: Deprecation: true, Sunset: 2025-06-01)
v1 → SUNSET (removida após data anunciada)
```

---

## OpenAPI 3.0 Template
```yaml
openapi: 3.0.3
info:
  title: [API Name]
  version: 1.0.0
  description: [descrição]

paths:
  /v1/users:
    get:
      summary: List users
      tags: [Users]
      security:
        - bearerAuth: []
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserListResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'
```

---

## Output Format

```markdown
## 🔌 API Design: [recurso/endpoint]

### Endpoints

| Método | Path | Descrição | Auth |
|--------|------|-----------|------|
| GET | /v1/[recurso] | Lista [recursos] | ✅ |
| POST | /v1/[recurso] | Cria [recurso] | ✅ |

### Request/Response

#### POST /v1/[recurso]
**Request:**
```json
{ "campo": "valor" }
```
**Response 201:**
```json
{ "id": "uuid", "campo": "valor" }
```
**Erros possíveis:** 400, 409, 422

### Breaking Changes
[Nenhuma / Lista de mudanças e estratégia de migração]
```
