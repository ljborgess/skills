# Como usar — Kiro Agent System

---

## Agentes

Sintaxe universal: `"use o agente [nome] para [tarefa]"`

### Planejamento

| Agente | Quando usar | Exemplo |
|--------|-------------|---------|
| `planner` | Tarefa > 2h ou escopo indefinido | `"use o agente planner para estruturar a feature de pagamentos"` |
| `researcher` | Antes de escolher tecnologia ou biblioteca | `"use o agente researcher para comparar React Query com SWR"` |
| `sprint-manager` | Início de sprint ou backlog desorganizado | `"use o agente sprint-manager para planejar o sprint 12"` |

### Qualidade

| Agente | Quando usar | Exemplo |
|--------|-------------|---------|
| `reviewer` | Antes de qualquer merge | `"use o agente reviewer para revisar o módulo de autenticação"` |
| `debugger` | Bug difícil de reproduzir ou causa raiz obscura | `"use o agente debugger para diagnosticar esse erro: [stack trace]"` |
| `test-writer` | Após implementação nova ou cobertura baixa | `"use o agente test-writer para cobrir o UserService"` |
| `code-health` | Antes de grandes features ou periodicamente | `"use o agente code-health para avaliar o módulo de pagamentos"` |
| `dependency-manager` | Antes de releases ou após alertas de segurança | `"use o agente dependency-manager para auditar as dependências"` |

### Arquitetura

| Agente | Quando usar | Exemplo |
|--------|-------------|---------|
| `architect` | Decisão difícil de reverter | `"use o agente architect para decidir entre monolito e microserviços"` |
| `migration-guide` | Migração de dados ou upgrade com breaking changes | `"use o agente migration-guide para renomear a coluna user_name sem downtime"` |

### Segurança

| Agente | Quando usar | Exemplo |
|--------|-------------|---------|
| `security-auditor` | Antes de deploy de feature sensível | `"use o agente security-auditor para auditar o módulo de autenticação"` |

### Operações

| Agente | Quando usar | Exemplo |
|--------|-------------|---------|
| `devops` | Infraestrutura, containers ou CI/CD | `"use o agente devops para criar o Dockerfile do serviço de auth"` |
| `performance-optimizer` | Algo está lento — sempre com baseline medido | `"use o agente performance-optimizer para investigar por que a página carrega em 8s"` |
| `incident-responder` | Incidente ativo (P0/P1) ou post-mortem | `"use o agente incident-responder — sistema de pagamentos está fora"` |

### Dados / Frontend / Docs / Pessoal

| Agente | Quando usar | Exemplo |
|--------|-------------|---------|
| `data-modeler` | Criar ou revisar schemas e índices | `"use o agente data-modeler para modelar o schema de pedidos"` |
| `ui-reviewer` | Após implementar componentes ou revisar PRs de frontend | `"use o agente ui-reviewer para auditar a acessibilidade da página de login"` |
| `doc-writer` | Qualquer documentação técnica | `"use o agente doc-writer para criar o README do projeto"` |
| `onboarding` | Novo dev no time ou projeto sem documentação de entrada | `"use o agente onboarding para criar o guia de onboarding do projeto X"` |
| `tcc-orientador` | Qualquer etapa do TCC | `"use o agente tcc-orientador para revisar minha introdução"` |

---

## Skills

Sintaxe: `"aplique a skill [nome] nesse código"` ou `"siga a skill [nome] para [tarefa]"`

| Skill | Quando usar |
|-------|-------------|
| `hard-rules` | Aplicar regras invioláveis de segurança e código |
| `refactor` | Refatorar de forma segura e incremental |
| `api-design` | Criar ou revisar endpoints REST |
| `testing-strategy` | Definir estratégia de testes de um módulo |
| `error-handling` | Implementar tratamento de erros em qualquer camada |
| `accessibility` | Revisar ou implementar componentes de interface |
| `architecture-patterns` | Tomar decisões de arquitetura |
| `database-patterns` | Modelar dados ou escrever queries |
| `docker-best-practices` | Criar ou revisar Dockerfiles |
| `observability` | Configurar logs, métricas ou alertas |
| `ci-cd-patterns` | Criar ou revisar pipelines de CI/CD |
| `code-review-checklist` | Code review manual estruturado |
| `priorizacao` | Priorizar backlog, features ou decisões técnicas |
| `output-format` | Padronizar formato das respostas |
| `prompt-engineering` | Escrever prompts mais eficazes |
| `git-workflow` | Fluxo de trabalho com Git e PRs |
| `adr-template` | Documentar uma decisão arquitetural |
| `tcc-workflow` | Qualquer fase do TCC |
| `setup-pre-commit` | Configurar validações automáticas antes de commits |
| `caveman-mode` | Reduzir tokens ~75% mantendo precisão técnica — modo ultra-comprimido |
| `skill-creator` | Criar novas skills seguindo o padrão do sistema |

---

## Hooks

**Como instalar:** Kiro IDE → Explorer → Agent Hooks → importar o `.json` da pasta `hooks/`

### Workflow
| Hook | Dispara quando | O que faz |
|------|---------------|-----------|
| `pre-task-review` | Antes de iniciar tarefa de spec | Valida objetivo, escopo e dependências — para se algo estiver ambíguo |
| `post-task-test` | Após concluir tarefa de spec | Roda testes dos arquivos modificados e verifica regressões |
| `agent-stop-summary` | Ao encerrar a sessão | Gera sumário: o que foi feito, decisões tomadas e próximos passos |
| `commit-message-validator` | Ao criar um commit | Valida padrão Conventional Commits |
| `adr-on-arch-change` | Ao editar arquivos de arquitetura | Solicita criação de ADR para a mudança |

### Quality
| Hook | Dispara quando | O que faz |
|------|---------------|-----------|
| `file-lint-on-save` | Ao salvar `.ts/.tsx/.js/.jsx` | Verifica erros de TypeScript, ESLint e hard rules |
| `complexity-check` | Ao salvar arquivos de código | Alerta se função ultrapassa complexidade > 10 ou > 30 linhas |
| `test-coverage-check` | Após criar ou editar código | Verifica se existe teste correspondente |
| `dead-code-detector` | Ao salvar arquivos de código | Detecta funções, variáveis e imports não utilizados |
| `performance-budget` | Ao editar código | Verifica padrões de N+1 queries ou bundle excessivo |

### Security
| Hook | Dispara quando | O que faz |
|------|---------------|-----------|
| `pre-write-security-check` | Antes de qualquer escrita | Bloqueia se detectar secrets hardcoded ou padrões inseguros |
| `dependency-audit` | Ao editar `package.json`, `requirements.txt`, etc. | Audita CVEs, licenças e dependências sem manutenção |
| `breaking-change-alert` | Ao editar interfaces ou contratos públicos | Alerta sobre breaking changes para consumidores |
| `env-var-validator` | Ao editar arquivos de configuração | Verifica se variáveis de ambiente estão documentadas |

---

## Steering

Os arquivos `auto` já estão ativos — nenhuma ação necessária.  
Para os `manual`, use `#` no chat:

```
#project-context    → injeta contexto do projeto atual
#tech-stack         → injeta stack com versões específicas
```

**Preencha antes de usar** (edite os arquivos em `steering/`):

| Arquivo | O que colocar |
|---------|---------------|
| `project-context.md` | Nome, tipo, stack, arquitetura, links úteis |
| `tech-stack.md` | Tecnologias e versões em uso |
| `domain-glossary.md` | Termos do domínio de negócio |
| `team-conventions.md` | Estrutura de pastas, naming, processo de PR |
| `anti-patterns.md` | Padrões que o time decidiu evitar |

---

## Pipelines recomendados

```
Nova feature:     planner → researcher → architect → [impl] → test-writer → reviewer → security-auditor → devops
Bug em produção:  debugger → test-writer → reviewer
Decisão técnica:  researcher → architect → doc-writer (ADR)
Sprint:           sprint-manager → planner → [execução] → sprint-manager (retro)
Incidente:        incident-responder → debugger → doc-writer (post-mortem)
Saúde do projeto: code-health → dependency-manager → security-auditor → planner
```
