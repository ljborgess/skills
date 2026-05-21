---
inclusion: manual
---

# 📖 Domain Glossary

> Glossário de termos de domínio para garantir linguagem ubíqua (Ubiquitous Language do DDD).  
> O agente usa estes termos exatamente como definidos aqui — sem sinônimos não listados.

---

## Como Usar Este Arquivo

Preencha com os termos específicos do seu domínio de negócio.  
Exemplo abaixo para um sistema de e-commerce — adapte para o seu contexto.

---

## Termos do Domínio

### [Termo 1]
**Definição:** [O que significa no contexto deste sistema]  
**Sinônimos aceitos:** [outros nomes que podem ser usados]  
**Sinônimos proibidos:** [nomes que NÃO devem ser usados — causam confusão]  
**Exemplo:** [exemplo concreto de uso]

---

## Exemplo Preenchido (E-commerce)

### Order (Pedido)
**Definição:** Intenção de compra confirmada pelo cliente, contendo itens, endereço e forma de pagamento.  
**Sinônimos aceitos:** pedido  
**Sinônimos proibidos:** compra, carrinho (carrinho é Cart, não Order)  
**Estados:** `pending` → `confirmed` → `shipped` → `delivered` | `cancelled`

### Cart (Carrinho)
**Definição:** Seleção temporária de produtos antes da confirmação do pedido. Não é um Order.  
**Sinônimos aceitos:** carrinho de compras  
**Sinônimos proibidos:** pedido (só vira pedido após checkout)

### Customer (Cliente)
**Definição:** Usuário que já realizou pelo menos um pedido.  
**Sinônimos proibidos:** user (User é a entidade de autenticação; Customer é a entidade de negócio)

### SKU (Stock Keeping Unit)
**Definição:** Identificador único de uma variação específica de produto (ex: camiseta azul, tamanho M).  
**Diferença de Product:** Product é o conceito (camiseta); SKU é a variação específica.

---

## Regras de Nomenclatura no Código

```ts
// ✅ Use os termos do glossário no código
class Order { ... }
class Cart { ... }
class Customer { ... }

// ❌ Não invente sinônimos
class Purchase { ... }  // use Order
class Basket { ... }    // use Cart
class Buyer { ... }     // use Customer
```

---

## Eventos de Domínio

| Evento | Quando ocorre | Dados |
|--------|--------------|-------|
| [EventName] | [quando] | [campos] |

---

## Regras de Negócio Críticas

> Regras que o agente deve conhecer para não gerar código incorreto.

1. [Regra de negócio 1 — ex: "Um pedido só pode ser cancelado se status for pending ou confirmed"]
2. [Regra de negócio 2]
3. [Regra de negócio 3]
