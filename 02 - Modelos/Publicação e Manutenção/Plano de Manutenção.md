# Plano de Manutenção

> **Copie para:** `05 - Projetos/SeuProjeto/08 - Publicação e Manutenção/Plano de Manutenção.md`
> Baseado na ETAPA 15. **Plano contínuo de operações, monitoramento, updates e evolução** pós-MVP.

---

## Objetivos de Manutenção

| Objetivo | Métrica | Target |
|----------|---------|--------|
| **Disponibilidade** | Uptime | > 99.9% (excluindo deploy) |
| **Confiabilidade** | Error rate (5xx) | < 0.1% |
| **Performance** | Dashboard p95 | < 500ms |
| **Performance** | Webhook → UI p95 | < 2s |
| **Segurança** | Vulnerabilidades high/critical | 0 |
| **Dados** | Backup RPO / RTO | < 1h / < 4h |
| **Evolução** | Deploy frequency | Semanal (patch) / Quinzenal (minor) |

---

## Rotinas Operacionais

### Diário (Automatizado + 5 min manual)
| Atividade | Responsável | Ferramenta | Critério Alerta |
|-----------|-------------|------------|-----------------|
| Verificar uptime | Automatizado | Upptime/UptimeRobot | < 99.9% → alerta |
| Error rate 5xx | Automatizado | Vercel Logs / Sentry | > 0.1% → alerta |
| Webhook latency p95 | Automatizado | Logs custom / Vercel | > 2s → alerta |
| SSE conexões ativas | Automatizado | Métrica custom | Queda > 20% → alerta |
| Neon: CPU, storage, conexões | Automatizado | Neon Dashboard | > 80% → alerta |
| Resend: delivery rate, bounces | Automatizado | Resend Dashboard | Bounce > 5% → alerta |
| Revisar logs erro (manual) | Você | Vercel Logs | Novos padrões de erro |
| Verificar dead letters | Você | Dashboard / Tabela | > 0 não resolvidas |

### Semanal (30 min)
| Atividade | Responsável | Descrição |
|-----------|-------------|-----------|
| **Métricas de negócio** | Você | Usuários ativos (DAU/WAU/MAU), pedidos/dia, conversão trial→paid, churn, NPS |
| **Revisão de alertas** | Você | Alertas disparados na semana: falsos positivos? ajustar thresholds |
| **Dependências** | Você | `npm outdated` + Dependabot PRs → merge patch/minor |
| **Segurança** | Você | `npm audit` + CodeQL + TruffleHog → 0 high/critical |
| **Backup verify** | Você | Neon PITR: timestamp último restore point < 1h |
| **Feedback usuários** | Você | Ler emails/suporte: bugs, requests, dor |
| **Runbook drill** | Você | Simular 1 incidente (tabletop): rollback, webhook down, DB down |

### Mensal (2 horas)
| Atividade | Responsável | Descrição |
|-----------|-------------|-----------|
| **Rotacionar segredos** | Você | WhatsApp token (60d), NextAuth secret (90d), Resend key (se necessário) |
| **Teste rollback** | Você | `vercel rollback` + smoke test 10 min → documentar tempo |
| **Teste restore DB** | Você | Neon PITR: restore point-in-time para branch teste → validar dados |
| **Revisão runbook** | Você | Procedimentos ainda válidos? Atualizar se arquitetura mudou |
| **Análise custos** | Você | Vercel/Neon/Resend: dentro do free tier? Projeção próximo mês |
| **Limpeza dead letters** | Você | Resolver/arquivar > 30 dias |
| **Análise performance** | Você | Lighthouse CI trends, bundle size, query latency trends |
| **Planejamento evolução** | Você | Priorizar backlog: bugs, features "Should", tech debt |

### Trimestral (Meio dia)
| Atividade | Responsável | Descrição |
|-----------|-------------|-----------|
| **Pen test leve** | Você/Externo | OWASP ZAP scan + manual critical paths |
| **LGPD audit** | Você | Consentimentos ativos, retenção respeitada, DPO info atualizada |
| **Revisão arquitetura** | Você | ADRs ainda válidos? Tech debt acumulado? Próximos ADRs? |
| **Capacity planning** | Você | Crescimento usuários/pedidos → quando upgrade Vercel/Neon? |
| **Disaster recovery test** | Você | Restore PITR completo → validar RTO/RPO reais |
| **Revisão contratos** | Você | Termos, privacidade, contratos terceiros (Meta, Resend, Neon, Vercel) |

---

## Monitoramento e Alertas

### Dashboards (Vercel + Neon + Custom)

| Dashboard | Métricas-Chave | Frequência Atualização |
|-----------|----------------|------------------------|
| **Vercel Analytics** | Web Vitals (LCP, FID, CLS), Function Duration, Invocations, Errors | Real-time |
| **Neon Dashboard** | CPU, Memory, Storage, Connections, Query Latency, PITR status | Real-time |
| **Custom (Vercel Logs / Sentry)** | Webhook latency (p50/p95/p99), SSE latency, Error rate by type, Dead letter queue | 1 min |
| **Business (Custom / Metabase)** | DAU, Pedidos/dia, Faturamento, Conversão, Churn, NPS | Diário |

### Alertas Configurados

| Alerta | Condição | Severidade | Canal | Ação Imediata |
|--------|----------|------------|-------|---------------|
| **Webhook Down** | Error rate > 10% / 5min | SEV-1 | Telegram + Email | Verificar Vercel logs + Meta status |
| **DB Down** | Health check falha 2x consecutivas | SEV-1 | Telegram + Email | Verificar Neon status + conexões |
| **Auth Down** | Login failure rate > 50% / 5min | SEV-1 | Telegram | Verificar NextAuth secret + Resend |
| **SSE Degradado** | Disconnect rate > 20% / 10min | SEV-2 | Telegram | Verificar Vercel function duration |
| **Latência Alta** | Dashboard p95 > 2s / 15min | SEV-2 | Telegram | Verificar Neon query latency |
| **Error Rate** | 5xx > 1% / 10min | SEV-2 | Telegram | Verificar logs + rollback se regressão |
| **Dead Letter** | > 5 não resolvidas | SEV-3 | Email | Investigar + resolver |
| **Storage Neon** | > 80% usado | SEV-3 | Email | Arquivar / Upgrade |
| **Certificado SSL** | Expira < 30 dias | SEV-3 | Email | Verificar renovação auto |

---

## Gestão de Mudanças (Change Management)

### Tipos de Mudança

| Tipo | Definição | Aprovação | Deploy |
|------|-----------|-----------|--------|
| **Emergency** | Hotfix crítico production (SEV-1) | Você (auto) | Imediato (push main) |
| **Standard** | Bug fix, feature minor, config change | Você + CI verde | Próximo deploy agendado |
| **Major** | Breaking change, nova arquitetura, migração DB | Você + revisão ADR | Agendado (janela manutenção) |

### Janela de Manutenção
- **Preferencial:** Terça/Quarta 10h-12h (baixo tráfego)
- **Emergency:** Qualquer hora
- **Comunicação:** Status page + email usuários se downtime > 5 min

### Processo Standard Change
1. Branch `feat/...` ou `fix/...` a partir de `main`
2. PR com descrição, testes, changelog
3. CI 100% verde
4. Auto-review (solo) ou reviewer designado
5. Merge → Deploy automático Vercel
6. Smoke test pós-deploy (10 min)
6. Monitoramento 2h
7. Fechar issue / atualizar changelog

---

## Gestão de Configuração (Feature Flags)

> **v1: Simples - Tabela `ConfiguracaoLoja.templatesJson` + env vars**
> **v1.1+: Vercel Edge Config / LaunchDarkly / Unleash**

| Flag | Tipo | Default | Uso |
|------|------|---------|-----|
| `ENABLE_SSE` | Boolean | `true` | Desligar SSE se instável |
| `ENABLE_PWA` | Boolean | `true` | Desligar SW se bug |
| `ENABLE_TEMPLATES` | Boolean | `true` | Desligar templates WhatsApp |
| `ENABLE_REPORTS` | Boolean | `true` | Desligar relatórios pesados |
| `MAX_PEDIDOS_PAGE` | Number | `20` | Ajustar paginação |
| `WEBHOOK_QUEUE_ENABLED` | Boolean | `true` | Desligar queue (processar sync) |

---

## Estratégia de Backup e Recuperação

### Neon (PostgreSQL)
| Aspecto | Configuração | Validação |
|---------|--------------|-----------|
| **PITR** | Ativado (7d free / 30d paid) | Mensal: restore point-in-time |
| **Branch `main`** | Production | Protegida (não deletável) |
| **Branch `preview-*`** | PRs (ephemeral) | Auto-delete 7d após merge |
| **RPO** | < 1h (continuous) | Verificar lag replicação |
| **RTO** | < 4h (restore + deploy) | Testar trimestral |

### Código e Config
| Item | Backup | Recuperação |
|------|--------|-------------|
| **Repo Git** | GitHub (remoto) + local clones | `git clone` |
| **Vercel Config** | `vercel.json` no repo + Vercel dashboard | Reimport project |
| **Variáveis Ambiente** | Documentadas no `ENV.md` (sem valores) + Vercel | Recreate no Vercel |
| **Secrets** | Gerenciador de senhas (1Password/Bitwarden) | Rotacionar + update Vercel |

---

## Gestão de Dívida Técnica

### Registro de Dívida (Tech Debt Log)
```markdown
# Tech Debt Log

| ID | Descrição | Impacto | Esforço | Prioridade | Sprint Alvo | Status |
|----|-----------|---------|---------|------------|-------------|--------|
| TD-001 | Parser WhatsApp: melhorar extração com LLM | Médio | 3 dias | Should | v1.1 | ☐ |
| TD-002 | Testes E2E flaky: `pedido-flow` intermitente | Alto | 1 dia | Must | Atual | 🔄 |
| TD-003 | Bundle size: `lucide-react` grande, trocar por `lucide-static` | Baixo | 2h | Could | v1.1 | ☐ |
| TD-004 | SSE: migrar para Pusher se > 100 conexões | Médio | 2 dias | Should | v1.2 | ☐ |
| TD-005 | LGPD: DPIA completo se necessário | Alto | 5 dias | Must | v1.1 | ☐ |
```

### Alocação de Capacidade
- **70%** Features / Bug fixes (roadmap)
- **20%** Tech debt / Refatoração / Testes
- **10%** Exploração / Spikes / Aprendizado

---

## Roadmap Pós-MVP (Visão 6 Meses)

| Mês | Foco | Entregas Principais |
|-----|------|---------------------|
| **Mês 1** | Estabilização | Bug fixes, performance, feedback usuários piloto, docs |
| **Mês 2** | Qualidade | Test coverage 90%, a11y audit, load test otimizado, tech debt |
| **Mês 3** | Features "Should" | Multi-user (funcionários), relatórios semanais automáticos, export PDF |
| **Mês 4** | Growth | Landing page, trial→paid flow, billing (Stripe), métricas avançadas |
| **Mês 5** | Escala | Multi-loja (RLS), Pusher SSE, cache Redis, otimização queries |
| **Mês 6** | Ecossistema | API pública (v1), webhooks saída, integração iFood (parceria), mobile app (PWA → TWA) |

---

## Orçamento Operacional (Estimado Mensal)

| Serviço | Free Tier | Paid Tier (se necessário) | Gatilho Upgrade |
|---------|-----------|---------------------------|-----------------|
| **Vercel** | Hobby: $0 | Pro: $20/mês | Function timeout > 10s, > 100 SSE, bandwidth > 100GB |
| **Neon** | Free: 512MB, 190h | Scale: $19/mês + usage | Storage > 400MB, compute > 150h |
| **Resend** | Free: 3k/dia | Pro: $20/mês (50k/dia) | Emails > 2.5k/dia |
| **Vercel Blob** | Free: 1GB | Paid: $0.15/GB | Storage > 800MB |
| **Monitoramento** | Upptime (grátis) | Datadog/Sentry: $0-50 | Necessidade APM real |

**Total Estimado MVP (Free):** $0/mês
**Total Estimado Growth (Paid):** ~$60-100/mês

---

## Checklist de Continuidade (Business Continuity)

- [ ] **Sucessão:** Documentação suficiente para outro dev assumir? (README, ADRs, Runbook)
- [ ] **Acesso:** Credenciais Vercel/Neon/Resend/Meta/GitHub no gerenciador de senhas compartilhado?
- [ ] **Domínio:** Renovação automática? DNS gerenciado onde?
- [ ] **Código:** Repo público/privado? Backup espelho (GitLab/Bitbucket)?
- [ ] **Dados:** Export automatizado semanal (pg_dump) para storage externo (S3/GCS)?
- [ ] **Comunicação:** Lista de contatos usuários-chave para aviso crítico?
- [ ] **Legal:** Contratos assinados? LGPD compliance documentado?

---

## Próxima Etapa
[[02 - Modelos/Publicação e Manutenção/Plano de Encerramento]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/08 - Publicação e Manutenção/ETAPA 15 - Monitorar e Manter]]
- [[01 - Manual de Desenvolvimento/08 - Publicação e Manutenção/ETAPA 16 - Encerrar ou Transferir o Projeto]]
- [[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]]
- [[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]]
- [[02 - Modelos/Publicação e Manutenção/Plano de Encerramento]]
- [[04 - Referência/Glossário/SLA]]
- [[04 - Referência/Glossário/RTO/RPO]]
- [[04 - Referência/Glossário/Business Continuity]]
- [[04 - Referência/Glossário/Feature Flags]]