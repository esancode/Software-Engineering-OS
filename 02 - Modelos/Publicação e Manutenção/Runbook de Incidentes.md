# Runbook de Incidentes

> **Copie para:** `05 - Projetos/SeuProjeto/08 - Publicação e Manutenção/Runbook de Incidentes.md`
> Baseado na ETAPA 15. **Procedimentos de resposta a incidentes** para cada componente crítico.

---

## Princípios de Resposta a Incidentes

1. **Detecte rápido** → Alertas automáticos + monitoramento
2. **Diagnostique rápido** → Logs estruturados + dashboards
3. **Mitigue rápido** → Rollback / Feature flag / Workaround
4. **Comunique** → Status page / Canal usuários / Stakeholders
5. **Aprenda** → Post-mortem blameless + action items

---

## Classificação de Severidade

| Nível | Definição | SLA Resposta | SLA Resolução | Exemplos |
|-------|-----------|--------------|---------------|----------|
| **SEV-1** (Crítico) | Serviço indisponível para maioria dos usuários; perda de dados | 15 min | 1 hora | Webhook down, DB down, Auth down, Deploy quebrado |
| **SEV-2** (Alto) | Funcionalidade core degradada; alguns usuários afetados | 30 min | 4 horas | SSE instável, lentidão > 5s, templates WhatsApp falhando |
| **SEV-3** (Médio) | Bug não-crítico; workaround existe | 2 horas | 24 horas | Relatório errado, config não salva, PWA install falha |
| **SEV-4** (Baixo) | Melhoria, cosmetic, documentação | Next sprint | Next release | Typo, cor errada, doc desatualizada |

---

## Contatos de Escalação

| Função | Nome | Contato | Disponibilidade |
|--------|------|---------|-----------------|
| **On-call Primário** | [Você] | Telegram/Telefone | 24/7 (solo) |
| **Backup** | [Colega/Mentor] | Telegram | Horário comercial |
| **Infra (Vercel)** | Support | Vercel Dashboard | Business hours |
| **DB (Neon)** | Support | Neon Dashboard | Business hours |
| **WhatsApp (Meta)** | Support | Meta Business Help | Business hours |
| **Email (Resend)** | Support | Resend Dashboard | Business hours |

---

## Incidentes por Componente

### 🔴 INC-001: Webhook WhatsApp Indisponível / Falhando

**Sintomas:**
- Alert: Webhook error rate > 10% em 5 min
- Logs: HTTP 5xx / timeout em `/api/webhook/whatsapp`
- Clientes: "Meu pedido não apareceu"

**Diagnóstico (ordem):**
1. Vercel Functions: `vercel logs --follow` → erros 5xx / timeout
2. Neon: Conexões ativas, CPU, queries lentas
3. Meta Status: `developers.facebook.com/status` → incidents?
4. WhatsApp Token: Expirado? (60 dias) → `curl -H "Authorization: Bearer $TOKEN" https://graph.facebook.com/v20.0/me`
5. HMAC: Secret correto no Vercel env?

**Mitigação Imediata:**
```bash
# 1. Rollback deploy se regressão recente
vercel rollback

# 2. Se token expirado → renovar no Meta → update Vercel env → redeploy
# 3. Se Meta com incidente → aguardar + comunicar usuários
# 4. Workaround: Processar dead letters manualmente + email lojista
```

**Comunicação:**
- Status page: "Investigando atraso no recebimento de pedidos via WhatsApp"
- Email/WhatsApp lojistas ativos: "Estamos com instabilidade no WhatsApp. Pedidos podem atrasar. Verifique o app manualmente."

**Pós-Incidente:**
- Root cause: Token? Meta? Código? Infra?
- Action item: Monitoramento proativo token expiry (alert 7 dias antes)

---

### 🔴 INC-002: Banco de Dados (Neon) Indisponível

**Sintomas:**
- Alert: Health check `/api/health` falha (DB connection error)
- Logs: `P1001: Can't reach database server` / `P1008: Operations timed out`
- Todas operações falham

**Diagnóstico:**
1. Neon Dashboard: Status do projeto, CPU, storage, conexões
2. Vercel: Function logs → connection errors
3. `pg_isready` via Neon console

**Mitigação:**
```bash
# 1. Se conexões esgotadas (pooling):
#    - Verificar leak: queries sem await, transações longas
#    - Reiniciar conexões: Vercel redeploy (não rebuild) → `vercel rollback` + push vazio

# 2. Se storage cheio (> 90%):
#    - Arquivar pedidos antigos (script)
#    - Upgrade Neon plan

# 3. Se Neon regional outage:
#    - Aguardar restore (PITR automático)
#    - Comunicar usuários: "Instabilidade temporária no banco"
```

**Comunicação:**
- "Estamos com instabilidade no sistema. Pedidos podem não aparecer. Trabalhando para restaurar."

---

### 🔴 INC-003: Autenticação (NextAuth) Falhando

**Sintomas:**
- Usuários não conseguem login (Magic Link / Credentials)
- Sessões expiram inesperadamente
- Loop de redirect `/login`

**Diagnóstico:**
1. `NEXTAUTH_SECRET` presente no Vercel env?
2. `NEXTAUTH_URL` correto (production URL)?
3. Resend: API key válida? Domínio verificado? Rate limit?
4. Cookies: `Secure; HttpOnly; SameSite=Lax` — domínio correto?
5. Refresh token: Tabela `RefreshToken` não corrompida?

**Mitigação:**
```bash
# 1. Se secret mudou → todos usuários deslogados (expected)
#    - Comunicar: "Por segurança, renovamos sessões. Faça login novamente."

# 2. Se Resend down → Magic Link falha
#    - Fallback: Credentials (se configurado)
#    - Ou: "Sistema de email instável. Tente login com senha."

# 3. Se cookies blocked (Safari ITP, Brave):
#    - Verificar SameSite=None + Secure se cross-site
#    - Documentar workaround usuários
```

---

### 🟠 INC-004: SSE (Tempo Real) Instável / Desconectando

**Sintomas:**
- Alert: SSE disconnect rate > 20% em 10 min
- Usuários: "Pedidos não aparecem em tempo real"
- Header: Indicador "LIVE" piscando/vermelho

**Diagnóstico:**
1. Vercel Function duration: SSE endpoint > 10s (timeout Hobby)?
2. Conexões ativas > limite? (Vercel Hobby ~100 concurrent)
3. Heartbeat: Cliente envia `last-event-id` na reconexão?
4. Neon: Pub/Sub (LISTEN/NOTIFY) funcionando?

**Mitigação:**
```bash
# 1. Se timeout Vercel (10s Hobby):
#    - Upgrade Vercel Pro (60s) OU
#    - Migrar SSE para Pusher/Ably (external)

# 2. Se muitas conexões:
#    - Cleanup: fechar conexão se aba hidden > 5min
#    - Heartbeat 30s (já implementado)

# 3. Workaround: Polling fallback (setInterval 10s) se SSE down
```

---

### 🟠 INC-005: Templates WhatsApp Falhando (Erro 131009/131042)

**Sintomas:**
- Logs: `error.code: 131009` (fora janela 24h) ou `131042` (template rejeitado)
- Lojista: "Não consigo avisar cliente"

**Diagnóstico:**
1. Janela 24h: Última mensagem cliente > 24h?
2. Template status no Meta: `APPROVED` / `REJECTED` / `PENDING`?
3. Variáveis do template corretas? (count + nomes)

**Mitigação:**
```bash
# 1. Se fora janela 24h → só template permitido (já implementado)
# 2. Se template rejected → criar novo no Meta, aguardar aprovação
# 3. Workaround: "Copie esta mensagem e envie manualmente pelo WhatsApp Business:"
#    "Seu pedido #1234 está PRONTO! Pode vir buscar 📦"
```

---

### 🟡 INC-006: Deploy Falhou / Build Error

**Sintomas:**
- Vercel build: TypeScript error / ESLint error / Test failure / Memory limit
- Deploy não completa

**Diagnóstico:**
1. Vercel Build Logs: erro exato
2. Local: `npm run build` reproduz?

**Mitigação:**
```bash
# 1. Se TypeScript/ESLint: corrigir código → novo PR
# 2. Se test flaky: `test.retry(2)` temporário → investigar depois
# 3. Se memory limit: otimizar bundle / dividir build
# 4. Se dependency install fail: `npm ci` local, check lockfile
```

---

### 🟡 INC-007: Performance Degradada (Latência Alta)

**Sintomas:**
- Alert: p95 dashboard > 2s / webhook > 5s
- Usuários: "App lento"

**Diagnóstico:**
1. Vercel Function Duration: p95, max
2. Neon: Query latency, CPU, conexões
3. Bundle size: `next-bundle-analyzer` — novo chunk grande?
4. Queries N+1: Prisma logs / `explain analyze`

**Mitigação:**
```bash
# 1. Se query lenta: adicionar índice / otimizar select / cache
# 2. Se cold start: keep-warm cron (já implementado)
# 3. Se bundle: code splitting / dynamic import
# 4. Rollback se regressão recente
```

---

### 🟡 INC-008: LGPD / Data Breach Suspeito

**Sintomas:**
- Acesso não autorizado a dados (logs, alertas)
- Usuário reporta: "Meus dados vazaram"
- ANPD notificação

**Resposta Imediata:**
1. **Isolar:** Revogar tokens suspeitos, bloquear IPs, rotacionar segredos
2. **Avaliar:** Quais dados? Quantos usuários? Vetor de ataque?
3. **Notificar:** ANPD em 48h (Art. 48 LGPD) + usuários afetados
4. **Documentar:** Timeline, evidências, ações tomadas
5. **Remediar:** Patch vulnerabilidade, melhorar monitoramento

**Contato ANPD:** `https://www.gov.br/anpd/pt-br/canais-de-atendimento`

---

## Procedimentos Padrão

### Rollback Rápido (< 2 min)
```bash
# Opção 1: Vercel CLI (preferido)
vercel rollback

# Opção 2: Git revert + push (se Vercel CLI falhar)
git log --oneline -5
git revert <commit-hash>
git push origin main
```

### Verificação de Saúde Pós-Mitigação
```bash
# 1. Health check
curl https://app.SEU_DOMINIO.com/api/health

# 2. Smoke test crítico
#    - Login
#    - Dashboard carrega
#    - Webhook responde 200
#    - SSE conecta

# 3. Métricas 10 min
#    - Error rate
#    - Latência p95
#    - Usuários ativos
```

### Comunicação Padrão

**Interno (Slack/Telegram canal #incidentes):**
```
🚨 INCIDENTE SEV-[1-4]: [Título]
Componente: [Webhook/DB/Auth/SSE/Deploy]
Início: [HH:MM]
Impacto: [Descrição]
Status: [Investigando / Mitigando / Resolvido]
Próxima atualização: [HH:MM]
```

**Externo (Status page / Email usuários):**
```
Título: [Breve descrição]
Status: [Investigando / Identificado / Corrigido / Resolvido]
Início: [DD/MM HH:MM]
Impacto: [O que usuários veem]
Ação: [O que estamos fazendo]
Próxima atualização: [HH:MM]
```

---

## Post-Mortem (Blameless) - Template

> **Preencher dentro de 48h após resolução de SEV-1/SEV-2**

```markdown
# Post-Mortem: [Título Incidente]

**Data:** YYYY-MM-DD
**Severidade:** SEV-1/2
**Duração:** X horas Y min
**Autor:** [Nome]

## Resumo
[1-2 parágrafos: o que aconteceu, impacto usuários, como resolveu]

## Timeline
| Horário | Evento |
|---------|--------|
| HH:MM | Alert disparado / Usuário reportou |
| HH:MM | Iniciou investigação |
| HH:MM | Root cause identificado |
| HH:MM | Mitigação aplicada |
| HH:MM | Serviço restaurado |
| HH:MM | Monitoramento confirmado |

## Root Cause (Análise 5 Porquês)
1. Por que [sintoma]? → [causa imediata]
2. Por que [causa imediata]? → [causa intermédia]
3. Por que [causa intermédia]? → [causa raiz]
4. Por que [causa raiz] não foi prevenida? → [gap processo/ferramenta]
5. Por que [gap] existe? → [raiz sistêmica]

## Impacto
- Usuários afetados: [Nº / %]
- Pedidos perdidos/atrasados: [Nº]
- Receita impactada: [R$ / N/A]
- Confiança: [Baixa/Média/Alta]

## O que Funcionou Bem
- [Ex: Alert detectou em 2 min]
- [Ex: Rollback funcionou em 30s]
- [Ex: Comunicação clara]

## O que Falhou
- [Ex: Não tinha alerta para X]
- [Ex: Runbook desatualizado]
- [Ex: Diagnóstico levou 30 min]

## Action Items (Prevenção)
| Ação | Responsável | Prazo | Status |
|------|-------------|-------|--------|
| [Ex: Adicionar alerta token expiry 7 dias] | [Você] | YYYY-MM-DD | ☐ |
| [Ex: Documentar workaround no runbook] | [Você] | YYYY-MM-DD | ☐ |
| [Ex: Testar rollback mensalmente] | [Você] | Mensal | ☐ |

## Lições Aprendidas
- [Lição 1]
- [Lição 2]
```

---

## Checklist Mensal de Prontidão

- [ ] Testar rollback (`vercel rollback` + smoke test)
- [ ] Verificar alertas: todos disparando? (simular)
- [ ] Revisar runbook: procedimentos ainda válidos?
- [ ] Rotacionar segredos se > 90 dias (WhatsApp token, NextAuth secret)
- [ ] Verificar backups Neon: PITR timestamp recente
- [ ] Testar restore point-in-time (trimestral)
- [ ] Atualizar dependências (patch/minor)
- [ ] Revisar logs de acesso suspeitos
- [ ] Verificar certificados SSL (renovação auto OK?)
- [ ] Validar runbook com "tabletop exercise" (simulação mental)

---

## Próxima Etapa
[[02 - Modelos/Publicação e Manutenção/Plano de Manutenção]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/08 - Publicação e Manutenção/ETAPA 15 - Monitorar e Manter]]
- [[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]]
- [[02 - Modelos/Publicação e Manutenção/Plano de Manutenção]]
- [[02 - Modelos/Segurança e Validação/Checklist de Segurança]]
- [[04 - Referência/Glossário/Incident Response]]
- [[04 - Referência/Glossário/Runbook]]
- [[04 - Referência/Glossário/Post-Mortem]]
- [[04 - Referência/Glossário/RTO/RPO]]