# Agent System — Kiro + Claude Code

Sistema de agentes, skills, hooks e contexto para automação com **Kiro IDE** e **Claude Code**.  
Os mesmos 19 agentes e 21 skills disponíveis nas duas ferramentas.

---

## Instalação

### Kiro IDE

```bash
# Windows (global — todos os projetos)
mkdir "%USERPROFILE%\.kiro\skills" 2>nul & mkdir "%USERPROFILE%\.kiro\steering" 2>nul
xcopy /E /I /Y kiro\agents\* "%USERPROFILE%\.kiro\skills\agents\"
xcopy /E /I /Y kiro\skills\* "%USERPROFILE%\.kiro\skills\"
xcopy /E /I /Y kiro\steering\* "%USERPROFILE%\.kiro\steering\"

# macOS/Linux (global)
cp -r kiro/agents/ kiro/skills/ ~/.kiro/skills/
cp -r kiro/steering/ ~/.kiro/steering/

# Hooks: Kiro IDE → Explorer → Agent Hooks → importar .json da pasta kiro/hooks/
```

### Claude Code

```bash
# Copie a pasta .claude/ e o CLAUDE.md para a raiz do seu projeto
cp -r claude/.claude/ seu-projeto/
cp claude/CLAUDE.md seu-projeto/

# Preencha os arquivos de contexto antes de usar
# .claude/context/project-context.md  → contexto do projeto
# .claude/context/tech-stack.md       → stack com versões
# .claude/context/domain-glossary.md  → termos do domínio
# .claude/context/team-conventions.md → convenções do time
```

---

## Arquitetura

### Kiro
```
VOCÊ
  ↓
STEERING (auto) → contexto injetado em tudo
  ↓
AGENTES → executam com disciplina definida
  ↓
SKILLS + HOOKS → regras e automações
```

### Claude Code
```
VOCÊ
  ↓
CLAUDE.md (auto) → contexto injetado em tudo
  ↓
AGENTS (.claude/agents/) → executam com disciplina definida
  ↓
COMMANDS (.claude/commands/) + HOOKS (settings.json) → regras e automações
```

---

## Agentes (19 — disponíveis nos dois)

### Planning
| Agente | O que faz | Kiro | Claude Code |
|--------|-----------|------|-------------|
| `planner` | Quebra objetivos em tarefas atômicas com riscos mapeados | `use o agente planner para...` | `@planner` |
| `researcher` | Compara tecnologias com critérios explícitos e fontes | `use o agente researcher para...` | `@researcher` |
| `sprint-manager` | Organiza sprint, calcula capacidade, conduz retro | `use o agente sprint-manager para...` | `@sprint-manager` |

### Quality
| Agente | O que faz | Kiro | Claude Code |
|--------|-----------|------|-------------|
| `reviewer` | Code review com severidade e fix proposto | `use o agente reviewer para...` | `@reviewer` |
| `debugger` | Diagnóstico sistemático em 6 fases com loop de feedback | `use o agente debugger para...` | `@debugger` |
| `test-writer` | Gera testes focados em comportamento e edge cases | `use o agente test-writer para...` | `@test-writer` |
| `code-health` | Detecta dívida técnica e hotspots com métricas | `use o agente code-health para...` | `@code-health` |
| `dependency-manager` | Audita CVEs, licenças e planeja upgrades | `use o agente dependency-manager para...` | `@dependency-manager` |

### Architecture
| Agente | O que faz | Kiro | Claude Code |
|--------|-----------|------|-------------|
| `architect` | Decisões de sistema, ADRs, trade-offs explícitos | `use o agente architect para...` | `@architect` |
| `migration-guide` | Migrações seguras com rollback e zero downtime | `use o agente migration-guide para...` | `@migration-guide` |

### Security
| Agente | O que faz | Kiro | Claude Code |
|--------|-----------|------|-------------|
| `security-auditor` | OWASP Top 10, CVEs, threat modeling STRIDE | `use o agente security-auditor para...` | `@security-auditor` |

### Ops
| Agente | O que faz | Kiro | Claude Code |
|--------|-----------|------|-------------|
| `devops` | Dockerfiles, pipelines CI/CD, deploy sem downtime | `use o agente devops para...` | `@devops` |
| `performance-optimizer` | Mede baseline, identifica bottleneck, otimiza | `use o agente performance-optimizer para...` | `@performance-optimizer` |
| `incident-responder` | Triagem, mitigação e post-mortem de incidentes | `use o agente incident-responder para...` | `@incident-responder` |

### Data / Frontend / Docs / Personal
| Agente | O que faz | Kiro | Claude Code |
|--------|-----------|------|-------------|
| `data-modeler` | Schemas com normalização, índices e constraints | `use o agente data-modeler para...` | `@data-modeler` |
| `ui-reviewer` | Acessibilidade WCAG 2.1 AA, render e UX | `use o agente ui-reviewer para...` | `@ui-reviewer` |
| `doc-writer` | README, JSDoc, OpenAPI, changelog, ADRs | `use o agente doc-writer para...` | `@doc-writer` |
| `onboarding` | Guia completo para novos devs | `use o agente onboarding para...` | `@onboarding` |
| `tcc-orientador` | Orientação acadêmica, ABNT, do tema à defesa | `use o agente tcc-orientador para...` | `@tcc-orientador` |

---

## Skills / Commands (21 — disponíveis nos dois)

| Skill | Kiro | Claude Code |
|-------|------|-------------|
| `hard-rules` | `aplique a skill hard-rules` | `/hard-rules` |
| `refactor` | `aplique a skill refactor` | `/refactor` |
| `api-design` | `siga a skill api-design` | `/api-design` |
| `testing-strategy` | `siga a skill testing-strategy` | `/testing-strategy` |
| `error-handling` | `siga a skill error-handling` | `/error-handling` |
| `accessibility` | `siga a skill accessibility` | `/accessibility` |
| `architecture-patterns` | `siga a skill architecture-patterns` | `/architecture-patterns` |
| `database-patterns` | `siga a skill database-patterns` | `/database-patterns` |
| `docker-best-practices` | `siga a skill docker-best-practices` | `/docker-best-practices` |
| `observability` | `siga a skill observability` | `/observability` |
| `ci-cd-patterns` | `siga a skill ci-cd-patterns` | `/ci-cd-patterns` |
| `code-review-checklist` | `siga a skill code-review-checklist` | `/code-review-checklist` |
| `priorizacao` | `siga a skill priorizacao` | `/priorizacao` |
| `output-format` | `siga a skill output-format` | `/output-format` |
| `prompt-engineering` | `siga a skill prompt-engineering` | `/prompt-engineering` |
| `git-workflow` | `siga a skill git-workflow` | `/git-workflow` |
| `adr-template` | `siga a skill adr-template` | `/adr-template` |
| `caveman-mode` | `siga a skill caveman-mode` | `/caveman-mode` |
| `tcc-workflow` | `siga a skill tcc-workflow` | `/tcc-workflow` |
| `setup-pre-commit` | `siga a skill setup-pre-commit` | `/setup-pre-commit` |
| `skill-creator` | `siga a skill skill-creator` | `/skill-creator` |

---

## Hooks (14 — Kiro) / Hooks (14 — Claude Code)

| Hook | O que faz | Kiro | Claude Code |
|------|-----------|------|-------------|
| `pre-write-security-check` | Bloqueia secrets e padrões inseguros antes de escrever | `.json` | `PreToolUse: Write` |
| `breaking-change-alert` | Alerta sobre breaking changes em contratos públicos | `.json` | `PreToolUse: Write` |
| `commit-message-validator` | Valida Conventional Commits | `.json` | `PreToolUse: Bash(git commit*)` |
| `file-lint-on-save` | Lint e type check ao salvar | `.json` | `PostToolUse: Write` |
| `complexity-check` | Alerta se função ultrapassa complexidade > 10 | `.json` | `PostToolUse: Write` |
| `dependency-audit` | Audita CVEs ao editar package.json | `.json` | `PostToolUse: Write` |
| `env-var-validator` | Garante documentação de variáveis de ambiente | `.json` | `PostToolUse: Write` |
| `adr-on-arch-change` | Solicita ADR em mudanças arquiteturais | `.json` | `PostToolUse: Write` |
| `test-coverage-check` | Verifica cobertura de testes após tarefa | `.json` | `Stop` |
| `dead-code-detector` | Detecta código morto ao fim da sessão | `.json` | `Stop` |
| `performance-budget` | Verifica N+1 e bundle após tarefa | `.json` | `Stop` |
| `post-task-test` | Roda testes após conclusão de tarefa | `.json` | `Stop` |
| `agent-stop-summary` | Sumário ao fim da sessão | `.json` | `Stop` |
| `pre-task-review` | Valida clareza antes de iniciar tarefa | `.json` | `UserPromptSubmit` |

---

## Contexto Global

### Kiro — Steering
| Arquivo | Inclusão | Como ativar |
|---------|----------|-------------|
| `global-standards` | auto | — |
| `code-quality` | auto | — |
| `anti-patterns` | auto | — |
| `domain-glossary` | manual | `#domain-glossary` |
| `team-conventions` | manual | `#team-conventions` |
| `project-context` | manual | `#project-context` |
| `tech-stack` | manual | `#tech-stack` |

### Claude Code — CLAUDE.md + Context
| Arquivo | Inclusão | Como ativar |
|---------|----------|-------------|
| `CLAUDE.md` | auto | — |
| `.claude/context/domain-glossary.md` | manual | mencionar no chat |
| `.claude/context/team-conventions.md` | manual | mencionar no chat |
| `.claude/context/project-context.md` | manual | mencionar no chat |
| `.claude/context/tech-stack.md` | manual | mencionar no chat |

**Preencha antes de usar:** `project-context`, `tech-stack`, `domain-glossary`, `team-conventions`

---

## Estrutura de Arquivos

```
skills/
├── README.md
├── kiro/
│   ├── agents/
│   │   ├── planning/        → planner, researcher, sprint-manager
│   │   ├── quality/         → reviewer, debugger, test-writer, code-health, dependency-manager
│   │   ├── architecture/    → architect, migration-guide
│   │   ├── security/        → security-auditor
│   │   ├── ops/             → devops, performance-optimizer, incident-responder
│   │   ├── data/            → data-modeler
│   │   ├── frontend/        → ui-reviewer
│   │   ├── docs/            → doc-writer, onboarding
│   │   └── personal/        → tcc-orientador
│   ├── skills/
│   │   ├── engineering/     → 12 skills
│   │   ├── productivity/    → 6 skills
│   │   ├── personal/        → tcc-workflow
│   │   ├── misc/            → setup-pre-commit
│   │   └── skill-creator/   → skill-creator
│   ├── hooks/
│   │   ├── workflow/        → 5 hooks
│   │   ├── quality/         → 5 hooks
│   │   └── security/        → 4 hooks
│   └── steering/            → 7 arquivos
└── claude/
    ├── CLAUDE.md            ← contexto global (auto)
    └── .claude/
        ├── settings.json    ← hooks portados
        ├── context/         → project-context, tech-stack, domain-glossary, team-conventions
        ├── agents/
        │   ├── planning/    → planner, researcher, sprint-manager
        │   ├── quality/     → reviewer, debugger, test-writer, code-health, dependency-manager
        │   ├── architecture/→ architect, migration-guide
        │   ├── security/    → security-auditor
        │   ├── ops/         → devops, performance-optimizer, incident-responder
        │   ├── data/        → data-modeler
        │   ├── frontend/    → ui-reviewer
        │   ├── docs/        → doc-writer, onboarding
        │   └── personal/    → tcc-orientador
        └── commands/
            ├── engineering/ → 12 commands
            ├── productivity/→ 6 commands
            ├── personal/    → tcc-workflow
            ├── misc/        → setup-pre-commit
            └── meta/        → skill-creator
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
