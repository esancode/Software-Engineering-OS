# Glossário Técnico

> **Local:** `04 - Referência/Glossário.md`
> **Objetivo:** Definições rápidas de termos usados no Software Engineering OS.

---

## A

**A11y (Accessibility)** — Acessibilidade digital. WCAG 2.1 AA é o padrão mínimo.

**ADR (Architecture Decision Record)** — Registro de decisão arquitetural. Formato: título, status, contexto, decisão, consequências.

**API (Application Programming Interface)** — Interface para comunicação entre sistemas. REST, GraphQL, Webhooks.

**Atomic Design** — Metodologia de design system: Átomos → Moléculas → Organismos → Templates → Páginas.

---

## B

**Backend for Frontend (BFF)** — Camada API otimizada para necessidades específicas do frontend.

**Branch (Git)** — Linha de desenvolvimento independente. `main` = production, `feat/*` = features, `fix/*` = bugs.

**Build** — Processo de compilar/empacotar código para produção. Next.js: `next build`.

**Bundle** — Arquivo(s) JavaScript/CSS resultante do build. Otimizar: code splitting, tree shaking.

---

## C

**CI/CD (Continuous Integration / Continuous Deployment)** — Integração e deploy contínuos. GitHub Actions + Vercel.

**CLI (Command Line Interface)** — Interface de linha de comando. Ex: `vercel`, `npm`, `gh`.

**CSP (Content Security Policy)** — Header HTTP que previne XSS controlando fontes permitidas.

**Cuidado (Care)** — Princípio: código legível, testado, documentado. "Deixe melhor do que encontrou".

---

## D

**DAU/WAU/MAU** — Daily/Weekly/Monthly Active Users. Métricas de engajamento.

**Dead Letter Queue (DLQ)** — Fila para mensagens que falharam processamento após retries.

**Deploy** — Colocar código em produção. Vercel: automático no merge `main`.

**Design Tokens** — Valores de design (cores, espaçamento, tipografia) como variáveis reutilizáveis.

**DPIA (Data Protection Impact Assessment)** — Avaliação de impacto à proteção de dados (LGPD Art. 38).

**DRY (Don't Repeat Yourself)** — Princípio: evitar duplicação. Abstrair lógica comum.

---

## E

**E2E (End-to-End)** — Testes que simulam usuário real no navegador. Playwright.

**Edge Computing** — Execução próxima ao usuário (Vercel Edge Functions, Cloudflare Workers).

**Event Sourcing** — Armazenar mudanças como sequência de eventos. Útil para auditoria.

**Eventual Consistency** — Modelo onde consistência é atingida após tempo finito. Comum em distributed systems.

---

## F

**Feature Flag** — Toggle para ligar/desligar funcionalidade sem deploy. Vercel Edge Config, LaunchDarkly.

**Flaky Test** — Teste que passa/falha intermitentemente sem mudança de código. Corrigir ou quarentenar.

**Frontend** — Camada de apresentação (React, Next.js, Tailwind).

---

## G

**Gherkin** — Linguagem para critérios de aceitação: `Given / When / Then`.

**GitOps** — Operações via Git. Deploy = merge em `main`. Infra como código.

**Glossário** — Este arquivo. Fonte única de verdade para termos do projeto.

---

## H

**HMAC (Hash-based Message Authentication Code)** — Assinatura criptográfica para validar integridade de webhook.

**Hotfix** — Correção urgente em production. Branch `hotfix/*` → `main` → tag patch.

**HSTS (HTTP Strict Transport Security)** — Header que força HTTPS. `max-age=63072000`.

---

## I

**Idempotência** — Operação que produz mesmo resultado se executada múltiplas vezes. Essencial para webhooks.

**Infra as Code (IaC)** — Infraestrutura versionada e automatizada. Terraform, Vercel config.

**Integration Test** — Teste que verifica integração entre componentes (API + DB, Server Action + Prisma).

---

## J

**JWT (JSON Web Token)** — Token stateless para autenticação. NextAuth usa JWT + refresh rotation.

---

## K

**k6** — Ferramenta de load testing. Scripts em JavaScript, métricas detalhadas.

---

## L

**LGPD (Lei Geral de Proteção de Dados)** — Lei brasileira 13.709/2018. Equivalente ao GDPR.

**Lighthouse** — Auditoria automatizada (Performance, A11y, SEO, Best Practices). CI: `lhci`.

**Load Test** — Testar sistema sob carga esperada. k6, artillery.

**Localhost** — Ambiente de desenvolvimento local. `npm run dev`.

---

## M

**Magic Link** — Autenticação sem senha: link único enviado por email. NextAuth provider.

**Mermaid** — Diagramas em markdown. Usado em ADRs, docs técnicos.

**Middleware (Next.js)** — Código que roda antes da requisição. Auth, redirects, headers, geo.

**Migration (DB)** — Mudança versionada no schema. Prisma: `prisma migrate dev`.

**Mock** — Simular dependência externa em testes. `vi.mock()`, MSW.

**Monorepo** — Múltiplos projetos em um repo. Turborepo, Nx.

**MoSCoW** — Priorização: Must / Should / Could / Won't.

**MVP (Minimum Viable Product)** — Versão mínima que entrega valor validador. "Processar 10 pedidos sem ajuda".

---

## N

**Next.js** — React framework full-stack. App Router, Server Components, Server Actions.

**Neon** — PostgreSQL serverless. Branch-based, PITR, autoscaling.

**NFR (Non-Functional Requirement)** — Requisito não-funcional: performance, segurança, usabilidade.

**No-Go** — Critério que bloqueia release. Ex: teste @smoke falha, vuln critical.

---

## O

**Observability** — Capacidade de entender estado interno via outputs: logs, métricas, traces.

**OWASP Top 10** — Top 10 vulnerabilidades web. Referência para security review.

---

## P

**PITR (Point-in-Time Recovery)** — Recuperação de banco para qualquer momento. Neon: contínuo.

**Playwright** — Framework E2E testing. Multi-browser, auto-wait, trace viewer.

**Poka-yoke** — Prevenção de erro por design. Ex: type safety, Zod validation.

**Post-Mortem** — Análise blameless pós-incidente. 5 Porquês, action items.

**PWA (Progressive Web App)** — Web app instalável, offline-capable. Service Worker, Manifest.

---

## Q

**Quality Gate** — Critério automatizado para bloquear merge. CI: testes, lint, coverage, security.

---

## R

**RAG (Red/Amber/Green)** — Status visual: 🔴 Crítico, 🟡 Atenção, 🟢 OK.

**RBAC (Role-Based Access Control)** — Controle de acesso por roles. NextAuth: `user.role`.

**RICE (Reach × Impact × Confidence / Effort)** — Framework de priorização quantitativa.

**RLS (Row Level Security)** — Segurança no nível da linha (PostgreSQL). Isolamento multi-tenant.

**RTO (Recovery Time Objective)** — Tempo máximo para restaurar serviço após incidente.

**RPO (Recovery Point Objective)** — Perda máxima de dados aceitável (tempo).

**Runbook** — Procedimento operacional padronizado para incidentes rotineiros.

---

## S

**SAST (Static Application Security Testing)** — Análise estática de código. Semgrep, CodeQL.

**SSE (Server-Sent Events)** — Conexão unidirecional server→client para tempo real. Mais simples que WebSocket.

**SLA (Service Level Agreement)** — Contrato de nível de serviço. Ex: 99.9% uptime.

**SLO (Service Level Objective)** — Target interno de confiabilidade. Ex: 99.95% uptime.

**SLI (Service Level Indicator)** — Métrica mensurável. Ex: uptime %, latency p95.

**Scope Creep** — Expansão não controlada do escopo. Prevenção: Won't list explícito, change control.

**SemVer (Semantic Versioning)** — `MAJOR.MINOR.PATCH`. Breaking / Feature / Fix.

**Server Action** — Função server-side chamada do client. Next.js 14+. Mutations seguras.

**Server Component** — React component que roda só no server. Default no App Router.

**Smoke Test** — Teste rápido de sanidade. "O sistema liga?" @smoke tag no Playwright.

**SOLID** — 5 princípios OO: Single Responsibility, Open/Closed, Liskov, Interface Segregation, Dependency Inversion.

**Sprint** — Iteração de tempo fixo (2 semanas). Planning → Review → Retro.

**SSO (Single Sign-On)** — Login único para múltiplos sistemas. SAML, OIDC.

**Storybook** — Catálogo de componentes isolados. Visual testing, docs.

---

## T

**Tailwind CSS** — Utility-first CSS. Design tokens via `@theme` ou CSS variables.

**Tech Debt (Dívida Técnica)** — Comprometimento qualidade por velocidade. Registrar, priorizar, pagar.

**Test Pyramid** — Unit (base) → Integration → E2E (topo). 70/20/10.

**Tracing** — Rastreamento de requisição através de serviços. OpenTelemetry, Vercel.

**Truflehog** — Scanner de secrets no código/git history. CI obrigatório.

**TypeScript** — JavaScript com tipagem estática. `strict: true` obrigatório.

---

## U

**Unit Test** — Teste de unidade isolada (função, hook, util). Vitest + Testing Library.

**Upptime** — Uptime monitor gratuito via GitHub Actions. Status page automática.

**Upsert** — Insert ou Update (ON CONFLICT). Prisma: `upsert()`.

---

## V

**Vercel** — Platform para Next.js. Deploy, Edge, Functions, Analytics, Blob, Cron.

**Vibe Coding** — Desenvolvimento assistido por IA (Claude Code). Humano arquiteta, IA executa.

**Vitest** — Test runner rápido, compatível Jest. Unit + Integration. Coverage nativo.

---

## W

**WAF (Web Application Firewall)** — Proteção contra ataques web. Vercel WAF gerenciado + custom rules.

**Webhook** — HTTP callback para notificação assíncrona. WhatsApp, Stripe, Resend.

**Won't (MoSCoW)** — Explícitamente fora do escopo. Documentado para evitar scope creep.

---

## X

**XSS (Cross-Site Scripting)** — Injeção de script malicioso. Prevenção: CSP, React auto-escape, DOMPurify.

---

## Y

**YAGNI (You Aren't Gonna Need It)** — Não implemente até precisar. Evita over-engineering.

---

## Z

**Zod** — Schema validation TypeScript-first. Parse + type inference. Obrigatório em inputs.

**Zero-Downtime Deploy** — Deploy sem interrupção. Vercel gerencia via rolling deploy.

---

## Referências Cruzadas

| Termo | Onde Aparece |
|-------|--------------|
| ADR | [[02 - Modelos/Requisitos e Arquitetura/ADR - Registro de Decisões Arquiteturais]] |
| Atomic Design | [[02 - Modelos/UX e UI/Especificação de Componentes]] |
| Feature Flags | [[03 - Guias Práticos/Guia de Publicação]] |
| Gherkin | [[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]] |
| LGPD | [[02 - Modelos/Segurança e Validação/Checklist de Segurança]] |
| MoSCoW | [[02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades]] |
| RICE | [[02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades]] |
| RLS | [[03 - Guias Práticos/Guia de Segurança]] |
| Runbook | [[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]] |
| SemVer | [[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]] |
| Test Pyramid | [[02 - Modelos/Testes/Estratégia de Testes]] |
| Vibe Coding | [[03 - Guias Práticos/Guia de Vibe Coding]] |