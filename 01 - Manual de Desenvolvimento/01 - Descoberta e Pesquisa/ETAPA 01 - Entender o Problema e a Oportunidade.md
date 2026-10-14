# ETAPA 01 — Entender o Problema e a Oportunidade

## Objetivo

Compreender profundamente o problema que o software vai resolver, para quem, em que contexto e por que isso importa — antes de propor qualquer solução.

## Por que esta etapa existe?

A maioria dos projetos falha não por tecnologia, mas por **resolver o problema errado** ou **não entender o problema real**. Esta etapa força a clareza sobre a necessidade antes de gastar tempo em código.

## Quando começar

- ETAPA 00 concluída (pasta criada, acompanhamento ativo, registro inicial preenchido)
- Você tem uma ideia, mas ainda não validou se o problema é real e relevante

## O que você precisa fazer

1. **Descreva o problema em linguagem simples** — o que está errado hoje?
2. **Identifique quem sofre com esse problema** — personas, perfis, roles
3. **Entenda o contexto de uso** — onde, quando, como a pessoa está quando tem o problema
4. **Liste as consequências** — o que acontece se o problema NÃO for resolvido?
5. **Identifique as necessidades reais** — o que a pessoa PRECISA (não o que ela pede)
6. **Defina o objetivo do projeto** — uma frase: "Este software permite que [quem] [faça o quê] para [benefício]"
7. **Registre hipóteses iniciais** — o que você acredita ser verdade mas precisa validar
8. **Liste restrições conhecidas** — técnico, tempo, orçamento, conhecimento, legais
9. **Defina indicadores de sucesso** — como você saberá que o problema foi resolvido?
10. **Produza o documento: Definição do Problema**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Descrever problema, contexto, personas | 👤 USUÁRIO |
| Ajudar a estruturar e refinar a descrição | 🤖 IA |
| Identificar necessidades vs. pedidos | 👤 USUÁRIO + 🤖 IA |
| Definir objetivo e indicadores de sucesso | 👤 USUÁRIO |
| Validar se o problema faz sentido | ✅ VALIDAÇÃO (você mesmo, lendo com olhar crítico) |

## Ferramentas recomendadas

- **Obsidian** — para escrever o documento
- **Claude Code** — para ajudar a estruturar, fazer perguntas socráticas, refinar a redação
- **Papel/quadro** — opcional, para rascunhar antes de digitalizar

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Definição do Problema | `02 - Modelos/Pesquisa e Planejamento/Definição do Problema.md` | `05 - Projetos/SeuProjeto/01 - Descoberta/` |

### Condicionais
- Mapa de Empatia (se o problema for complexo ou envolver múltiplos perfis)
- Jornada Atual do Usuário (se o fluxo atual for complicado)

### Opcionais
- Canvas de Proposta de Valor (se quiser estruturar mais)

## Como produzir os documentos

### Definição do Problema
Use o modelo e preencha todas as seções:

**1. Problema (1-3 frases)**
> Ex: "Pequenos comerciantes perdem vendas porque não conseguem gerenciar pedidos de WhatsApp de forma organizada."

**2. Quem tem o problema (personas/perfis)**
> Ex: "Dono de loja de roupas (30-50 anos), 1-2 funcionários, usa WhatsApp Business, não tem sistema."

**3. Contexto de uso**
> Ex: "Durante o atendimento, recebe 20-50 msgs/dia, precisa anotar pedidos, conferir estoque, responder rápido."

**4. Consequências se não resolver**
> Ex: "Pedidos perdidos, clientes insatisfeitos, retrabalho manual, estresse, perda de faturamento estimado em 15%."

**5. Necessidades identificadas**
> Ex: "Centralizar pedidos, ver status, responder rápido, saber o que tem em estoque, gerar relatório simples."

**6. Objetivo do projeto**
> Ex: "Permitir que pequenos comerciantes gerenciem pedidos de WhatsApp em uma tela única, reduzindo perdas em 80%."

**7. Hipóteses iniciais**
> Ex: "H1: O maior problema é organizar pedidos, não estoque. H2: WhatsApp é o canal principal. H3: Eles pagariam R$ 50-100/mês."

**8. Restrições conhecidas**
> Ex: "Orçamento zero para infra. Uma pessoa desenvolve. Precisa rodar no celular. Sem backend complexo v1."

**9. Indicadores de sucesso**
> Ex: "Reduzir pedidos perdidos de ~15% para <3%. Tempo médio de resposta <2 min. 3 usuários ativos em 30 dias."

> [!important]
> Diferencie **necessidade** (o que resolve a dor) de **solução pedida** (o que a pessoa acha que quer).
> Ex: "Quero um app" = solução pedida. "Preciso não perder pedidos" = necessidade.

## Como trabalhar com IA nesta etapa

**Prompt para estruturar o problema:**

```
Tenho esta ideia bruta: [cole sua descrição do Registro Inicial].

Me ajude a preencher a Definição do Problema fazendo perguntas socráticas, uma por vez, para eu responder.
Não escreva o documento final — só me faça as perguntas certas para eu pensar.

Comece: "Qual é o problema central em uma frase?"
```

**Prompt para refinar necessidades:**

```
A pessoa diz que quer: "[o que ela pede]".
O contexto é: [contexto].
Quais são as necessidades REAIS por trás desse pedido? Liste 3-5 possibilidades.
```

**Contexto a fornecer:** Registro Inicial + suas anotações brutas.
**Resultado esperado:** Definição do Problema estruturada e completa.
**Como verificar:** Leia em voz alta — faz sentido para alguém que não conhece o projeto?

## Checklist de execução

- [ ] Problema descrito em 1-3 frases claras
- [ ] Personas/perfis identificados (mínimo 1, ideal 2-3)
- [ ] Contexto de uso descrito
- [ ] Consequências de não resolver listadas
- [ ] Necessidades reais separadas de soluções pedidas
- [ ] Objetivo do projeto escrito (formato: permite que [quem] [faça o quê] para [benefício])
- [ ] Hipóteses iniciais registradas (mínimo 3)
- [ ] Restrições conhecidas listadas
- [ ] Indicadores de sucesso definidos (mensuráveis quando possível)
- [ ] Documento Definição do Problema salvo na pasta do projeto

## Critérios de conclusão

- [ ] Documento existe e está completo
- [ ] Você consegue explicar o problema para outra pessoa em 30 segundos
- [ ] O objetivo do projeto é claro e direciona decisões
- [ ] Hipóteses são testáveis (podem ser validadas na ETAPA 02)
- [ ] Indicadores de sucesso permitem dizer "sim/não" no futuro

## Verificações e testes

- **Teste do elevador:** Consegue explicar o problema e o objetivo em 2 frases?
- **Teste da necessidade:** Cada item em "Necessidades" resolve algo em "Consequências"?
- **Teste da hipótese:** Cada hipótese pode ser validada com pesquisa ou experimento?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Descrever a solução, não o problema | Foque no "o que está errado", não no "o que vou construir" |
| Confundir persona com "todo mundo" | Seja específico: "dono de loja de roupas" não "comerciantes" |
| Hipóteses vagas ("vai dar certo") | Escreva hipótese falsificável: "X% dos usuários têm Y problema" |
| Indicadores subjetivos ("ficar bom") | Use números: "% de redução", "tempo médio", "número de usuários" |
| Pular esta etapa e ir direto para features | Sem problema claro, features são chute |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Problema parece genérico demais | Volte para "Quem tem o problema" — seja mais específico |
| Muitas personas, sem foco | Priorize 1 persona principal + 1 secundária |
| Objetivo não dá direção | Reformule: deve conter quem, o quê, benefício |
| Hipóteses não testáveis | Transforme em pergunta de pesquisa: "Quantos X fazem Y?" |
| Não sabe indicadores | Pergunte: "Daqui a 3 meses, como sei que valeu a pena?" |

## Condições para avançar

- ✅ Definição do Problema salva e completa
- ✅ Objetivo do projeto claro e acionável
- ✅ Hipóteses registradas e testáveis
- ✅ Indicadores de sucesso definidos
- ✅ Você consegue dizer: "Sei exatamente qual problema estou resolvendo e para quem"

## Próxima etapa

[[ETAPA 02 - Pesquisar e Validar a Ideia]]

## Materiais relacionados

- [[02 - Modelos/Pesquisa e Planejamento/Definição do Problema]]
- [[03 - Guias Práticos/UI e UX/Entender o Usuário e Suas Tarefas]]
- [[04 - Referência/Glossário/Problema]]
- [[04 - Referência/Glossário/Hipótese]]
- [[04 - Referência/Glossário/Indicador de Sucesso]]