# ETAPA 16 — Encerrar ou Transferir o Projeto

## Objetivo

Garantir que o projeto possa ser **compreendido, mantido e evoluído no futuro** — seja por você daqui a 6 meses, por outro desenvolvedor, ou por uma equipe.

## Por que esta etapa existe?

Projetos "acabados" sem documentação de continuidade viram **caixas pretas**: ninguém sabe como rodar, como fazer deploy, onde estão os segredos, por que decisões foram tomadas. Esta etapa evita o abandono técnico.

## Quando começar

- Você **decide** encerrar o projeto (fim de vida, migração, perda de interesse)
- OU precisa **transferir** para outra pessoa/equipe
- OU quer deixar **tudo documentado** para retomada futura (mesmo que continue mantendo)

> [!note]
> Esta etapa NÃO é automática após a 15. Ela é acionada por **decisão consciente** de encerrar/transferir.

## O que você precisa fazer

1. **Organize a documentação final** — reúna todos os documentos das etapas 00-15
2. **Registre tecnologias utilizadas** — versões exatas, por que escolheu, alternativas
3. **Registre decisões importantes** — ADRs consolidados, trade-offs, arrependimentos
4. **Documente instalação e execução local** — passo a passo do zero
5. **Documente processo de deploy** — ambientes, variáveis, comandos, rollback
6. **Liste configurações necessárias** — DNS, serviços externos, webhooks, segredos
6. **Liste dependências externas** — APIs, serviços, contratos, contatos
7. **Documente limitações conhecidas** — technical debt, bugs conhecidos, workarounds
8. **Liste pendências e roadmap não feito** — o que ficaria para v2
9. **Registre informações de manutenção** — rotinas, monitoramento, backup, alertas
10. **Verifique acessos e credenciais** — quem tem acesso a quê, rotação necessária
11. **Organize repositórios** — branches, tags, releases, wiki, issues
12. **Registre estado final do projeto** — versão, data, status, lições aprendidas
13. **Produza os documentos: Documentação de Continuidade + Checklist de Encerramento/Transferência**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Decidir encerrar/transferir | 👤 USUÁRIO (decisão de negócio) |
| Compilar documentação final | 👤 USUÁRIO + 🤖 IA (organiza) |
| Documentar instalação/execução | 🤖 IA (gera do CLAUDE.md + configs) |
| Documentar deploy/operações | 🤖 IA (gera do Plano de Manutenção) |
| Verificar acessos/credenciais | 👤 USUÁRIO (auditoria real) |
| Organizar repositório | 👤 USUÁRIO + 🤖 IA (scripts) |
| Registrar lições aprendidas | 👤 USUÁRIO (reflexão pessoal) |
| Validar documentação (teste real) | ✅ VALIDAÇÃO (tente seguir os docs) |
| Assinar encerramento/transferência | 👤 USUÁRIO (+ recebedor se transferência) |

## Ferramentas recomendadas

- **Obsidian** — compilação final (este próprio cofre)
- **GitHub/GitLab** — repositório, wiki, releases, issues
- **1Password/Bitwarden** — compartilhamento seguro de credenciais
- **Notion/Confluence** — se transferir para equipe (wiki externa)
- **Scripts de verificação** — `pnpm install && pnpm dev` do zero

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Documentação de Continuidade | `02 - Modelos/Publicação e Manutenção/Documentação de Continuidade.md` | `05 - Projetos/SeuProjeto/09 - Encerramento/` |
| Checklist de Encerramento/Transferência | `02 - Modelos/Publicação e Manutenção/Checklist de Encerramento.md` | `05 - Projetos/SeuProjeto/09 - Encerramento/` |

### Condicionais
- Termo de Transferência (se transferir para terceiro)
- Inventário de Ativos (domínios, contas, assinaturas)

### Opcionais
- Apresentação de Encerramento (para stakeholders)
- Relatório de Lições Aprendidas (pessoal)

## Como produzir os documentos

### Documentação de Continuidade

```markdown
# Documentação de Continuidade — PedidosSimples

## Identificação do Projeto
| Campo | Valor |
|-------|-------|
| **Nome** | PedidosSimples |
| **Versão Final** | v1.2.3 (ou "v1.0.0 - encerrado") |
| **Status** | [ENCERRADO / EM MANUTENÇÃO MÍNIMA / TRANSFERIDO PARA X] |
| **Data Encerramento/Transferência** | 2027-03-15 |
| **Último Deploy Produção** | 2027-03-10 (v1.2.3) |
| **Mantenedor Original** | [Seu Nome] |
| **Novo Mantenedor (se transferência)** | [Nome/Equipe] |
| **Repositório** | https://github.com/seuusuario/pedidosimples |
| **Produção** | https://pedidosimples.com (Vercel) |
| **Banco** | Neon: `pedidosimples-prod` |

---

## 1. Tecnologias Utilizadas (Versões Exatas)

### Runtime & Framework
```json
{
  "node": "20.12.0",
  "pnpm": "9.1.0",
  "next": "14.2.3",
  "react": "18.3.1",
  "typescript": "5.4.5"
}
```

### Banco & ORM
```json
{
  "postgresql": "16.3 (Neon serverless)",
  "prisma": "5.13.0",
  "@prisma/client": "5.13.0"
}
```

### Autenticação & Segurança
```json
{
  "next-auth": "4.24.7",
  "bcryptjs": "2.4.3",
  "jose": "5.2.3",
  "arcjet": "1.0.0-beta"
}
```

### UI & Estilo
```json
{
  "tailwindcss": "3.4.3",
  "@radix-ui/*": "latest compatíveis",
  "lucide-react": "0.372.0",
  "clsx": "2.1.1",
  "tailwind-merge": "2.3.0"
}
```

### Testes & Qualidade
```json
{
  "vitest": "1.6.0",
  "@playwright/test": "1.44.0",
  "@testing-library/react": "14.2.0",
  "eslint": "8.57.0",
  "prettier": "3.2.5"
}
```

### Infra & Deploy
```json
{
  "vercel": "34.2.0 (CLI)",
  "pino": "9.0.0",
  "@sentry/nextjs": "7.110.0"
}
```

### Por que estas escolhas? (Resumo dos ADRs)
| Decisão | Escolha | Alternativa Rejeitada | Motivo Principal |
|---------|---------|----------------------|------------------|
| Framework | Next.js 14 App Router | Remix, Vite+React | Vercel nativo, RSC, maturidade |
| Banco | PostgreSQL + Prisma | MongoDB, SQLite, Supabase | Relacional, type-safe, serverless ready |
| Auth | NextAuth v4 | Clerk, Lucia, custom | Integrado Next.js, grátis, flexível |
| Tempo Real | SSE Nativo | Pusher, Ably, Socket.io | Zero custo, simples, suficiente v1 |
| Deploy | Vercel | Railway, Render, AWS | DX, preview deploys, edge functions |
| Estilo | Tailwind + shadcn/ui | Chakra, MUI, CSS Modules | Tokens, composição, bundle size |

---

## 2. Decisões Arquiteturais Importantes (Consolidado dos ADRs)

### ADR-001: Next.js App Router (Server Components First)
**Decisão:** Server Components por default, Client Components só para interatividade.
**Trade-off:** Curva de aprendizado RSC, mas performance e DX superiores.
**Arrependimento:** Nenhum — padrão da indústria agora.

### ADR-002: Prisma ORM + Neon Serverless
**Decisão:** Prisma para type-safety + Neon para serverless Postgres.
**Trade-off:** Cold start Prisma em serverless (mitigado com pooling).
**Arrependimento:** Deveria ter configurado `prisma generate` no `postinstall` desde o início.

### ADR-003: NextAuth Credentials + Email Magic Link
**Decisão:** Dois providers, sem OAuth social no MVP.
**Trade-off:** Magic link depende de email delivery (Resend).
**Arrependimento:** Adicionar "Remember me" estenderia sessão — v1.1.

### ADR-004: SSE para Tempo Real (v1)
**Decisão:** Server-Sent Events nativo, não WebSocket.
**Trade-off:** Não bidirecional nativo, mas unidirecional (server→client) basta para notificações.
**Arrependimento:** Escalabilidade limitada — v2 planeja Pusher/Ably.

### ADR-005: Zod Validation Compartilhada (Client + Server)
**Decisão:** Schemas em `lib/validations/` importados both sides.
**Trade-off:** Bundle client inclui Zod (~50kb gzipped).
**Arrependimento:** Vale a pena — single source of truth, zero drift.

### ADR-006: shadcn/ui como Base (Copiado, não dependência)
**Decisão:** Copiar componentes para `components/ui/`, adaptar tokens.
**Trade-off:** Manutenção manual de updates upstream.
**Arrependimento:** Zero — controle total, sem breaking changes surpresa.

---

## 3. Como Instalar e Rodar Localmente (Do Zero)

### Pré-requisitos
- Node.js 20.12.0 (use `nvm` ou `volta`)
- pnpm 9.1.0 (`corepack enable && corepack prepare pnpm@9.1.0 --activate`)
- PostgreSQL local (Docker recomendado) OU conta Neon (gratuita)
- Conta Meta Developers (WhatsApp Cloud API) — para webhook real
- Conta Resend (email) — para Magic Link
- Conta Sentry (gratuito) — para error tracking (opcional local)

### Passos
```bash
# 1. Clone
git clone https://github.com/seuusuario/pedidosimples.git
cd pedidosimples

# 2. Instale dependências
pnpm install
# → roda `prisma generate` automaticamente (postinstall)

# 3. Configure variáveis de ambiente
cp .env.example .env.local
# Edite .env.local com SUAS credenciais (ver seção 6)

# 4. Banco de dados
# Opção A: Docker (recomendado local)
docker run --name pg-pedidos -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=pedidosimples -p 5432:5432 -d postgres:16

# Opção B: Neon (cloud)
# Use connection string da branch `dev` ou `main`

# 5. Migrações
pnpm db:migrate:dev
# Ou para reset total: pnpm db:reset

# 6. (Opcional) Seed dados de teste
pnpm db:seed

# 7. Rode em desenvolvimento
pnpm dev
# → http://localhost:3000

# 8. Testes
pnpm test           # Unit + Integration (Vitest)
pnpm test:e2e       # E2E (Playwright - precisa dev rodando)
pnpm test:smoke     # Smoke tests rápidos
```

### Verificação Rápida
- [ ] `pnpm dev` abre sem erros
- [ ] Login Magic Link funciona (Resend sandbox)
- [ ] Dashboard carrega (estado vazio ok)
- [ ] `pnpm build` passa sem warnings
- [ ] `pnpm test` passa

---

## 4. Processo de Deploy

### Ambientes
| Ambiente | Branch | URL | Trigger |
|----------|--------|-----|---------|
| Development | Qualquer (local) | localhost:3000 | `pnpm dev` |
| Preview | `feature/*`, `fix/*` | `pedidosimples-git-feature.vercel.app` | Push branch |
| Staging | `staging` | `staging-pedidosimples.vercel.app` | Push `staging` |
| Produção | `main` + tag `v*` | `pedidosimples.com` | Push `main` OU tag |

### Variáveis de Ambiente por Ambiente (Configure na Vercel)
```bash
# TODAS obrigatórias em TODOS ambientes (valores diferentes):
DATABASE_URL
NEXTAUTH_SECRET
NEXTAUTH_URL
WHATSAPP_API_TOKEN
WHATSAPP_PHONE_NUMBER_ID
WHATSAPP_WEBHOOK_VERIFY_TOKEN
RESEND_API_KEY
EMAIL_FROM
SENTRY_DSN

# Apenas Produção:
# (Nenhuma extra — mesma lista, valores prod)
```

### Comandos de Deploy
```bash
# Preview (automático no push)
git push origin feature/nova-feature

# Staging
git push origin staging
# Ou: vercel --token=$VERCEL_TOKEN --scope=team

# Produção (2 opções):
# Opção 1: Merge para main (auto-deploy configurado)
git checkout main && git merge feature/nova-feature && git push origin main

# Opção 2: Tag de versão (recomendado para releases)
git tag v1.2.3 && git push origin v1.2.3
# → GitHub Action ou Vercel detecta tag e deploya produção
```

### Migrações de Banco (Produção)
- **Automático:** `prisma migrate deploy` roda no build Vercel (configurado em `vercel.json` ou build script)
- **Manual (se necessário):** `vercel env pull .env.production && pnpm prisma migrate deploy`

### Rollback
```bash
# Via Vercel CLI (mais rápido)
vercel rollback https://pedidosimples-<deployment-id>.vercel.app --token=$VERCEL_TOKEN

# Via Dashboard: Vercel → Project → Deployments → "..." → Rollback
# Tempo alvo: < 1 minuto
# Banco: Migrações são backward-compatible (apenas additive v1.x)
```

---

## 5. Configurações Necessárias (Checklist)

### DNS & Domínio
- [ ] `pedidosimples.com` → CNAME `cname.vercel-dns.com` (Vercel)
- [ ] `www.pedidosimples.com` → Redirect para apex (Vercel config)
- [ ] SSL: Automático (Let's Encrypt via Vercel)

### WhatsApp Cloud API (Meta Developers Console)
- [ ] App: `PedidosSimples Prod`
- [ ] Webhook URL: `https://pedidosimples.com/api/webhooks/whatsapp`
- [ ] Verify Token: `[WHATSAPP_WEBHOOK_VERIFY_TOKEN]`
- [ ] Campos assinados: `messages`, `message_template_status_update`
- [ ] Phone Number ID: `[WHATSAPP_PHONE_NUMBER_ID]`
- [ ] Access Token: `[WHATSAPP_API_TOKEN]` (rotacionar a cada 60 dias)

### Resend (Email)
- [ ] Domínio: `pedidosimples.com` verificado (DKIM, SPF, DMARC)
- [ ] API Key: `[RESEND_API_KEY]`
- [ ] From: `noreply@pedidosimples.com`
- [ ] Templates: Magic Link (custom HTML)

### Sentry
- [ ] Project: `pedidosimples-prod`
- [ ] DSN: `[SENTRY_DSN]`
- [ ] Alertas: Error rate > 1%/5min, Crash rate > 0.1%
- [ ] Release tracking: `@sentry/nextjs` + `vercel` integration

### UptimeRobot
- [ ] Monitor: `https://pedidosimples.com` (HTTP 200, 1 min interval)
- [ ] Alertas: Email + Webhook (Slack/Discord se configurado)

### Neon (Banco)
- [ ] Branch: `production` (Protected: no delete, no direct push)
- [ ] Pooling: Habilitado (PgBouncer)
- [ ] PITR: 7 dias (automático)
- [ ] Backup manual: Branch snapshot pré-deploy

---

## 6. Dependências Externas (Contratos e Contatos)

| Serviço | Uso | Contato/Suporte | Contrato/Plano | Renovação |
|---------|-----|-----------------|----------------|-----------|
| Vercel | Hosting + Functions | support@vercel.com | Pro ($20/mês) | Mensal |
| Neon | PostgreSQL Serverless | support@neon.tech | Scale ($19/mês) | Mensal |
| Meta (WhatsApp) | Business API | Meta Business Support | Gratuito (até 1k conv/dia) | N/A |
| Resend | Email Transactional | support@resend.com | Pro ($20/mês) | Mensal |
| Sentry | Error Tracking | support@sentry.io | Team ($26/mês) | Mensal |
| UptimeRobot | Uptime Monitoring | support@uptimerobot.com | Pro ($7/mês) | Mensal |
| 1Password | Segredos | support@1password.com | Teams ($7.99/usuário) | Anual |

> [!important]
> **Nenhuma dependência "crítica única" sem alternativa.** WhatsApp API = risco concentrado. Monitorar anúncios Meta.

---

## 7. Limitações Conhecidas e Technical Debt

| Item | Descrição | Impacto | Workaround | Prioridade Fix |
|------|-----------|---------|------------|----------------|
| Rate Limiting API | Não implementado (v1) | Risco abuso/DDoS | WhatsApp tem limite próprio | P1 (v1.1) |
| Rate Limiting Auth | Apenas NextAuth padrão | Brute force possível | Monitorar Sentry | P1 (v1.1) |
| Relatório Semanal | Apenas total dia | Usuário não tem histórico semanal | Export manual CSV | P2 (v1.1) |
| Runbook DR | Não documentado | Recuperação lenta se catástrofe | Neon PITR + Vercel rollback | P2 (v1.1) |
| Push Notifications | Não nativo (PWA só) | Engajamento menor | Service Worker planejado | P3 (v1.2) |
| Multi-estabelecimento | Single tenant only | Não atende redes/franquias | Conta separada por loja | P3 (v1.2) |
| SSE Escala | Conexão por aba/usuário | Limite ~10k conexões simultâneas | Migrar Pusher v2 | P2 (v2.0) |
| Prisma Cold Start | ~200ms primeira query | Latência primeira request | Neon pooling + keep-alive | P3 (otimização) |

---

## 8. Pendências e Roadmap Não Feito

### Backlog Priorizado (Próximas Versões)
| Prioridade | Item | Esforço | Valor | Status |
|------------|------|---------|-------|--------|
| P1 | Rate Limiting (Arcjet) | 1 semana | Segurança | Planejado v1.1 |
| P1 | Relatório Semanal + Export | 2 semanas | Feature core | Planejado v1.1 |
| P2 | Runbook Disaster Recovery | 3 dias | Operacional | Planejado v1.1 |
| P2 | Push Notifications (SW) | 1 semana | Retenção | Backlog v1.2 |
| P3 | Multi-estabelecimento | 4-6 semanas | Expansão mercado | Backlog v1.2 |
| P3 | Integração iFood/Rappi | 3-4 semanas | Novo canal | Backlog v1.3 |
| P3 | API Pública (tRPC) | 2 semanas | Ecossistema | Backlog v2.0 |

### Ideias Não Priorizadas (Icebox)
- Dashboard analytics avançado (gráficos, comparativos)
- Programa de fidelidade / cupons
- App mobile nativo (React Native)
- White-label para agências
- Marketplace de templates de resposta

---

## 9. Informações de Manutenção (Resumo do Plano)

### Rotinas Essenciais
| Rotina | Frequência | Responsável | Doc Referência |
|--------|------------|-------------|----------------|
| Verificar Sentry/Uptime/Vercel | Diária (5 min) | Mantenedor | Plano de Manutenção |
| `pnpm audit` + Dependabot | Semanal | Mantenedor | Plano de Manutenção |
| Restore Test (Neon → Staging) | Mensal | Mantenedor | Plano de Manutenção |
| Revisão Riscos/Segurança | Mensal | Mantenedor | Revisão Segurança ETAPA 12 |
| Rotação Segredos | Trimestral | Mantenedor | Plano de Manutenção |

### Alertas Críticos (Configurados)
- Sentry: Error rate > 1%/5min → Email + Push
- UptimeRobot: Downtime > 1 min → Email + Push
- Vercel: Function error rate > 5% → Email
- Neon: Storage > 80% → Email
- WhatsApp Webhook: Falhas > 5/min → Sentry Alert

---

## 10. Acessos e Credenciais (Auditoria)

| Recurso | Acesso Atual | Ação Necessária (Encerramento/Transferência) |
|---------|--------------|---------------------------------------------|
| GitHub Repo | [Seu usuário] Owner | Transfer ownership OU adicionar novo owner |
| Vercel Project | [Sua conta/team] Owner | Transfer project OU adicionar team member |
| Neon Project | [Sua conta] Owner | Transfer project OU adicionar member |
| Meta Developers | [Sua conta] Admin | Adicionar novo admin, remover você |
| Resend | [Sua conta] Owner | Transfer domain + API key |
| Sentry | [Sua conta/team] Owner | Transfer project |
| UptimeRobot | [Sua conta] Owner | Transfer monitor |
| 1Password Vault | [Seu vault] | Compartilhar vault OU exportar itens |
| Domínio (registro.br/Cloudflare) | [Sua conta] | Transfer domínio OU adicionar contato técnico |

> [!warning]
> **NUNCA compartilhe segredos por email/chat não criptografado.** Use 1Password sharing, Bitwarden send, ou call de vídeo com compartilhamento de tela.

---

## 11. Organização do Repositório

### Branches Principais
- `main` → Produção (protegida, required reviews, status checks)
- `staging` → Staging (auto-deploy)
- `feature/*`, `fix/*`, `hotfix/*` → Trabalho (delete after merge)

### Tags de Versão
- `v1.0.0` → MVP inicial
- `v1.0.1` → Hotfix webhook timeout
- `v1.1.0` → Relatórios semanais
- ... (todas tags SemVer)

### Releases (GitHub)
- Cada tag `v*` → GitHub Release automático (via Action)
- Changelog no corpo do release
- Assets: nenhum (binários não aplicável)

### Issues & Labels
- Labels padronizados: `bug`, `feature`, `maintenance`, `incident`, `P0`, `P1`, `P2`, `security`, `docs`, `dependencies`
- Issues abertas no encerramento: [link para query GitHub]

### Wiki / Docs
- `README.md` → Visão geral + quickstart
- `CLAUDE.md` → Contexto para IA (este projeto)
- `RUNBOOK.md` → Operações de emergência (deploy, rollback, incidentes)
- `ARCHITECTURE.md` → ADRs consolidados
- `docs/` → Documentação técnica adicional (se houver)

---

## 12. Estado Final do Projeto

| Campo | Valor |
|-------|-------|
| **Versão Final** | v1.2.3 |
| **Data Encerramento** | 2027-03-15 |
| **Status** | ENCERRADO (manutenção apenas segurança crítica) |
| **Último Commit** | `fgh789ijk` (main) |
| **Última Tag** | `v1.2.3` |
| **Cobertura Testes** | 82% linhas, 71% branches |
| **Bugs Abertos** | 0 P0, 0 P1, 2 P2 (documentados) |
| **Vulnerabilidades** | 0 critical/high (auditado 2027-03-10) |
| **Custo Mensal Final** | ~$72 (Vercel + Neon + Resend + Sentry + UptimeRobot) |

---

## 13. Lições Aprendidas (Reflexão Pessoal)

### O que funcionou bem
1. **Processo estruturado (este manual)** — evitou caos, deu clareza em cada passo
2. **Vibe Coding com portões** — IA acelerou 5-10x, mas revisão humana garantiu qualidade
3. **ADRs** — decisões documentadas evitam "por que fizemos assim?" depois
4. **Testes desde o início** — Vitest + Playwright pagaram dividendos em refatorações
5. **Design Tokens + shadcn/ui** — consistência visual sem design system pesado
6. **Staging idêntico a prod** — pegou 3 bugs que passariam em prod

### O que faria diferente
1. **Rate limiting desde o início** — subestimei risco de API externa
2. **Runbook DR antes do primeiro deploy** — improvisei na primeira falha
3. **Mais testes de contrato (Pact) para WhatsApp** — quebra de API pegou de surpresa
4. **Observabilidade de negócio (métricas de pedidos)** — só técnico, não produto
5. **Documentar "por que NÃO fez X"** — decisões negativas também importam

### Conselhos para próximo mantenedor
1. **Leia os ADRs antes de mudar arquitetura** — contexto histórico evita regressão
2. **Teste restore mensal** — não confie, verifique
3. **WhatsApp API é volátil** — queue + retry + monitoramento de latência
4. **Next.js App Router evolui rápido** — `pnpm outdated` major = planeje migração
5. **Este manual funciona** — siga as etapas, não pule portões

---

## Assinatura de Encerramento

**Mantenedor Original:** [Seu Nome]
**Data:** 2027-03-15
**Assinatura:** [Digital]

> **Este projeto foi desenvolvido com o Software Engineering OS.**
> A documentação completa reside neste cofre Obsidian, na pasta do projeto.
```

### Checklist de Encerramento/Transferência

```markdown
# Checklist de Encerramento/Transferência — PedidosSimples

## Tipo: [ ] ENCERRAMENTO  [ ] TRANSFERÊNCIA para: [Nome/Equipe]

---

### Documentação
- [ ] Documentação de Continuidade completa e salva
- [ ] Todos ADRs consolidados em `ARCHITECTURE.md`
- [ ] `CLAUDE.md` atualizado com estado final
- [ ] `RUNBOOK.md` completo (deploy, rollback, incidentes, rotinas)
- [ ] `CHANGELOG.md` atualizado até versão final
- [ ] `README.md` reflete estado final (badges, status, links)
- [ ] Lições aprendidas registradas (pessoal + projeto)

### Código e Repositório
- [ ] `main` branch protegida e estável
- [ ] Todas tags de versão pushadas (`v1.0.0` a `vX.Y.Z`)
- [ ] GitHub Releases criados para cada tag
- [ ] Branches de feature/fix antigas limpas (merged + deleted)
- [ ] Issues abertas: triadas (fechadas ou movidas para backlog doc)
- [ ] `.gitignore` correto (sem segredos, build artifacts, node_modules)
- [ ] `pnpm-lock.yaml` commitado

### Instalação e Execução (Validado)
- [ ] `git clone → pnpm install → cp .env.example .env.local → pnpm db:migrate:dev → pnpm dev` FUNCIONA do zero
- [ ] Documentado passo a passo na Documentação de Continuidade
- [ ] Pré-requisitos listados (Node, pnpm, Docker/Neon, contas externas)

### Deploy e Operações
- [ ] Processo de deploy documentado (staging + produção)
- [ ] Variáveis de ambiente por ambiente documentadas (valores = placeholders)
- [ ] Rollback testado e funcionando (< 1 min)
- [ ] Migrações de banco documentadas (automáticas no build)
- [ ] Monitoramento configurado (Sentry, UptimeRobot, Vercel Analytics)
- [ ] Alertas testados (forçar erro → alerta dispara)
- [ ] Backup/Restore testado mensalmente (últimos 3 meses OK)

### Segredos e Acessos
- [ ] **Nenhum segredo no git** (auditoria: `git-secrets`, `truffleHog`, `git log --all -- "**/.env*"`)
- [ ] Segredos produção apenas na Vercel/Neon/1Password
- [ ] Auditoria de acessos: quem tem acesso a quê (tabela na Doc Continuidade)
- [ ] Rotação de segredos documentada (frequência, procedimento)
- [ ] Se transferência: credenciais compartilhadas via 1Password/Bitwarden (NUNCA email/chat)

### Dependências Externas
- [ ] Contratos/Planos listados com contatos e renovações
- [ ] WhatsApp Webhook URL correta em produção
- [ ] Domínio DNS configurado e SSL ativo
- [ ] Contas de serviço (Meta, Resend, Sentry) com acesso transferido/separado

### Conformidade e Legal
- [ ] LGPD: Política de privacidade acessível, exclusão funcionando
- [ ] Termos de uso atualizados
- [ ] Cookies/Tracking: consentimento se aplicável
- [ ] Dados de teste/desenvolvimento removidos de produção

### Financeiro
- [ ] Custos mensais documentados
- [ ] Assinaturas canceladas (se encerramento) OU transferidas (se transferência)
- [ ] Faturas/Recibos organizados

### Validação Final (Obrigatório)
- [ ] **Teste real:** Segui a Documentação de Continuidade do zero em máquina limpa → funcionou
- [ ] **Teste de conhecimento:** Novo mantenedor (ou você daqui 6 meses) consegue deployar só lendo os docs?

---

## Assinaturas

### Encerramento
**Mantenedor:** [Seu Nome]
**Data:** 2027-03-15
**Assinatura:** [Digital]

### Transferência (se aplicável)
**Novo Mantenedor:** [Nome]
**Data Recebimento:** [Data]
**Confirmação:** "Recebi documentação completa, testei instalação local e deploy, tenho todos os acessos necessários."
**Assinatura:** [Digital]

---

> [!important]
> **Este checklist deve estar 100% ✅ antes de considerar o projeto encerrado/transferido.**
> Itens pendentes = dívida para o futuro (que custará mais caro).
```

---

## Como trabalhar com IA nesta etapa

**Prompt para compilar Documentação de Continuidade:**
```
Tenho todos os documentos do projeto (ETAPAs 00-15) neste cofre.
Gere Documentação de Continuidade completa compilando:
1. Tecnologias com versões exatas (do package.json + lockfile)
2. ADRs consolidados (da pasta 03 - Arquitetura)
3. Instalação local (do CLAUDE.md + Configuração Ambientes)
4. Deploy (do Plano de Manutenção + Checklist Publicação)
5. Configurações necessárias (checklist)
6. Dependências externas (tabela)
7. Limitações conhecidas (ETAPA 13 + 12 + bugs P2 abertos)
8. Pendências/Roadmap (ETAPA 15 + backlog)
9. Manutenção (resumo Plano de Manutenção)
10. Acessos (auditoria real)
11. Organização repo (branches, tags, releases)
12. Estado final (métricas)
13. Lições aprendidas (sua reflexão)
Formato: Markdown completo, tabelas, pronto para salvar.
```

**Prompt para gerar Checklist de Encerramento:**
```
Gere Checklist de Encerramento/Transferência abrangente baseado em:
- Documentação de Continuidade gerada
- Boas práticas de handoff técnico
- Conformidade LGPD/segurança
- Auditoria de segredos e acessos
Formato: Tabelas com checkboxes, seções obrigatórias/condicionais, validação final, assinaturas.
```

**Prompt para validar documentação (teste real):**
```
Simule: "Sou novo mantenedor. Tenho apenas a Documentação de Continuidade e acesso ao GitHub."
Liste TODOS os passos que eu precisaria fazer para:
1. Clonar e rodar localmente
2. Fazer deploy em staging
3. Fazer deploy em produção
4. Investigar um erro no Sentry
5. Fazer rollback
6. Rotacionar um segredo
Para cada: comando exato, pré-requisito, onde encontrar credencial.
Identifique gaps na documentação.
```

**Contexto a fornecer:** Todos os documentos do projeto (ETAPAs 00-15), repositório real, credenciais reais (para auditoria), decisão de encerrar/transferir.
**Resultado esperado:** Documentação de Continuidade completa + Checklist 100% ✅ + Validação real passada.
**Como verificar:** Teste real de instalação do zero seguindo apenas a doc. Se travar em qualquer ponto = gap na doc.

## Checklist de execução

- [ ] Decisão consciente: ENCERRAR ou TRANSFERIR (registre motivo)
- [ ] Documentação de Continuidade compilada e completa (13 seções)
- [ ] Checklist de Encerramento/Transferência 100% verificado
- [ ] Teste real de instalação do zero seguindo APENAS a doc → funcionou
- [ ] Auditoria de segredos: zero no git, todos na plataforma/1Password
- [ ] Acessos transferidos/documentados (se transferência)
- [ ] Repositório organizado: tags, releases, branches limpos, issues triadas
- [ ] CHANGELOG + README + CLAUDE.md + RUNBOOK atualizados
- [ ] Lições aprendidas registradas (valor pessoal + projeto)
- [ ] Assinaturas coletadas (encerramento + transferência se aplicável)
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Documentação de Continuidade permite que terceiro assuma sem você
- [ ] Checklist 100% ✅ (zero itens pendentes não justificados)
- [ ] Teste real de onboarding passou (você ou outro seguiu a doc e conseguiu)
- [ ] Segredos auditados e seguros
- [ ] Repositório em estado limpo e versionado
- [ ] Se transferência: novo mantenedor confirma recebimento e capacidade

## Verificações e testes

- **Teste do onboarding:** Máquina limpa → clone → doc → `pnpm dev` → login → dashboard → OK?
- **Teste do deploy:** Doc → staging deploy → smoke test → OK?
- **Teste do rollback:** Doc → `vercel rollback` → funciona?
- **Teste dos segredos:** `git-secrets` scan → limpo? Vercel env vars → só placeholders no git?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Documentar "como era" não "como é" | Documente o estado ATUAL (versão final), não o plano inicial |
| Esquecer credenciais "óbvias" | Auditoria sistemática: tabela de 10+ recursos na Doc Continuidade |
| Não testar a doc | **Teste real obrigatório** — você segue sua própria doc do zero |
| Deixar branches "lixo" no remote | `git branch -r --merged main` → delete merged branches |
| Não rotacionar segredos na transferência | **Obrigatório:** novos tokens para novo mantenedor |
| Subestimar "lições aprendidas" | Escreva. É o ativo mais valioso para SUA próxima vez. |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Teste real da doc falha | Corrija a doc no ponto de falha. Re-teste. Não assuma "ele descobre". |
| Segredo encontrado no git | 1. Rotacione IMEDIATAMENTE 2. `git filter-repo` / BFG 3. Force push 4. Re-audite |
| Novo mantenedor não consegue | Pair programming session (1-2h) para passar contexto. Atualize doc com gaps. |
| Domínio/assinatura não transferível | Documente limitação. Novo mantenedor registra novo domínio. Migração planejada. |
| Dependência crítica encerra (ex: WhatsApp API) | Monitorar anúncios. Plano B: fallback email/SMS. Documentado em Riscos. |

## Condições para considerar CONCLUÍDO

- ✅ Documentação de Continuidade salva
- ✅ Checklist 100% ✅
- ✅ Teste real de onboarding passou
- ✅ Assinaturas coletadas
- ✅ Projeto arquivado (GitHub: Archive repository) OU transferido com confirmação

---

## Fim do Ciclo de Vida

> **Parabéns.** Você completou o ciclo completo: Ideia → Descoberta → Produto → Arquitetura → UX/UI → Desenvolvimento → Testes → Segurança → Validação → Publicação → Manutenção → Encerramento.
>
> Este manual (Software Engineering OS) foi testado na prática neste projeto.
> Atualize-o com o que aprendeu. Use-o no próximo.

## Materiais relacionados

- [[02 - Modelos/Publicação e Manutenção/Documentação de Continuidade]]
- [[02 - Modelos/Publicação e Manutenção/Checklist de Encerramento]]
- [[03 - Guias Práticos/Publicação/Runbook de Operações]]
- [[04 - Referência/Glossário/Technical Debt]]
- [[04 - Referência/Glossário/Handoff]]
- [[Início]] — Voltar ao começo do manual