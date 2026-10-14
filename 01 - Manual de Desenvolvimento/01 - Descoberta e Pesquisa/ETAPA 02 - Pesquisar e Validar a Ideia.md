# ETAPA 02 — Pesquisar e Validar a Ideia

## Objetivo

Obter informações suficientes para decidir **se vale a pena desenvolver** a solução proposta, **como** ela deve ser diferente do que existe e **quais riscos** principais existem.

## Por que esta etapa existe?

Evita construir algo que ninguém usa, que já existe melhor, ou que tem barreiras intransponíveis. Transforma hipóteses em decisões baseadas em evidências.

## Quando começar

- ETAPA 01 concluída (Definição do Problema salva, hipóteses registradas)
- Você tem clareza sobre o problema e objetivo, mas não validou o mercado/usuários

## O que você precisa fazer

1. **Planeje a pesquisa** — quais hipóteses testar, quais perguntas responder
2. **Pesquise soluções existentes** — concorrentes diretos, indiretos, alternativas manuais
3. **Analise produtos semelhantes** — o que fazem bem, o que fazem mal, gaps
4. **Investigue necessidades dos usuários** — entrevistas, observação, fóruns, reviews
5. **Identifique alternativas** — como o usuário resolve hoje? (planilha, papel, outro app, não resolve)
6. **Registre fontes** — links, datas, citações para rastreabilidade
7. **Separe fatos, hipóteses e opiniões** — marque claramente cada um
8. **Identifique riscos e oportunidades** — técnicos, mercadológicos, regulatórios
9. **Decida: continuar, reformular ou abandonar** — com justificativa
10. **Produza o documento: Pesquisa e Validação + Decisão de Continuidade**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Definir perguntas de pesquisa | 👤 USUÁRIO |
| Executar buscas, coletar dados | 🤖 IA (com sua direção) |
| Entrevistar/observar usuários reais | 👤 USUÁRIO (IA não substitui) |
| Analisar concorrentes | 🤖 IA + 👤 USUÁRIO |
| Separar fatos/hipóteses/opiniões | 👤 USUÁRIO (com ajuda da IA) |
| Decidir continuar/reformular/abandonar | 👤 USUÁRIO |
| Validar qualidade da pesquisa | ✅ VALIDAÇÃO |

## Ferramentas recomendadas

| Finalidade | Ferramentas |
|------------|-------------|
| Busca de mercado/concorrentes | Google, GitHub, Product Hunt, App Stores, G2, Capterra |
| Análise de tráfego/popularidade | SimilarWeb, Ubersuggest, Google Trends |
| Reviews de usuários | App Store, Play Store, G2, Capterra, Reddit, Twitter/X |
| Entrevistas | Zoom/Meet + gravação, formulários (Typeform, Google Forms) |
| Organização de findings | Obsidian (este manual), Notion, Miro |
| IA para síntese | Claude Code (resumir reviews, comparar features, extrair padrões) |

> [!important]
> **IA pesquisa, você valida.** Não confie cegamente em resumos de IA — verifique fontes originais das decisões críticas.

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Pesquisa e Validação | `02 - Modelos/Pesquisa e Planejamento/Pesquisa e Validação.md` | `05 - Projetos/SeuProjeto/01 - Descoberta/` |
| Decisão de Continuidade | `02 - Modelos/Pesquisa e Planejamento/Decisão de Continuidade.md` | `05 - Projetos/SeuProjeto/01 - Descoberta/` |

### Condicionais
- Análise de Concorrentes detalhada (se mercado competitivo)
- Registro de Entrevistas (se fez entrevistas)
- Análise de Viabilidade Técnica (se houver dúvida técnica crítica)

### Opcionais
- Canvas de Lean Canvas / Business Model Canvas
- Mapa de Cenários Competitivos

## Como produzir os documentos

### Pesquisa e Validação

**Estrutura do modelo:**

#### 1. Perguntas de Pesquisa (derivadas das hipóteses da ETAPA 01)
| Hipótese | Pergunta de Pesquisa | Método | Status |
|----------|---------------------|--------|--------|
| H1: Maior problema é organizar pedidos | Como comerciantes organizam pedidos hoje? | Entrevistas + reviews | ✅ Feito |
| H2: WhatsApp é canal principal | Qual % de pedidos vem por WhatsApp? | Entrevistas | 🔄 Em andamento |

#### 2. Soluções Existentes (Concorrentes Diretos)
| Produto | Público | Preço | Pontos Fortes | Pontos Fracos | Gap para nós |
|---------|---------|-------|---------------|---------------|--------------|
| Competidor A | Médias empresas | R$ 200/mês | Integração ERP | Caro, complexo | Simplicidade + preço |
| Competidor B | Pequenos | Freemium | Fácil uso | Sem WhatsApp | Integração WhatsApp |

#### 3. Alternativas Atuais (Como o usuário resolve HOJE)
| Alternativa | Vantagens | Desvantagens | Por que não resolve tudo |
|-------------|-----------|--------------|--------------------------|
| Anotar no papel | Grátis, simples | Perde info, não escala | Erros frequentes |
| Planilha Excel | Organiza, grátis | Manual, não mobile | Não integra WhatsApp |
| Não resolve | - | Perde vendas | - |

#### 4. Necessidades dos Usuários (Evidências)
| Necessidade | Evidência (fonte) | Tipo | Prioridade |
|-------------|-------------------|------|------------|
| Ver todos pedidos em um lugar | 8/10 entrevistados citaram | Fato (entrevista) | Alta |
| Responder rápido no WhatsApp | Reviews de apps atuais | Fato (review) | Alta |
| Relatório de vendas semanal | 3/10 citaram | Hipótese | Média |

#### 5. Riscos Identificados
| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| API WhatsApp mudar/fechar | Média | Alto | Arquitetura desacoplada, fallback email |
| Concorrente grande lança similar | Baixa | Alto | Foco em nicho, speed to market |
| Usuários não pagam | Média | Médio | Validar disposição a pagar na pesquisa |

#### 6. Oportunidades Identificadas
| Oportunidade | Evidência | Ação |
|--------------|-----------|------|
| Nenhum app foca em micro-comerciante | Gap concorrentes | Posicionamento claro |
| WhatsApp Business API acessível | Doc técnica | MVP com integração nativa |

#### 7. Fontes Consultadas (registro para rastreabilidade)
| Fonte | Tipo | Data | Link/Referência | Confiabilidade |
|-------|------|------|-----------------|----------------|
| Entrevista João (dono loja) | Primária | 2026-10-10 | Gravação local | Alta |
| Reviews App Store "Gestão Pedidos" | Secundária | 2026-10-10 | Link App Store | Média |
| Artigo "Mercado varejo BR 2024" | Secundária | 2026-10-09 | IBGE/SEBRAE | Alta |

### Decisão de Continuidade

**Formato:**
```
## Decisão: [CONTINUAR / REFORMULAR / ABANDONAR]

### Justificativa
- Fato principal 1
- Fato principal 2
- Risco aceito: X (mitigação Y)

### Próximos passos se CONTINUAR
- Ajustar hipótese H2 baseado em finding X
- Priorizar feature Y (validada como alta necessidade)
- Definir MVP na ETAPA 03 incluindo...

### Próximos passos se REFORMULAR
- Novo problema: ...
- Novas hipóteses: ...
- Voltar para ETAPA 01

### Próximos passos se ABANDONAR
- Motivo: ...
- Lição aprendida: ...
- Registrar no Acompanhamento e arquivar projeto
```

> [!warning]
> **Não pule a Decisão de Continuidade.** Mesmo que pareça óbvio que vai continuar, registre explicitamente. Isso evita viés de confirmação.

## Como trabalhar com IA nesta etapa

**Prompt para análise de concorrentes:**
```
Pesquise os principais concorrentes para: [problema + público da ETAPA 01].
Para cada um, me dê: nome, público-alvo, preço, features principais, reviews negativos comuns.
Formato: tabela Markdown.
Fontes: busque em Product Hunt, G2, Capterra, GitHub, App Stores.
Marque claramente o que é fato (com fonte) vs. sua inferência.
```

**Prompt para síntese de reviews:**
```
Analise estes reviews de usuários de [concorrente/app similar]: [cole 20-50 reviews].
Extraia:
1. Top 5 dores/reclamações recorrentes
2. Top 5 elogios/points fortes
3. Features pedidas que não existem
4. Padrões por tipo de usuário (se der para inferir)
Formato: lista com contagem de menções.
```

**Prompt para estruturar findings:**
```
Tenho estes dados brutos de pesquisa: [cole suas anotações/links].
Me ajude a organizar no modelo de Pesquisa e Validação:
- Separe fatos, hipóteses, opiniões
- Identifique riscos e oportunidades
- Sugira perguntas para validar gaps
Não invente dados. Só organize.
```

**Contexto a fornecer:** Definição do Problema + hipóteses da ETAPA 01.
**Resultado esperado:** Documento Pesquisa e Validação preenchido + Decisão de Continuidade.
**Como verificar:** Cada afirmação crítica tem fonte? Fatos separados de opiniões? Decisão fundamentada?

## Checklist de execução

- [ ] Perguntas de pesquisa definidas (baseadas nas hipóteses da ETAPA 01)
- [ ] Mínimo 3 concorrentes/alternativas analisados
- [ ] Mínimo 5 reviews de usuários lidos (ou 3 entrevistas realizadas)
- [ ] Alternativas atuais do usuário mapeadas
- [ ] Necessidades validadas com evidências (fonte registrada)
- [ ] Riscos e oportunidades identificados
- [ ] Fontes consultadas registradas com data e link
- [ ] Fatos, hipóteses e opiniões claramente separados
- [ ] Decisão de Continuidade escrita e justificada
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Documento Pesquisa e Validação completo
- [ ] Decisão de Continuidade registrada (CONTINUAR / REFORMULAR / ABANDONAR)
- [ ] Se CONTINUAR: sabe exatamente o que ajustar no MVP
- [ ] Se REFORMULAR: sabe qual problema atacar agora
- [ ] Fontes rastreáveis para decisões críticas
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste da fonte:** Para cada "fato" crítico, consegue apontar a fonte em 10 segundos?
- **Teste do viés:** Procurou ativamente evidências CONTRA sua ideia?
- **Teste da decisão:** A justificativa da decisão convence alguém que não conhece o projeto?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Só buscar confirmações (viés de confirmação) | Busque ativamente "por que isso NÃO funcionaria" |
| Confiar cegamente em resumo de IA | Verifique 2-3 fontes primárias das decisões críticas |
| Analisar concorrentes grandes demais | Foque em quem atende MESMO público/necessidade |
| Não registrar fontes | Crie o hábito: toda afirmação → [fonte] |
| Pesquisa infinita (analysis paralysis) | Timebox: 2-4h para projeto pequeno, 1-2 dias para médio |
| Ignorar alternativas manuais (papel, planilha) | Elas são sua concorrência real no início |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Nenhum concorrente encontrado | Ótimo sinal OU mercado inexistente. Valide necessidade com usuários reais. |
| Concorrente forte domina tudo | Procure nicho não atendido, ou diferencial radical (preço, UX, canal). |
| Usuários dizem "não preciso" | Reavalie problema (ETAPA 01) — pode ser solução procurando problema. |
| Dados contraditórios | Separe por segmento de usuário. Diferentes perfis = necessidades diferentes. |
| Tempo/esforço de pesquisa muito alto | Reduza escopo: valide só a hipótese MAIS CRÍTICA (riskiest assumption). |

## Condições para avançar

- ✅ Decisão de Continuidade = CONTINUAR (ou REFORMULAR com novo caminho claro)
- ✅ Pesquisa documentada com fontes
- ✅ Top 3 necessidades validadas com evidências
- [ ] Principais riscos conhecidos e mitigações esboçadas
- [ ] Você consegue explicar: "Vale a pena porque [evidência 1], [evidência 2], apesar de [risco 1]"

## Próxima etapa

[[ETAPA 03 - Definir o Produto e seu Escopo]]

## Materiais relacionados

- [[02 - Modelos/Pesquisa e Planejamento/Pesquisa e Validação]]
- [[02 - Modelos/Pesquisa e Planejamento/Decisão de Continuidade]]
- [[02 - Modelos/Pesquisa e Planejamento/Análise de Concorrentes]]
- [[03 - Guias Práticos/UI e UX/Pesquisa de Usuários]]
- [[04 - Referência/Glossário/Validação]]
- [[04 - Referência/Glossário/MVP]]