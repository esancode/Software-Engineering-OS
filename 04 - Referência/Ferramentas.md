# Ferramentas e Stack Tecnológica

> **Local:** `04 - Referência/Ferramentas.md`
> **Objetivo:** Catálogo completo de ferramentas usadas no Software Engineering OS, com comandos, configuração e troubleshooting.

---

## Core Stack (Obrigatório)

### Next.js 15 (App Router)
```bash
# Instalação
npx create-next-app@latest meu-app --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

# Comandos essenciais
npm run dev          # Desenvolvimento (Turbopack)
npm run build        # Build produção
npm run start        # Servir build
npm run lint         # ESLint
npm run typecheck    # tsc --noEmit
```

**Configurações-chave (`next.config.js`):**
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  experimental: {
    serverActions: { bodySizeLimit: "2mb" },
  },
  images: {
    remotePatterns: [{ protocol: "https", hostname: "**.facebook.com" }],
  },
  async headers() {
    return [{ source: "/:path*", headers: securityHeaders }]
  },
}
module.exports = nextConfig
```

### TypeScript (Strict Mode)
```json
// tsconfig.json - trechos essenciais
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "paths": { "@/*": ["./src/*"] }
  }
}
```

### Tailwind CSS v4
```css
/* src/app/globals.css */
@import "tailwindcss";
@theme {
  --color-primary-500: #3b82f6;
  --color-success: #16a34a;
  --color-error: #dc2626;
  --font-sans: "Inter", system-ui, sans-serif;
  --radius-base: 8px;
  --shadow-base: 0 1px 3px 0 rgb(0 0 0 / 0.1);
}
@utility container { @apply mx-auto max-w-7xl px-4 sm:px-6 lg:px-8; }
```

### Prisma ORM + Neon (PostgreSQL)
```bash
# Setup
npm install prisma @prisma/client
npx prisma init --datasource-provider postgresql

# Desenvolvimento
npx prisma migrate dev --name init
npx prisma studio
npx prisma generate

# Produção (CI/CD)
npx prisma migrate deploy
```

**Schema exemplo (`prisma/schema.prisma`):**
```prisma
generator client {
  provider = "prisma-client-js"
  previewFeatures = ["postgresqlExtensions"]
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}

model Loja {
  id            String   @id @default(cuid())
  nome          String
  telefone      String   @unique
  email         String   @unique
  passwordHash  String?
  ativa         Boolean  @default(true)
  role          Role     @default(LOJISTA)
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  
  pedidos       Pedido[]
  configuracao  ConfiguracaoLoja?
  
  @@map("lojas")
}

model Pedido {
  id            String        @id @default(cuid())
  numero        Int           @unique
  lojaId        String
  clienteNome   String
  clienteTelefone String
  status        StatusPedido  @default(NOVO)
  total         Decimal       @default(0)
  canal         Canal         @default(WHATSAPP)
  observacao    String?
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt
  
  loja          Loja          @relation(fields: [lojaId], references: [id], onDelete: Cascade)
  itens         ItemPedido[]
  
  @@index([lojaId, status])
  @@index([lojaId, createdAt])
  @@map("pedidos")
}

enum StatusPedido { NOVO PREPARANDO PRONTO ENTREGUE CANCELADO }
enum Canal { WHATSAPP MANUAL }
enum Role { LOJISTA FUNCIONARIO ADMIN }
```

---

## Autenticação

### NextAuth v5 (Auth.js)
```bash
npm install next-auth@beta @auth/prisma-adapter bcryptjs
npm install -D @types/bcryptjs
```

**Config (`src/lib/auth.ts`):**
```typescript
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
        const { Resend } = await import("resend")
        const resend = new Resend(process.env.RESEND_API_KEY)
        await resend.emails.send({
          from: "SEU_APP <noreply@SEU_DOMINIO.com>",
          to: identifier,
          subject: "Seu link de acesso ao PedidosSimples",
          html: `<p>Clique para entrar: <a href="${url}">${url}</a></p>`,
        })
      },
    }),
    Credentials({
      name: "credentials",
      credentials: { email: {}, password: {} },
      authorize: async (credentials) => {
        const user = await prisma.loja.findUnique({ where: { email: credentials?.email } })
        if (!user?.passwordHash) return null
        const valid = await bcrypt.compare(credentials!.password, user.passwordHash)
        if (!valid) return null
        return { id: user.id, email: user.email, lojaId: user.id, role: user.role }
      },
    }),
  ],
  session: { strategy: "jwt", maxAge: 30 * 24 * 60 * 60 },
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

---

## Testes

### Vitest (Unit + Integration)
```bash
npm install -D vitest @vitest/coverage-v8 @testing-library/react @testing-library/jest-dom jsdom
npm install -D @testing-library/user-event
```

**Config (`vitest.config.ts`):**
```typescript
import { defineConfig } from "vitest/config"
import react from "@vitejs/plugin-react"
import path from "path"

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./tests/setup.ts"],
    include: ["src/**/*.test.{ts,tsx}"],
    coverage: {
      provider: "v8",
      reporter: ["text", "json", "html", "lcov"],
      thresholds: { lines: 80, functions: 80, branches: 70, statements: 80 },
      exclude: ["node_modules/", "tests/", "*.config.*", "src/app/**", "src/components/ui/**"],
    },
    alias: { "@": path.resolve(__dirname, "./src") },
  },
})
```

### Playwright (E2E)
```bash
npm install -D @playwright/test
npx playwright install --with-deps chromium
```

**Config (`playwright.config.ts`):**
```typescript
import { defineConfig, devices } from "@playwright/test"

export default defineConfig({
  testDir: "./tests/e2e",
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [["html", { outputFolder: "playwright-report" }], ["json", { outputFile: "test-results/results.json" }]],
  use: { baseURL: "http://localhost:3000", trace: "on-first-retry", screenshot: "only-on-failure" },
  projects: [
    { name: "chromium", use: { ...devices["Desktop Chrome"] } },
    { name: "firefox", use: { ...devices["Desktop Firefox"] } },
    { name: "webkit", use: { ...devices["Desktop Safari"] } },
    { name: "Mobile Chrome", use: { ...devices["Pixel 5"] } },
    { name: "Mobile Safari", use: { ...devices["iPhone 12"] } },
  ],
  webServer: { command: "npm run dev", url: "http://localhost:3000", reuseExistingServer: !process.env.CI, timeout: 120000 },
})
```

### axe-playwright (A11y)
```bash
npm install -D axe-playwright
```

### k6 (Load Testing)
```bash
# Instalação (binary)
# Windows: choco install k6
# Mac: brew install k6
# Linux: sudo gpg -k && echo 'deb https://dl.k6.io/deb stable main' | sudo tee /etc/apt/sources.list.d/k6.list && sudo apt update && sudo apt install k6
```

---

## Segurança

### ESLint + Security Plugins
```bash
npm install -D eslint @eslint/js typescript-eslint eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-plugin-security eslint-plugin-import
```

**Config (`eslint.config.js`):**
```javascript
import js from "@eslint/js"
import tseslint from "typescript-eslint"
import react from "eslint-plugin-react"
import reactHooks from "eslint-plugin-react-hooks"
import jsxA11y from "eslint-plugin-jsx-a11y"
import security from "eslint-plugin-security"
import importPlugin from "eslint-plugin-import"

export default tseslint.config(
  { ignores: ["node_modules/", ".next/", "out/", "dist/", "*.config.*"] },
  js.configs.recommended,
  ...tseslint.configs.recommended,
  {
    plugins: { react, "react-hooks": reactHooks, "jsx-a11y": jsxA11y, security, import: importPlugin },
    settings: { react: { version: "18.3" }, "import/resolver": { typescript: true, node: true } },
    rules: {
      ...react.configs.recommended.rules,
      ...reactHooks.configs.recommended.rules,
      ...jsxA11y.configs.recommended.rules,
      "security/detect-object-injection": "warn",
      "security/detect-non-literal-regexp": "warn",
      "security/detect-unsafe-regex": "warn",
      "security/detect-buffer-noassert": "warn",
      "security/detect-child-process": "warn",
      "security/detect-disable-mustache-escape": "warn",
      "security/detect-eval-with-expression": "error",
      "security/detect-no-csrf-before-method-override": "warn",
      "security/detect-non-literal-fs-filename": "warn",
      "security/detect-possible-timing-attacks": "warn",
      "security/detect-pseudoRandomBytes": "warn",
      "@typescript-eslint/no-explicit-any": "error",
      "@typescript-eslint/no-unused-vars": ["error", { argsIgnorePattern: "^_" }],
      "import/order": ["error", { groups: ["builtin", "external", "internal", "parent", "sibling", "index"], "newlines-between": "always" }],
    },
  }
)
```

### TruffleHog (Secrets Scanning)
```bash
# CLI
npm install -g @trufflehog/trufflehog
# ou
go install github.com/trufflesecurity/trufflehog/v3@latest

# Uso
trufflehog filesystem . --no-verification
trufflehog git file://. --since-commit HEAD~10
```

### Semgrep (SAST)
```bash
# Instalação
pip install semgrep
# ou
brew install semgrep

# Scan
semgrep scan --config=auto --error
semgrep scan --config=p/ci --config=p/secrets --config=p/typescript
```

### CodeQL (GitHub Advanced Security)
```yaml
# .github/workflows/codeql.yml
name: CodeQL
on: [push, pull_request, schedule: [{ cron: "0 0 * * 0" }]]
jobs:
  analyze:
    runs-on: ubuntu-latest
    permissions: { security-events: write, actions: read, contents: read }
    steps:
      - uses: actions/checkout@v4
      - uses: github/codeql-action/init@v3
        with: { languages: typescript, queries: +security-and-quality }
      - uses: github/codeql-action/analyze@v3
```

---

## Deploy & Infra

### Vercel CLI
```bash
npm install -g vercel@latest
vercel login

# Comandos
vercel              # Deploy preview
vercel --prod       # Deploy production
vercel rollback     # Rollback instantâneo
vercel logs         # Ver logs funções
vercel env ls       # Listar env vars
vercel env add KEY  # Adicionar env var
vercel inspect URL  # Debug deploy
```

### Neon (PostgreSQL Serverless)
```bash
# CLI
npm install -g @neondatabase/cli
neon auth
neon projects list
neon branches list --project-id <id>
neon branches create --project-id <id> --name preview-123 --parent main
neon sql --project-id <id> --branch main "SELECT * FROM pedidos LIMIT 5"
```

### Upptime (Uptime Monitor)
```yaml
# .github/workflows/uptime.yml
name: Uptime Monitor
on:
  schedule: [{ cron: "*/5 * * * *" }]
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
          # Opcional: status page
          # owner: SEU_USUARIO_GITHUB
          # repo: status-page
```

---

## Comunicação & Integrações

### WhatsApp Business API (Meta)
```bash
# Meta Developer Console: developers.facebook.com
# 1. Criar app tipo "Business"
# 2. Adicionar produto "WhatsApp Business API"
# 3. Configurar webhook: URL + Verify Token
# 4. Assinar campos: messages, message_template_status_update
# 5. Templates: submeter para aprovação (categoria: UTILITY)
# 6. Token de acesso: gerar permanente (expira 60 dias) ou usar system user token
```

**Webhook endpoint:**
```typescript
// src/app/api/webhook/whatsapp/route.ts
import { headers } from "next/headers"
import crypto from "crypto"
import { NextRequest, NextResponse } from "next/server"

const APP_SECRET = process.env.WHATSAPP_APP_SECRET!

export async function POST(req: NextRequest) {
  const body = await req.text()
  const signature = headers().get("x-hub-signature-256")
  const expected = "sha256=" + crypto.createHmac("sha256", APP_SECRET).update(body).digest("hex")
  
  if (!crypto.timingSafeEqual(Buffer.from(signature || ""), Buffer.from(expected))) {
    return NextResponse.json({ error: "Invalid signature" }, { status: 401 })
  }
  
  // Processar...
  return NextResponse.json({ success: true })
}

export async function GET(req: NextRequest) {
  const searchParams = req.nextUrl.searchParams
  const mode = searchParams.get("hub.mode")
  const token = searchParams.get("hub.verify_token")
  const challenge = searchParams.get("hub.challenge")
  
  if (mode === "subscribe" && token === process.env.WHATSAPP_VERIFY_TOKEN) {
    return new NextResponse(challenge, { status: 200 })
  }
  return NextResponse.json({ error: "Forbidden" }, { status: 403 })
}
```

### Resend (Email)
```bash
npm install resend

# Config
# 1. resend.com → Domínios → Verificar domínio
# 2. API Keys → Criar chave
# 3. Vercel env: RESEND_API_KEY
```

```typescript
// src/lib/email.ts
import { Resend } from "resend"
const resend = new Resend(process.env.RESEND_API_KEY)

export async function sendMagicLink(email: string, url: string) {
  return resend.emails.send({
    from: "SEU_APP <noreply@SEU_DOMINIO.com>",
    to: email,
    subject: "Seu link de acesso",
    html: `<p>Olá! Clique para entrar: <a href="${url}">${url}</a></p><p>Expira em 15 min.</p>`,
  })
}

export async function sendTemplate(to: string, templateId: string, vars: Record<string, string>) {
  // Para WhatsApp templates, use Meta API diretamente
}
```

### Vercel Blob (Storage)
```bash
npm install @vercel/blob
# Vercel Dashboard → Storage → Create Blob Store → Token
```

```typescript
// Upload
import { put } from "@vercel/blob"
const blob = await put(`pedidos/${pedidoId}/comprovante.pdf`, file, { access: "public" })
// blob.url = URL pública
```

---

## Qualidade & DX

### Husky + lint-staged (Pre-commit)
```bash
npm install -D husky lint-staged
npx husky init
```

**`.husky/pre-commit`:**
```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"
npx lint-staged
```

**`package.json`:**
```json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md,css}": ["prettier --write"]
  }
}
```

### Prettier
```bash
npm install -D prettier prettier-plugin-tailwindcss
```

**`.prettierrc`:**
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100,
  "bracketSpacing": true,
  "arrowParens": "always",
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

### Changesets (Versionamento)
```bash
npm install -D @changesets/cli
npx changeset init
```

**Workflow:**
```bash
# Nova mudança
npx changeset  # Interativo: patch/minor/major + descrição

# Release
npx changeset version  # Atualiza CHANGELOG + package.json
git add . && git commit -m "chore: version bump"
git push origin main
# GitHub Action publica no npm se configurado
```

---

## Monitoramento & Logs

### Sentry (Error Tracking)
```bash
npm install @sentry/nextjs
npx @sentry/wizard@latest -i nextjs
```

**`sentry.client.config.ts`:**
```typescript
import * as Sentry from "@sentry/nextjs"
Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
  replaysSessionSampleRate: 0.1,
  integrations: [Sentry.replayIntegration()],
})
```

### Vercel Analytics (Web Vitals)
```bash
npm install @vercel/analytics
```

**`src/app/layout.tsx`:**
```tsx
import { Analytics } from "@vercel/analytics/react"
export default function RootLayout({ children }) {
  return (
    <html lang="pt-BR">
      <body>{children}<Analytics /></body>
    </html>
  )
}
```

---

## Utilitários Comuns

### Zod (Validation)
```bash
npm install zod
```

### date-fns (Datas)
```bash
npm install date-fns
npm install -D @types/date-fns  # se necessário
```

### clsx / tailwind-merge (Class Names)
```bash
npm install clsx tailwind-merge
```

```typescript
// src/lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

export function formatarMoeda(valor: number | string) {
  return new Intl.NumberFormat("pt-BR", { style: "currency", currency: "BRL" }).format(Number(valor))
}

export function formatarHora(data: Date | string) {
  return new Intl.DateTimeFormat("pt-BR", { hour: "2-digit", minute: "2-digit" }).format(new Date(data))
}

export function formatarData(data: Date | string) {
  return new Intl.DateTimeFormat("pt-BR", { day: "2-digit", month: "2-digit", year: "numeric" }).format(new Date(data))
}
```

### React Hook Form + Zod Resolver
```bash
npm install react-hook-form @hookform/resolvers zod
```

### Zustand (State Management - se necessário)
```bash
npm install zustand
```

### Lucide React (Icons)
```bash
npm install lucide-react
# Para bundle menor em produção:
npm install lucide-static
```

---

## Troubleshooting Rápido

| Problema | Comando / Solução |
|----------|-------------------|
| `Module not found` | `rm -rf node_modules package-lock.json && npm install` |
| TypeScript errors em build | `npm run typecheck` localmente primeiro |
| Prisma Client out of sync | `npx prisma generate` |
| Migration falha | `npx prisma migrate reset` (dev) / `npx prisma migrate resolve --rolled-back` (prod) |
| Vercel build timeout | Otimizar bundle, dividir build, ou upgrade Pro |
| SSE desconecta no Vercel Hobby | Timeout 10s → Upgrade Pro (60s) ou Pusher |
| WhatsApp webhook 401 | Verificar `WHATSAPP_APP_SECRET` no Vercel env |
| Magic Link não chega | Verificar domínio verificado no Resend + logs |
| Hydration mismatch | Client component wrapper ou `suppressHydrationWarning` |
| Flaky test E2E | `test.retry(2)` temporário + issue para fix root cause |
| Bundle size alto | `npm run build && npx @next/bundle-analyzer` |

---

## Versões Fixadas (Lockfile Strategy)

```json
// package.json - engines
{
  "engines": {
    "node": ">=20.0.0",
    "npm": ">=10.0.0"
  },
  "packageManager": "npm@10.8.0"
}
```

**Dependências Críticas (fixar major):**
```json
{
  "dependencies": {
    "next": "15.0.x",
    "react": "18.3.x",
    "react-dom": "18.3.x",
    "next-auth": "5.0.0-beta.x",
    "@prisma/client": "5.22.x",
    "zod": "3.23.x",
    "resend": "4.0.x"
  },
  "devDependencies": {
    "typescript": "5.6.x",
    "tailwindcss": "3.4.x",
    "vitest": "2.1.x",
    "@playwright/test": "1.48.x",
    "eslint": "9.13.x"
  }
}
```

---

## Referências Cruzadas

| Ferramenta | Guia Relacionado |
|------------|------------------|
| Next.js | [[03 - Guias Práticos/Guia de Vibe Coding]], [[03 - Guias Práticos/Guia de UI/UX]] |
| Prisma | [[03 - Guias Práticos/Guia de Segurança]] (RLS), [[02 - Modelos/Requisitos e Arquitetura/Modelo de Dados]] |
| NextAuth | [[03 - Guias Práticos/Guia de Segurança]] (Auth), [[02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth]] |
| Vitest | [[03 - Guias Práticos/Guia de Testes]] |
| Playwright | [[03 - Guias Práticos/Guia de Testes]] |
| k6 | [[03 - Guias Práticos/Guia de Testes]] (Load) |
| Vercel | [[03 - Guias Práticos/Guia de Publicação]] |
| Neon | [[03 - Guias Práticos/Guia de Publicação]] (Backup) |
| TruffleHog | [[03 - Guias Práticos/Guia de Segurança]] |
| Semgrep | [[03 - Guias Práticos/Guia de Segurança]] |
| Resend | [[02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth]] |
| WhatsApp API | [[02 - Modelos/Requisitos e Arquitetura/Registro de Integrações]] |

---

## Atualização de Ferramentas

```bash
# Semanal (patch/minor seguro)
npm outdated
npx npm-check-updates -u  # Atualiza package.json
npm install
npm run test && npm run build

# Major: manual, com testes completos + changelog
# Ex: Next.js 14 → 15: ler migration guide, testar app router changes
```