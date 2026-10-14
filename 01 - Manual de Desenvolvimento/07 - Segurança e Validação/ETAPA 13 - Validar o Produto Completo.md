# ETAPA 13 — Validar o Produto Completo

## Objetivo

Confirmar que o **software construído corresponde ao que foi planejado** — requisitos, critérios de aceitação, fluxos, qualidade visual e integrações — antes de autorizar a publicação.

## Por que esta etapa existe?

Desenvolvimento incremental (ETAPA 10) foca em partes. Esta etapa é a **visão do todo**: o produto integrado funciona como uma unidade coerente? Atende ao objetivo original? Está pronto para usuários reais?

## Quando começar

- ETAPA 11 concluída (Quality Gates atendidos, testes passando)
- ETAPA 12 concluída (Revisão de Segurança assinada)
- Código "feature complete" e estável no branch `main` ou `release/v1`

## O que você precisa fazer

1. **Revise TODOS os requisitos funcionais** — cada RF da ETAPA 04 implementado e testado?
2. **Revise TODOS os critérios de aceitação** — cada CA (Gherkin) validado (automatizado ou manual)?
3. **Teste TODOS os fluxos principais** — caminho feliz de cada feature Must/Should (ETAPA 06)
4. **Teste fluxos alternativos importantes** — erros, vazios, bordas, permissões
5. **Verificação visual completa** — cada tela do Inventário (ETAPA 08) vs. Direção Visual (ETAPA 07)
6. **Verificação de responsividade** — 3 breakpoints em todas as telas (ETAPA 08)
7. **Revisão de mensagens e estados da interface** — textos, erros, loading, vazios, sucesso
8. **Verificação de integrações** — WhatsApp, Email, Tempo real, Banco — funcionando em staging
9. **Verificação de permissões** — RBAC testado (OWNER vs STAFF), auth bypass tentado
10. **Revisão de limitações conhecidas** — do Registro de Bugs (ETAPA 11) e Riscos Residuais (ETAPA 12)
11. **Avaliação de problemas impeditivos** — algum bug P0/P1 não corrigido? Bloqueia deploy?
12. **Decisão: APROVAR PUBLICAÇÃO / REQUER CORREÇÕES / ADIAR**
13. **Produza o documento: Validação Final + Decisão de Publicação**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Revisar requisitos vs. implementação | 👤 USUÁRIO (checklist) + 🤖 IA (rastreabilidade) |
| Testar fluxos principais (E2E manual) | 👤 USUÁRIO (você navega como usuário) |
| Testar fluxos alternativos/erro | 👤 USUÁRIO + 🤖 IA (automatizados) |
| Verificação visual (UI vs. Design) | 👤 USUÁRIO (olho humano) |
| Verificação responsividade | 👤 USUÁRIO (DevTools) |
| Verificar integrações em staging | ✅ VALIDAÇÃO (você testa endpoints reais) |
| Verificar permissões/RBAC | 🤖 IA (testes) + ✅ VALIDAÇÃO |
| Revisar limitações/riscos | 👤 USUÁRIO (decisão de negócio) |
| Decidir publicação | 👤 USUÁRIO (VOCÊ decide) |
| Registrar validação | 👤 USUÁRIO + 🤖 IA (documenta) |

## Ferramentas recomendadas

- **Staging environment** — deploy idêntico à produção (Vercel Preview / staging branch)
- **Checklist físico/digital** — marque item a item (não confie na memória)
- **Playwright** — rodar suite E2E completa em staging
- **Lighthouse** — performance/a11y em staging
- **axe-core** — acessibilidade em staging
- **DevTools Device Toolbar** — responsividade
- **Sentry/Logs** — monitorar erros durante validação

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Validação Final | `02 - Modelos/Publicação e Manutenção/Validação Final.md` | `05 - Projetos/SeuProjeto/08 - Publicação/` |
| Decisão de Publicação | `02 - Modelos/Publicação e Manutenção/Decisão de Publicação.md` | `05 - Projetos/SeuProjeto/08 - Publicação/` |

### Condicionais
- Relatório de Gaps (se houver desvios planejados vs. implementado)

### Opcionais
- Sessão de Validação com Usuário Real (se disponível)

## Como produzir os documentos

### Validação Final

```markdown
# Validação Final — PedidosSimples v1.0.0

## Informações Gerais
**Versão Candidata:** v1.0.0-rc1
**Commit:** `abc123def` (main)
**Deploy Staging:** https://staging-pedidosimples.vercel.app
**Data Validação:** 2026-10-16
**Validador:** [Seu Nome]

---

## 1. Rastreabilidade Requisitos → Implementação

| RF ID | Descrição | Implementado? | Testado? (Auto/Manual) | CA Validada? | Observação |
|-------|-----------|---------------|------------------------|--------------|------------|
| RF-001 | Receber pedido WhatsApp | ✅ Sim | Auto (unit/int) + Manual | ✅ CA-RF-001 | Webhook HMAC ok |
| RF-002 | Listar pedidos com filtro | ✅ Sim | Auto + Manual | ✅ CA-RF-002 | SSE tempo real ok |
| RF-003 | Detalhes pedido | ✅ Sim | Auto + Manual | ✅ CA-RF-003 | Modal funciona |
| RF-004 | Atualizar status | ✅ Sim | Auto + Manual | ✅ CA-RF-004 | Transições validadas |
| RF-005 | Responder cliente | ✅ Sim | Auto + Manual | ✅ CA-RF-005 | Templates + custom |
| RF-006 | Relatório simples | ⚠️ Parcial | Manual | ⚠️ CA-RF-006 | Só total dia (v1.1 semanal) |
| RF-007 | Cadastro produtos | ✅ Sim | Auto + Manual | ✅ CA-RF-007 | CRUD completo |
| RF-008 | Horário funcionamento | ✅ Sim | Auto + Manual | ✅ CA-RF-008 | Fora horário automático |

**Resumo:** 7/8 RFs Must/Should 100%. RF-006 parcial (aceito para v1 — documentado).

---

## 2. Critérios de Aceitação (Gherkin) — Validação

| CA ID | Cenário | Status | Evidência |
|-------|---------|--------|-----------|
| CA-RF-001-1 | Pedido válido recebido | ✅ Aprovado | Playwright + Manual staging |
| CA-RF-001-2 | Mensagem não reconhecida | ✅ Aprovado | Unit test + Log staging |
| CA-RF-001-3 | Fora de horário | ✅ Aprovado | Manual staging (config) |
| CA-RF-002-1 | Lista carrega pedidos | ✅ Aprovado | E2E + Manual |
| CA-RF-002-2 | Filtro status funciona | ✅ Aprovado | E2E + Manual |
| CA-RF-002-3 | Estado vazio | ✅ Aprovado | Manual (conta nova) |
| CA-RF-004-1 | Atualizar status válido | ✅ Aprovado | E2E + Manual |
| CA-RF-004-2 | Transição inválida bloqueada | ✅ Aprovado | Unit test (entregue→novo) |
| CA-RF-005-1 | Responder com template | ✅ Aprovado | E2E + Manual staging |
| CA-RF-005-2 | Falha WhatsApp → erro claro | ✅ Aprovado | Manual (token inválido) |
| ... | ... | ... | ... |

**Total CAs Must/Should:** 28 | **Aprovados:** 27 | **Parciais:** 1 (RF-006 relatório) | **Reprovados:** 0

---

## 3. Fluxos Principais (ETAPA 06) — Teste Manual Completo

| Fluxo | Status | Observação |
|-------|--------|------------|
| Login → Dashboard → Ver Pedidos | ✅ OK | < 10s cold start |
| Novo Pedido WhatsApp → Aparece na lista (SSE) | ✅ OK | < 2s latência |
| Marcar "Preparando" → Badge atualiza tempo real | ✅ OK | Otimista + confirma |
| Marcar "Pronto" → Disponível para entrega | ✅ OK | |
| Responder Cliente (Template) → Enviado WhatsApp | ✅ OK | Confirmação toast |
| Responder Cliente (Custom) → Enviado WhatsApp | ✅ OK | |
| Ver Histórico → Filtros data/status → Resultado | ✅ OK | Paginação ok |
| Configurar Estabelecimento → Salva → Reflete app | ✅ OK | Horários afetam fora_horario |
| Configurar Produtos → CRUD → Usado no parsing | ✅ OK | Parsing usa preço cadastrado |
| Onboarding Primeira Vez → 3 passos → Dashboard | ✅ OK | WhatsApp conecta via Meta |

---

## 4. Fluxos Alternativos e Edge Cases

| Cenário | Testado? | Resultado |
|---------|----------|-----------|
| Offline → Reconexão SSE | ✅ | Reconecta auto (backoff) |
| WhatsApp API 500 → Retry + Log | ✅ | Queue local + retry |
| Pedido duplicado (mesmo message_id) | ✅ | Idempotência: 1 pedido |
| Usuário STAFF tenta acessar Integrações | ✅ | 403 Forbidden |
| Sessão expira durante uso | ✅ | Redirect login suave |
| Mobile: Touch targets ≥ 44px | ✅ | Verificado DevTools |
| Teclado: Tab order lógico | ✅ | Navegação completa |
| Screen reader: Labels/ARIA | ✅ | NVDA testado |

---

## 5. Verificação Visual (UI vs. Direção Visual ETAPA 07)

| Tela (Inventário ETAPA 08) | Cores/Tokens | Tipografia | Espaçamento | Bordas/Sombras | Ícones | Estados (hover/focus/disabled/loading) | Aprovado? |
|----------------------------|--------------|------------|-------------|----------------|--------|----------------------------------------|-----------|
| SC-01 Login | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-02 Dashboard | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-03 Detalhes Pedido | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-04 Reply Modal | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-05 Histórico | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-06 a SC-10 Config | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-11 Onboarding | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| SC-12 Erro Global | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

**Nota:** Todos os tokens da Direção Visual aplicados consistentemente. Zero hardcoded values encontrados.

---

## 6. Responsividade (3 Breakpoints)

| Tela | < 640px (Mobile) | 640-1024px (Tablet) | > 1024px (Desktop) |
|------|------------------|---------------------|-------------------|
| SC-01 Login | ✅ Centralizado, teclado não cobre | ✅ | ✅ |
| SC-02 Dashboard | ✅ Tabs scroll, FAB, 1 col | ✅ Tabs visíveis, 2 col | ✅ Sidebar fixa, 2+ col |
| SC-03 Detalhes | ✅ Modal full-screen | ✅ Modal centered | ✅ Modal centered |
| SC-04 Reply | ✅ Modal full | ✅ Modal centered | ✅ Modal centered |
| SC-05 Histórico | ✅ Cards stack | ✅ 2 col grid | ✅ 3 col grid |
| SC-06-10 Config | ✅ Stack vertical | ✅ 2 col (form + preview) | ✅ 2 col |
| SC-11 Onboarding | ✅ Passo a passo full | ✅ Centered card | ✅ Centered card |

---

## 7. Mensagens e Estados da Interface

| Categoria | Verificação | Status |
|-----------|-------------|--------|
| Textos em português brasileiro | Revisão completa | ✅ OK |
| Mensagens de erro: claras, acionáveis, sem jargão | Lista de erros revisada | ✅ OK |
| Estados loading: skeleton/spinner + texto | Todas telas | ✅ OK |
| Estados vazios: ilustração + CTA orientativo | Todas listas | ✅ OK |
| Estados sucesso: toast discreto + confirmação visual | Todas ações | ✅ OK |
| Validação inline: campo + mensagem + ícone | Formulários | ✅ OK |
| Acessibilidade: aria-live em toasts, focus visível | Automático + manual | ✅ OK |

---

## 8. Integrações (Staging)

| Integração | Endpoint | Auth | Funcionando? | Observação |
|------------|----------|------|--------------|------------|
| WhatsApp Cloud API | `graph.facebook.com/v18.0` | Bearer Token | ✅ | Webhook + Send testados |
| NextAuth Email (Resend) | `api.resend.com` | API Key | ✅ | Magic Link entrega < 30s |
| SSE (Tempo Real) | `/api/pedidos/stream` | Session Cookie | ✅ | Reconexão automática |
| PostgreSQL (Neon) | `neon.tech` | Connection String | ✅ | Pooling ok, migrações ok |

---

## 9. Permissões e Autenticação

| Cenário | Esperado | Testado | Resultado |
|---------|----------|---------|-----------|
| Não logado → /dashboard | Redirect /login | ✅ | 302 → /login?callbackUrl=... |
| Logado STAFF → /configuracoes/integracoes | 403 | ✅ | Middleware block |
| Logado OWNER → /configuracoes/integracoes | 200 | ✅ | Acesso total |
| Session expiry → Ação | Redirect login | ✅ | next-auth default |
| Magic Link expirado (1h) | Erro "Link expirado" | ✅ | Página erro amigável |
| Magic Link usado 2x | Erro "Já utilizado" | ✅ | Token single-use |

---

## 10. Limitações Conhecidas e Riscos Residuais

| Item | Origem | Decisão | Mitigação v1.1 |
|------|--------|---------|----------------|
| Relatório semanal (RF-006) | ETAPA 03 (Won't v1) | Aceito — apenas total dia | Implementar v1.1 |
| Rate limiting API | ETAPA 12 (Pendente) | Aceito — WhatsApp tem limite próprio | Arcjet v1.1 |
| Runbook DR | ETAPA 12 (Pendente) | Aceito — Neon/Vercel SLAs | Documentar v1.1 |
| Rate limit auth | ETAPA 12 (Pendente) | Aceito — NextAuth básico | Arcjet v1.1 |
| Push notifications nativas | ETAPA 03 (Won't v1) | Aceito — PWA instalável | Service Worker v1.1 |

---

## 11. Problemas Impeditivos (P0/P1)

| Bug ID | Descrição | Severidade | Status | Bloqueia Deploy? |
|--------|-----------|------------|--------|------------------|
| — | Nenhum | — | — | **NÃO** |

---

## 12. Decisão de Validação

> **☑ APROVADO PARA PUBLICAÇÃO**
>
> O produto atende aos critérios de qualidade definidos para o MVP v1. Limitações conhecidas documentadas e aceitas. Riscos residuais gerenciáveis. Nenhum problema impeditivo.
>
> **Validador:** [Seu Nome] | **Data:** 2026-10-16 | **Assinatura:** [Digital]
```

### Decisão de Publicação

```markdown
# Decisão de Publicação — PedidosSimples v1.0.0

## Decisão: **PUBLICAR**

### Justificativa
- Todos os requisitos Must/Should implementados e validados (1 RF parcial aceito)
- 27/28 CAs aprovados, 1 parcial documentado
- Quality Gates ETAPA 11 atendidos
- Revisão Segurança ETAPA 12 assinada (riscos residuais aceitos)
- Zero bugs P0/P1
- Staging validado end-to-end

### Versão a Publicar
**Tag:** `v1.0.0`
**Commit:** `abc123def` (main)
**Changelog:** `CHANGELOG.md` atualizado

### Checklist Pré-Publicação (Confirmação Final)
- [ ] Tag `v1.0.0` criada e pushada
- [ ] `CHANGELOG.md` atualizado com features v1
- [ ] Variáveis de ambiente PRODUÇÃO configuradas na Vercel
- [ ] Domínio `pedidosimples.com` configurado (DNS + Vercel)
- [ ] Webhook WhatsApp atualizado para URL produção
- [ ] Sentry DSN produção configurado
- [ ] UptimeRobot monitorando domínio produção
- [ ] Alertas Sentry testados em produção (forçar erro)
- [ ] Backup Neon produção verificado (branch proteção)
- [ ] Rollback testado: `vercel rollback` funciona

### Plano de Rollback
**Se crítico em < 1h pós-deploy:**
1. `vercel rollback [deployment-url-anterior]` (via CLI ou dashboard)
2. Verificar Neon: dados íntegros (migrações são backward-compatible v1)
3. Comunicar usuários se necessário (status page / email)

**Critério de Rollback Automático:**
- Error rate > 5% por 5 min (Sentry alert)
- P95 latency > 3s por 10 min
- Uptime < 99% em 30 min

### Pós-Deploy Imediato (Primeiras 2h)
- [ ] Smoke test fluxos críticos em produção
- [ ] Sentry recebendo eventos
- [ ] UptimeRobot OK
- [ ] Logs estruturados visíveis
- [ ] Métricas Vercel Analytics ativas
- [ ] WhatsApp webhook recebendo (teste real)

### Responsável pela Publicação
**Nome:** [Seu Nome]
**Data/Hora Planejada:** 2026-10-17 10:00 BRT
**Contato Emergência:** [Seu telefone/email]

### Aprovação Final
**Decisor:** [Seu Nome]
**Data:** 2026-10-16
**Assinatura:** [Digital]

> [!warning]
> **Não publique se qualquer item do Checklist Pré-Publicação não estiver ✅.**
> Não delegue esta decisão.
```

## Como trabalhar com IA nesta etapa

**Prompt para rastreabilidade requisitos → implementação:**
```
Tenho: Requisitos Funcionais (ETAPA 04) + Inventário de Telas (ETAPA 08) + Código implementado.
Gere matriz de rastreabilidade: RF ID | Descrição | Arquivos Implementados | Testes (unit/int/e2e/manual) | CA Validada | Status.
Identifique gaps: RF sem implementação, RF sem teste, CA não validada.
Formato: Tabela Markdown pronta para Validação Final.
```

**Prompt para checklist visual automatizado:**
```
Analise componentes vs Direção Visual (tokens ETAPA 07).
Verifique em cada componente (components/ui/*, components/pedidos/*):
- Cores: apenas tokens (sem hardcoded hex)
- Tipografia: tokens de tamanho/peso/line-height
- Espaçamento: escala 4px (space-1 a space-12)
- Border-radius: tabela ETAPA 07
- Sombras: tokens ETAPA 07
- Ícones: Lucide, currentColor, tamanhos padrão
- Estados: hover, focus, active, disabled, loading definidos
Formato: Relatório por componente com ✅/❌ + arquivo:linha se ❌.
```

**Prompt para validar fluxos em staging:**
```
Gere script de validação manual (checklist) para staging:
Base: Fluxos ETAPA 06 + Inventário ETAPA 08 + CAs ETAPA 04.
Para cada fluxo Must/Should: passos exatos, dados de teste, resultado esperado, como verificar no staging.
Inclua: fluxos alternativos (erro, vazio, permissão), responsividade (3 breakpoints), acessibilidade (teclado, NVDA).
Formato: Checklist Markdown para imprimir/usar no tablet durante validação.
```

**Contexto a fornecer:** Todos os documentos das ETAPAs 01-12 + código em staging + deploy staging URL.
**Resultado esperado:** Validação Final completa + Decisão de Publicação assinada.
**Como verificar:** Cada item do checklist verificado em staging com evidência. Decisão = seu julgamento final.

## Checklist de execução

- [ ] Matriz rastreabilidade RF → Implementação → Testes → CA completa
- [ ] Todos CAs Must/Should validados (auto + manual)
- [ ] Todos fluxos principais (ETAPA 06) testados manualmente em staging
- [ ] Fluxos alternativos/edge cases testados
- [ ] Verificação visual 100% telas vs. Direção Visual
- [ ] Responsividade 3 breakpoints em todas as telas
- [ ] Mensagens/estados UI revisados (textos, erros, loading, vazios, sucesso)
- [ ] Integrações funcionando em staging (WhatsApp, Email, SSE, DB)
- [ ] Permissões/RBAC testadas (bypass tentado)
- [ ] Limitações conhecidas e riscos residuais revisados
- [ ] Problemas impeditivos: ZERO P0/P1
- [ ] Decisão de Publicação: APROVADO / REQUER CORREÇÕES / ADIADO
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Validação Final documentada com evidências
- [ ] Decisão de Publicação assinada por você
- [ ] Se APROVADO: Checklist Pré-Publicação 100% pronto
- [ ] Se REQUER CORREÇÕES: lista clara do que corrigir + volta para ETAPA 10/11
- [ ] Se ADIADO: justificativa registrada + data de revalidação
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste da rastreabilidade:** Cada RF Must tem implementação + teste + CA?
- **Teste do staging:** Staging = réplica exata de produção (env vars, DB, domínio)?
- **Teste do rollback:** `vercel rollback` testado e funcionando?
- **Teste da decisão:** Você consegue explicar em 2 min por que APROVA/REPROVA?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Validar só "funciona" não "correspondente ao planejado" | Use matriz rastreabilidade: RF → CA → Teste → Evidência |
| Pular verificação visual "porque passou no lint" | Olho humano pega o que ferramenta não pega (alinhamento, espaçamento, feel) |
| Não testar em staging idêntico a prod | Staging deve espelhar prod: env vars, DB, domínio, CDN |
| Aceitar limitação sem documentar | Toda limitação = linha na Validação Final com decisão + mitigação |
| Delegar decisão de publicação | **VOCÊ assina.** Responsabilidade é sua. |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Bug P0 encontrado na validação | **PARE.** Volte para ETAPA 10 (corrigir) → 11 (testar) → 12 (segurança) → 13 (revalidar). Não publique com P0. |
| Gap de requisito (RF não implementado) | Decida: implementar agora (volta ETAPA 10) OU mover para Won't v1 (atualiza ETAPA 03 + 04 + 13). |
| Staging ≠ Produção (config) | Corrija staging primeiro. Não valide em ambiente diferente. |
| Integração falha só em produção | Teste com credenciais produção em staging (se possível) ou use feature flag. |
| Pressa para publicar | **Não pule validação.** Deploy ruim custa mais que atraso. |

## Condições para avançar

- ✅ Decisão de Publicação = **APROVADO** (assinada por você)
- ✅ Checklist Pré-Publicação 100% ✅
- ✅ Tag de versão criada
- ✅ CHANGELOG atualizado
- ✅ Rollback testado

## Próxima etapa

[[ETAPA 14 - Publicar o Software]]

## Materiais relacionados

- [[02 - Modelos/Publicação e Manutenção/Validação Final]]
- [[02 - Modelos/Publicação e Manutenção/Decisão de Publicação]]
- [[02 - Modelos/Publicação e Manutenção/Checklist de Publicação]]
- [[03 - Guias Práticos/Publicação/Checklist de Deploy]]
- [[04 - Referência/Glossário/Staging]]
- [[04 - Referência/Glossário/Smoke Test]]
- [[04 - Referência/Glossário/Rollback]]