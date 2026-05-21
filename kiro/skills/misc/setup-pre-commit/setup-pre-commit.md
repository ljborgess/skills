# 🔧 Skill: Setup Pre-Commit

## Description
Guia completo para configurar hooks de pre-commit que garantem qualidade de código antes de cada commit. Cobre lint, formatação, testes e verificações de segurança automatizadas.

---

## Hard Rules
1. **NUNCA** use `--no-verify` para pular hooks sem justificativa documentada
2. **SEMPRE** mantenha hooks rápidos (< 30s) para não prejudicar o fluxo
3. **NUNCA** adicione verificações que geram falsos positivos frequentes
4. **SEMPRE** versione a configuração dos hooks no repositório

---

## Stack Recomendada

### Node.js / TypeScript
```bash
npm install --save-dev husky lint-staged @commitlint/cli @commitlint/config-conventional
npx husky init
```

### Python
```bash
pip install pre-commit
pre-commit install
```

---

## Configuração Completa (Node.js)

### package.json
```json
{
  "scripts": {
    "prepare": "husky"
  },
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write",
      "tsc --noEmit"
    ],
    "*.{js,jsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md,yml}": [
      "prettier --write"
    ]
  }
}
```

### .husky/pre-commit
```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged
```

### .husky/commit-msg
```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx --no -- commitlint --edit ${1}
```

### commitlint.config.js
```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [2, 'always', [
      'feat',     // nova feature
      'fix',      // correção de bug
      'refactor', // refatoração sem mudança de comportamento
      'test',     // adição/correção de testes
      'docs',     // documentação
      'chore',    // tarefas de manutenção
      'perf',     // melhoria de performance
      'ci',       // mudanças de CI/CD
      'revert',   // reverter commit
    ]],
    'subject-max-length': [2, 'always', 72],
    'subject-case': [2, 'always', 'lower-case'],
  }
}
```

---

## Configuração Python (.pre-commit-config.yaml)
```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: detect-private-key

  - repo: https://github.com/psf/black
    rev: 23.12.0
    hooks:
      - id: black

  - repo: https://github.com/pycqa/flake8
    rev: 7.0.0
    hooks:
      - id: flake8

  - repo: https://github.com/pycqa/isort
    rev: 5.13.2
    hooks:
      - id: isort
```

---

## Verificação de Segurança (Adicional)

### Detectar secrets com gitleaks
```bash
# Instalar
brew install gitleaks  # macOS
choco install gitleaks # Windows

# Adicionar ao pre-commit
cat >> .husky/pre-commit << 'EOF'
gitleaks protect --staged --redact
EOF
```

---

## Output Format

```markdown
## 🔧 Setup Pre-Commit: [projeto]

### Stack detectada: [Node/Python/outro]

### Instalação
```bash
[comandos]
```

### Arquivos criados
- `.husky/pre-commit`
- `.husky/commit-msg`
- `commitlint.config.js`

### Verificações ativas
- [ ] ESLint (erros de código)
- [ ] Prettier (formatação)
- [ ] TypeScript (tipos)
- [ ] Commitlint (mensagens de commit)
- [ ] Secret detection
```
