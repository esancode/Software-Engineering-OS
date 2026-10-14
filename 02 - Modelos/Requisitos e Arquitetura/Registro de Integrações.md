# Registro de Integrações

> **Copie para:** `05 - Projetos/SeuProjeto/03 - Arquitetura/Registro de Integrações.md`
> Baseado na ETAPA 05. **Todas as integrações externas** (APIs, webhooks, serviços) documentadas.

---

## Visão Geral das Integrações

| # | Integração | Tipo | Criticidade | Status | Owner |
|---|------------|------|-------------|--------|-------|
| 1 | WhatsApp Cloud API (Meta) | REST + Webhook | **Crítica** | Planejada | [Você] |
| 2 | Resend (Email Transacional) | REST | **Alta** | Planejada | [Você] |
| 3 | Neon (PostgreSQL Serverless) | Driver (Prisma) | **Crítica** | Planejada | [Você] |
| 4 | Vercel (Deploy + Edge + Cron) | Plataforma | **Crítica** | Planejada | [Você] |
| 5 | Vercel Blob (Storage mídia) | REST | Média | Planejada | [Você] |
| 6 | Inngest / Vercel Queue (Async) | SDK + HTTP | **Alta** | Planejada | [Você] |

---

## 1. WhatsApp Cloud API (Meta)

### Configuração Básica
| Item | Valor |
|------|-------|
| **Base URL** | `https://graph.facebook.com/v20.0/` |
| **Auth** | Bearer Token (WhatsApp Business Account Token) |
| **Phone Number ID** | `PHONE_NUMBER_ID` (env) |
| **Business Account ID** | `WABA_ID` (env) |
| **App Secret** | `WHATSAPP_APP_SECRET` (env - para HMAC) |
| **Webhook Verify Token** | `WEBHOOK_VERIFY_TOKEN` (env - desafio GET) |

### Endpoints Usados

| Operação | Endpoint | Método | Frequência |
|----------|----------|--------|------------|
| Enviar template | `/{phone_number_id}/messages` | POST | Por ação usuário |
| Enviar texto livre | `/{phone_number_id}/messages` | POST | Por ação usuário (janela 24h) |
| Enviar mídia | `/{phone_number_id}/media` + messages | POST | Could v1.1 |
| Baixar mídia | `/{media_id}` | GET | Quando recebe imagem |
| Marcar como lida | `/{phone_number_id}/messages` | POST | Opcional |
| Registrar webhook | `/{app_id}/webhooks` | POST | Setup único |
| Health check | `/{waba_id}` | GET | Monitoramento |

### Webhook (Recebimento)

| Item | Detalhe |
|------|---------|
| **URL** | `https://seu-app.vercel.app/api/webhook/whatsapp` |
| **Eventos assinados** | `messages` (entrada), `message_template_status_update` (opcional) |
| **Validação** | HMAC SHA256 header `X-Hub-Signature-256` com `APP_SECRET` |
| **Desafio GET** | `hub.mode=subscribe&hub.challenge=XYZ&hub.verify_token=TOKEN` |
| **Timeout resposta** | **< 5s** (WhatsApp reenvia após 20s sem ACK) |
| **Retentativa WhatsApp** | Exponencial: 5s, 10s, 20s, 40s, 80s... (máx 24h) |

### Payload Webhook (Exemplo)
```json
{
  "object": "whatsapp_business_account",
  "entry": [{
    "id": "WABA_ID",
    "changes": [{
      "value": {
        "messaging_product": "whatsapp",
        "metadata": { "display_phone_number": "15551234567", "phone_number_id": "PHONE_ID" },
        "contacts": [{ "profile": { "name": "João" }, "wa_id": "5511999999999" }],
        "messages": [{
          "from": "5511999999999",
          "id": "wamid.1234567890",
          "timestamp": "1700000000",
          "type": "text",
          "text": { "body": "Quero 2 x-burger" }
        }]
      },
      "field": "messages"
    }]
  }]
}
```

### Templates Necessários (MVP)

| Nome | Categoria | Idioma | Variáveis | Status Meta |
|------|-----------|--------|-----------|-------------|
| `status_atualizado` | UTILITY | pt_BR | `{{1}}`=pedido_id, `{{2}}`=status, `{{3}}`=msg_custom | Aprovar |
| `pedido_confirmado` | UTILITY | pt_BR | `{{1}}`=pedido_id, `{{2}}`=tempo_estimado | Aprovar |
| `fora_horario` | UTILITY | pt_BR | `{{1}}`=proxima_abertura | Aprovar |

> **Nota:** Templates devem ser aprovados pelo Meta antes de usar. Processo: 24-48h.

### Rate Limits (Meta)
| Limite | Valor | Ação se Atingir |
|--------|-------|-----------------|
| Mensagens/segundo | 100 (padrão) | Queue local + backoff |
| Mensagens/dia (novos contatos) | 1000 (Tier 1) | Solicitar upgrade Tier |
| Tamanho payload | 256 KB | Validar antes enviar |

### Tratamento de Erros Comuns
| Código | Significado | Ação |
|--------|-------------|------|
| 131009 | Fora da janela 24h, template necessário | Usar template aprovado |
| 131042 | Template rejeitado/não aprovado | Verificar status no Meta Business |
| 131047 | Rate limit | Backoff exponencial, retry |
| 131051 | Número não opt-in | Não enviar (apenas template) |
| 100 | Token inválido/expirado | Renovar token (long-lived 60 dias) |

### Segurança
- [ ] HMAC validation em 100% webhooks (RN-011)
- [ ] Token armazenado apenas em env (Vercel/Neon)
- [ ] Webhook URL HTTPS apenas
- [ ] Log de auditoria: message_id, timestamp, direção, status

---

## 2. Resend (Email Transacional)

### Configuração
| Item | Valor |
|------|-------|
| **Base URL** | `https://api.resend.com` |
| **Auth** | Bearer Token (`RESEND_API_KEY`) |
| **Domínio verificado** | `seu-dominio.com` (ou `onboarding@resend.dev` para teste) |
| **From padrão** | `PedidosSimples <noreply@seu-dominio.com>` |

### Endpoints Usados
| Operação | Endpoint | Método | Uso |
|----------|----------|--------|-----|
| Enviar email | `/emails` | POST | Magic Link, notificações |
| Buscar email | `/emails/{id}` | GET | Debug/status |
| Cancelar agendado | `/emails/{id}` | DELETE | Raro |

### Payload Magic Link
```json
{
  "from": "PedidosSimples <noreply@seu-dominio.com>",
  "to": ["joao@loja.com"],
  "subject": "Seu link de acesso ao PedidosSimples",
  "html": "<p>Olá,<br/>Clique no link abaixo para acessar:<br/><a href=\"https://app.seudominio.com/auth/callback?token=abc123\">Acessar PedidosSimples</a><br/>Link expira em 15 minutos.</p>",
  "tags": [{ "name": "type", "value": "magic_link" }]
}
```

### Rate Limits (Resend Free)
| Limite | Valor | Ação |
|--------|-------|------|
| Emails/dia | 3.000 | Upgrade Pro (50k/dia) ou otimizar |
| Emails/segundo | 10 | Queue se burst |
| Tamanho email | 10 MB | Não aplicável (texto simples) |

### Templates de Email (Sugeridos)
| Template | Variáveis | Quando |
|----------|-----------|--------|
| `magic_link` | `token`, `expires_in`, `app_url` | Login Magic Link |
| `welcome` | `nome`, `loja_nome`, `app_url` | Primeiro login |
| `pedido_alert` | `pedido_id`, `cliente`, `total` | Opcional: alerta novo pedido por email |

---

## 3. Neon (PostgreSQL Serverless)

### Configuração
| Item | Valor |
|------|-------|
| **Connection String** | `postgresql://user:pass@ep-xxx.sa-east-1.aws.neon.tech/db?sslmode=require&pgbouncer=true` |
| **Região** | `sa-east-1` (São Paulo) - LGPD |
| **Pooling** | PgBouncer built-in (`pgbouncer=true`) |
| **Branching** | `main` (prod), `preview-*` (PRs) |
| **Point-in-Time Recovery** | Ativado (7 dias free, 30 dias paid) |

### Conexão Prisma
```env
# .env
DATABASE_URL="postgresql://user:pass@ep-xxx.sa-east-1.aws.neon.tech/db?sslmode=require&pgbouncer=true"
# Para Prisma Migrate (conexão direta, sem pooler)
DIRECT_URL="postgresql://user:pass@ep-xxx.sa-east-1.aws.neon.tech/db?sslmode=require"
```

### Limites Free Tier
| Recurso | Limite | Ação se Atingir |
|---------|--------|-----------------|
| Storage | 512 MB | Arquivar pedidos antigos, upgrade Scale |
| Compute hours | 190 hrs/mês | Otimizar queries, upgrade |
| Branches | 10 | Limpar branches antigas |
| PITR | 7 dias | Upgrade para 30 dias |

### Otimizações Neon
- [ ] `pgbouncer=true` na connection string
- [ ] `prisma.$connect()` no middleware (warm connection)
- [ ] Vercel Cron job a cada 5 min para keep-warm (evita cold start)
- [ ] Índices criados nas migrations (ver Modelo de Dados)
- [ ] `prepared statements` habilitado (default Prisma)

---

## 4. Vercel (Deploy + Edge + Cron)

### Configuração
| Item | Valor |
|------|-------|
| **Framework** | Next.js (detecção automática) |
| **Build Command** | `prisma generate && next build` |
| **Output Directory** | `.next` (default) |
| **Install Command** | `npm ci` |
| **Node Version** | 20.x (LTS) |
| **Regiões** | `gru1` (São Paulo) - latência Neon |

### Vercel Functions (Serverless)
| Rota | Max Duration | Memory | Notas |
|------|--------------|--------|-------|
| `/api/webhook/whatsapp` | 10s (Hobby) / 60s (Pro) | 1024 MB | ACK < 5s, processamento assíncrono |
| `/api/events` (SSE) | 10s/60s | 512 MB | Conexão longa, heartbeat 30s |
| `/api/auth/*` | 10s | 512 MB | NextAuth handlers |
| `/api/pedidos/*` | 10s | 512 MB | CRUD pedidos |
| `POST /api/pedidos/[id]/status` | 10s | 512 MB | Transação status + histórico |

### Vercel Cron Jobs
| Job | Schedule | Função | Descrição |
|-----|----------|--------|-----------|
| `keep-warm` | `*/5 * * * *` | `GET /api/health` | Evita cold start Neon |
| `cleanup-dead-letter` | `0 3 * * *` | `POST /api/admin/cleanup` | Marcar dead letters > 7d resolvidas |
| `daily-report` | `0 6 * * *` | `POST /api/reports/daily` | Gerar/enviar relatório diário (opcional) |

### Vercel Blob (Storage Mídia)
| Item | Valor |
|------|-------|
| **SDK** | `@vercel/blob` |
| **Token** | `BLOB_READ_WRITE_TOKEN` (env) |
| **Limite Free** | 1 GB storage, 10 GB bandwidth/mês |
| **Uso** | Upload direto client-side (signed URL) para imagens WhatsApp |

### Variáveis de Ambiente (Vercel Dashboard)
```env
# Obrigatórias
DATABASE_URL
DIRECT_URL
NEXTAUTH_SECRET
NEXTAUTH_URL
WHATSAPP_APP_SECRET
WHATSAPP_PHONE_NUMBER_ID
WHATSAPP_TOKEN
RESEND_API_KEY
BLOB_READ_WRITE_TOKEN
WEBHOOK_VERIFY_TOKEN

# Opcionais / Feature Flags
NEXT_PUBLIC_APP_URL
NEXT_PUBLIC_ENABLE_SSE
NEXT_PUBLIC_ENABLE_PWA
```

---

## 5. Inngest / Vercel Queue (Processamento Assíncrono)

> **Decisão:** Inngest (mais maduro, dashboard, retry automático) ou Vercel Queue (nativo, mais simples). Avaliar na ETAPA 05 spike.

### Inngest (Se Escolhido)
| Item | Valor |
|------|-------|
| **SDK** | `inngest` + `inngest/next` |
| **Event Key** | `INNGEST_EVENT_KEY` (env) |
| **Signing Key** | `INNGEST_SIGNING_KEY` (env) |
| **Funções** | `process-whatsapp-webhook`, `send-whatsapp-template`, `generate-report` |

### Vercel Queue (Se Escolhido)
| Item | Valor |
|------|-------|
| **SDK** | `@vercel/queue` |
| **Queue Name** | `whatsapp-webhook`, `whatsapp-send`, `reports` |
| **Concurrency** | 10 (configurável) |
| **Retention** | 7 dias |

### Padrão de Uso (Ambos)
```typescript
// Webhook handler (síncrono, < 5s)
export async function POST(req: Request) {
  const payload = await req.json();
  await validateHmac(req);
  // Enfileirar processamento pesado
  await queue.enqueue('process-whatsapp', { payload });
  return Response.json({ ok: true }); // ACK imediato
}

// Worker (assíncrono, sem timeout rigoroso)
export const processWhatsapp = inngest.createFunction(
  { id: 'process-whatsapp', retries: 3 },
  { event: 'whatsapp/webhook' },
  async ({ event, step }) => {
    const parsed = await step.run('parse', () => parseMessage(event.data.payload));
    const pedido = await step.run('create-pedido', () => createPedido(parsed));
    await step.run('broadcast-sse', () => broadcastSSE(pedido.lojaId, 'novo_pedido', pedido));
  }
);
```

---

## 6. Monitoramento e Observabilidade (Integrações Implícitas)

| Ferramenta | O que Monitora | Integração |
|------------|----------------|------------|
| **Vercel Analytics** | Web Vitals, page views, function duration | Automático (script no layout) |
| **Vercel Logs** | Function logs, errors, invocations | Automático |
| **Neon Dashboard** | CPU, storage, connections, query latency | Automático |
| **Inngest Dashboard** | Function runs, retries, duration, errors | Se Inngest |
| **Resend Dashboard** | Email delivery, opens, clicks, bounces | Automático |
| **Sentry (Opcional)** | Error tracking, performance, release health | `@sentry/nextjs` se necessário |
| **Upptime / UptimeRobot** | Uptime endpoint `/api/health` | Externo, gratuito |

---

## Checklist de Integrações (Pré-Desenvolvimento)

- [ ] Contas criadas em todos serviços (WhatsApp Business, Resend, Neon, Vercel, Inngest)
- [ ] Variáveis de ambiente documentadas e seguras
- [ ] Webhook WhatsApp registrado e testado (ngrok local → Vercel preview)
- [ ] Templates WhatsApp submetidos e aprovados no Meta
- [ ] Domínio verificado no Resend (ou usar onboarding@resend.dev)
- [ ] Neon branch `main` criada, connection string testada
- [ ] Vercel project conectado ao GitHub, deploy preview funcionando
- [ ] Health check endpoint `/api/health` implementado e retornando 200
- [ ] Rate limits entendidos e estratégia de backoff definida
- [ ] Dead letter queue desenhada para cada integração crítica
- [ ] Runbook de incidentes para cada integração (o que fazer se cai)

---

## Próxima Etapa
[[02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/03 - Arquitetura e Planejamento/ETAPA 05 - Planejar a Arquitetura e a Solução Técnica]]
- [[02 - Modelos/Requisitos e Arquitetura/Visão Técnica]]
- [[02 - Modelos/Requisitos e Arquitetura/Modelo de Dados]]
- [[02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth]]
- [[05 - Projetos/Exemplo-PedidosSimples/03 - Arquitetura/ADR-001-stack-tecnologica]]
- [[04 - Referência/Glossário/Webhook]]
- [[04 - Referência/Glossário/HMAC]]
- [[04 - Referência/Glossário/Idempotência]]
- [[04 - Referência/Glossário/Serverless]]