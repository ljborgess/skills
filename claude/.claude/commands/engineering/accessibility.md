# ♿ Skill: Accessibility (WCAG 2.1 AA)

## Description
Guia de acessibilidade para interfaces web seguindo WCAG 2.1 nível AA. Cobre os padrões mais impactantes, componentes acessíveis e como testar. Acessibilidade não é opcional — é requisito legal em muitos países e melhora a experiência para todos.

---

## Hard Rules
1. **NUNCA** use apenas cor para transmitir informação
2. **SEMPRE** forneça texto alternativo para imagens informativas
3. **NUNCA** remova o outline de foco sem substituir por alternativa visível
4. **SEMPRE** garanta contraste mínimo de 4.5:1 para texto normal
5. **NUNCA** use `tabindex > 0` — quebra a ordem natural de foco
6. **SEMPRE** associe labels a inputs com `for`/`id` ou `aria-labelledby`
7. **NUNCA** crie componentes interativos apenas com `div`/`span` sem ARIA
8. **SEMPRE** teste navegação por teclado antes de considerar pronto

---

## WCAG 2.1 AA — Critérios Principais

### Perceptível
- **1.1.1** Alt text em imagens informativas
- **1.3.1** Estrutura semântica (headings, listas, tabelas)
- **1.3.3** Instruções não dependem apenas de forma/cor/posição
- **1.4.1** Cor não é o único meio de transmitir informação
- **1.4.3** Contraste de texto: 4.5:1 (normal), 3:1 (grande)
- **1.4.4** Texto redimensionável até 200% sem perda de conteúdo
- **1.4.10** Reflow: conteúdo funciona em 320px de largura

### Operável
- **2.1.1** Toda funcionalidade acessível por teclado
- **2.1.2** Sem armadilhas de teclado (foco não fica preso)
- **2.4.3** Ordem de foco lógica e consistente
- **2.4.4** Texto do link descreve o destino (não "clique aqui")
- **2.4.7** Foco visível em todos os elementos interativos

### Compreensível
- **3.1.1** Idioma da página declarado (`lang="pt-BR"`)
- **3.2.1** Foco não causa mudança de contexto inesperada
- **3.3.1** Erros de formulário identificados e descritos
- **3.3.2** Labels ou instruções em todos os inputs

### Robusto
- **4.1.1** HTML válido (sem erros de parsing)
- **4.1.2** Nome, função e valor para todos os componentes UI
- **4.1.3** Mensagens de status acessíveis (aria-live)

---

## Componentes Acessíveis

### Botão
```tsx
// ✅ Botão semântico
<button type="button" onClick={handleClick}>
  Salvar alterações
</button>

// ✅ Botão com ícone
<button type="button" aria-label="Fechar modal">
  <XIcon aria-hidden="true" />
</button>

// ❌ Div como botão (sem semântica, sem teclado)
<div onClick={handleClick}>Salvar</div>
```

### Formulário
```tsx
// ✅ Input com label associado
<div>
  <label htmlFor="email">
    Email <span aria-hidden="true">*</span>
    <span className="sr-only">(obrigatório)</span>
  </label>
  <input
    id="email"
    type="email"
    required
    aria-describedby={error ? 'email-error' : undefined}
    aria-invalid={!!error}
  />
  {error && (
    <span id="email-error" role="alert">
      {error}
    </span>
  )}
</div>
```

### Modal/Dialog
```tsx
// ✅ Modal acessível
function Modal({ isOpen, onClose, title, children }) {
  const firstFocusRef = useRef<HTMLButtonElement>(null)

  useEffect(() => {
    if (isOpen) firstFocusRef.current?.focus()
  }, [isOpen])

  return (
    <div
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-title"
      // Trap focus dentro do modal
    >
      <h2 id="modal-title">{title}</h2>
      {children}
      <button ref={firstFocusRef} onClick={onClose}>
        Fechar
      </button>
    </div>
  )
}
```

### Notificações (aria-live)
```tsx
// ✅ Anúncio para screen readers
<div aria-live="polite" aria-atomic="true" className="sr-only">
  {statusMessage} {/* "Salvo com sucesso" aparece aqui */}
</div>

// aria-live="polite" → anuncia quando usuário para de interagir
// aria-live="assertive" → anuncia imediatamente (use para erros críticos)
```

---

## CSS Utilitário

```css
/* Screen reader only — visualmente oculto mas acessível */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* Foco visível customizado */
:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
  border-radius: 2px;
}

/* Nunca remova o foco sem substituir */
/* ❌ */ :focus { outline: none; }
/* ✅ */ :focus:not(:focus-visible) { outline: none; }
```

---

## Checklist de Teste

### Teclado
- [ ] Tab navega por todos os elementos interativos
- [ ] Shift+Tab navega em ordem reversa
- [ ] Enter/Space ativa botões e links
- [ ] Escape fecha modais e dropdowns
- [ ] Setas navegam em menus e selects
- [ ] Foco não fica preso em nenhum componente

### Screen Reader (NVDA/VoiceOver)
- [ ] Imagens têm alt text significativo
- [ ] Formulários anunciam labels e erros
- [ ] Modais anunciam título ao abrir
- [ ] Notificações são anunciadas automaticamente
- [ ] Headings criam estrutura navegável

### Visual
- [ ] Contraste 4.5:1 para texto normal
- [ ] Contraste 3:1 para texto grande (18px+ ou 14px+ bold)
- [ ] Foco visível em todos os elementos
- [ ] Funciona com zoom de 200%
- [ ] Funciona em 320px de largura

---

## Output Format

```markdown
## ♿ Accessibility Review: [componente/página]

### Issues Encontrados
| Critério WCAG | Severidade | Elemento | Correção |
|---------------|-----------|---------|---------|
| 1.4.3 Contraste | AA | `.btn-secondary` | Mudar cor de #999 para #767676 |

### Código Corrigido
[antes → depois]

### Teste Manual Necessário
- [ ] Testar com NVDA/VoiceOver
- [ ] Testar navegação por teclado
```
