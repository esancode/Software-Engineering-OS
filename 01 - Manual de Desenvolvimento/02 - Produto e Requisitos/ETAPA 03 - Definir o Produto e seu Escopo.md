# ETAPA 03 — Definir o Produto e seu Escopo

## Objetivo

Transformar a ideia validada em uma **definição clara do produto** que será construído: objetivos, público, proposta de valor, MVP, prioridades e limites.

## Por que esta etapa existe?

Sem definição clara, o projeto sofre de *scope creep*, funcionalidades desnecessárias, decisões inconsistentes e desperdício. Esta etapa cria o "contrato" do que será construído.

## Quando começar

- ETAPA 02 concluída com decisão **CONTINUAR**
- Pesquisa documentada, necessidades validadas, riscos conhecidos
- Você sabe *para quem* e *qual problema* resolve

## O que você precisa fazer

1. **Defina os objetivos do produto** — 3-5 objetivos mensuráveis alinhados aos indicadores da ETAPA 01
2. **Descreva o público-alvo** — persona principal + secundárias (baseado na pesquisa)
3. **Escreva a proposta de valor** — uma frase: "Para [público], [produto] é a [categoria] que [benefício único] ao contrário de [alternativa]"
4. **Liste as principais tarefas do usuário** — o que a pessoa precisa conseguir fazer (Jobs to be Done)
5. **Defina funcionalidades necessárias** — derive das tarefas + necessidades validadas
6. **Defina funcionalidades FORA do escopo** — o que NÃO será feito agora (crítico para foco)
7. **Defina o MVP (Minimum Viable Product)** — mínimo para validar a hipótese principal com usuários reais
8. **Priorize funcionalidades** — use MoSCoW (Must/Should/Could/Won't) ou RICE
9. **Defina critérios de sucesso do produto** — métricas que definem "produto funcionando"
10. **Liste restrições e limitações** — técnico, tempo, orçamento, regulatório
11. **Registre riscos conhecidos** — do que você sabe agora
12. **Defina critérios para mudanças de escopo** — como aprovar adições depois
13. **Produza os documentos: Definição do Produto + Escopo e Limites + Priorização**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Definir objetivos, público, proposta de valor | 👤 USUÁRIO |
| Listar tarefas do usuário (Jobs to be Done) | 👤 USUÁRIO + 🤖 IA |
| Derivar funcionalidades das necessidades | 🤖 IA (você aprova) |
| Definir MVP e priorização | 👤 USUÁRIO (decisão de negócio) |
| Definir fora de escopo | 👤 USUÁRIO |
| Validar se escopo é realista | ✅ VALIDAÇÃO (com input técnico da ETAPA 05 depois) |

## Ferramentas recomendadas

- **Obsidian** — documentos
- **Claude Code** — ajudar a derivar features, sugerir priorização, redigir propostas de valor
- **Framework de priorização** — MoSCoW (simples) ou RICE (mais rigoroso)

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Definição do Produto | `02 - Modelos/Pesquisa e Planejamento/Definição do Produto.md` | `05 - Projetos/SeuProjeto/02 - Produto/` |
| Escopo e Limites | `02 - Modelos/Pesquisa e Planejamento/Escopo e Limites.md` | `05 - Projetos/SeuProjeto/02 - Produto/` |
| Priorização de Funcionalidades | `02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades.md` | `05 - Projetos/SeuProjeto/02 - Produto/` |

### Condicionais
- Canvas de Proposta de Valor (se quiser mais estrutura)
- Matriz de Rastreabilidade Requisito-Feature (projetos maiores)

### Opcionais
- Roadmap de alto nível (próximas versões pós-MVP)

## Como produzir os documentos

### Definição do Produto

**1. Objetivos do Produto (3-5, mensuráveis)**
| Objetivo | Métrica | Meta | Prazo |
|----------|---------|------|-------|
| Reduzir pedidos perdidos | % pedidos perdidos | < 3% | 3 meses |
| Agilizar resposta | Tempo médio resposta | < 2 min | 1 mês |
| Validar disposição a pagar | % usuários pagando | > 20% | 3 meses |

**2. Público-Alvo**
| Persona | Descrição | Necessidade Principal | Prioridade |
|---------|-----------|----------------------|------------|
| Dono de loja roupas | 30-50 anos, 1-2 func, WhatsApp Business | Organizar pedidos sem perder | Principal |
| Funcionário de loja | 20-35 anos, usa celular da loja | Ver pedidos pendentes rápido | Secundária |

**3. Proposta de Valor**
> "Para donos de micro-lojas que vendem por WhatsApp, o **PedidosSimples** é o gerenciador de pedidos que centraliza tudo em uma tela no celular, ao contrário de planilhas ou apps complexos caros."

**4. Tarefas do Usuário (Jobs to be Done)**
| Job | Contexto | Outcome Esperado |
|-----|----------|------------------|
| Ver pedidos novos | Recebe notificação | Lista clara em < 2 seg |
| Marcar como preparado | Cozinhando/separando | Status atualizado instantâneo |
| Responder cliente | Cliente pergunta status | Resposta com info real em < 30 seg |
| Fechar pedido | Entregue/retirado | Registro automático + relatório |

**5. Funcionalidades Necessárias (derivadas dos Jobs)**
- F1: Receber pedidos via WhatsApp (integração API)
- F2: Lista de pedidos com status (novo, preparando, pronto, entregue)
- F3: Detalhes do pedido (itens, observações, cliente)
- F4: Atualizar status (botões rápidos)
- F5: Responder cliente via app (template + envio WhatsApp)
- F6: Relatório diário/semanal simples
- F7: Cadastro básico de produtos/preços
- F8: Configuração de horário de funcionamento

### Escopo e Limites

**DENTRO do escopo (MVP v1)**
- [x] F1-F8 acima
- [x] Autenticação simples (email/senha ou magic link)
- [x] Um estabelecimento por conta
- [x] Interface mobile-first (PWA)
- [x] Dados locais + sync simples

**FORA do escopo (v1) — explícito para evitar scope creep**
- [ ] Múltiplos estabelecimentos/filiais
- [ ] Gestão de estoque avançada (baixa automática, alertas)
- [ ] Integração com iFood, Rappi, marketplaces
- [ ] Painel administrativo web separado
- [ ] Relatórios avançados (ABC, curva ABC, ticket médio)
- [ ] Programa de fidelidade/cupons
- [ ] Múltiplos usuários com perfis (admin, cozinha, caixa)
- [ ] API pública para terceiros
- [ ] Pagamentos integrados (PIX, cartão)
- [ ] Notificações push nativas (app store)

> [!important]
> **Lista "FORA DO ESCOPO" é tão importante quanto a de dentro.** Protege o foco. Revise a cada nova ideia.

### Priorização de Funcionalidades (MoSCoW)

| Funcionalidade | MoSCoW | Justificativa | Esforço (1-5) | Dependência |
|----------------|--------|---------------|---------------|-------------|
| F1: Receber pedidos WhatsApp | **Must** | Core value prop | 4 | API WhatsApp |
| F2: Lista pedidos com status | **Must** | Core value prop | 2 | F1 |
| F3: Detalhes pedido | **Must** | Core value prop | 2 | F1 |
| F4: Atualizar status | **Must** | Core value prop | 1 | F2 |
| F5: Responder via app | **Should** | Diferencial forte | 3 | F1, API WhatsApp |
| F6: Relatório simples | **Should** | Valor percebido | 2 | F2, F4 |
| F7: Cadastro produtos | **Could** | Facilita uso | 2 | - |
| F8: Horário funcionamento | **Could** | Qualidade | 1 | - |
| Auth email/senha | **Must** | Segurança básica | 2 | - |
| Multi-estabelecimento | **Won't** | Fora escopo v1 | 5 | - |
| Estoque avançado | **Won't** | Fora escopo v1 | 4 | - |

**Legenda MoSCoW:**
- **Must** = MVP não funciona sem isso
- **Should** = Importante, mas MVP funciona sem (pode entrar se der tempo)
- **Could** = Nice to have, só se sobrar capacidade
- **Won't** = Explicitamente fora desta versão

## Como trabalhar com IA nesta etapa

**Prompt para derivar features:**
```
Tenho estas tarefas do usuário (Jobs to be Done): [cole da ETAPA 01/02].
E estas necessidades validadas: [cole da pesquisa].
Me sugira uma lista de funcionalidades candidatas, uma por Job/Necessidade.
Formato: tabela com colunas: Funcionalidade, Job Relacionado, Necessidade Atendida, Complexidade Estimada (1-5).
Não priorize — só liste candidatas.
```

**Prompt para proposta de valor:**
```
Meu produto: [descrição 1 linha].
Público: [persona principal].
Alternativa principal: [como resolvem hoje].
Benefício único: [o que só você faz].
Me dê 3 opções de proposta de valor no formato:
"Para [público], [produto] é a [categoria] que [benefício único] ao contrário de [alternativa]."
```

**Prompt para priorização MoSCoW:**
```
Tenho estas funcionalidades candidatas: [lista com esforço estimado].
Meu MVP deve validar: [hipótese principal da ETAPA 01].
Classifique cada uma em Must/Should/Could/Won't justificando.
Critério: Must = sem isso a hipótese principal não pode ser testada com usuários reais.
```

**Contexto a fornecer:** Definição do Problema + Pesquisa e Validação + hipótese principal.
**Resultado esperado:** 3 documentos preenchidos e aprovados por você.
**Como verificar:** MVP testa a hipótese principal? Fora de escopo protege foco? Priorização faz sentido?

## Checklist de execução

- [ ] 3-5 objetivos do produto definidos com métricas
- [ ] Persona principal + secundárias descritas
- [ ] Proposta de valor em uma frase clara
- [ ] Tarefas do usuário (Jobs) listadas (mínimo 4-6)
- [ ] Funcionalidades necessárias derivadas dos Jobs
- [ ] Funcionalidades FORA do escopo listadas explicitamente (mínimo 5)
- [ ] MVP definido: conjunto mínimo para testar hipótese principal
- [ ] Priorização MoSCoW (ou RICE) feita
- [ ] Critérios de sucesso do produto definidos
- [ ] Restrições e limitações registradas
- [ ] Riscos conhecidos atualizados
- [ ] Critérios para mudança de escopo definidos
- [ ] 3 documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Definição do Produto completa e aprovada por você
- [ ] Escopo e Limites com lista FORA DO ESCOPO clara
- [ ] Priorização feita com justificativa
- [ ] MVP testa a hipótese de risco MAIOR da ETAPA 01
- [ ] Você consegue dizer "não" a uma feature nova olhando o fora de escopo
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste do MVP:** Se remover qualquer "Must", a hipótese principal deixa de ser testável?
- **Teste do fora de escopo:** Alguém sugere feature X — você consegue apontar onde ela está (dentro/fora) em 5 seg?
- **Teste da proposta de valor:** Alguém lê e entende para quem é, o que faz, por que é diferente?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| MVP grande demais ("precisa ter tudo") | Pergunte: "Qual a UMA hipótese que se eu errar, o projeto falha?" MVP testa só essa. |
| Não definir fora de escopo | Gaste 15 min listando o que NÃO fará. Salva semanas depois. |
| Priorização baseada em "gosto" | Use critério objetivo: risco da hipótese, valor para usuário, esforço. |
| Proposta de valor genérica | "Melhor app para X" não é proposta. Seja específico no benefício único. |
| Esquecer restrições conhecidas | Liste: orçamento, prazo, tech stack decidida, equipe de 1, LGPD, etc. |

## Se algo der errado

| Situação | Ação |
|----------|------|
| MVP ainda muito grande | Corte "Should" para "Could", "Could" para "Won't". MVP = só "Must". |
| Não consegue priorizar | Falta clareza na hipótese principal. Volte para ETAPA 01/02. |
| Stakeholder (você mesmo) quer tudo | Use fora de escopo como escudo. "Está no Won't v1 — vamos revisar no v2." |
| Priorização técnica vs. negócio conflita | Negócio decide prioridade; técnica diz esforço. Negocie escopo, não prioridade. |

## Condições para avançar

- ✅ 3 documentos salvos e completos
- ✅ MVP claro, testável, focado na hipótese de maior risco
- ✅ Fora de escopo protege contra scope creep
- ✅ Priorização justificada
- ✅ Você sabe exatamente o que será construído no v1

## Próxima etapa

[[ETAPA 04 - Organizar os Requisitos]]

## Materiais relacionados

- [[02 - Modelos/Pesquisa e Planejamento/Definição do Produto]]
- [[02 - Modelos/Pesquisa e Planejamento/Escopo e Limites]]
- [[02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades]]
- [[03 - Guias Práticos/Vibe Coding/Como Definir MVP]]
- [[04 - Referência/Glossário/MVP]]
- [[04 - Referência/Glossário/Scope Creep]]
- [[04 - Referência/Glossário/MoSCoW]]