# ETAPA 14 — Publicar o Software

## Objetivo

Disponibilizar o software em **produção** de forma segura, verificável e com capacidade de rollback rápido.

## Por que esta etapa existe?

Deploy não é "rodar um comando". É um processo com verificação pré e pós, plano de reversão, e validação de que o software realmente funciona no ambiente real com usuários reais.

## Quando começar

- ETAPA 13 concluída: **Decisão de Publicação = APROVADO** (assinada por você)
- Checklist Pré-Publicação 100% ✅
- Tag de versão criada (`v1.0.0`)
- CHANGELOG atualizado

## O que você precisa fazer

1. **Escolha/confirme o ambiente de publicação** — Vercel (já definido na ETAPA 05)
2. **Configure domínio personalizado** — DNS + Vercel + SSL
3. **Configure variáveis de ambiente de PRODUÇÃO** — na plataforma (Vercel), não no código
4. **Configure serviços externos para produção** — WhatsApp webhook URL, Resend domínio, Sentry DSN
5. **Configure banco de dados de produção** — Neon branch protection, connection pooling
6. **Execute migrações (se houver)** — `prisma migrate deploy` (automático no build ou manual)
7. **Prepare dados iniciais (seed)** — se necessário (ex: configs padrão)
8. **Realize cópia de segurança pré-deploy** — Neon branch snapshot
9. **Execute o deploy** — `vercel --prod` ou merge para `main` (se auto-deploy)
10. **Verificação pós-deploy (Smoke Tests)** — fluxos críticos em produção
11. **Teste de rollback** — confirme que `vercel rollback` funciona
12. **Monitore nas primeiras 2 horas** — Sentry, UptimeRobot, logs, métricas
13. **Registre a versão publicada** — tag, commit, data, observações
14. **Produza o documento: Registro de Versão + Checklist de Publicação Executado**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Configurar domínio/DNS | 👤 USUÁRIO (registro.br, Cloudflare, etc.) |
| Configurar env vars produção na Vercel | 👤 USUÁRIO (valores reais) |
| Atualizar webhook WhatsApp para URL prod | 👤 USUÁRIO (Meta Developers Console) |
| Executar deploy | 👤 USUÁRIO (comando) ou 🤖 IA (CI/CD auto) |
| Smoke tests produção | ✅ VALIDAÇÃO (você testa fluxos críticos) |
| Testar rollback | ✅ VALIDAÇÃO (você executa `vercel rollback`) |
| Monitorar primeiras 2h | 👤 USUÁRIO (Sentry, UptimeRobot, Vercel Analytics) |
| Registrar versão | 👤 USUÁRIO + 🤖 IA (documenta) |
| Aprovar "produção estável" | 👤 USUÁRIO (após 24h sem incidentes) |

## Ferramentas recomendadas

| Finalidade | Ferramenta |
|------------|------------|
| Deploy | Vercel CLI (`vercel --prod`) ou Git push `main` (auto-deploy) |
| DNS | Cloudflare, registro.br, GoDaddy, etc. |
| SSL | Vercel (automático + Let's Encrypt) |
| Variáveis de ambiente | Vercel Dashboard → Settings → Environment Variables |
| WhatsApp Webhook | Meta Developers Console → WhatsApp → Configuration |
| Banco | Neon Console → Branches → Protection → Production |
| Monitoramento | Sentry, UptimeRobot, Vercel Analytics, Vercel Logs |
| Rollback | `vercel rollback [deployment-url]` ou Dashboard |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Checklist de Publicação | `02 - Modelos/Publicação e Manutenção/Checklist de Publicação.md` | `05 - Projetos/SeuProjeto/08 - Publicação/` |
| Registro de Versão | `02 - Modelos/Publicação e Manutenção/Registro de Versão.md` | `05 - Projetos/SeuProjeto/08 - Publicação/` |

### Condicionais
- Runbook de Deploy (se processo complexo/multi-step)
- Plano de Comunicação para Usuários (se migração de sistema existente)

### Opcionais
- Post-mortem de Deploy (se houver incidente)

## Como produzir os documentos

### Checklist de Publicação (Executado)

```markdown
# Checklist de Publicação Executado — PedidosSimples v1.0.0

## Informações
**Versão:** v1.0.0
**Commit:** `abc123def`
**Data/Hora Deploy:** 2026-10-17 10:05 BRT
**Deploy URL:** https://pedidosimples-abc123.vercel.app
**Domínio Produção:** https://pedidosimples.com
**Responsável:** [Seu Nome]

---

## Pré-Deploy (Confirmado na ETAPA 13)
- [x] Decisão de Publicação = APROVADO (assinada)
- [x] Tag `v1.0.0` criada: `git tag v1.0.0 && git push origin v1.0.0`
- [x] `CHANGELOG.md` atualizado
- [x] Variáveis de ambiente PRODUÇÃO configuradas na Vercel (12 vars)
- [x] Domínio `pedidosimples.com` → Vercel (DNS CNAME + verificação)
- [x] SSL ativo (Vercel auto)
- [x] Webhook WhatsApp atualizado: `https://pedidosimples.com/api/webhooks/whatsapp`
- [x] Sentry DSN produção configurado
- [x] UptimeRobot monitorando `pedidosimples.com`
- [x] Neon branch `production` protegida (no delete, no direct push)
- [x] Backup Neon pré-deploy: branch `pre-deploy-v1.0.0` criada
- [x] Rollback testado em staging: `vercel rollback` funcionou

---

## Deploy
| Passo | Comando/Ação | Resultado | Hora |
|-------|--------------|-----------|------|
| 1 | `vercel --prod --token=$VERCEL_TOKEN` | Build + Deploy iniciado | 10:05 |
| 2 | Build Next.js | ✅ Sucesso (2m 34s) | 10:08 |
| 3 | Prisma Migrate Deploy | ✅ Sucesso (12s) | 10:08 |
| 4 | Funções deployadas | ✅ 12 functions | 10:09 |
| 5 | Domínio apontado | ✅ `pedidosimples.com` ativo | 10:10 |
| 6 | Deploy finalizado | ✅ `https://pedidosimples.com` | 10:10 |

**Tempo total:** 5 minutos

---

## Pós-Deploy Imediato (Smoke Tests - Primeiros 15 min)

| Teste | Fluxo | Resultado | Evidência |
|-------|-------|-----------|-----------|
| 1 | Acesso `https://pedidosimples.com` → Redirect login | ✅ OK | Screenshot |
| 2 | Login Magic Link → Dashboard | ✅ OK | Email recebido < 30s |
| 3 | Dashboard carrega lista pedidos | ✅ OK | < 2s |
| 4 | Webhook WhatsApp (teste real) → Pedido criado | ✅ OK | Meta test webhook + Dashboard |
| 5 | Atualizar status "Preparando" → Tempo real | ✅ OK | SSE confirmado |
| 6 | Responder cliente (template) → WhatsApp entrega | ✅ OK | Meta delivery receipt |
| 7 | Configurações → Salvar horários | ✅ OK | Persiste + reflete |
| 8 | Histórico → Filtros → Resultado | ✅ OK | Paginação ok |
| 9 | Logout → Redirect login | ✅ OK | Sessão limpa |
| 10 | Acessibilidade rápida (Tab + NVDA) | ✅ OK | Navegação ok |

**Todos smoke tests: ✅ APROVADOS**

---

## Verificação de Rollback (Obrigatório)

| Teste | Comando | Tempo | Resultado |
|-------|---------|-------|-----------|
| Rollback para deploy anterior | `vercel rollback https://pedidosimples-abc123.vercel.app` | 45s | ✅ Funcionou |
| Verificar dados íntegros pós-rollback | Neon: queries de contagem | 10s | ✅ Dados ok |
| Re-deploy versão atual | `vercel --prod` | 3m | ✅ Funcionou |

**Rollback validado: ✅ < 1 minuto**

---

## Monitoramento Primeiras 2 Horas

| Métrica | Ferramenta | Limite Alerta | Status 2h |
|---------|------------|---------------|-----------|
| Error Rate | Sentry | > 1% / 5min | 0% ✅ |
| Uptime | UptimeRobot | < 99.9% | 100% ✅ |
| P95 Latency API | Vercel Analytics | > 2s | 450ms ✅ |
| P95 Latency Pages | Vercel Analytics | > 3s | 1.2s ✅ |
| Webhook WhatsApp | Sentry + Logs | Falhas > 0 | 0 falhas ✅ |
| Auth Success Rate | NextAuth + Sentry | < 95% | 100% ✅ |
| CPU/Memory (Vercel) | Vercel Dashboard | > 80% | 12% / 45MB ✅ |

**Nenhum alerta disparado nas primeiras 2h. ✅**

---

## Checklist Pós-Deploy (Primeiras 24h)
- [x] Smoke tests passam
- [x] Rollback testado e funcional
- [x] Monitoramento ativo (Sentry, UptimeRobot, Vercel)
- [x] Alertas testados (forçar erro → Sentry alerta)
- [x] Logs estruturados visíveis (Vercel + Sentry)
- [x] Auditoria ações sensíveis registrando
- [x] Métricas Vercel Analytics ativas
- [x] Domínio acessível globalmente (teste externo)
- [x] WhatsApp webhook recebendo eventos reais
- [x] Nenhum erro 500 não tratado

---

## Assinatura
**Deploy Executado por:** [Seu Nome]
**Data/Hora:** 2026-10-17 10:05 BRT
**Status Final:** **DEPLOY CONCLUÍDO COM SUCESSO**
**Próxima Revisão (Estabilidade):** 2026-10-18 10:00 BRT (24h)
```

### Registro de Versão

```markdown
# Registro de Versão — PedidosSimples v1.0.0

## Identificação
| Campo | Valor |
|-------|-------|
| **Versão** | v1.0.0 |
| **Tag Git** | `v1.0.0` |
| **Commit** | `abc123def456789` (main) |
| **Data Release** | 2026-10-17 |
| **Autor** | [Seu Nome] |
| **Ambiente** | Produção (Vercel + Neon) |
| **Domínio** | https://pedidosimples.com |

---

## O que foi Entregue (MVP v1)

### Features Principais (Must/Should ETAPA 03)
- ✅ Recebimento de pedidos via WhatsApp Business API (Webhook)
- ✅ Dashboard com lista de pedidos em tempo real (SSE)
- ✅ Atualização de status: Novo → Preparando → Pronto → Entregue
- ✅ Resposta ao cliente via WhatsApp (templates + mensagem customizada)
- ✅ Histórico de pedidos com filtros (data, status, cliente)
- ✅ Configuração de estabelecimento (nome, telefone, horários)
- ✅ Cadastro de produtos (nome, descrição, preço em centavos)
- ✅ Onboarding guiado (3 passos: WhatsApp, Loja, Produtos)
- ✅ Autenticação: Magic Link (email) + Credentials (fallback)
- ✅ RBAC: OWNER (acesso total) vs STAFF (apenas pedidos)

### Features Parciais / Won't v1 (Documentadas)
- ⚠️ Relatório semanal (RF-006): apenas total do dia implementado
- ⚠️ Rate limiting API: aceito risco residual (v1.1)
- ⚠️ Runbook Disaster Recovery: documentar v1.1
- ⚠️ Push notifications nativas: PWA instalável como alternativa

---

## Mudanças Técnicas Relevantes

### Arquitetura
- Next.js 14 App Router + TypeScript strict
- Server Components por default, Client Components só quando necessário
- Prisma ORM + PostgreSQL (Neon serverless)
- NextAuth.js (Credentials + Email Magic Link)
- Tempo real: Server-Sent Events (SSE) nativo
- Deploy: Vercel (Edge Functions para API, Static para páginas)

### Padrões Estabelecidos
- Validação Zod compartilhada (client + server)
- Error handling: `ApiError` class + `jsonError` helper
- Logs estruturados `pino` (requestId, userId, action, duration)
- Auditoria: tabela `AuditLog` para ações sensíveis
- Componentes: shadcn/ui base + tokens Direção Visual (Tailwind config)
- Testes: Vitest (unit/int) + Playwright (e2e) — >70% cobertura

---

## Configurações de Produção

### Variáveis de Ambiente (Vercel)
| Variável | Configurada? |
|----------|--------------|
| DATABASE_URL (Neon production) | ✅ |
| NEXTAUTH_SECRET (produção único) | ✅ |
| NEXTAUTH_URL (https://pedidosimples.com) | ✅ |
| WHATSAPP_API_TOKEN (prod) | ✅ |
| WHATSAPP_PHONE_NUMBER_ID (prod) | ✅ |
| WHATSAPP_WEBHOOK_VERIFY_TOKEN (prod) | ✅ |
| RESEND_API_KEY (prod) | ✅ |
| EMAIL_FROM (noreply@pedidosimples.com) | ✅ |
| SENTRY_DSN (prod) | ✅ |

### Serviços Externos
| Serviço | Configuração Produção |
|---------|----------------------|
| WhatsApp Webhook | `https://pedidosimples.com/api/webhooks/whatsapp` |
| Resend Domínio | `pedidosimples.com` verificado |
| Sentry Project | `pedidosimples-prod` |
| UptimeRobot | `https://pedidosimples.com` (1 min interval) |
| Neon Branch | `production` (protected, pooling enabled) |

---

## Métricas de Qualidade (No Momento do Release)

| Métrica | Valor | Meta | Status |
|---------|-------|------|--------|
| Cobertura Unit/Int | 78% | > 70% | ✅ |
| Cobertura Branches | 65% | > 60% | ✅ |
| Testes E2E Fluxos Críticos | 100% (7/7) | 100% | ✅ |
| Bugs P0 | 0 | 0 | ✅ |
| Bugs P1 | 0 | ≤ 2 | ✅ |
| Bugs P2 | 3 | ≤ 5 | ✅ |
| Acessibilidade (axe-core) | 0 violations | 0 | ✅ |
| Lighthouse Performance | 92 | > 80 | ✅ |
| Lighthouse Acessibilidade | 100 | > 90 | ✅ |
| `pnpm audit --prod` | 0 critical/high | 0 | ✅ |
| Build Time | 2m 34s | < 5m | ✅ |
| Deploy Time | 5m | < 10m | ✅ |

---

## Riscos Residuais Aceitos (da ETAPA 12)

| Risco | Aceitação | Mitigação v1.1 |
|-------|-----------|----------------|
| Rate limiting API | ✅ Aceito | Arcjet/Upstash |
| Rate limiting Auth | ✅ Aceito | Arcjet/Upstash |
| Runbook DR | ✅ Aceito | Documentar v1.1 |
| Relatório semanal | ✅ Aceito (Won't v1) | Implementar v1.1 |

---

## Próximos Passos (v1.1 Roadmap)

1. **Rate Limiting** (Arcjet) — Segurança P1
2. **Relatório Semanal/Export CSV** — Feature Should
3. **Runbook Disaster Recovery** — Operacional
4. **Push Notifications (Service Worker)** — UX
5. **Multi-estabelecimento** — Feature Could (feedback usuários)
6. **Integração iFood/Rappi** — Feature Could (se demanda)

---

## Arquivos de Referência
- Validação Final: `08 - Publicação/Validação Final.md`
- Decisão de Publicação: `08 - Publicação/Decisão de Publicação.md`
- Revisão Segurança: `07 - Segurança/Revisão de Segurança.md`
- Plano de Testes: `06 - Testes/Plano de Testes.md`
- CHANGELOG: `CHANGELOG.md` (raiz do projeto)
```

## Como trabalhar com IA nesta etapa

**Prompt para gerar Checklist de Publicação personalizado:**
```
Meu stack: Next.js + Vercel + Neon + WhatsApp API + Resend + Sentry.
Gere Checklist de Publicação completo com:
1. Pré-deploy (env vars, domínio, webhooks, backup, rollback test)
2. Deploy (comandos, tempos esperados, success criteria)
3. Smoke tests pós-deploy (10 fluxos críticos com passos exatos)
4. Rollback test (comando, tempo alvo < 1min, verificação dados)
5. Monitoramento 2h (métricas, ferramentas, limites alerta)
6. Checklist 24h
Formato: Tabelas Markdown prontas para preencher durante deploy.
```

**Prompt para gerar Registro de Versão:**
```
Com base em: Validação Final + Decisão de Publicação + Plano de Implementação + CHANGELOG.
Gere Registro de Versão v1.0.0 com:
- Identificação completa
- Features entregues (Must/Should) vs Parciais/Won't
- Mudanças técnicas relevantes
- Configurações produção (env vars, serviços)
- Métricas de qualidade no release
- Riscos residuais aceitos
- Roadmap v1.1
Formato: Markdown completo.
```

**Prompt para smoke tests automatizados (Playwright):**
```
Gere suite Playwright de SMOKE TESTS para rodar IMEDIATAMENTE após deploy produção.
Fluxos: 1) Login → Dashboard 2) Webhook WhatsApp → Pedido aparece 3) Status update 4) Reply client 5) Config save.
Requisitos: rodar em < 3 min, headless, baseURL via env, falhar rápido se crítico.
Gere: `e2e/smoke.prod.spec.ts` + script `pnpm test:smoke:prod`.
```

**Contexto a fornecer:** Decisão de Publicação + Validação Final + Arquitetura (ETAPA 05) + Configuração Ambientes (ETAPA 09).
**Resultado esperado:** Checklist de Publicação 100% executado + Registro de Versão completo + Deploy estável confirmado após 24h.
**Como verificar:** Cada item do checklist com evidência (screenshot, log, timestamp). Rollback testado. Monitoramento limpo 2h.

## Checklist de execução

- [ ] Domínio configurado (DNS + Vercel + SSL ativo)
- [ ] Variáveis de ambiente PRODUÇÃO todas configuradas na Vercel
- [ ] Webhook WhatsApp atualizado para URL produção
- [ ] Sentry DSN produção configurado
- [ ] UptimeRobot monitorando domínio produção
- [ ] Neon branch `production` protegida
- [ ] Backup pré-deploy realizado (Neon branch snapshot)
- [ ] Rollback testado em staging (funciona < 1 min)
- [ ] Deploy executado (`vercel --prod` ou merge `main`)
- [ ] Build + Migrações + Funções deployadas com sucesso
- [ ] Smoke tests 10/10 passando em produção
- [ ] Rollback testado em produção (funciona < 1 min)
- [ ] Monitoramento 2h: zero alertas, métricas saudáveis
- [ ] Checklist 24h: tudo verde
- [ ] Tag de versão pushada (`git push origin v1.0.0`)
- [ ] CHANGELOG.md atualizado
- [ ] Registro de Versão completo
- [ ] Checklist de Publicação preenchido e assinado
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Deploy em produção realizado com sucesso
- [ ] Smoke tests críticos passando
- [ ] Rollback validado (funciona e dados íntegros)
- [ ] Monitoramento ativo e limpo por 2h+
- [ ] Checklist de Publicação 100% executado
- [ ] Registro de Versão completo
- [ ] Tag Git + CHANGELOG sincronizados
- [ ] Após 24h sem incidentes: **Produção Estável ✅**
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste do domínio:** `curl -I https://pedidosimples.com` → 200, SSL válido, headers segurança
- **Teste do webhook:** Meta Developers Console → "Test Webhook" → 200 OK + pedido criado
- **Teste do rollback:** `vercel rollback` → URL anterior responde + dados ok
- **Teste do monitoramento:** Forçar erro 500 → Sentry alerta em < 2 min

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Deploy sem testar rollback | **Obrigatório testar rollback ANTES do deploy produção** |
| Env vars produção diferentes de staging | Checklist: compare cada var staging vs prod antes do deploy |
| Webhook WhatsApp não atualizado | Atualize no Meta Console ANTES do deploy. Teste "Send Test" |
| Não monitorar pós-deploy | 2h mínimo. Alertas configurados. Você fica disponível. |
| Esquecer tag Git / CHANGELOG | Automatize: `release` script ou GitHub Action no tag push |
| Deploy sexta à tarde / véspera feriado | **Nunca.** Deploy terça-quarta, manhã. Você disponível 24h pós. |

## Se algo der errado

| Situação | Ação Imediata |
|----------|---------------|
| Build falha | Ver logs Vercel → corrigir → novo deploy (não afeta produção atual) |
| Deploy passa mas smoke test falha | **ROLLBACK IMEDIATO** (`vercel rollback`) → investigar → corrigir → novo deploy |
| Erro 500 em produção | Sentry alerta → `vercel rollback` se crítico → hotfix → deploy |
| WhatsApp webhook falha | Verificar token, URL, logs → corrigir env var → redeploy (sem migração) |
| Domínio não propaga | Verificar DNS (Cloudflare/registro.br) → aguardar TTL → testar `dig` |
| Migração Prisma falha | `vercel rollback` → corrigir migração (novo `prisma migrate dev`) → novo deploy |

## Condições para avançar

- ✅ Deploy produção realizado
- ✅ Smoke tests passando
- ✅ Rollback testado e funcional
- ✅ 2h monitoramento limpo
- ✅ 24h sem incidentes críticos = **Produção Estável**
- ✅ Registro de Versão + Checklist salvos
- ✅ Acompanhamento: Etapa 14 concluída

## Próxima etapa

[[ETAPA 15 - Monitorar e Manter]]

## Materiais relacionados

- [[02 - Modelos/Publicação e Manutenção/Checklist de Publicação]]
- [[02 - Modelos/Publicação e Manutenção/Registro de Versão]]
- [[03 - Guias Práticos/Publicação/Checklist de Deploy]]
- [[03 - Guias Práticos/Publicação/Runbook de Rollback]]
- [[04 - Referência/Glossário/Deploy]]
- [[04 - Referência/Glossário/Smoke Test]]
- [[04 - Referência/Glossário/Rollback]]
- [[04 - Referência/Glossário/Blue-Green Deploy]]