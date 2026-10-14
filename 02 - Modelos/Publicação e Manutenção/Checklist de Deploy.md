# Checklist de Deploy

> **Copie para:** `05 - Projetos/SeuProjeto/08 - Publicação e Manutenção/Checklist de Deploy.md`
> Baseado na ETAPA 14. **Checklist obrigatório para cada deploy** (preview e production).

---

## Princípios de Deploy

- **Deploy = Merge em `main`** (GitOps)
- **Preview = Pull Request** (automático no Vercel)
- **Rollback = `vercel rollback` ou `git revert` + push** (< 2 min)
- **Zero-downtime** (Vercel gerencia)
- **Feature flags** para lançamentos graduais (opcional v1)

---

## Checklist Pré-Deploy (Todo PR / Push main)

### Código e Qualidade
- [ ] `npm run lint` — 0 errors, 0 warnings
- [ ] `npm run typecheck` — 0 errors
- [ ] `npm run test` — 100% pass, coverage ≥ targets
- [ ] `npm run test:e2e` — @smoke + @core passam
- [ ] `npm run build` — Sucesso (Vercel build idêntico)
- [ ] Bundle size < 200KB gzipped (first load JS)
- [ ] Lighthouse CI — Perf ≥ 90, A11y ≥ 95

### Segurança
- [ ] `npm audit` — 0 high/critical (ou documentados com prazo)
- [ ] CodeQL / Semgrep — 0 high/critical
- [ ] TruffleHog / GitLeaks — 0 secrets
- [ ] Variáveis de ambiente: apenas em Vercel/Neon (não no repo)
- [ ] Dependências atualizadas (patch/minor sem breaking)

### Banco de Dados
- [ ] Migrações Prisma: `prisma migrate deploy` roda sem erro
- [ ] Migrações reversíveis (ou plano de rollback DB)
- [ ] Seed development não roda em production
- [ ] Índices criados nas migrações (não depois)

### Integrações
- [ ] Webhook URL correta (preview vs production)
- [ ] WhatsApp templates aprovados no Meta
- [ ] Resend domínio verificado
- [ ] Neon branch correta (main = production)

---

## Checklist Deploy Preview (Pull Request)

> **Automático no Vercel a cada PR**

- [ ] Preview URL gerada e acessível
- [ ] Variáveis de ambiente de preview configuradas
- [ ] Banco de dados preview branch (Neon) isolado
- [ ] Smoke test automatizado roda no preview (opcional: GitHub Action)
- [ ] Preview comentado no PR com link
- [ ] Reviewer testa manualmente no preview
- [ ] Merge apenas após CI verde + aprovação

---

## Checklist Deploy Production (Merge em `main`)

### Antes do Merge
- [ ] PR aprovado (mesmo solo: auto-review documentado)
- [ ] CI 100% verde (lint, typecheck, test, build, e2e, lighthouse)
- [ ] Changelog atualizado (`CHANGELOG.md`)
- [ ] Versão semântica definida (patch/minor/major)
- [ ] Tag git criada: `git tag v1.0.0 && git push origin v1.0.0`

### Durante/Após Deploy (Vercel Automático)
- [ ] Build production inicia automaticamente
- [ ] Deploy conclui sem erros (verificar Vercel dashboard)
- [ ] Funções deployment: todas `READY`
- [ ] Domínio custom acessível: `https://app.SEU_DOMINIO.com`
- [ ] SSL válido (certificado Vercel/Let's Encrypt)
- [ ] Health check: `GET /api/health` → 200 OK

### Smoke Test Pós-Deploy (Obrigatório - 10 min)
| Teste | Ação | Critério |
|-------|------|----------|
| 1 | Acessar `/login` | Carrega, sem erros console |
| 2 | Login Magic Link (email real) | Email chega < 30s, link funciona |
| 3 | Acessar `/dashboard` | Lista carrega < 500ms |
| 4 | Simular webhook WhatsApp | Pedido aparece via SSE < 2s |
| 5 | Fluxo status: Novo → Entregue | 4 cliques, timeline atualiza |
| 6 | Enviar template WhatsApp | Chega no WhatsApp real |
| 7 | Relatório "Hoje" | Métricas corretas |
| 8 | Config: Produto + Horário | Salva, reflete no app |
| 9 | Logout + Login novamente | Sessão funciona |
| 10 | Verificar logs Vercel | 0 erros 5xx, latência normal |

### Monitoramento Pós-Deploy (Primeiras 2 Horas)
- [ ] Error rate < 0.1% (Vercel Logs / Sentry)
- [ ] Webhook latency p95 < 2s
- [ ] SSE conexões estáveis (sem picos desconexão)
- [ ] Database: CPU, connections, latency normais (Neon dashboard)
- [ ] Uptime monitor: 100% (Upptime/UptimeRobot)

---

## Rollback Plan (Se Smoke Test Falhar)

| Cenário | Comando | Tempo | Validação Pós-Rollback |
|---------|---------|-------|------------------------|
| Build falhou | Não deploya (Vercel bloqueia) | - | Corrigir + novo PR |
| Deploy succeeded mas smoke test falha | `vercel rollback` | < 30s | Smoke test no deployment anterior |
| Bug crítico em production | `vercel rollback` OU `git revert <commit> && git push origin main` | < 2 min | Smoke test + monitoramento 30 min |
| Migração DB problemática | `vercel rollback` + `prisma migrate resolve --rolled-back` | < 10 min | Verificar schema + dados |

> **Regra:** Rollback SEMPRE preferível a hotfix em production sob pressão.

---

## Variáveis de Ambiente por Ambiente

| Variável | Preview | Production | Onde Configurar |
|----------|---------|------------|-----------------|
| `NEXTAUTH_URL` | `https://pr-xxx.vercel.app` | `https://app.SEU_DOMINIO.com` | Vercel |
| `NEXTAUTH_SECRET` | Mesmo (compartilhado) | Mesmo | Vercel (Environment Variable) |
| `DATABASE_URL` | Neon branch `preview-xxx` | Neon branch `main` | Vercel |
| `DIRECT_URL` | Neon branch `preview-xxx` (direct) | Neon branch `main` (direct) | Vercel |
| `WHATSAPP_TOKEN` | Token test/sandbox | Token production | Vercel |
| `WHATSAPP_PHONE_NUMBER_ID` | Test number ID | Production number ID | Vercel |
| `RESEND_API_KEY` | Mesmo | Mesmo | Vercel |
| `BLOB_READ_WRITE_TOKEN` | Preview token | Production token | Vercel |
| `NEXT_PUBLIC_APP_URL` | Preview URL | Production URL | Vercel |

---

## Versionamento Semântico (SemVer)

| Tipo | Quando | Exemplo |
|------|--------|---------|
| **Patch** (1.0.0 → 1.0.1) | Bug fixes, docs, chores, refactors sem breaking | Fix typo, ajuste CSS, update dep patch |
| **Minor** (1.0.0 → 1.1.0) | Nova feature backward-compatible | Novo template, novo relatório, campo opcional |
| **Major** (1.0.0 → 2.0.0) | Breaking change | Remove campo, muda API, muda auth flow |

```bash
# Script sugerido (package.json)
"version:patch": "npm version patch && git push origin main --tags",
"version:minor": "npm version minor && git push origin main --tags",
"version:major": "npm version major && git push origin main --tags",
```

---

## Checklist Pós-Deploy (Dia Seguinte)

- [ ] Métricas: usuários ativos, pedidos processados, error rate
- [ ] Feedback usuários piloto (se houver)
- [ ] Logs: procurar warnings/errors novos
- [ ] Dependabot: PRs de segurança criados?
- [ ] Backup Neon: PITR funcionando (verificar timestamp)
- [ ] Certificado SSL: renovação automática OK

---

## Próxima Etapa
[[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/08 - Publicação e Manutenção/ETAPA 14 - Publicar o Software]]
- [[01 - Manual de Desenvolvimento/08 - Publicação e Manutenção/ETAPA 15 - Monitorar e Manter]]
- [[02 - Modelos/Segurança e Validação/Plano de Validação]]
- [[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]]
- [[02 - Modelos/Publicação e Manutenção/Plano de Manutenção]]
- [[04 - Referência/Glossário/Deploy]]
- [[04 - Referência/Glossário/Rollback]]
- [[04 - Referência/Glossário/Semantic Versioning]]
- [[04 - Referência/Glossário/GitOps]]