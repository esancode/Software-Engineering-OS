# Visão Geral das Etapas

> Detalhes de cada uma das 17 etapas do manual de desenvolvimento.

---

## Índice rápido

| Etapa | Título | Link |
|-------|--------|------|
| 00 | Iniciar um novo projeto | [[ETAPA 00 - Início do Projeto]] |
| 01 | Entender o problema e a oportunidade | [[ETAPA 01 - Entender o Problema e a Oportunidade]] |
| 02 | Pesquisar e validar a ideia | [[ETAPA 02 - Pesquisar e Validar a Ideia]] |
| 03 | Definir o produto e seu escopo | [[ETAPA 03 - Definir o Produto e seu Escopo]] |
| 04 | Organizar os requisitos | [[ETAPA 04 - Organizar os Requisitos]] |
| 05 | Planejar a arquitetura e a solução técnica | [[ETAPA 05 - Planejar a Arquitetura e a Solução Técnica]] |
| 06 | Planejar a experiência do usuário (UX) | [[ETAPA 06 - Planejar a Experiência do Usuário (UX)]] |
| 07 | Pesquisar referências e definir a direção visual | [[ETAPA 07 - Pesquisar Referências e Definir a Direção Visual]] |
| 08 | Estruturar a interface (UI) | [[ETAPA 08 - Estruturar a Interface (UI)]] |
| 09 | Preparar o ambiente e o plano de implementação | [[ETAPA 09 - Preparar o Ambiente e o Plano de Implementação]] |
| 10 | Desenvolver o software por partes | [[ETAPA 10 - Desenvolver o Software por Partes]] |
| 11 | Executar testes e garantir a qualidade | [[ETAPA 11 - Executar Testes e Garantir a Qualidade]] |
| 12 | Revisar segurança, privacidade e confiabilidade | [[ETAPA 12 - Revisar Segurança, Privacidade e Confiabilidade]] |
| 13 | Validar o produto completo | [[ETAPA 13 - Validar o Produto Completo]] |
| 14 | Publicar o software | [[ETAPA 14 - Publicar o Software]] |
| 15 | Monitorar e manter | [[ETAPA 15 - Monitorar e Manter]] |
| 16 | Encerrar ou transferir o projeto | [[ETAPA 16 - Encerrar ou Transferir o Projeto]] |

---

## Estrutura padrão de cada etapa

Todas as notas de etapa seguem este modelo:

```
# ETAPA XX — Nome da Etapa

## Objetivo
## Por que esta etapa existe?
## Quando começar (pré-requisitos)
## O que você precisa fazer (ações numeradas)
## Quem faz o quê (Usuário / IA / Validação)
## Ferramentas recomendadas
## Documentos necessários (Obrigatórios / Condicionais / Opcionais)
## Como produzir os documentos
## Como trabalhar com IA nesta etapa
## Checklist de execução
## Critérios de conclusão
## Verificações e testes
## Erros comuns
## Se algo der errado
## Condições para avançar
## Próxima etapa (link)
## Materiais relacionados (links para modelos/guias)
```

---

## Resumo do fluxo por fase

### Fase 0: Início (1 etapa)
**ETAPA 00** — Preparar o terreno: criar estrutura, registrar ideia, definir rigor.

### Fase 1: Descoberta e Pesquisa (2 etapas)
**ETAPA 01** — Entender profundamente o problema antes de solucionar.
**ETAPA 02** — Validar se vale a pena construir (pesquisa, concorrentes, usuários).

### Fase 2: Produto e Requisitos (2 etapas)
**ETAPA 03** — Transformar ideia em definição de produto (MVP, prioridades, escopo).
**ETAPA 04** — Especificar comportamento esperado (funcionais, qualidade, aceitação).

### Fase 3: Arquitetura e Planejamento (1 etapa)
**ETAPA 05** — Decidir tech stack, dados, APIs, estrutura de código, riscos técnicos.

### Fase 4: UX e UI (3 etapas)
**ETAPA 06** — Jornadas, fluxos, navegação, tarefas do usuário.
**ETAPA 07** — Referências visuais, direção estética, cores, tipografia.
**ETAPA 08** — Telas, componentes, design system proporcional, estados.

### Fase 5: Preparação e Desenvolvimento (2 etapas)
**ETAPA 09** — Ambiente, repo, config, plano de implementação, contexto para IA.
**ETAPA 10** — Implementar incremental: contexto → IA → revisar → testar → registrar.

### Fase 6: Testes e Qualidade (1 etapa)
**ETAPA 11** — Testes proporcionais: unit, integração, E2E, manual, acessibilidade, segurança.

### Fase 7: Segurança e Validação (2 etapas)
**ETAPA 12** — Auth, dados, deps, config, validação, monitoramento, recuperação.
**ETAPA 13** — Conferir produto vs. planejado: requisitos, fluxos, aceitação.

### Fase 8: Publicação e Manutenção (3 etapas)
**ETAPA 14** — Deploy, verificação pós-deploy, smoke tests, rollback.
**ETAPA 15** — Monitoramento, manutenção, atualizações, melhorias contínuas.
**ETAPA 16** — Documentação final, handoff, lições aprendidas, encerramento.

---

## Como ler as etapas

1. **Comece pelo Objetivo** — entenda o propósito
2. **Verifique Quando começar** — confirme pré-requisitos
3. **Leia O que você precisa fazer** — sequência de ações
4. **Consulte Quem faz o quê** — saiba o que delegar e o que decidir
5. **Use Documentos necessários** — copie modelos da pasta `02 - Modelos/`
6. **Execute o Checklist** — marque conforme avança
7. **Confira Critérios de conclusão** — só avance se atendidos
8. **Clique em Próxima etapa** — continue o fluxo

---

## Navegação

- [[Fluxo Completo]] — Ver mapa visual e tabela completa
- [[Início]] — Voltar ao começo
- [[02 - Modelos/Índice de Modelos]] — Catálogo de modelos
- [[03 - Guias Práticos/Índice de Guias]] — Catálogo de guias práticos