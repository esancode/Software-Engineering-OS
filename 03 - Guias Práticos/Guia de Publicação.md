# Guia de Publicação

> **Local:** `03 - Guias Práticos/Guia de Publicação.md`
> **Baseado em:** [[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]], [[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]], [[02 - Modelos/Publicação e Manutenção/Plano de Manutenção]]
> **Objetivo:** Referência prática para deploy, monitoramento, incidentes e operações contínuas.

---

## Visão Geral do Pipeline

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Develop   │────▶│   Preview   │────▶│  Production │────▶│  Monitor    │
│  (local)    │     │  (PR/Vercel)│     │  (main)     │     │  (24/7)     │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
      │                   │                   │                   │
      ▼                   ▼                   ▼                   ▼
  lint, test,        Auto deploy,       Auto deploy,       Alertas,
  typecheck,         isolated DB,       smoke test,        dashboards,
  build              preview URL        rollback < 2min    runbook drills
```

---

## Deploy (GitOps via Vercel)

### Pré-requisitos
- Repo no GitHub conectado ao Vercel
- `main` = Production, PRs = Preview
- Variáveis de ambiente configuradas no Vercel (não no repo)

### Variáveis por Ambiente
| Variável | Preview | Production | Onde |
|----------|---------|------------|------|
| `NEXTAUTH_URL` | `https://pr-XXX.vercel.app` | `https://app.SEU_DOMINIO.com` | Vercel |
| `NEXTAUTH_SECRET` | Compartilhado | Compartilhado | Vercel |
| `DATABASE_URL` | Neon branch `preview-XXX` | Neon branch `main` | Vercel |
| `WHATSAPP_TOKEN` | Token test/sandbox | Token production | Vercel |
| `RESEND_API_KEY` | Mesmo | Mesmo | Vercel |

### Checklist Pré-Deploy (Todo PR)
```bash
# Execute LOCALMENTE antes de push
npm run lint        # 0 errors
npm run typecheck   # 0 errors
npm run test        # 100% pass
npm run build       # Success
```

### Checklist Deploy Production (Merge main)
- [ ] PR aprovado (auto-review documentado se solo)
- [ ] CI 100% verde (lint, typecheck, test, build, e2e, lighthouse)
- [ ] Changelog atualizado (`CHANGELOG.md`)
- [ ] Versão semântica definida (patch/minor/major)
- [ ] Tag git: `git tag v1.0.0 && git push origin v1.0.0`

### Smoke Test Pós-Deploy (Obrigatório - 10 min)
| # | Teste | Critério |
|---|-------|----------|
| 1 | Acessar `/login` | Carrega, sem erros console |
| 2 | Login Magic Link | Email < 30s, link funciona |
| 3 | `/dashboard` carrega | < 500ms |
| 4 | Webhook WhatsApp | Pedido aparece via SSE < 2s |
| 5 | Fluxo status completo | 4 cliques, timeline atualiza |
| 6 | Template WhatsApp | Chega no WhatsApp real |
| 7 | Relatório "Hoje" | Métricas corretas |
| 8 | Config: Produto + Horário | Salva, reflete no app |
| 9 | Logout + Login | Sessão funciona |
| 10 | Logs Vercel | 0 erros 5xx, latência normal |

### Rollback (< 2 min)
```bash
# Opção 1: Vercel CLI (preferido)
vercel rollback

# Opção 2: Git revert + push
git log --oneline -5
git revert <commit-hash>
git push origin main
```

---

## Monitoramento (Observabilidade)

### Dashboards Principais
| Dashboard | Métricas-Chave | Ferramenta |
|-----------|----------------|------------|
| **Vercel Analytics** | Web Vitals, Function Duration, Invocations, Errors | Vercel Dashboard |
| **Neon Dashboard** | CPU, Memory, Storage, Connections, Query Latency, PITR | Neon Console |
| **Custom (Logs)** | Webhook latency p50/p95/p99, SSE latency, Error rate, Dead letters | Vercel Logs / Sentry |
| **Business** | DAU, Pedidos/dia, Faturamento, Conversão, Churn, NPS | Metabase / Custom |

### Alertas Configurados
| Alerta | Condição | Severidade | Canal | Ação |
|--------|----------|------------|-------|------|
| **Webhook Down** | Error rate > 10% / 5min | SEV-1 | Telegram + Email | Verificar logs + Meta status |
| **DB Down** | Health check falha 2x | SEV-1 | Telegram + Email | Verificar Neon + conexões |
| **Auth Down** | Login failure > 50% / 5min | SEV-1 | Telegram | Verificar NextAuth + Resend |
| **SSE Degradado** | Disconnect > 20% / 10min | SEV-2 | Telegram | Verificar Vercel function duration |
| **Latência Alta** | Dashboard p95 > 2s / 15min | SEV-2 | Telegram | Verificar Neon query latency |
| **Error Rate** | 5xx > 1% / 10min | SEV-2 | Telegram | Verificar logs + rollback se regressão |
| **Dead Letter** | > 5 não resolvidas | SEV-3 | Email | Investigar + resolver |
| **Storage Neon** | > 80% usado | SEV-3 | Email | Arquivar / Upgrade |

### Setup Alertas (Telegram Bot)
```bash
# 1. Criar bot no @BotFather → obter TOKEN
# 2. Obter chat_id: curl "https://api.telegram.org/bot<TOKEN>/getUpdates"
# 3. Configurar no Vercel/Upptime/Sentry

# Exemplo Vercel Integration:
# Settings → Integrations → Telegram → Add Bot Token + Chat ID
```

### Upptime (Uptime Monitor Gratuito)
```yaml
# .github/workflows/uptime.yml (Upptime)
name: Uptime Monitor
on:
  schedule:
    - cron: "*/5 * * * *"  # A cada 5 min
  workflow_dispatch:

jobs:
  uptime:
    runs-on: ubuntu-latest
    steps:
      - uses: upptime/upptime-action@v2
        with:
          urls: "https://app.SEU_DOMINIO.com/api/health"
          telegram-bot-token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          telegram-chat-id: ${{ secrets.TELEGRAM_CHAT_ID }}
```

---

## Runbook de Incidentes (Resumo Rápido)

### Classificação
| Nível | Definição | Resposta | Resolução |
|-------|-----------|----------|-----------|
| **SEV-1** | Serviço down / perda dados | 15 min | 1 hora |
| **SEV-2** | Core degradado | 30 min | 4 horas |
| **SEV-3** | Bug não-crítico | 2 horas | 24 horas |

### Top 3 Incidentes + Mitigação

#### 🔴 INC-001: Webhook WhatsApp Falhando
```bash
# 1. Verificar Vercel logs
vercel logs --follow

# 2. Verificar token WhatsApp (expira 60 dias)
curl -H "Authorization: Bearer $TOKEN" https://graph.facebook.com/v20.0/me

# 3. Rollback se regressão
vercel rollback

# 4. Workaround: processar dead letters manualmente
```

#### 🔴 INC-002: Banco Indisponível
```bash
# 1. Neon Dashboard: status, CPU, storage, conexões
# 2. Se conexões esgotadas: vercel rollback (reinicia pool)
# 3. Se storage cheio: arquivar pedidos antigos + upgrade
# 4. Se outage regional: aguardar PITR restore
```

#### 🟠 INC-004: SSE Instável
```bash
# 1. Vercel Function duration > 10s? (timeout Hobby)
# 2. Conexões > 100? (limite Hobby)
# 3. Mitigação: upgrade Vercel Pro OU migrar para Pusher/Ably
# 4. Workaround: polling fallback 10s
```

### Comunicação Padrão
```markdown
**Interno (Telegram #incidentes):**
🚨 INCIDENTE SEV-1: Webhook WhatsApp Down
Componente: Webhook
Início: 14:32
Impacto: Pedidos não aparecem no dashboard
Status: Investigando
Próxima atualização: 14:45

**Externo (Status page / Email):**
Título: Instabilidade no recebimento de pedidos
Status: Investigando
Início: 14:32
Impacto: Pedidos via WhatsApp podem atrasar
Ação: Identificando causa, trabalhando para restaurar
Próxima atualização: 14:45
```

### Post-Mortem (Obrigatório SEV-1/2 em 48h)
```markdown
# Post-Mortem: [Título]

**Data:** 2026-01-15 | **Severidade:** SEV-1 | **Duração:** 45 min

## Resumo
Webhook falhou por token expirado. Rollback não ajudou (token no env). Renovado token no Meta → redeploy → restaurado.

## Timeline
| Horário | Evento |
|---------|--------|
| 14:32 | Alert disparado |
| 14:34 | Iniciou investigação |
| 14:38 | Root cause: token expirado |
| 14:40 | Token renovado no Meta |
| 14:42 | Deploy com novo token |
| 14:45 | Serviço restaurado |

## Root Cause (5 Porquês)
1. Por que webhook falhou? Token inválido (401)
2. Por que token inválido? Expirou (60 dias)
3. Por que não renovado? Sem alerta de expiração
4. Por que sem alerta? Não implementado
5. Por que não implementado? Priorização incorreta

## Action Items
| Ação | Responsável | Prazo | Status |
|------|-------------|-------|--------|
| Alerta token expiry 7 dias antes | Você | 2026-01-22 | ☐ |
| Documentar renovação no runbook | Você | 2026-01-20 | ☐ |
| Testar rollback mensalmente | Você | Mensal | ☐ |
```

---

## Rotinas Operacionais

### Diário (5 min - Automatizado + Manual)
- [ ] Uptime > 99.9% (Upptime)
- [ ] Error rate 5xx < 0.1% (Vercel/Sentry)
- [ ] Webhook latency p95 < 2s (Logs)
- [ ] SSE conexões estáveis (Métrica custom)
- [ ] Neon: CPU, storage, conexões < 80%
- [ ] Resend: delivery rate, bounce < 5%
- [ ] **Manual:** Revisar logs erro (novos padrões)
- [ ] **Manual:** Verificar dead letters > 0

### Semanal (30 min)
- [ ] Métricas negócio: DAU/WAU/MAU, pedidos, conversão, churn, NPS
- [ ] Alertas da semana: falsos positivos? Ajustar thresholds
- [ ] `npm outdated` + Dependabot PRs → merge patch/minor
- [ ] `npm audit` + CodeQL + TruffleHog → 0 high/critical
- [ ] Backup verify: Neon PITR timestamp < 1h
- [ ] Feedback usuários: ler emails/suporte
- [ ] Runbook drill: simular 1 incidente (tabletop)

### Mensal (2 horas)
- [ ] Rotacionar segredos: WhatsApp token (60d), NextAuth secret (90d)
- [ ] Teste rollback: `vercel rollback` + smoke test 10 min
- [ ] Teste restore DB: Neon PITR → branch teste → validar
- [ ] Revisão runbook: procedimentos válidos?
- [ ] Análise custos: dentro do free tier?
- [ ] Limpeza dead letters > 30 dias
- [ ] Lighthouse CI trends, bundle size, query latency
- [ ] Planejamento evolução: bugs, features "Should", tech debt

### Trimestral (Meio dia)
- [ ] Pen test leve: OWASP ZAP + manual critical paths
- [ ] LGPD audit: consentimentos, retenção, DPO
- [ ] Revisão arquitetura: ADRs válidos? Tech debt? Próximos ADRs?
- [ ] Capacity planning: crescimento → quando upgrade?
- [ ] Disaster recovery test: Restore PITR completo → validar RTO/RPO
- [ ] Revisão contratos: Termos, privacidade, terceiros

---

## Feature Flags (v1 Simples)

```typescript
// src/lib/flags.ts
export const flags = {
  ENABLE_SSE: process.env.ENABLE_SSE !== "false",
  ENABLE_PWA: process.env.ENABLE_PWA !== "false",
  ENABLE_TEMPLATES: process.env.ENABLE_TEMPLATES !== "false",
  ENABLE_REPORTS: process.env.ENABLE_REPORTS !== "false",
  MAX_PEDIDOS_PAGE: Number(process.env.MAX_PEDIDOS_PAGE) || 20,
  WEBHOOK_QUEUE_ENABLED: process.env.WEBHOOK_QUEUE_ENABLED !== "false",
} as const

// Uso:
if (flags.ENABLE_SSE) { /* ... */ }
```

**Configuração no Vercel:** Settings → Environment Variables → Adicionar flags como boolean/number.

---

## Backup e Recuperação

### Neon (PostgreSQL)
| Aspecto | Config | Validação |
|---------|--------|-----------|
| **PITR** | Ativado (7d free / 30d paid) | Mensal: restore point-in-time |
| **Branch `main`** | Production | Protegida |
| **Branch `preview-*`** | PRs (ephemeral) | Auto-delete 7d pós-merge |
| **RPO** | < 1h (continuous) | Verificar lag replicação |
| **RTO** | < 4h | Testar trimestral |

### Código e Config
| Item | Backup | Recuperação |
|------|--------|-------------|
| **Repo Git** | GitHub + local clones | `git clone` |
| **Vercel Config** | `vercel.json` no repo + dashboard | Reimport project |
| **Env Vars** | `ENV.md` (sem valores) + Vercel | Recreate no Vercel |
| **Secrets** | 1Password/Bitwarden | Rotacionar + update Vercel |

---

## Dívida Técnica (Gestão Contínua)

### Tech Debt Log
```markdown
# Tech Debt Log

| ID | Descrição | Impacto | Esforço | Prioridade | Sprint | Status |
|----|-----------|---------|---------|------------|--------|--------|
| TD-001 | Parser WhatsApp: melhorar com LLM | Médio | 3 dias | Should | v1.1 | ☐ |
| TD-002 | Testes E2E flaky: pedido-flow | Alto | 1 dia | Must | Atual | 🔄 |
| TD-003 | Bundle: lucide-react → lucide-static | Baixo | 2h | Could | v1.1 | ☐ |
| TD-004 | SSE: migrar Pusher se > 100 conn | Médio | 2 dias | Should | v1.2 | ☐ |
| TD-005 | LGPD: DPIA completo se necessário | Alto | 5 dias | Must | v1.1 | ☐ |
```

### Alocação Capacidade
- **70%** Features / Bug fixes (roadmap)
- **20%** Tech debt / Refatoração / Testes
- **10%** Exploração / Spikes / Aprendizado

---

## Checklist de Continuidade (Business Continuity)

- [ ] **Sucessão:** Documentação suficiente para outro dev assumir? (README, ADRs, Runbook)
- [ ] **Acesso:** Credenciais Vercel/Neon/Resend/Meta/GitHub no gerenciador compartilhado?
- [ ] **Domínio:** Renovação automática? DNS gerenciado onde?
- [ ] **Código:** Repo público/privado? Backup espelho (GitLab/Bitbucket)?
- [ ] **Dados:** Export automatizado semanal (pg_dump) para storage externo (S3/GCS)?
- [ ] **Comunicação:** Lista contatos usuários-chave para aviso crítico?
- [ ] **Legal:** Contratos assinados? LGPD compliance documentado?

---

## Roadmap Pós-MVP (6 Meses)

| Mês | Foco | Entregas |
|-----|------|----------|
| **1** | Estabilização | Bug fixes, performance, feedback piloto, docs |
| **2** | Qualidade | Test coverage 90%, a11y audit, load test, tech debt |
| **3** | Features "Should" | Multi-user, relatórios automáticos, export PDF |
| **4** | Growth | Landing page, trial→paid, billing (Stripe), métricas |
| **5** | Escala | Multi-loja (RLS), Pusher SSE, cache Redis, otimização |
| **6** | Ecossistema | API pública, webhooks saída, iFood, mobile app (TWA) |

---

## Orçamento Operacional

| Serviço | Free Tier | Paid Tier | Gatilho Upgrade |
|---------|-----------|-----------|-----------------|
| **Vercel** | Hobby: $0 | Pro: $20/mês | Function timeout > 10s, > 100 SSE, bandwidth > 100GB |
| **Neon** | Free: 512MB, 190h | Scale: $19/mês + usage | Storage > 400MB, compute > 150h |
| **Resend** | Free: 3k/dia | Pro: $20/mês (50k/dia) | Emails > 2.5k/dia |
| **Vercel Blob** | Free: 1GB | $0.15/GB | Storage > 800MB |
| **Monitoramento** | Upptime (grátis) | Datadog/Sentry: $0-50 | Necessidade APM real |

**MVP (Free):** $0/mês | **Growth (Paid):** ~$60-100/mês

---

## Próximos Passos
- [[04 - Referência/Glossário]] — Termos técnicos
- [[04 - Referência/Ferramentas]] — Stack completa
- [[04 - Referência/Conceitos]] — Conceitos fundamentais
- [[05 - Projetos/README.md]] — Como iniciar novo projeto
- [[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]] — Checklist completo
- [[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]] — Runbook detalhado
- [[02 - Modelos/Publicação e Manutenção/Plano de Manutenção]] — Plano completo

---

## Referências Rápidas
- [[04 - Referência/Glossário/Deploy]]
- [[04 - Referência/Glossário/Rollback]]
- [[04 - Referência/Glossário/Semantic Versioning]]
- [[04 - Referência/Glossário/GitOps]]
- [[04 - Referência/Glossário/Observability]]
- [[04 - Referência/Glossário/SLA/SLO/SLI]]
- [[04 - Referência/Glossário/RTO/RPO]]
- [[04 - Referência/Glossário/Feature Flags]]
- [[04 - Referência/Ferramentas/Vercel]]
- [[04 - Referência/Ferramentas/Neon]]
- [[04 - Referência/Ferramentas/Upptime]]