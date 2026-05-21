# Kiro Agent System

Sistema de agentes, skills, hooks e steering para automação com Kiro IDE.

próximas implementações: 
Claude 
Qualquer IDE

---

## Instalação

```bash
# Clone
https://github.com/ljborgess/skills

# Windows (global — todos os projetos)
mkdir "%USERPROFILE%\.kiro\skills" 2>nul & mkdir "%USERPROFILE%\.kiro\steering" 2>nul
xcopy /E /I /Y agents\* "%USERPROFILE%\.kiro\skills\agents\"
xcopy /E /I /Y skills\* "%USERPROFILE%\.kiro\skills\"
xcopy /E /I /Y steering\* "%USERPROFILE%\.kiro\steering\"

# macOS/Linux (global)
cp -r agents/ skills/ ~/.kiro/skills/
cp -r steering/ ~/.kiro/steering/

# Hooks: Kiro IDE → Explorer → Agent Hooks → importar .json da pasta hooks/
```

---

## Arquitetura

```
VOCÊ
  ↓
STEERING (auto) → contexto injetado em tudo
  ↓
AGENTES → executam com disciplina definida
  ↓
SKILLS + HOOKS → regras e automações
```

---

## Agentes por Grupo

### Planning (planejamento)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| planner | Quebra ideias em tarefas com prazo e dependências | `use o agente planner para...` |
| researcher | Compara tecnologias com dados reais | `use o agente researcher para...` |
| sprint-manager | Organiza sprint, calcula capacidade, conduz retro | `use o agente sprint-manager para...` |

### Quality (qualidade)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| reviewer | Code review com severidade e fix proposto | `use o agente reviewer para...` |
| debugger | Diagnóstico sistemático em 6 fases | `use o agente debugger para...` |
| test-writer | Gera testes com edge cases | `use o agente test-writer para...` |
| code-health | Detecta dívida técnica com métricas | `use o agente code-health para...` |
| dependency-manager | Audita CVEs, licenças, versões | `use o agente dependency-manager para...` |

### Architecture (arquitetura)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| architect | Decisões de sistema, ADRs, trade-offs | `use o agente architect para...` |
| migration-guide | Migrações sem downtime | `use o agente migration-guide para...` |

### Security (segurança)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| security-auditor | OWASP Top 10, CVEs, threat modeling | `use o agente security-auditor para...` |

### Ops (operações)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| devops | Dockerfiles, pipelines CI/CD | `use o agente devops para...` |
| performance-optimizer | Mede, identifica bottleneck, otimiza | `use o agente performance-optimizer para...` |
| incident-responder | Conduz resposta a incidentes | `use o agente incident-responder para...` |

### Data (dados)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| data-modeler | Projeta schemas com normalização e índices | `use o agente data-modeler para...` |

### Frontend (interface)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| ui-reviewer | Audita acessibilidade e performance de render | `use o agente ui-reviewer para...` |

### Docs (documentação)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| doc-writer | README, JSDoc, OpenAPI, changelog | `use o agente doc-writer para...` |
| onboarding | Guia completo para novos devs | `use o agente onboarding para...` |

### Personal (pessoal)
| Agente | O que faz | Chamar com |
|--------|-----------|------------|
| tcc-orientador | Orientação acadêmica, ABNT, TCC | `use o agente tcc-orientador para...` |

---

## Skills por Grupo

### Engineering
`hard-rules` `refactor` `code-review-checklist` `architecture-patterns` `api-design` `database-patterns` `testing-strategy` `error-handling` `accessibility` `docker-best-practices` `observability` `ci-cd-patterns`

### Productivity
`priorizacao` `output-format` `prompt-engineering` `git-workflow` `adr-template`

### Personal
`tcc-workflow`

### Misc
`setup-pre-commit`

### Meta
`skill-creator`

**Como usar:**
```
"aplique a skill refactor nesse código"
"siga a skill api-design para criar esse endpoint"
```

---

## Hooks por Grupo

### Workflow (fluxo de trabalho)
- `pre-task-review` — valida clareza antes de iniciar tarefa
- `post-task-test` — roda testes após conclusão
- `agent-stop-summary` — sumário ao fim da sessão
- `commit-message-validator` — valida Conventional Commits
- `adr-on-arch-change` — solicita ADR em mudanças arquiteturais

### Quality (qualidade)
- `file-lint-on-save` — lint automático ao salvar
- `complexity-check` — verifica complexidade ciclomática
- `test-coverage-check` — verifica se há teste correspondente
- `dead-code-detector` — detecta código morto
- `performance-budget` — verifica bundle size e N+1

### Security (segurança)
- `pre-write-security-check` — bloqueia secrets
- `dependency-audit` — verifica CVEs ao editar package.json
- `breaking-change-alert` — alerta sobre breaking changes
- `env-var-validator` — documenta variáveis de ambiente

**Como usar:** Importe os `.json` no painel Agent Hooks do Kiro.

---

## Steering (contexto global)

| Arquivo | Inclusão | Como ativar | O que injeta |
|---------|----------|-------------|--------------|
| `global-standards` | auto | — | Padrões de código e segurança |
| `code-quality` | auto | — | Métricas, naming, commits |
| `anti-patterns` | auto | — | Padrões proibidos |
| `domain-glossary` | manual | `#domain-glossary` | Termos do domínio |
| `team-conventions` | manual | `#team-conventions` | Estrutura, naming, PR |
| `project-context` | manual | `#project-context` | Contexto do projeto |
| `tech-stack` | manual | `#tech-stack` | Stack com versões |

**Preencha antes de usar:**
- `steering/project-context.md`
- `steering/tech-stack.md`
- `steering/domain-glossary.md`
- `steering/anti-patterns.md`
- `steering/team-conventions.md`

---

## Estrutura de Arquivos

```
kiro/
├── agents/
│   ├── planning/        → planner, researcher, sprint-manager
│   ├── quality/         → reviewer, debugger, test-writer, code-health, dependency-manager
│   ├── architecture/    → architect, migration-guide
│   ├── security/        → security-auditor
│   ├── ops/             → devops, performance-optimizer, incident-responder
│   ├── data/            → data-modeler
│   ├── frontend/        → ui-reviewer
│   ├── docs/            → doc-writer, onboarding
│   └── personal/        → tcc-orientador
├── skills/
│   ├── engineering/     → 12 skills
│   ├── productivity/    → 5 skills
│   ├── personal/        → tcc-workflow
│   ├── misc/            → setup-pre-commit
│   └── skill-creator/   → skill-creator
├── hooks/
│   ├── workflow/        → 5 hooks
│   ├── quality/         → 5 hooks
│   └── security/        → 4 hooks
└── steering/            → 7 arquivos
```

---

## Pipeline entre Agentes

```
Nova feature:
  planner → researcher → architect → [implementação] → 
  test-writer → reviewer → security-auditor → devops

Bug em produção:
  debugger → test-writer → reviewer

Decisão técnica:
  researcher → architect → doc-writer (ADR)

Sprint:
  sprint-manager → planner → [execução] → sprint-manager (retro)

Incidente:
  incident-responder → debugger → doc-writer (post-mortem)
```
