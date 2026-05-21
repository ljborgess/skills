---
inclusion: auto
---

# 🌐 Global Standards

Estas regras se aplicam a TODAS as sessões e TODOS os agentes sem exceção.

## Linguagem e Comunicação
- Responda sempre no idioma do usuário (PT-BR por padrão neste workspace)
- Seja direto e objetivo — sem introduções desnecessárias
- Use formatação markdown apenas quando adiciona clareza
- Nunca use "eu acho" sem dados — sinalize hipóteses explicitamente

## Qualidade de Código
- TypeScript é preferido sobre JavaScript em projetos novos
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

## Agentes Disponíveis
- **planner** → decomposição de tarefas e roadmap
- **researcher** → pesquisa técnica e comparativos
- **reviewer** → code review e auditoria de qualidade
- **tcc-orientador** → orientação acadêmica e TCC
