# Checklist de Código

> **Copie para:** `05 - Projetos/SeuProjeto/05 - Preparação e Desenvolvimento/Checklist de Código.md`
> Baseado na ETAPA 10. **Checklist obrigatório para todo PR** - qualidade, segurança, performance, acessibilidade.

---

## Como Usar

1. **Automatizado:** CI roda lint, typecheck, testes, build
2. **Revisor:** Marca cada item ao revisar PR
3. **Autor:** Garante que passou antes de pedir review
4. **Merge:** Só com ✅ em todos os itens aplicáveis

---

## ✅ Checklist por Categoria

### TypeScript & Types
- [ ] **Zero `any`** - Use `unknown`, generics, type guards
- [ ] **Zero `@ts-ignore`** - Se precisar, documente *por que* em comentário
- [ ] **Strict mode** - `strict: true` no tsconfig (já configurado)
- [ ] **Types compartilhados** - Em `src/types/` ou `prisma/zod` generated
- [ ] **Props interfaces** - `ComponentProps` extends `BaseComponentProps`
- [ ] **Discriminated unions** - Para variantes (ex: `status: 'novo' | 'preparando'`)
- [ ] **Utility types** - `Pick`, `Omit`, `Partial`, `Required` ao invés de duplicar

### React & Next.js
- [ ] **Server Components por padrão** - Só `'use client'` quando necessário (interatividade, hooks, browser API)
- [ ] **Server Actions para mutações** - Não API Routes para CRUD
- [ ] **Suspense boundaries** - Em volta de componentes async (lista, detalhe)
- [ ] **Error boundaries** - Página `/error.tsx` + component-level onde faz sentido
- [ ] **Loading states** - `loading.tsx` + skeletons componentes
- [ ] **Metadata** - `generateMetadata` em páginas públicas (SEO, OG)
- [ ] **Dynamic params** - `generateStaticParams` se SSG/ISR
- [ ] **Revalidação** - `revalidatePath` / `revalidateTag` em Server Actions
- [ ] **Image optimization** - `next/image` com `sizes`, `priority` acima da dobra
- [ ] **Fonts** - `next/font` (Inter, JetBrains Mono) - self-hosted

### Prisma & Database
- [ ] **Queries otimizadas** - `select` / `include` só campos necessários
- [ ] **Sem N+1** - `include` relations em vez de loop queries
- [ ] **Índices** - Verificar `@@index` no schema para queries frequentes
- [ ] **Transações** - `prisma.$transaction` para operações atômicas (status + historico)
- [ ] **Soft delete** - Middleware `deletedAt` aplicado automaticamente
- [ ] **Migrações** - `prisma migrate dev` (dev) / `prisma migrate deploy` (CI)
- [ ] **Connection pooling** - `pgbouncer=true` na connection string
- [ ] **Prepared statements** - Habilitado por default no Prisma

### Validação (Zod)
- [ ] **Schema por input externo** - Webhooks, Server Actions, Forms, API Routes
- [ ] **Coerção** - `z.coerce.number()`, `z.coerce.date()` para query params
- [ ] **Refinamentos** - `refine` para regras de negócio (ex: transição status)
- [ ] **Error messages** - Em português, acionáveis ("Status inválido: não pode voltar de ENTREGUE para PREPARANDO")
- [ ] **Types inferidos** - `z.infer<typeof schema>` para tipagem automática

### Segurança
- [ ] **Auth check** - `getServerSession()` + `requireRole()` em Server Actions
- [ ] **Rate limit** - Middleware (auth: 30/min/IP, API: 100/min/user)
- [ ] **CSRF** - NextAuth built-in + `csrfToken` em forms customizados
- [ ] **XSS** - Sem `dangerouslySetInnerHTML`, sanitização se necessário
- [ ] **Headers** - CSP, HSTS, X-Frame-Options, Referrer-Policy no `next.config.js`
- [ ] **Secrets** - Zero no código, só em env (Vercel/Neon)
- [ ] **HMAC** - Webhook WhatsApp validado 100%
- [ ] **SQL Injection** - Prisma ORM (parametrizado), sem raw SQL não sanitizado
- [ ] **LGPD** - Consentimento, anonimização, exclusão, retenção

### Performance
- [ ] **Bundle size** - `next-bundle-analyzer` < 200KB gzipped (first load JS)
- [ ] **Queries** - `EXPLAIN ANALYZE` nas queries críticas (< 50ms p95)
- [ ] **Cache** - `fetch` com `next: { revalidate }` ou `unstable_cache`
- [ ] **Images** - WebP/AVIF, `sizes`, `priority`, `placeholder="blur"`
- [ ] **Fonts** - `display: swap`, preload, subset
- [ ] **Third-party scripts** - `next/script` com `strategy="lazyOnload"`
- [ ] **SSE** - Heartbeat 30s, cleanup aba hidden > 5min, conexões monitoradas
- [ ] **Database** - Connection pooling, índices, prepared statements

### Acessibilidade (WCAG 2.1 AA)
- [ ] **Semantic HTML** - `header`, `main`, `nav`, `section`, `article`, `footer`
- [ ] **Headings** - Hierarquia h1→h2→h3 (sem pular)
- [ ] **Labels** - Todo input tem `<label htmlFor>` ou `aria-label`
- [ ] **Focus visible** - `focus-visible:ring-2 focus-visible:ring-primary` em TUDO interativo
- [ ] **Focus order** - DOM order = visual order
- [ ] **ARIA** - `role`, `aria-*` em componentes custom (modal, tabs, select, toast)
- [ ] **Live regions** - `aria-live="polite"` (toasts), `"assertive"` (erros)
- [ ] **Contraste** - 4.5:1 texto, 3:1 UI (ver Design Tokens)
- [ ] **Touch targets** - Mínimo 44×44px (mobile)
- [ ] **Reduced motion** - `@media (prefers-reduced-motion)` respeitado
- [ ] **Zoom 200%** - Layout não quebra, sem scroll horizontal
- [ ] **Keyboard** - Tab, Shift+Tab, Enter, Space, Arrows, Esc funcionam

### Testes
- [ ] **Unit (Vitest)** - Funções puras, validadores, utils, parsers (≥80% coverage)
- [ ] **Integration (Vitest + Test DB)** - Server Actions, Prisma middleware, SSE broadcast
- [ ] **E2E (Playwright)** - Fluxos core: login, onboarding, pedido→entrega, relatórios
- [ ] **A11y (Playwright + axe)** - 0 critical/serious violations
- [ ] **Visual regression** - (Opcional) Chromatic / Playwright screenshots
- [ ] **Test data** - Factories/fixtures, não hardcoded em testes
- [ ] **Isolamento** - Cada teste independente, cleanup automático

### Git & Commits
- [ ] **Branch name** - `feat/short-description`, `fix/...`, `chore/...`
- [ ] **Commits atômicos** - Uma mudança lógica por commit
- [ ] **Mensagem convencional** - `tipo(escopo): descrição curta`
  - `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `ci`
- [ ] **Descrição** - O que + por que (não como)
- [ ] **Issues vinculadas** - `Closes #123` ou `Refs #123`
- [ ] **Sem WIP** - Não merge work in progress
- [ ] **Rebase** - Histórico linear (squash and merge)

### Documentação
- [ ] **JSDoc** - Funções públicas, Server Actions, hooks, types complexos
- [ ] **README** - Atualizado se novos comandos, env vars, setup
- [ ] **ADR** - Nova decisão arquitetural = novo ADR
- [ ] **Changelog** - `CHANGELOG.md` (Keep a Changelog format)
- [ ] **Comentários** - *Por que*, não *o que* (código já mostra o que)

---

## Checklist por Tipo de PR

### Feature Nova
- [ ] Todos itens gerais acima
- [ ] RFs/RNFs vinculados na descrição do PR
- [ ] Wireframe/Design seguido
- [ ] Estados: loading, empty, error, success
- [ ] Testes: unit + integration + e2e (happy + edge + error)
- [ ] Acessibilidade validada
- [ ] Performance validada (Lighthouse CI)

### Bug Fix
- [ ] Reprodução documentada (passos, esperado vs atual)
- [ ] Teste de regressão adicionado (unit ou e2e)
- [ ] Fix mínimo (não refatorar junto)
- [ ] Root cause identificado no commit message

### Refatoração
- [ ] Comportamento idêntico (testes passam sem mudança)
- [ ] Motivo claro (performance, legibilidade, manutenibilidade)
- [ ] Sem mudança de API pública (ou breaking documentado)

### Atualização Dependência
- [ ] Changelog da lib lido
- [ ] Breaking changes testados
- [ ] Lockfile atualizado
- [ ] Build + testes passam

---

## Ferramentas de Apoio (Automatize)

```json
// package.json scripts
{
  "lint": "next lint",
  "typecheck": "tsc --noEmit",
  "test": "vitest run --coverage",
  "test:e2e": "playwright test",
  "test:a11y": "playwright test --grep @a11y",
  "build": "next build",
  "db:push": "prisma db push",
  "db:migrate": "prisma migrate dev",
  "db:studio": "prisma studio",
  "check": "npm run lint && npm run typecheck && npm run test && npm run build",
  "prepare": "husky install"
}
```

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci
      - run: npm run check
      - run: npm run test:e2e
  # Opcional: Lighthouse CI, bundle size, etc.
```

```json
// .husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"
npx lint-staged
```

```json
// lint-staged.config.js
{
  "*.{ts,tsx,js,jsx}": ["eslint --fix", "prettier --write"],
  "*.{json,md,css}": ["prettier --write"]
}
```

---

## Definição de Pronto (Definition of Done) - Nível PR

> **Um PR só pode ser mergeado se TUDO abaixo for ✅**

| Critério | Verificação |
|----------|-------------|
| **Código** | Lint ✅, TypeCheck ✅, Build ✅ |
| **Testes** | Unit ✅ (≥80%), Integration ✅, E2E core ✅, A11y ✅ |
| **Segurança** | Auth ✅, Rate Limit ✅, Validação ✅, Secrets ✅ |
| **Acessibilidade** | Axe 0 critical/serious ✅, Teclado ✅, Screen Reader ✅ |
| **Performance** | Bundle ✅, Queries ✅, Lighthouse Perf ≥ 90 ✅ |
| **UX** | Match wireframe ✅, Estados ✅, Responsivo ✅, Dark mode ✅ |
| **Documentação** | JSDoc ✅, README se necessário ✅, ADR se arquitetural ✅ |
| **Deploy** | Preview deployment ✅, Smoke test manual ✅ |
| **Review** | Aprovado por você (solo) ou revisor designado ✅ |

---

## Checklist Rápido (Daily - 2 min)

Antes de começar a codar:
- [ ] `git status` limpo? (ou changes em branch certa)
- [ ] `npm run dev` roda sem erro?
- [ ] Último `main` pullado? (`git pull origin main`)

Antes de commit:
- [ ] `npm run lint` passa?
- [ ] `npm run typecheck` passa?
- [ ] Testes relacionados passam?
- [ ] Mensagem commit convencional?

Antes de PR:
- [ ] `npm run check` passa tudo?
- [ ] `npm run test:e2e` core flows passam?
- [ ] Preview deployment funcionando?
- [ ] Descrição PR clara com RFs vinculados?

---

## Próxima Etapa
[[02 - Modelos/Testes/Estratégia de Testes]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/05 - Preparação e Desenvolvimento/ETAPA 10 - Desenvolver o Software por Partes]]
- [[02 - Modelos/Desenvolvimento/Plano de Implementação]]
- [[02 - Modelos/Desenvolvimento/Guia de Vibe Coding com IA]]
- [[02 - Modelos/Testes/Estratégia de Testes]]
- [[02 - Modelos/Testes/Plano de Testes]]
- [[02 - Modelos/Testes/Casos de Teste]]
- [[04 - Referência/Glossário/Definition of Done]]
- [[04 - Referência/Glossário/Conventional Commits]]
- [[04 - Referência/Glossário/Semantic Versioning]]