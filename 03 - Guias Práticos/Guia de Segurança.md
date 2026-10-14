# Guia de Segurança

> **Local:** `03 - Guias Práticos/Guia de Segurança.md`
> **Baseado em:** [[02 - Modelos/Segurança e Validação/Checklist de Segurança]], [[02 - Modelos/Segurança e Validação/Plano de Validação]]
> **Objetivo:** Referência prática para implementar segurança em todas as camadas (código, infra, dados, LGPD).

---

## Princípios de Segurança

| Princípio | Aplicação |
|-----------|-----------|
| **Defesa em Profundidade** | Múltiplas camadas: código, rede, dados, monitoramento |
| **Menor Privilégio** | Tokens com escopo mínimo, RLS no DB, roles NextAuth |
| **Falha Segura** | Erro = negação, logs sem dados sensíveis, rollback automático |
| **Privacidade por Design** | LGPD desde o onboarding, minimização de dados, consentimento |
| **Segurança como Código** | Checks automatizados (CI), IaC, policy as code |

---

## Checklist Rápido por Deploy

```bash
# Rode SEMPRE antes de merge/deploy
npm run lint              # ESLint + security plugins
npm run typecheck         # TypeScript strict
npm audit                 # 0 high/critical
npx @trufflehog/trufflehog@latest filesystem . --no-verification  # Secrets
npx semgrep scan --config=auto --error  # SAST
```

### GitHub Actions Security Job
```yaml
# .github/workflows/security.yml
security:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with: { node-version: '20', cache: 'npm' }
    - run: npm ci
    - run: npm audit --audit-level=high
    - uses: trufflesecurity/trufflehog@main
      with: path: ., base: main, head: HEAD
    - uses: returntocorp/semgrep-action@v1
      with: config: auto
    - uses: github/codeql-action/init@v3
      with: languages: typescript
    - uses: github/codeql-action/analyze@v3
```

---

## Segurança no Código (Aplicação)

### 1. Validação de Input (Zod - Obrigatório)
```typescript
// NUNCA confie em input do cliente
// SEMPRE valide em Server Actions / API Routes

import { z } from "zod"

const AtualizarPedidoSchema = z.object({
  pedidoId: z.string().cuid(),
  status: z.enum(["NOVO", "PREPARANDO", "PRONTO", "ENTREGUE", "CANCELADO"]),
  motivoCancelamento: z.string().max(500).optional(),
}).refine(
  (data) => data.status !== "CANCELADO" || data.motivoCancelamento,
  { message: "Motivo obrigatório para cancelamento", path: ["motivoCancelamento"] }
)

// Em Server Action:
export async function atualizarStatus(input: unknown) {
  const parsed = AtualizarPedidoSchema.safeParse(input)
  if (!parsed.success) {
    return { success: false, error: parsed.error.flatten().fieldErrors }
  }
  // ... lógica segura com parsed.data
}
```

### 2. Autenticação e Autorização
```typescript
// src/lib/auth.ts - NextAuth v5 Config
import NextAuth from "next-auth"
import Credentials from "next-auth/providers/credentials"
import { MagicLink } from "next-auth/providers/magic-link"
import { PrismaAdapter } from "@auth/prisma-adapter"
import { prisma } from "@/lib/prisma"
import bcrypt from "bcryptjs"

export const { handlers, signIn, signOut, auth } = NextAuth({
  adapter: PrismaAdapter(prisma),
  providers: [
    MagicLink({
      sendVerificationRequest: async ({ identifier, url, provider }) => {
        // Usar Resend
        await resend.emails.send({
          from: "SEU_APP <noreply@SEU_DOMINIO.com>",
          to: identifier,
          subject: "Seu link de acesso",
          html: `<a href="${url}">Entrar no PedidosSimples</a>`,
        })
      },
    }),
    Credentials({
      name: "credentials",
      credentials: { email: {}, password: {} },
      authorize: async (credentials) => {
        const user = await prisma.usuario.findUnique({ where: { email: credentials.email } })
        if (!user || !user.passwordHash) return null
        const valid = await bcrypt.compare(credentials.password, user.passwordHash)
        if (!valid) return null
        return { id: user.id, email: user.email, lojaId: user.lojaId, role: user.role }
      },
    }),
  ],
  session: { strategy: "jwt", maxAge: 30 * 24 * 60 * 60 }, // 30 dias
  jwt: { maxAge: 30 * 24 * 60 * 60 },
  callbacks: {
    async jwt({ token, user }) {
      if (user) { token.lojaId = user.lojaId; token.role = user.role }
      return token
    },
    async session({ session, token }) {
      session.user.lojaId = token.lojaId
      session.user.role = token.role
      return session
    },
  },
  pages: { signIn: "/login", error: "/login" },
  secret: process.env.NEXTAUTH_SECRET,
})
```

### 3. Row Level Security (RLS) - Prisma + PostgreSQL
```sql
-- migrations/xxxx_rls_policies.sql
ALTER TABLE "Pedido" ENABLE ROW LEVEL SECURITY;
ALTER TABLE "ItemPedido" ENABLE ROW LEVEL SECURITY;
ALTER TABLE "Loja" ENABLE ROW LEVEL SECURITY;

-- Policy: Usuário só vê pedidos da sua loja
CREATE POLICY "pedido_loja_isolation" ON "Pedido"
  USING ("lojaId" = current_setting('app.current_loja_id')::uuid);

CREATE POLICY "item_loja_isolation" ON "ItemPedido"
  USING ("pedidoId" IN (SELECT id FROM "Pedido" WHERE "lojaId" = current_setting('app.current_loja_id')::uuid));
```

```typescript
// src/lib/prisma.ts - Middleware para setar RLS context
import { PrismaClient } from "@prisma/client"

const prisma = new PrismaClient()

prisma.$use(async (params, next) => {
  if (params.model && ["Pedido", "ItemPedido", "Loja"].includes(params.model)) {
    const session = await getServerSession() // NextAuth
    if (session?.user?.lojaId) {
      // Set RLS context via SET LOCAL
      await prisma.$executeRaw`SET LOCAL app.current_loja_id = ${session.user.lojaId}`
    }
  }
  return next(params)
})

export { prisma }
```

### 4. Proteção de Webhook (HMAC + Idempotência)
```typescript
// src/app/api/webhook/whatsapp/route.ts
import { headers } from "next/headers"
import crypto from "crypto"
import { prisma } from "@/lib/prisma"

const WHATSAPP_APP_SECRET = process.env.WHATSAPP_APP_SECRET!

export async function POST(req: Request) {
  // 1. Verificar HMAC
  const body = await req.text()
  const signature = headers().get("x-hub-signature-256")
  
  const expected = "sha256=" + crypto
    .createHmac("sha256", WHATSAPP_APP_SECRET)
    .update(body)
    .digest("hex")
  
  if (!crypto.timingSafeEqual(Buffer.from(signature || ""), Buffer.from(expected))) {
    return new Response("Invalid signature", { status: 401 })
  }

  // 2. Idempotência: verificar message_id
  const payload = JSON.parse(body)
  const messageId = payload.entry?.[0]?.changes?.[0]?.value?.messages?.[0]?.id
  
  if (messageId) {
    const existing = await prisma.webhookEvent.findUnique({ where: { messageId } })
    if (existing) {
      return Response.json({ success: true, duplicate: true })
    }
  }

  // 3. Processar (salvar evento primeiro para idempotência)
  await prisma.webhookEvent.create({
    data: { messageId, payload, status: "RECEBIDO" }
  })

  // 4. Processar assíncrono (não bloquear webhook)
  processarWebhookAsync(payload).catch(console.error)

  return Response.json({ success: true })
}
```

### 5. Sanitização de Output (XSS Prevention)
```typescript
// React: SEMPRE use JSX (auto-escape)
// Perigo: dangerouslySetInnerHTML - EVITE
// Se necessário: DOMPurify

import DOMPurify from "isomorphic-dompurify"

function SafeHTML({ html }: { html: string }) {
  const clean = DOMPurify.sanitize(html, { ALLOWED_TAGS: ["b", "i", "em", "strong", "a", "p", "br"] })
  return <div dangerouslySetInnerHTML={{ __html: clean }} />
}
```

### 6. Rate Limiting (API Routes)
```typescript
// src/lib/rate-limit.ts
import { Ratelimit } from "@upstash/ratelimit"
import { Redis } from "@upstash/redis"

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(100, "1 m"), // 100 req/min
  analytics: true,
})

export async function rateLimit(identifier: string) {
  const { success, limit, remaining, reset } = await ratelimit.limit(identifier)
  return { success, limit, remaining, reset }
}

// Uso em API route:
export async function POST(req: Request) {
  const ip = req.headers.get("x-forwarded-for") || "anonymous"
  const { success } = await rateLimit(`webhook:${ip}`)
  if (!success) return new Response("Rate limited", { status: 429 })
  // ...
}
```

### 7. Headers de Segurança (Next.js)
```typescript
// next.config.js
const securityHeaders = [
  { key: "X-DNS-Prefetch-Control", value: "on" },
  { key: "Strict-Transport-Security", value: "max-age=63072000; includeSubDomains; preload" },
  { key: "X-Content-Type-Options", value: "nosniff" },
  { key: "X-Frame-Options", value: "DENY" },
  { key: "Referrer-Policy", value: "origin-when-cross-origin" },
  { key: "Permissions-Policy", value: "camera=(), microphone=(), geolocation=()" },
  { 
    key: "Content-Security-Policy", 
    value: [
      "default-src 'self'",
      "script-src 'self' 'unsafe-eval' 'unsafe-inline' https://vercel.live",
      "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
      "font-src 'self' https://fonts.gstatic.com",
      "img-src 'self' data: blob: https:",
      "connect-src 'self' https://*.vercel.app wss://*.vercel.app https://graph.facebook.com",
      "frame-ancestors 'none'",
      "base-uri 'self'",
      "form-action 'self'",
    ].join("; ")
  },
]

module.exports = { async headers() { return [{ source: "/:path*", headers: securityHeaders }] } }
```

---

## Segurança de Dados (LGPD/GDPR)

### 1. Minimização e Consentimento
```typescript
// Onboarding: coletar SÓ o necessário
const OnboardingSchema = z.object({
  nomeLoja: z.string().min(2).max(100),
  telefone: z.string().regex(/^\+55\d{10,11}$/), // E.164
  email: z.string().email(),
  // NÃO coletar: CPF, endereço completo, dados bancários (v1)
  consentimentoLGPD: z.literal(true, { message: "Deve aceitar os termos" }),
  consentimentoMarketing: z.boolean().default(false), // Opt-in
})
```

### 2. Direitos do Titular (API)
```typescript
// src/app/api/lgpd/route.ts
export async function POST(req: Request) {
  const session = await auth()
  if (!session) return new Response("Unauthorized", { status: 401 })
  
  const { acao } = await req.json() // "exportar" | "excluir" | "retificar"
  
  switch (acao) {
    case "exportar":
      const dados = await prisma.loja.findUnique({
        where: { id: session.user.lojaId },
        include: { pedidos: { include: { itens: true } }, configuracoes: true },
      })
      // Remover dados sensíveis de terceiros
      return Response.json({ dados: anonimizarTerceiros(dados) })
    
    case "excluir":
      // Soft delete + anonimização (LGPD Art. 16)
      await prisma.$transaction([
        prisma.pedido.updateMany({ where: { lojaId: session.user.lojaId }, data: { clienteNome: "ANONIMIZADO", clienteTelefone: null } }),
        prisma.loja.update({ where: { id: session.user.lojaId }, data: { ativa: false, deletedAt: new Date() } }),
      ])
      await signOut()
      return Response.json({ success: true })
    
    case "retificar":
      // Permitir atualizar dados cadastrais
      // ...
  }
}
```

### 3. Retenção e Exclusão Automática
```typescript
// src/jobs/retencao.ts (Cron job - Vercel Cron ou Upstash)
export async function jobRetencaoDados() {
  const seisMesesAtras = new Date(Date.now() - 180 * 24 * 60 * 60 * 1000)
  
  // Anonimizar pedidos antigos de lojas inativas
  const lojasInativas = await prisma.loja.findMany({
    where: { ativa: false, deletedAt: { lt: seisMesesAtras } },
    select: { id: true },
  })
  
  for (const loja of lojasInativas) {
    await prisma.pedido.updateMany({
      where: { lojaId: loja.id },
      data: { clienteNome: "ANONIMIZADO", clienteTelefone: null, observacao: null },
    })
  }
  
  // Log para auditoria
  console.log(`[LGPD] Anonimizados pedidos de ${lojasInativas.length} lojas inativas > 6 meses`)
}
```

### 4. Registro de Processamento (ROPA)
```markdown
# ROPA - Registro de Operações de Tratamento

| Finalidade | Base Legal | Dados | Compartilhamento | Retenção |
|------------|------------|-------|------------------|----------|
| Processar pedidos WhatsApp | Execução contrato (Art. 7º V LGPD) | Nome, tel, itens, obs | Meta (WhatsApp API) | 6 meses pós-entrega |
| Autenticação usuário | Legítimo interesse / Consentimento | Email, hash senha, lojaId | Resend (email) | Conta ativa + 30 dias |
| Comunicação templates | Execução contrato | Tel, nome, status pedido | Meta (WhatsApp API) | 24h (janela) |
| Analytics agregado | Legítimo interesse | Métricas anônimas | Vercel Analytics | 13 meses |
```

---

## Segurança de Infraestrutura

### 1. Variáveis de Ambiente (NUNCA no repo)
```bash
# .env.local (local only - .gitignored)
NEXTAUTH_SECRET=super-secret-32-chars-min
DATABASE_URL=postgresql://...
WHATSAPP_TOKEN=EAAG...
WHATSAPP_APP_SECRET=...
RESEND_API_KEY=re_...
BLOB_READ_WRITE_TOKEN=vercel_blob_...

# Vercel Dashboard → Settings → Environment Variables
# Separar: Preview vs Production
```

### 2. Secrets Rotation
```bash
# Script mensal (rotacionar WhatsApp token a cada 60 dias)
#!/bin/bash
# rotate-secrets.sh

# 1. Gerar novo token no Meta Developer Console
# 2. Atualizar no Vercel
vercel env add WHATSAPP_TOKEN production < new_token.txt
vercel env add WHATSAPP_TOKEN preview < new_token.txt

# 3. Remover antigo (manter 1 versão anterior para rollback)
vercel env rm WHATSAPP_TOKEN_OLD production
vercel env add WHATSAPP_TOKEN_OLD production < old_token.txt

# 4. Redeploy
vercel --prod
```

### 3. Database Security (Neon)
```sql
-- Revogar permissões públicas
REVOKE ALL ON SCHEMA public FROM PUBLIC;
GRANT USAGE ON SCHEMA public TO app_user;

-- Usuário da aplicação com permissões mínimas
CREATE ROLE app_user WITH LOGIN PASSWORD 'strong-password';
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_user;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO app_user;

-- RLS policies já configuradas (ver acima)
```

### 4. Vercel Security
- **Edge Middleware**: Bloquear IPs maliciosos, bot detection
- **Firewall**: Vercel WAF (regras gerenciadas + custom)
- **Function Isolation**: Cada deploy isolado
- **DDoS Protection**: Automático

---

## Monitoramento de Segurança

### Alertas Críticos
| Evento | Ferramenta | Ação |
|--------|------------|------|
| Tentativa login falhada > 10/min | Vercel Logs / NextAuth | Bloquear IP temporário |
| Webhook signature falha | Logs custom | Alerta SEV-2 |
| Acesso DB fora horário | Neon Audit Logs | Investigar |
| Secret detectado no repo | TruffleHog / GitLeaks | Revogar + rotate imediatamente |
| Vulnerabilidade high/critical | npm audit / CodeQL | Patch em 24h |

### Logs Estruturados (Sem Dados Sensíveis)
```typescript
// src/lib/logger.ts
import { v4 as uuidv4 } from "uuid"

export function logSecurity(event: string, data: Record<string, unknown>) {
  const sanitized = { ...data }
  // Remover/hashear campos sensíveis
  delete sanitized.password
  delete sanitized.token
  delete sanitized.authorization
  if (sanitized.email) sanitized.email = hashEmail(sanitized.email as string)
  if (sanitized.telefone) sanitized.telefone = maskPhone(sanitized.telefone as string)
  
  console.log(JSON.stringify({
    timestamp: new Date().toISOString(),
    level: "SECURITY",
    event,
    requestId: uuidv4(),
    ...sanitized,
  }))
}

// Uso:
logSecurity("LOGIN_FAILED", { email: "user@test.com", ip: "1.2.3.4", reason: "invalid_credentials" })
logSecurity("WEBHOOK_INVALID_SIGNATURE", { ip: "1.2.3.4", messageId: "msg-123" })
```

---

## Resposta a Incidentes de Segurança

### Playbook Rápido
```markdown
## INCIDENTE: Vazamento de Segredo (Token/Chave)

**Detecção:** TruffleHog alert / GitHub secret scanning / Logs

**Resposta Imediata (< 15 min):**
1. Revogar segredo comprometido (Meta, Resend, Neon, Vercel)
2. Gerar novo segredo
3. Atualizar no Vercel (Preview + Production)
4. Deploy imediato (`vercel --prod`)
5. Verificar logs: uso indevido do segredo antigo?

**Pós-Incidente (< 24h):**
1. Rotacionar TODOS segredos relacionados
2. Auditoria de acesso: quem teve acesso ao repo/logs?
3. Atualizar .gitignore / .env.example
4. Post-mortem blameless
5. Melhorar prevenção: pre-commit hooks, secret scanning CI
```

### Contatos de Emergência
| Serviço | Contato | SLA |
|---------|---------|-----|
| Vercel Security | security@vercel.com | 4h (Pro) |
| Neon Support | support@neon.tech | Business hours |
| Meta Business | Meta Business Help | Business hours |
| Resend | support@resend.com | Business hours |
| ANPD (LGPD) | https://www.gov.br/anpd/ | 48h notificação |

---

## Checklist de Segurança por Feature

### Nova API Route / Server Action
- [ ] Validação Zod em TODOS inputs
- [ ] Autenticação verificada (`auth()`)
- [ ] Autorização verificada (RLS + check manual se necessário)
- [ ] Rate limiting aplicado
- [ ] Logs de segurança (auditoria)
- [ ] Tratamento de erro sem vazar stack trace
- [ ] Testes: input inválido, não autenticado, não autorizado, rate limit

### Novo Componente Frontend
- [ ] Sem `dangerouslySetInnerHTML` (ou DOMPurify se inevitável)
- [ ] Props validadas (TypeScript)
- [ ] Event handlers: prevenir XSS (não `onClick={eval}`)
- [ ] CSP compatível (inline styles/scripts mínimos)

### Nova Integração Externa
- [ ] Secrets apenas em Vercel env
- [ ] Validação de webhook (HMAC, assinatura)
- [ ] Idempotência implementada
- [ ] Timeout e retry configurados
- [ ] Logs de request/response (sanitizados)
- [ ] Circuit breaker / fallback

---

## Ferramentas de Segurança

| Ferramenta | Uso | Frequência |
|------------|-----|------------|
| `npm audit` | Vulnerabilidades deps | Todo PR / Semanal |
| `trufflehog` | Secrets no código | Todo PR / Push |
| `semgrep` | SAST (padrões inseguros) | Todo PR |
| `CodeQL` | Análise profunda | Semanal / Release |
| `OWASP ZAP` | DAST (scan dinâmico) | Mensal / Pré-release |
| `k6` | Load + stress (DoS) | Mensal |
| `axe` | A11y (também segurança) | Todo PR |

---

## Próximos Passos
- [[03 - Guias Práticos/Guia de Publicação]] — Deploy, monitoramento, runbook
- [[02 - Modelos/Segurança e Validação/Checklist de Segurança]] — 100+ itens detalhados
- [[02 - Modelos/Segurança e Validação/Plano de Validação]] — Go/No-Go criteria
- [[04 - Referência/Glossário/OWASP Top 10]]
- [[04 - Referência/Glossário/LGPD]]
- [[04 - Referência/Glossário/RLS]]
- [[04 - Referência/Glossário/HMAC]]
- [[04 - Referência/Glossário/CSP]]
- [[04 - Referência/Ferramentas/TruffleHog]]
- [[04 - Referência/Ferramentas/Semgrep]]
- [[04 - Referência/Ferramentas/CodeQL]]