# 🚨 Skill: Hard Rules

## Description
Regras invioláveis que se aplicam a TODOS os agentes e sessões. Nenhum agente pode sobrescrever estas regras. São o contrato base de qualidade e segurança do sistema.

---

## Regras Universais

### 🔐 Segurança — INVIOLÁVEL
- **HR-SEC-001** Nunca hardcode secrets, tokens, passwords ou API keys
- **HR-SEC-002** Sempre use variáveis de ambiente para credenciais
- **HR-SEC-003** Nunca exponha stack traces em respostas de API em produção
- **HR-SEC-004** Sempre valide e sanitize inputs do usuário antes de processar
- **HR-SEC-005** Nunca confie em dados vindos do cliente sem validação server-side
- **HR-SEC-006** Sempre use HTTPS — nunca HTTP para dados sensíveis
- **HR-SEC-007** Nunca armazene senhas em plain text — sempre hash (bcrypt/argon2)

### 🏗️ Arquitetura — INVIOLÁVEL
- **HR-ARCH-001** Nunca misture lógica de negócio com lógica de apresentação
- **HR-ARCH-002** Nunca crie dependências circulares entre módulos
- **HR-ARCH-003** Sempre defina contratos (interfaces/types) antes de implementar
- **HR-ARCH-004** Nunca acesse banco de dados diretamente de controllers/routes
- **HR-ARCH-005** Sempre separe configuração de código (12-factor app)

### 💻 Código — INVIOLÁVEL
- **HR-CODE-001** Nunca commite código com `console.log` de debug em produção
- **HR-CODE-002** Nunca ignore erros silenciosamente (`catch(e) {}`)
- **HR-CODE-003** Sempre tipifique — nunca use `any` em TypeScript sem justificativa
- **HR-CODE-004** Nunca duplique lógica — DRY (Don't Repeat Yourself)
- **HR-CODE-005** Sempre trate o caso de erro antes do caso de sucesso (fail-fast)
- **HR-CODE-006** Nunca faça funções com mais de uma responsabilidade (SRP)
- **HR-CODE-007** Sempre use nomes descritivos — nunca `x`, `temp`, `data` genérico

### 🧪 Qualidade — INVIOLÁVEL
- **HR-QA-001** Nunca faça deploy sem testes passando
- **HR-QA-002** Nunca escreva testes que testam implementação, não comportamento
- **HR-QA-003** Sempre cubra casos de erro nos testes, não apenas o happy path
- **HR-QA-004** Nunca mocke o que você está testando

### 📝 Comunicação — INVIOLÁVEL
- **HR-COM-001** Nunca assuma requisitos ambíguos — sempre pergunte
- **HR-COM-002** Sempre documente decisões arquiteturais (ADR)
- **HR-COM-003** Nunca entregue código sem explicar o que foi feito e por quê
- **HR-COM-004** Sempre sinalize quando uma tarefa está fora do escopo acordado

---

## Violação de Hard Rules

Quando uma hard rule é violada:
1. **Pare** a execução atual
2. **Sinalize** a violação com: `⛔ HR-[CÓDIGO]: [descrição]`
3. **Explique** o risco concreto
4. **Proponha** a correção antes de continuar

```markdown
⛔ HR-SEC-001 VIOLADA
Problema: API key hardcoded em `config.ts:15`
Risco: Exposição de credencial em repositório público
Correção: Mover para variável de ambiente `API_KEY` e usar `process.env.API_KEY`
```
