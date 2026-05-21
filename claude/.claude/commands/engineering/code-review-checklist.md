# ✅ Skill: Code Review Checklist

## Description
Checklist sistemático e exaustivo para code review. Usado pelo agente Reviewer e como referência para revisões manuais. Cobre segurança, performance, manutenibilidade e acessibilidade.

---

## Checklist Completo

### 1. Segurança
- [ ] Inputs validados e sanitizados
- [ ] Sem SQL injection (queries parametrizadas)
- [ ] Sem XSS (output escapado)
- [ ] Sem CSRF em endpoints mutantes
- [ ] Autenticação verificada nos endpoints protegidos
- [ ] Autorização (não apenas autenticação) verificada
- [ ] Secrets fora do código (env vars)
- [ ] Dependências sem CVEs conhecidos
- [ ] Rate limiting em endpoints públicos
- [ ] Logs não expõem dados sensíveis

### 2. Funcionalidade
- [ ] Lógica implementa corretamente os requisitos
- [ ] Edge cases tratados (null, undefined, empty, zero, negative)
- [ ] Overflow/underflow considerado em operações numéricas
- [ ] Concorrência tratada (race conditions, deadlocks)
- [ ] Idempotência garantida onde necessário
- [ ] Transações atômicas onde necessário

### 3. Performance
- [ ] Sem N+1 queries
- [ ] Índices de banco adequados para queries novas
- [ ] Paginação em listagens
- [ ] Cache utilizado onde apropriado
- [ ] Sem operações síncronas bloqueantes em I/O
- [ ] Sem loops aninhados em datasets grandes
- [ ] Lazy loading onde aplicável

### 4. Error Handling
- [ ] Todos os erros capturados e tratados
- [ ] Mensagens de erro úteis para debug (logs internos)
- [ ] Mensagens de erro seguras para o usuário (sem stack trace)
- [ ] Retry logic para operações falháveis (network, DB)
- [ ] Fallbacks definidos para dependências externas

### 5. Manutenibilidade
- [ ] Complexidade ciclomática < 10 por função
- [ ] Funções < 30 linhas
- [ ] Naming claro e consistente com o projeto
- [ ] Sem código comentado (use git para histórico)
- [ ] Sem TODOs sem issue rastreável
- [ ] Documentação atualizada (JSDoc, README)
- [ ] Sem magic numbers/strings

### 6. Testes
- [ ] Happy path coberto
- [ ] Error paths cobertos
- [ ] Edge cases cobertos
- [ ] Testes independentes (sem ordem de execução)
- [ ] Sem testes frágeis (dependentes de timing, ordem)
- [ ] Mocks apenas para dependências externas

### 7. Acessibilidade (Frontend)
- [ ] Imagens com alt text
- [ ] Formulários com labels associados
- [ ] Contraste de cores WCAG AA (4.5:1)
- [ ] Navegação por teclado funcional
- [ ] ARIA roles onde necessário
- [ ] Focus management em modais/dialogs

### 8. TypeScript (se aplicável)
- [ ] Sem `any` sem justificativa
- [ ] Interfaces definidas para objetos complexos
- [ ] Return types explícitos em funções públicas
- [ ] Enums ou const objects para valores fixos
- [ ] Generics usados corretamente

---

## Scoring

```
Total de itens aplicáveis: N
Itens aprovados: X
Score: (X/N) × 100%

≥ 95% → ✅ APROVADO
80-94% → ⚠️ APROVADO COM RESSALVAS
< 80%  → ❌ BLOQUEADO
```
