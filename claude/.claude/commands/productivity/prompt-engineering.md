# 💬 Skill: Prompt Engineering

## Description
Como escrever prompts eficientes para Claude e outros LLMs. Elimina respostas vagas, reduz iterações desnecessárias e maximiza a qualidade do output na primeira tentativa.

---

## Hard Rules
1. **NUNCA** faça pergunta aberta quando quer resposta específica
2. **SEMPRE** forneça contexto suficiente — o agente não conhece seu projeto
3. **NUNCA** combine múltiplas tarefas não relacionadas em um prompt
4. **SEMPRE** especifique o formato de output desejado
5. **NUNCA** use "melhore isso" sem definir o critério de melhoria
6. **SEMPRE** forneça exemplos quando o padrão não é óbvio

---

## Anatomia de um Bom Prompt

```
[CONTEXTO]    → Quem você é, qual o projeto, qual a stack
[TAREFA]      → O que exatamente precisa ser feito
[CONSTRAINTS] → Restrições, o que NÃO fazer, limites
[FORMATO]     → Como quer a resposta (código, lista, tabela)
[EXEMPLO]     → Exemplo do input/output esperado (quando aplicável)
```

---

## Templates por Tipo de Tarefa

### Implementação de Feature
```
Contexto: API REST em Node.js + TypeScript + Express + PostgreSQL.
Padrão do projeto: Repository pattern, erros tipados com AppError.

Tarefa: Implementa o endpoint POST /v1/orders que:
- Recebe: { userId, items: [{productId, quantity}] }
- Valida: userId existe, produtos existem, estoque disponível
- Cria: order + order_items em transação
- Retorna: order criada com status 201

Constraints:
- Não use ORM — queries SQL diretas com pg
- Siga o padrão de error handling do projeto
- Inclua JSDoc na função principal

Formato: Código TypeScript completo com os arquivos necessários.
```

### Code Review
```
Contexto: PR adicionando autenticação JWT. Stack: Node.js + TypeScript.

Tarefa: Faz code review focado em:
1. Segurança (OWASP Top 10, especialmente A07 Auth Failures)
2. Tratamento de erros
3. Edge cases não cobertos

Constraints:
- Classifica cada issue como CRITICAL/HIGH/MEDIUM/LOW
- Para cada issue, mostra código problemático E correção
- Não comenta sobre estilo/formatação

Formato: Lista de issues com severidade, arquivo:linha, problema e fix.
```

### Debug
```
Contexto: API Node.js em produção. Stack: Express + PostgreSQL + Redis.

Problema: Endpoint GET /api/orders retorna 500 intermitentemente (~5% das req).

Stack trace:
[cole aqui]

Tarefa: Identifica causa raiz e propõe fix com teste de regressão.

Constraints:
- Só hipóteses com justificativa — sem "pode ser X"
- Fix não pode introduzir breaking changes
```

---

## Anti-Patterns de Prompt

```
❌ "Melhora esse código"
✅ "Refatora para reduzir complexidade ciclomática, mantendo comportamento idêntico"

❌ "Tem algum problema aqui?"
✅ "Faz code review focado em segurança e error handling, classificando por severidade"

❌ "Cria uma API"
✅ "Cria endpoint POST /v1/users em Express+TypeScript que [requisitos específicos]"

❌ "Explica isso"
✅ "Explica como JWT refresh token rotation funciona, com exemplo de código Node.js"
```

---

## Output Format

```markdown
## 💬 Prompt Otimizado: [tarefa]

### Problemas no Prompt Original
- [o que estava faltando ou era ambíguo]

### Prompt Otimizado
[versão melhorada]

### Por que é melhor
[explicação das melhorias]
```
