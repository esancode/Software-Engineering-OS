# ETAPA 09 — Preparar o Ambiente e o Plano de Implementação

## Objetivo

Preparar **todas as condições técnicas** para desenvolver com segurança e organização: repositório, ferramentas, estrutura de código, dependências, ambientes, estratégia de testes, convenções e contexto para IA.

## Por que esta etapa existe?

Ambiente mal configurado gera atrito constante, bugs de "funciona na minha máquina", perda de tempo debugando setup em vez de features, e contexto insuficiente para a IA produzir código correto.

## Quando começar

- ETAPA 08 concluída (UI estruturada, componentes definidos, design system pronto)
- ETAPA 05 concluída (Arquitetura definida, tech stack escolhida, ADRs registrados)
- Pronto para começar a codificar

## O que você precisa fazer

1. **Prepare o ambiente de desenvolvimento local** — Node, ferramentas, extensões, configurações
2. **Configure o repositório Git** — remote, branch strategy, commit conventions, hooks
3. **Crie a estrutura inicial do projeto** — pastas, arquivos base, configurações (TS, ESLint, Prettier, Tailwind)
4. **Instale e configure dependências** — runtime, dev, UI, validação, auth, banco, etc.
5. **Configure ambientes** — .env.local (dev), .env.example, variáveis por ambiente (staging/prod)
6. **Configure gestão de segredos** — nunca no git, plataforma de deploy para prod
7. **Defina estratégia de testes** — unit, integration, e2e, ferramentas, cobertura mínima
8. **Defina estratégia de tratamento de erros** — error boundaries, logging, monitoramento
9. **Estabeleça convenções importantes** — naming, imports, componentes, hooks, tipos
10. **Organize as tarefas de implementação** — divida features em tasks pequenas, ordenadas
11. **Defina critérios de conclusão por funcionalidade** — Definition of Done por feature
12. **Prepare o contexto para IA** — arquivos de contexto, prompts base, padrões do projeto
13. **Verifique se o ambiente realmente funciona** — roda dev, build, testes, lint
14. **Produza os documentos: Plano de Implementação + Configuração de Ambientes + Contexto para IA**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Configurar ambiente local (Node, VS Code, etc.) | 👤 USUÁRIO |
| Inicializar repositório, configurar Git | 👤 USUÁRIO (IA pode gerar configs) |
| Criar estrutura inicial do projeto | 🤖 IA (você aprova estrutura) |
| Instalar/configurar dependências | 🤖 IA (comando) + 👤 USUÁRIO (aprova package.json) |
| Configurar ambientes e segredos | 👤 USUÁRIO (valores reais) + 🤖 IA (estrutura) |
| Definir estratégia de testes/erros | 👤 USUÁRIO + 🤖 IA (sugere) |
| Estabelecer convenções | 🤖 IA (sugere baseadas no stack) + 👤 USUÁRIO (aprova) |
| Organizar tarefas e DoD | 👤 USUÁRIO (com ajuda da IA para quebrar) |
| Preparar contexto para IA | 🤖 IA (gera arquivos) + 👤 USUÁRIO (valida) |
| Verificar ambiente (dev, build, test, lint) | ✅ VALIDAÇÃO (você executa) |
| Aprovar plano de implementação | 👤 USUÁRIO |

## Ferramentas recomendadas

| Categoria | Ferramentas (exemplo stack Next.js/TS) |
|-----------|----------------------------------------|
| Runtime | Node.js 20+ (LTS), pnpm (recomendado) ou npm |
| Editor | VS Code + extensões: ESLint, Prettier, Tailwind CSS IntelliSense, Prisma, TypeScript |
| Lint/Format | ESLint (flat config), Prettier, eslint-plugin-tailwindcss |
| Type Check | TypeScript 5+ (strict mode) |
| Testes | Vitest (unit/integration), Playwright (e2e), Testing Library |
| Git Hooks | Husky + lint-staged (pre-commit: lint + format + typecheck) |
| Commit | Conventional Commits + commitlint |
| Banco | Prisma CLI (migrate, studio, generate) |
| Deploy | Vercel CLI, ou plataforma escolhida |
| IA Context | Arquivos `.claude/` ou `CLAUDE.md` no projeto |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Plano de Implementação | `02 - Modelos/Desenvolvimento/Plano de Implementação.md` | `05 - Projetos/SeuProjeto/05 - Desenvolvimento/` |
| Configuração de Ambientes | `02 - Modelos/Desenvolvimento/Configuração de Ambientes.md` | `05 - Projetos/SeuProjeto/05 - Desenvolvimento/` |
| Contexto para IA (CLAUDE.md) | `02 - Modelos/Desenvolvimento/Contexto para IA.md` | Raiz do projeto (`05 - Projetos/SeuProjeto/`) |

### Condicionais
- Estratégia de Testes Detalhada (se projeto médio/grande)
- Convenções de Código (se team >1 ou projeto complexo)
- Guia de Debugging/Logs (se arquitetura complexa)

### Opcionais
- Scripts de Utilidade (seed, migração, deploy local)
- Documentação de Onboarding Técnico (se team >1)

## Como produzir os documentos

### Plano de Implementação

```markdown
# Plano de Implementação — PedidosSimples v1

## Princípios
- **Vertical slices:** Implementar feature completa (backend + frontend + teste) por vez
- **Pequenas tarefas:** Cada task = 1-3 commits, < 2h de trabalho
- **Teste junto:** Não separar "codar" de "testar"
- **Contexto para IA:** Cada task tem prompt base com requisitos, componentes, critérios

---

## Ordem de Implementação (Sugerida)

### Fase 0: Fundação (Semana 1)
| Task ID | Descrição | Estimativa | Dependência | DoD |
|---------|-----------|------------|-------------|-----|
| T0.1 | Setup Next.js + TS + Tailwind + ESLint + Prettier | 2h | - | `pnpm dev` roda, `pnpm build` passa, `pnpm lint` passa |
| T0.2 | Setup Prisma + PostgreSQL (Neon local/dev) + schema inicial | 2h | T0.1 | `prisma migrate dev` roda, `prisma studio` abre |
| T0.3 | Setup NextAuth (Credentials + Email) + middleware proteção | 3h | T0.2 | Login/logout funciona, rotas protegidas |
| T0.4 | Componentes base (Button, Input, Badge, Card, Modal, etc.) | 4h | T0.1 | Storybook roda, componentes documentados |
| T0.5 | Layout base (Header, Sidebar/Nav, Page wrapper) + Responsive | 3h | T0.4 | Navegação funciona mobile/desktop |
| T0.6 | Configuração CI/CD (GitHub Actions: lint, typecheck, test, build) | 2h | T0.1 | Pipeline passa no push |

### Fase 1: Core - Pedidos (Semana 2-3)
| Task ID | Descrição | Estimativa | Dependência | DoD |
|---------|-----------|------------|-------------|-----|
| T1.1 | API: Webhook WhatsApp (receber, parsear, criar pedido) | 4h | T0.2, T0.3 | Webhook recebe payload, cria pedido no BD, retorna 200 |
| T1.2 | API: Listar pedidos (com filtro status, paginação) | 2h | T1.1 | GET /api/pedidos retorna lista paginada, filtros funcionam |
| T1.3 | API: Atualizar status pedido | 2h | T1.1 | PATCH /api/pedidos/[id] valida transição, atualiza |
| T1.4 | UI: Dashboard (Lista) - Server Component + Suspense | 3h | T0.5, T1.2 | Lista carrega, estados vazio/carregando/erro |
| T1.5 | UI: OrderCard + ActionButtons (atualizar status) | 3h | T0.4, T1.4 | Card renderiza, botões disparam API, feedback visual |
| T1.6 | Tempo real: SSE para novos pedidos / atualizações | 3h | T1.2, T1.3 | Novo pedido aparece sem refresh, status atualiza ao vivo |
| T1.7 | UI: Detalhes do Pedido (Modal) | 2h | T1.4 | Modal abre, mostra detalhes, ações funcionam |
| T1.8 | Testes: API pedidos (unit + integration) | 3h | T1.1-1.3 | Cobertura >80% APIs pedidos |
| T1.9 | Testes: UI Dashboard (e2e Playwright) | 3h | T1.4-1.7 | Fluxo feliz: login → ver lista → atualizar status |

### Fase 2: Responder Cliente (Semana 3)
| Task ID | Descrição | Estimativa | Dependência | DoD |
|---------|-----------|------------|-------------|-----|
| T2.1 | API: Enviar mensagem WhatsApp (template + custom) | 3h | T0.2 | POST /api/pedidos/[id]/reply envia via WhatsApp API |
| T2.2 | UI: ReplyModal (templates + editor + envio) | 3h | T0.4, T1.7 | Modal abre, templates funcionam, envio com feedback |
| T2.3 | Testes: API reply + UI ReplyModal | 2h | T2.1, T2.2 | Cobertura APIs, e2e fluxo responder |

### Fase 3: Configurações (Semana 4)
| Task ID | Descrição | Estimativa | Dependência | DoD |
|---------|-----------|------------|-------------|-----|
| T3.1 | API: CRUD Estabelecimento (nome, telefone, horários) | 3h | T0.2 | CRUD completo, validações |
| T3.2 | API: CRUD Produtos | 3h | T3.1 | CRUD completo, preço em centavos |
| T3.3 | UI: Páginas Configurações (abas: Perfil, Estabelecimento, Horários, Produtos, Integrações) | 4h | T0.5, T3.1, T3.2 | Navegação tabs, formulários salvam, validação |
| T3.4 | UI: Cadastro/Edição Produtos (Modal) | 2h | T3.2 | Modal CRUD produtos |
| T3.5 | Testes: Configurações | 2h | T3.1-3.4 | Cobertura APIs + e2e fluxos principais |

### Fase 4: Onboarding + Polimento (Semana 4-5)
| Task ID | Descrição | Estimativa | Dependência | DoD |
|---------|-----------|------------|-------------|-----|
| T4.1 | Onboarding: Wizard primeira vez (conectar WhatsApp, configurar loja) | 4h | T0.3, T3.1 | 3 passos, salva tudo, redireciona dashboard |
| T4.2 | Empty states, loading states, error states - revisão geral | 3h | Todas UI | Todas telas têm estados completos |
| T4.3 | Acessibilidade: revisão + correções (foco, ARIA, contraste) | 3h | Todas UI | axe-core 0 violations, navegação teclado |
| T4.4 | Performance: otimizações (React.memo, useCallback, dynamic import) | 2h | Todas UI | Lighthouse >90 performance |
| T4.5 | Testes de regressão completos | 2h | Todas | Suite completa passa |

---

## Definition of Done (Por Feature)

Uma feature está **DONE** quando:

- [ ] Código implementado seguindo convenções do projeto
- [ ] TypeScript strict mode passa (zero erros)
- [ ] ESLint + Prettier passam (zero warnings/errors)
- [ ] Testes unitários/integration escritos e passando (>80% cobertura da feature)
- [ ] Testes e2e do fluxo principal passando (Playwright)
- [ ] Build de produção passa (`pnpm build`)
- [ ] Preview deploy funciona (Vercel preview ou local build)
- [ ] Feature testada manualmente no fluxo completo (smoke test)
- [ ] Documentação técnica atualizada se API mudou
- [ ] Contexto para IA atualizado se novos padrões surgiram
- [ ] Aprovado por você (USUÁRIO) — revisão de código + demo

---

## Riscos e Mitigações no Plano

| Risco | Prob. | Impacto | Mitigação |
|-------|-------|---------|-----------|
| WhatsApp API webhook não recebe em localhost | Alta | Alto | ngrok/Cloudflare Tunnel + script de setup automatizado |
| Prisma + Serverless cold start | Média | Médio | Neon pooler, connection limit, testar cedo |
| NextAuth config complexa | Média | Alto | Seguir docs oficial, testar auth isoladamente primeiro |
| SSE não escala | Baixa (v1) | Médio | Aceitar para v1, planejar Pusher v2 (ADR) |
| Escopo cresce (scope creep) | Alta | Alto | DoD rígido, fora de escopo documentado (ETAPA 03), revisão semanal |

---

## Contexto para IA (Arquivos a Criar/Manter)

| Arquivo | Conteúdo | Atualização |
|---------|----------|-------------|
| `CLAUDE.md` (raiz projeto) | Visão geral, stack, convenções, como rodar, padrões | Início + quando muda padrão |
| `.claude/context/requirements.md` | Requisitos funcionais resumidos + critérios aceitação | Início |
| `.claude/context/components.md` | Registro de componentes (props, estados, uso) | Quando adiciona componente |
| `.claude/context/api-patterns.md` | Padrões de API (naming, validação Zod, erros, resposta) | Quando define padrão |
| `.claude/context/db-patterns.md` | Padrões Prisma (naming, queries, transações) | Quando define padrão |
| `.claude/context/testing.md` | Padrões de teste (vitest, playwright, fixtures, mocks) | Quando define padrão |
```

### Configuração de Ambientes

```markdown
# Configuração de Ambientes — PedidosSimples

## Variáveis de Ambiente

### Desenvolvimento Local (.env.local)
```bash
# Database
DATABASE_URL="postgresql://user:pass@localhost:5432/pedidosimples?schema=public"

# NextAuth
NEXTAUTH_SECRET="dev-secret-gere-com-openssl-rand-base64-32"
NEXTAUTH_URL="http://localhost:3000"

# WhatsApp Cloud API (Meta Developers)
WHATSAPP_API_TOKEN="EAAG...token-do-app..."
WHATSAPP_PHONE_NUMBER_ID="1234567890"
WHATSAPP_WEBHOOK_VERIFY_TOKEN="meu-verify-token-seguro"

# Email (Resend/SendGrid) - para Magic Link
RESEND_API_KEY="re_..."
EMAIL_FROM="noreply@seudominio.com"

# Optional: Tempo real v1 (SSE nativo - não precisa config)
# PUSHER_APP_ID=""
# PUSHER_KEY=""
# PUSHER_SECRET=""
# PUSHER_CLUSTER=""
```

### Staging (.env.staging - na Vercel)
```bash
DATABASE_URL="postgresql://...neon.tech/pedidosimples_staging"
NEXTAUTH_SECRET="staging-secret-diferente-do-dev"
NEXTAUTH_URL="https://staging-pedidosimples.vercel.app"
WHATSAPP_API_TOKEN="token-staging"
WHATSAPP_PHONE_NUMBER_ID="id-staging"
WHATSAPP_WEBHOOK_VERIFY_TOKEN="verify-staging"
RESEND_API_KEY="re_...staging"
EMAIL_FROM="noreply@staging.seudominio.com"
```

### Produção (.env.production - na Vercel)
```bash
DATABASE_URL="postgresql://...neon.tech/pedidosimples_prod"
NEXTAUTH_SECRET="prod-secret-unico-forte"
NEXTAUTH_URL="https://pedidosimples.com"
WHATSAPP_API_TOKEN="token-prod"
WHATSAPP_PHONE_NUMBER_ID="id-prod"
WHATSAPP_WEBHOOK_VERIFY_TOKEN="verify-prod-unico"
RESEND_API_KEY="re_...prod"
EMAIL_FROM="noreply@pedidosimples.com"
```

## Checklist de Configuração por Ambiente

| Item | Local | Staging | Produção |
|------|-------|---------|----------|
| Banco de dados | PostgreSQL local ou Neon dev branch | Neon staging branch | Neon production branch |
| WhatsApp Webhook URL | ngrok/Cloudflare Tunnel → `/api/webhooks/whatsapp` | `https://staging.../api/webhooks/whatsapp` | `https://pedidosimples.com/api/webhooks/whatsapp` |
| NextAuth URL | `http://localhost:3000` | `https://staging...` | `https://pedidosimples.com` |
| Segredos no Git | **NUNCA** | Vercel Environment Variables | Vercel Environment Variables |
| SSL/HTTPS | localhost (HTTP OK) | HTTPS (Vercel) | HTTPS (Vercel + domínio) |

## Comandos de Verificação por Ambiente

```bash
# Desenvolvimento
pnpm dev                    # Roda next dev
pnpm build                  # Build produção (testa tipos + build)
pnpm lint                   # ESLint
pnpm typecheck              # tsc --noEmit
pnpm test                   # Vitest (unit/integration)
pnpm test:e2e               # Playwright (precisa dev rodando)
pnpm db:studio              # Prisma Studio
pnpm db:migrate:dev         # Migrações dev

# Staging/Produção (Vercel faz build automático)
# Verificar: Build logs, Function logs, Analytics
```

## Segredos - Regras Obrigatórias

- [ ] **NUNCA** commitar `.env.local`, `.env.*.local` ou qualquer segredo real
- [ ] `.env.example` no git com placeholders (sem valores reais)
- [ ] Segredos de staging/prod **apenas** nas Environment Variables da plataforma (Vercel, Railway, etc.)
- [ ] Rotacionar segredos se expostos acidentalmente
- [ ] `NEXTAUTH_SECRET` único por ambiente (gerar: `openssl rand -base64 32`)
```

### Contexto para IA (CLAUDE.md na raiz do projeto)

```markdown
# CLAUDE.md — Contexto do Projeto PedidosSimples

> Para Claude Code: Este arquivo dá contexto completo para você trabalhar neste projeto eficientemente.

---

## Visão Rápida
**Produto:** PedidosSimples - Gerenciador de pedidos WhatsApp para micro-lojas
**Stack:** Next.js 14 (App Router) + TypeScript + Tailwind + Prisma + NextAuth
**Deploy:** Vercel + Neon PostgreSQL
**MVP:** Receber pedidos WhatsApp → Lista dashboard → Atualizar status → Responder cliente

---

## Como Rodar
```bash
pnpm dev          # Desenvolvimento (localhost:3000)
pnpm build        # Build produção
pnpm lint         # ESLint
pnpm typecheck    # TypeScript check
pnpm test         # Vitest unit/integration
pnpm test:e2e     # Playwright e2e
pnpm db:studio    # Prisma Studio
pnpm db:migrate:dev # Migrações dev
```

---

## Estrutura de Pastas (src/)
```
app/
├── (auth)/           # Login, register, magic link
├── (dashboard)/      # Rotas protegidas
│   ├── pedidos/      # Lista, [id], novo
│   ├── historico/
│   └── configuracoes/
├── api/
│   ├── webhooks/whatsapp/
│   ├── pedidos/
│   └── auth/
components/
├── ui/               # Atoms (shadcn-based): Button, Input, Badge, Card, Modal, etc.
├── pedidos/          # Molecules/Organisms: OrderCard, StatusTabs, ReplyModal, etc.
├── layout/           # Header, Footer, PageWrapper
lib/
├── prisma.ts         # Prisma singleton
├── auth.ts           # NextAuth config
├── whatsapp.ts       # WhatsApp API client
├── validations/      # Zod schemas (shared client/server)
└── utils.ts
hooks/                # Custom hooks (usePedidos, useWebSocket, etc.)
types/                # Types globais
styles/               # globals.css + tailwind.css
```

---

## Convenções Obrigatórias

### TypeScript
- `strict: true` no tsconfig
- Tipos explícitos em props de componentes e APIs
- Zod schemas para TODA validação (input API, formulários, webhooks)
- Types compartilhados em `types/` ou `lib/validations/`

### Componentes
- Base: shadcn/ui (copiado em `components/ui/`) — adaptar tokens, não reescrever
- Padrão: `components/{domain}/{component-name}.tsx`
- Props interface nomeada: `ComponentNameProps`
- Export default + named export types
- `forwardRef` para componentes base (Button, Input)
- Server Components por default; `'use client'` só quando necessário (interatividade, hooks)

### API Routes
- Padrão: `app/api/{resource}/route.ts` (list/create) + `app/api/{resource}/[id]/route.ts` (get/update/delete)
- Validação: Zod schema no início do handler
- Erros: `return NextResponse.json({ error: "mensagem" }, { status })`
- Sucesso: `return NextResponse.json(data, { status: 201/200 })`
- Auth: `getServerSession(authOptions)` no topo de rotas protegidas

### Banco (Prisma)
- Schema em `prisma/schema.prisma`
- Models em PascalCase singular (`User`, `Order`, `Product`)
- Campos em camelCase (`createdAt`, `phoneNumber`)
- Relations explícitas com `@relation`
- Indexes em campos de busca/filtro frequentes
- `prisma migrate dev` para migrações dev
- `prisma generate` após schema change

### Testes
- **Vitest:** Unit/Integration em `**/*.test.ts` ao lado do código
- **Playwright:** E2E em `e2e/**/*.spec.ts`
- Fixtures em `test/fixtures/`
- Mocks: MSW para API externa (WhatsApp), Prisma mock para unit
- Cobertura alvo: >80% em APIs e utils críticos

### Git/Commits
- Conventional Commits: `feat:`, `fix:`, `refactor:`, `test:`, `chore:`, `docs:`
- Branch: `feature/{id}-{descricao}`, `fix/{id}-{descricao}`
- PR: Título convencional, descrição + checklist DoD
- Pre-commit: Husky + lint-staged (lint + format + typecheck)

---

## Padrões Importantes (Para IA Seguir)

### Validação Zod Compartilhada
```typescript
// lib/validations/order.ts
export const createOrderSchema = z.object({
  customerName: z.string().min(1).max(100),
  customerPhone: z.string().regex(/^\+?[1-9]\d{1,14}$/),
  items: z.array(z.object({
    name: z.string().min(1),
    quantity: z.number().int().positive(),
    unitPriceCents: z.number().int().nonnegative().optional(),
  })).min(1),
  notes: z.string().optional(),
});
// Uso: API (server) + Form (client) = mesmo schema
```

### Error Handling API
```typescript
// lib/api-error.ts
export class ApiError extends Error {
  constructor(public status: number, message: string, public code?: string) {
    super(message);
  }
}

// No handler:
try {
  const data = await validateAndProcess(schema, request);
  return NextResponse.json(data, { status: 201 });
} catch (e) {
  if (e instanceof z.ZodError) return jsonError(400, "Dados inválidos", e.flatten());
  if (e instanceof ApiError) return jsonError(e.status, e.message, e.code);
  console.error(e); return jsonError(500, "Erro interno");
}
```

### Server Actions (Quando Usar)
- Mutations simples ligadas a formulários (Server Components)
- Revalidam cache automaticamente (`revalidatePath`, `revalidateTag`)
- Não substituem API Routes para webhooks, endpoints externos, SSE

### Tempo Real (SSE v1)
- `app/api/pedidos/stream/route.ts` — SSE endpoint
- Client: `EventSource` em hook `usePedidosStream`
- Reconexão automática com backoff
- Fallback: polling se SSE falhar

---

## O que NÃO Fazer
- ❌ Não use `any` — tipifique corretamente
- ❌ Não coloque lógica de negócio em componentes — use `lib/` ou Server Actions
- ❌ Não faça fetch direto no cliente para APIs internas — use Server Components ou Server Actions
- ❌ Não commite segredos — use `.env.local` (local) / Vercel Env (prod)
- ❌ Não crie componentes duplicados — verifique `components/ui/` e `components/{domain}/` primeiro
- ❌ Não ignore TypeScript errors — `// @ts-ignore` só com justificativa em comentário
- ❌ Não pule testes — DoD exige testes passando

---

## Referências Rápidas
- Requisitos: `.claude/context/requirements.md`
- Componentes: `.claude/context/components.md`
- API Patterns: `.claude/context/api-patterns.md`
- DB Patterns: `.claude/context/db-patterns.md`
- Testing: `.claude/context/testing.md`
- ADRs: `../03 - Arquitetura/Decisões de Arquitetura (ADR).md`
- Fluxos UX: `../04 - UX-UI/Fluxos do Usuário.md`
- Inventário UI: `../04 - UX-UI/Inventário de Telas.md`
- Componentes UI: `../04 - UX-UI/Registro de Componentes e Padrões.md`
```

## Como trabalhar com IA nesta etapa

**Prompt para gerar setup inicial (package.json, configs):**
```
Stack: Next.js 14 App Router + TypeScript + Tailwind + Prisma + NextAuth + Zod + Vitest + Playwright + ESLint/Prettier + Husky.
Gere:
1. package.json com dependencies/devDependencies corretas (versões compatíveis)
2. tsconfig.json (strict, paths @/*, nextjs plugin)
3. tailwind.config.ts (tokens da ETAPA 07, plugins forms/typography)
4. eslint.config.mjs (flat config, next, typescript, prettier, tailwind)
5. prettier.config.mjs
6. .env.example
7. prisma/schema.prisma (base do modelo da ETAPA 05)
8. next.config.mjs (experimental: serverActions, images domains)
9. .husky/pre-commit (lint-staged)
10. .github/workflows/ci.yml (lint, typecheck, test, build)
Formato: arquivos completos prontos para salvar.
```

**Prompt para gerar Plano de Implementação:**
```
Tenho: Inventário de Telas (ETAPA 08), Registro de Componentes (ETAPA 08), Arquitetura (ETAPA 05), Requisitos (ETAPA 04).
Gere Plano de Implementação com:
- Fases lógicas (fundação → core → features → polimento)
- Tasks granulares (1-3h cada) com ID, descrição, estimativa, dependência, DoD
- Ordem sugerida (vertical slices)
- Definition of Done padrão
- Riscos e mitigações
- Arquivos de contexto para IA a criar
Formato: Markdown tabelas prontas para copiar no modelo.
```

**Prompt para gerar CLAUDE.md (contexto IA):**
```
Com base em todo o planejamento (ETAPAs 01-08), gere o CLAUDE.md completo para a raiz do projeto.
Inclua: visão rápida, como rodar, estrutura pastas, convenções TS/componentes/API/DB/testes/git, padrões importantes (Zod, error handling, server actions, SSE), o que NÃO fazer, referências rápidas.
Formato: Markdown pronto para salvar como CLAUDE.md na raiz do projeto.
```

**Contexto a fornecer:** Arquitetura (ETAPA 05) + UI/Componentes (ETAPA 08) + Requisitos (ETAPA 04) + Stack decidida.
**Resultado esperado:** Projeto rodando localmente (`pnpm dev`), build passando, CI configurado, CLAUDE.md na raiz, Plano de Implementação aprovado.
**Como verificar:** `pnpm dev` abre app? `pnpm build` passa? `pnpm lint` + `pnpm typecheck` + `pnpm test` passam? CI roda no push?

## Checklist de execução

- [ ] Ambiente local: Node 20+, pnpm, VS Code + extensões configuradas
- [ ] Repositório Git: init, remote, branch main, commit convencional
- [ ] Estrutura inicial do projeto criada (pastas, configs base)
- [ ] Dependências instaladas (runtime + dev)
- [ ] `.env.local` preenchido com valores de dev
- [ ] `.env.example` no git com placeholders
- [ ] Segredos staging/prod configurados na plataforma (Vercel)
- [ ] Prisma: schema sincronizado, migração dev rodando, studio abre
- [ ] NextAuth: login/logout funciona, rotas protegidas
- [ ] Componentes base (shadcn/ui adaptados) implementados e no Storybook
- [ ] Layout base (Header, Nav, PageWrapper) responsivo funcionando
- [ ] CI/CD: GitHub Actions rodando lint + typecheck + test + build no push
- [ ] Estratégia de testes definida (vitest + playwright, cobertura alvo)
- [ ] Estratégia de erros definida (ApiError, error boundaries, logging)
- [ ] Convenções documentadas e aprovadas
- [ ] Tarefas de implementação organizadas (Plano de Implementação)
- [ ] DoD por feature definido e aprovado
- [ ] Contexto para IA criado (CLAUDE.md + arquivos .claude/context/)
- [ ] Verificação completa: `pnpm dev` + `pnpm build` + `pnpm lint` + `pnpm typecheck` + `pnpm test` + `pnpm test:e2e` (smoke) — TODOS PASSAM
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Ambiente 100% funcional localmente
- [ ] Build de produção passa sem erros
- [ ] CI/CD passa no push
- [ ] Plano de Implementação aprovado por você
- [ ] CLAUDE.md e contexto IA completos na raiz do projeto
- [ ] Você consegue iniciar a primeira task de implementação (T1.1) amanhã
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste do ambiente limpo:** Clone o repo em outra pasta → `pnpm install` → `pnpm dev` → funciona?
- **Teste do build:** `pnpm build` passa sem warnings/errors?
- **Teste do CI:** Push para branch → GitHub Actions passa?
- **Teste do contexto IA:** Abre Claude Code na pasta → lê CLAUDE.md → consegue sugerir próxima task?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Pular verificação `pnpm build` antes de commit | Pre-commit hook OBRIGATÓRIO com build/typecheck |
| Segredos no git | `.env.local` no `.gitignore` SEMPRE. Audite: `git ls-files | grep .env` |
| Prisma client não gera | `postinstall: "prisma generate"` no package.json |
| NextAuth não funciona em produção | `NEXTAUTH_URL` correto em cada ambiente, `NEXTAUTH_SECRET` único por env |
| Componentes shadcn não adaptados aos tokens | Copie → adapte cores/tokens → teste visual. Não use "como veio". |
| Plano de implementação vago | Tasks = 1-3h, DoD explícito, dependências claras |
| Contexto IA genérico | CLAUDE.md específico do SEU projeto (stack, convenções, padrões) |

## Se algo der errado

| Situação | Ação |
|----------|------|
| `pnpm build` falha com erro obscuro | Leia erro completo. Comum: types, import path, config next. Corrija na fonte. |
| CI falha mas local passa | Verifique Node version, cache, env vars no CI. Rode `pnpm install --frozen-lockfile` local. |
| Prisma migrate falha | `prisma migrate reset` (dev only) ou `prisma db push` para prototipar. |
| NextAuth loop redirect | Verifique `callbacks.authorized`, `pages.signIn`, middleware matcher. |
| Storybook não mostra componentes | Configure `stories: ['../components/**/*.stories.@(js|jsx|ts|tsx)']`. |
| Contexto IA não ajuda | Atualize CLAUDE.md com padrão novo que surgiu. IA só sabe o que você documenta. |

## Condições para avançar

- ✅ `pnpm dev`, `pnpm build`, `pnpm lint`, `pnpm typecheck`, `pnpm test` — TODOS PASSAM
- ✅ CI/CD passando no GitHub Actions
- ✅ Plano de Implementação aprovado
- ✅ CLAUDE.md + contexto IA completos
- ✅ Próxima task (T1.1) clara e pronta para começar

## Próxima etapa

[[ETAPA 10 - Desenvolver o Software por Partes]]

## Materiais relacionados

- [[02 - Modelos/Desenvolvimento/Plano de Implementação]]
- [[02 - Modelos/Desenvolvimento/Configuração de Ambientes]]
- [[02 - Modelos/Desenvolvimento/Contexto para IA]]
- [[03 - Guias Práticos/Vibe Coding/Como Preparar Contexto para IA]]
- [[03 - Guias Práticos/Vibe Coding/Fluxo de Trabalho por Feature]]
- [[04 - Referência/Glossário/CI/CD]]
- [[04 - Referência/Glossário/Definition of Done]]