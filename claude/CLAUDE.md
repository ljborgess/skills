# CLAUDE.md — Contexto Global do Sistema

> Este arquivo é carregado automaticamente em todas as sessões do Claude Code.  
> Equivalente ao steering `inclusion: auto` do Kiro.

---

## Contexto do Sistema

Este workspace contém um sistema de agentes, skills e hooks para desenvolvimento de software.  
Todos os agentes, comandos e hooks listados abaixo estão disponíveis nesta sessão.

**Localização dos recursos:**
- Agentes: `.claude/agents/`
- Comandos: `.claude/commands/`
- Hooks: `.claude/settings.json`

---

# 🌐 Global Standards

Estas regras se aplicam a TODAS as sessões e TODOS os agentes sem exceção.

## Linguagem e Comunicação
- Responda sempre no idioma do usuário (PT-BR por padrão neste workspace)
- Seja direto e objetivo — sem introduções desnecessárias
- Use formatação markdown apenas quando adiciona clareza
- Nunca use "eu acho" sem dados — sinalize hipóteses explicitamente

## Qualidade de Código
- TypeScript é preferido sobre JavaScript em projetos novos (quando aplicável à stack do projeto)
- Sempre tipifique — `any` requer justificativa explícita
- Funções devem ter responsabilidade única (SRP)
- Complexidade ciclomática máxima: 10 por função
- Nomes devem ser descritivos e em inglês (código) ou PT-BR (comentários/docs)

## Segurança (Inviolável)
- Nunca hardcode secrets, tokens ou credenciais
- Sempre valide inputs antes de processar
- Sempre trate erros — nunca `catch(e) {}`
- Nunca exponha stack traces em produção

## Fluxo de Trabalho
- Planejar antes de implementar em tarefas complexas
- Verificar código existente antes de criar novo
- Testar após cada mudança significativa
- Documentar decisões arquiteturais relevantes

---

# 📊 Code Quality Standards

Métricas e critérios de qualidade aplicados automaticamente em todas as sessões de código.

## Métricas de Qualidade

| Métrica | Limite | Ação se violado |
|---------|--------|-----------------|
| Complexidade ciclomática | ≤ 10 | Extrair funções |
| Linhas por função | ≤ 30 | Decompor |
| Parâmetros por função | ≤ 3 | Usar objeto de config |
| Profundidade de aninhamento | ≤ 3 | Guard clauses / early return |
| Duplicação de código | 0 ocorrências | Extrair abstração |
| Cobertura de testes | ≥ 80% | Adicionar testes |

## Padrões de Nomenclatura

```
Variáveis/funções:  camelCase       → userSessionToken, calculateTotal
Classes/Types:      PascalCase      → UserService, OrderItem
Constantes:         UPPER_SNAKE     → MAX_RETRY_COUNT, API_BASE_URL
Arquivos:           kebab-case      → user-service.ts, order-item.ts
Testes:             [nome].test.ts  → user-service.test.ts
```

## Estrutura de Projeto (Node.js/TypeScript)

```
src/
├── domain/          → entidades e regras de negócio
├── application/     → casos de uso / services
├── infrastructure/  → DB, APIs externas, frameworks
├── presentation/    → controllers, routes, DTOs
└── shared/          → utils, types, constants
```

## Commit Message Format

```
<type>(<scope>): <subject>

type: feat | fix | refactor | test | docs | chore | perf | ci
scope: módulo afetado (opcional)
subject: imperativo, lowercase, sem ponto final, max 72 chars

Exemplos:
feat(auth): add JWT refresh token rotation
fix(orders): prevent duplicate order creation on retry
refactor(users): extract validation to separate service
```

## Code Smells — Sempre Sinalizar

- **God Object**: classe com mais de 2 responsabilidades
- **Long Method**: função > 30 linhas
- **Magic Numbers**: números sem nome (`if (status === 3)`)
- **Dead Code**: código comentado ou nunca executado
- **Primitive Obsession**: usar string/number onde deveria ser tipo próprio
- **Feature Envy**: método que usa mais dados de outra classe que da própria
- **Shotgun Surgery**: mudança simples requer alterações em muitos arquivos

---

# 🚫 Anti-Patterns — Proibidos Neste Projeto

> Padrões que foram explicitamente banidos neste projeto com justificativa.  
> O agente NUNCA deve sugerir ou usar estes padrões.

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

## Padrões de Código Banidos

| Padrão | Motivo | Alternativa |
|--------|--------|-------------|
| `var` | Escopo confuso, use `const`/`let` | `const` por padrão |
| Callbacks aninhados | Callback hell, difícil de ler | async/await |
| `==` (loose equality) | Coerção implícita perigosa | `===` sempre |
| `Object.assign` para imutabilidade | Não é deep clone | spread operator ou structuredClone |

## Decisões de Tecnologia Banidas

| Tecnologia | Motivo do Ban | Alternativa |
|-----------|--------------|-------------|
| moment.js | Deprecated, bundle pesado | date-fns |

---

# 🤖 Agentes Disponíveis

Sintaxe: `"use o agente [nome] para [tarefa]"`

## Planejamento
| Agente | Descrição |
|--------|-----------|
| `planner` | Decompõe objetivos vagos em planos executáveis com tarefas atômicas e riscos mapeados |
| `researcher` | Investiga, compara e sintetiza informação técnica com critérios explícitos e fontes |
| `sprint-manager` | Organiza backlog, calcula capacidade real e conduz cerimônias de sprint |

## Qualidade
| Agente | Descrição |
|--------|-----------|
| `reviewer` | Audita código com olhar crítico, classifica issues por severidade e propõe correções |
| `debugger` | Diagnóstico sistemático de bugs — constrói loop de feedback antes de hipóteses |
| `test-writer` | Gera testes unitários, integração e E2E focados em comportamento e edge cases |
| `code-health` | Detecta dívida técnica, hotspots e gera plano de remediação priorizado |
| `dependency-manager` | Audita CVEs, licenças e versões desatualizadas; planeja upgrades com breaking changes |

## Arquitetura
| Agente | Descrição |
|--------|-----------|
| `architect` | Decisões de sistema com trade-offs explícitos, ADRs e análise de requisitos não-funcionais |
| `migration-guide` | Transições seguras de banco, APIs e stack com rollback garantido e zero downtime |

## Segurança
| Agente | Descrição |
|--------|-----------|
| `security-auditor` | Auditoria OWASP Top 10, threat modeling STRIDE e relatórios de vulnerabilidade acionáveis |

## Operações
| Agente | Descrição |
|--------|-----------|
| `devops` | Infraestrutura, containers, CI/CD e estratégias de deploy sem downtime |
| `performance-optimizer` | Mede baseline, identifica bottleneck real e otimiza frontend, backend e banco |
| `incident-responder` | Triagem, mitigação e post-mortem de incidentes — mitiga antes de investigar |

## Dados / Frontend / Docs / Pessoal
| Agente | Descrição |
|--------|-----------|
| `data-modeler` | Schemas relacionais e não-relacionais com normalização, índices e constraints corretos |
| `ui-reviewer` | Revisão de componentes quanto a acessibilidade WCAG 2.1 AA, render e UX |
| `doc-writer` | READMEs, JSDoc, OpenAPI, changelogs e ADRs que são lidos e mantidos |
| `onboarding` | Guias completos para novos devs — arquitetura, convenções e armadilhas conhecidas |
| `tcc-orientador` | Orientação acadêmica do tema à defesa com rigor metodológico e conformidade ABNT |

---

# ⚡ Comandos Disponíveis

Sintaxe: `/[comando]` no chat

| Comando | Descrição |
|---------|-----------|
| `/accessibility` | Auditoria e implementação de acessibilidade WCAG 2.1 AA |
| `/hard-rules` | Aplicar regras invioláveis de segurança e código |
| `/refactor` | Refatorar de forma segura e incremental |
| `/api-design` | Criar ou revisar endpoints REST |
| `/testing-strategy` | Definir estratégia de testes de um módulo |
| `/error-handling` | Implementar tratamento de erros em qualquer camada |
| `/architecture-patterns` | Tomar decisões de arquitetura com padrões estabelecidos |
| `/database-patterns` | Modelar dados ou escrever queries otimizadas |
| `/docker-best-practices` | Criar ou revisar Dockerfiles seguros e eficientes |
| `/observability` | Configurar logs estruturados, métricas e alertas |
| `/ci-cd-patterns` | Criar ou revisar pipelines de CI/CD |
| `/code-review-checklist` | Code review manual estruturado e completo |
| `/priorizacao` | Priorizar backlog, features ou decisões técnicas |
| `/output-format` | Padronizar formato das respostas do agente |
| `/prompt-engineering` | Escrever prompts mais eficazes |
| `/git-workflow` | Fluxo de trabalho com Git e PRs |
| `/adr-template` | Documentar uma decisão arquitetural |
| `/tcc-workflow` | Qualquer fase do TCC com rigor metodológico |
| `/setup-pre-commit` | Configurar validações automáticas antes de commits |
| `/caveman-mode` | Reduzir tokens ~75% mantendo precisão técnica — modo ultra-comprimido |
| `/skill-creator` | Criar novas skills seguindo o padrão do sistema |

---

# 📂 Contexto Adicional (Arquivos Manuais)

Os arquivos abaixo contêm contexto específico do projeto e **não são carregados automaticamente**.  
Mencione o arquivo no chat quando precisar que o agente conheça esse contexto.

| Arquivo | Quando usar |
|---------|-------------|
| `.claude/context/project-context.md` | Para injetar contexto do projeto atual (nome, stack, arquitetura, links) |
| `.claude/context/tech-stack.md` | Para injetar a stack com versões específicas e libs aprovadas/proibidas |
| `.claude/context/domain-glossary.md` | Para injetar o glossário de termos de domínio (linguagem ubíqua) |
| `.claude/context/team-conventions.md` | Para injetar convenções do time (estrutura de pastas, naming, processo de PR) |

**Exemplo de uso:**
```
"Leia o arquivo .claude/context/project-context.md e me ajude a planejar a feature X"
"Considerando o .claude/context/tech-stack.md, qual biblioteca devo usar para validação?"
```

---

## Pipelines Recomendados

```
Nova feature:     planner → researcher → architect → [impl] → test-writer → reviewer → security-auditor → devops
Bug em produção:  debugger → test-writer → reviewer
Decisão técnica:  researcher → architect → doc-writer (ADR)
Sprint:           sprint-manager → planner → [execução] → sprint-manager (retro)
Incidente:        incident-responder → debugger → doc-writer (post-mortem)
Saúde do projeto: code-health → dependency-manager → security-auditor → planner
```
