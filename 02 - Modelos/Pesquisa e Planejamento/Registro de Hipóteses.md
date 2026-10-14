# Registro de Hipóteses

> **Copie para:** `05 - Projetos/SeuProjeto/01 - Descoberta/Registro de Hipóteses.md`
> Baseado nas ETAPAs 01-03. Mantenha vivo durante todo o projeto.

---

## Formato de Hipótese
**Estrutura:** "Acredito que **[público/quantidade]** **[faz/quer/precisa]** **[ação/resultado]** porque **[razão]**."
**Critério:** Deve ser **falsificável** (testável com sim/não).

---

## Hipóteses do Problema (ETAPA 01)

| ID | Hipótese | Tipo | Como Validar | Status | Evidência | Decisão |
|----|----------|------|--------------|--------|-----------|---------|
| H1 | [Ex: 80%+ micro-lojistas perdem pedidos no WhatsApp] | [Problema] | [Entrevistas: "Perde pedidos? Quantos?"] | [Validada / Refutada / Em teste] | [8/10 entrevistados confirmaram] | [Manter / Ajustar / Descartar] |
| H2 | [Ex: WhatsApp é canal principal (90%+ pedidos)] | [Canal] | [Perguntar: "Por onde recebe? % cada canal"] | | | |
| H3 | [Ex: Maior dor é organização, não estoque] | [Prioridade] | [Ranking de dores nas entrevistas] | | | |
| H4 | | | | | | |
| H5 | | | | | | |

---

## Hipóteses de Solução (ETAPA 02-03)

| ID | Hipótese | Tipo | Como Validar | Status | Evidência | Decisão |
|----|----------|------|--------------|--------|-----------|---------|
| HS1 | [Ex: Integração WhatsApp oficial resolve 90% da dor] | [Solução] | [Spike técnico + teste usuários] | [Validada / Refutada / Em teste] | [Webhook recebe 100% mensagens teste] | [Manter / Ajustar / Descartar] |
| HS2 | [Ex: Lista com status coloridos resolve visualização] | [UX] | [Teste usabilidade protótipo] | | | |
| HS3 | [Ex: Templates de resposta agilizam em 5x] | [Eficiência] | [Cronometrar: manual vs template] | | | |
| HS4 | [Ex: Pagariam R$ 49/mês por isso] | [Negócio] | [Landing page + pré-venda / pesquisa] | | | |
| HS5 | [Ex: Onboarding 3 passos < 5 min] | [Ativação] | [Teste usuários novos] | | | |

---

## Hipóteses Técnicas (ETAPA 05)

| ID | Hipótese | Tipo | Como Validar | Status | Evidência | Decisão |
|----|----------|------|--------------|--------|-----------|---------|
| HT1 | [Ex: Next.js + Vercel suporta carga v1] | [Arquitetura] | [Load test staging] | [Validada / Refutada / Em teste] | [100 req/s OK, p95 < 500ms] | [Manter / Ajustar / Descartar] |
| HT2 | [Ex: Prisma + Neon serverless OK para produção] | [Banco] | [Teste migração + carga] | | | |
| HT3 | [Ex: SSE nativo escala para 1k conexões] | [Tempo Real] | [Simulação 1k clients] | | | |
| HT4 | [Ex: NextAuth Magic Link entrega < 30s] | [Auth] | [Teste 50 envios Resend] | | | |

---

## Hipóteses de Negócio (Contínuo)

| ID | Hipótese | Métrica de Sucesso | Prazo Validação | Status | Resultado |
|----|----------|-------------------|-----------------|--------|-----------|
| HN1 | [Ex: 20% trial → paid em 30 dias] | [Conversion rate] | [YYYY-MM-DD] | [Em andamento] | [XX%] |
| HN2 | [Ex: Churn < 5% ao mês] | [Churn rate] | [YYYY-MM-DD] | | |
| HN3 | [Ex: NPS > 40 após 60 dias] | [NPS score] | [YYYY-MM-DD] | | |
| HN4 | [Ex: CAC < LTV / 3] | [CAC / LTV] | [YYYY-MM-DD] | | |

---

## Log de Mudanças de Hipóteses

| Data | Hipótese | Mudança | Motivo (Evidência) | Decisão |
|------|----------|---------|-------------------|---------|
| YYYY-MM-DD | H3: "Maior dor é organização" | Ajustada para "Organização + Resposta rápida" | Entrevistas: 60% citaram resposta rápida como dor #2 | Manter expandida |
| YYYY-MM-DD | HS4: "Pagariam R$ 49/mês" | Reduzida para "Testar R$ 29-49/mês" | Pesquisa: 60% "talvez", 20% "sim" | Testar faixa |
| | | | | |

---

## Checklist de Validação Contínua

- [ ] Cada hipótese tem **dono** (quem valida)
- [ ] Cada hipótese tem **método** claro de validação
- [ ] Cada hipótese tem **prazo** ou **gatilho** de validação
- [ ] Resultados registrados com **evidência** (link, dado, observação)
- [ ] Hipóteses refutadas → **documentadas** (evita repetir erro)
- [ ] Hipóteses validadas → **viraram requisitos** ou **decisões**
- [ ] Revisão semanal: alguma hipótese crítica não validada?

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/01 - Descoberta e Pesquisa/ETAPA 01 - Entender o Problema e a Oportunidade]]
- [[01 - Manual de Desenvolvimento/01 - Descoberta e Pesquisa/ETAPA 02 - Pesquisar e Validar a Ideia]]
- [[02 - Modelos/Pesquisa e Planejamento/Definição do Problema]]
- [[02 - Modelos/Pesquisa e Planejamento/Pesquisa e Validação]]