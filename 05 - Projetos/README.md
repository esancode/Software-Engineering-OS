# Projetos - Software Engineering OS

> **Local:** `05 - Projetos/README.md`
> **Objetivo:** Instruções para criar, estruturar e gerenciar projetos reais usando este OS.

---

## Como Iniciar um Novo Projeto

### 1. Copiar a Estrutura Base
```bash
# Opção A: Clone este repositório e copie a pasta Template
git clone https://github.com/esancode/Software-Engineering-OS.git
cp -r Software-Engineering-OS/05\ -\ Projetos/Template MeuNovoProjeto
cd MeuNovoProjeto

# Opção B: Baixe o ZIP do GitHub e extraia a pasta Template
# 1. Acesse: https://github.com/esancode/Software-Engineering-OS
# 2. Code → Download ZIP
# 3. Extraia e copie a pasta 05 - Projetos/Template para onde quiser
```

### 2. Personalizar (Buscar/Substituir)
Substitua **TODAS** ocorrências:
- `PedidosSimples` → Nome do seu projeto
- `pedidos-simples` → slug/kebab-case
- `SEU_DOMINIO.com` → seu domínio
- `Loja` / `Pedido` → suas entidades principais

### 3. Inicializar Git
```bash
git init
git add .
git commit -m "chore: init project from Software Engineering OS template"
git branch -M main
# Substitua pelo seu usuário/repo real
git remote add origin git@github.com:SEU_USUARIO/seu-repo.git
git push -u origin main
```

### 4. Conectar Vercel
1. Vercel Dashboard → Add New Project → Import Git Repository
2. Configure Environment Variables (ver checklist abaixo)
3. Deploy automático ativado

### 5. Configurar Neon
1. Neon Console → Create Project
2. Copy `DATABASE_URL` e `DIRECT_URL` → Vercel Env Vars
3. `npx prisma migrate deploy` (rodará no primeiro deploy)

### 6. Configurar Integrações
| Serviço | Ação |
|---------|------|
| **Meta/WhatsApp** | Criar App → Webhook URL → Verify Token → Templates |
| **Resend** | Verificar domínio → API Key |
| **Vercel Blob** | Create Store → Token |
| **Upstash (Rate Limit/Flags)** | Create Database/Config → URL + Token |

---

## Estrutura de Pastas do Projeto

```
MeuProjeto/
├── 01 - Descoberta e Validação/
│   ├── Definição do Problema.md
│   ├── Pesquisa e Validação.md
│   ├── Decisão de Continuidade.md
│   ├── Análise de Concorrentes.md
│   ├── Registro de Hipóteses.md
│   ├── Definição do Produto.md
│   ├── Escopo e Limites.md
│   └── Priorização de Funcionalidades.md
├── 02 - Requisitos e Arquitetura/
│   ├── Requisitos Funcionais.md
│   ├── Requisitos de Qualidade.md
│   ├── Critérios de Aceitação.md
│   ├── Regras de Negócio.md
│   ├── ADR - Registro de Decisões Arquiteturais.md
│   ├── Visão Técnica.md
│   ├── Modelo de Dados.md
│   ├── Registro de Integrações.md
│   └── Estratégia de Auth.md
├── 03 - UX e UI/
│   ├── Jornada do Usuário.md
│   ├── Wireframes e Fluxos.md
│   ├── Design System - Tokens.md
│   ├── Especificação de Componentes.md
│   └── Acessibilidade e Checklist UI.md
├── 04 - Desenvolvimento/
│   ├── Plano de Implementação.md
│   ├── Guia de Vibe Coding com IA.md
│   └── Checklist de Código.md
├── 05 - Testes/
│   ├── Estratégia de Testes.md
│   ├── Plano de Testes.md
│   └── Casos de Teste.md
├── 06 - Segurança e Validação/
│   ├── Checklist de Segurança.md
│   └── Plano de Validação.md
├── 07 - Publicação e Manutenção/
│   ├── Checklist de Deploy.md
│   ├── Runbook de Incidentes.md
│   ├── Plano de Manutenção.md
│   └── Plano de Encerramento.md
├── 08 - Código Fonte/          # Repositório real (git)
│   ├── src/
│   ├── tests/
│   ├── prisma/
│   ├── .github/workflows/
│   ├── package.json
│   ├── next.config.js
│   ├── tsconfig.json
│   ├── vercel.json
│   └── ...
├── 09 - Documentação Viva/
│   ├── CHANGELOG.md
│   ├── DECISIONS.md
│   └── RUNBOOK.md
└── README.md                   # Este arquivo (personalizado)
```

---

## Checklist de Setup Inicial (Obrigatório)

### Código e Qualidade
- [ ] `npm install` roda sem erros
- [ ] `npm run dev` sobe localmente
- [ ] `npm run lint` → 0 errors
- [ ] `npm run typecheck` → 0 errors
- [ ] `npm run test` → 100% pass
- [ ] `npm run build` → Success
- [ ] Husky pre-commit configurado

### Banco de Dados
- [ ] `DATABASE_URL` e `DIRECT_URL` no Vercel (Preview + Production)
- [ ] `npx prisma migrate deploy` roda no CI
- [ ] RLS policies aplicadas (se multi-tenant)
- [ ] Seed development NÃO roda em production

### Autenticação
- [ ] `NEXTAUTH_SECRET` gerado (32+ chars): `openssl rand -base64 32`
- [ ] `NEXTAUTH_URL` correto por ambiente
- [ ] Resend domínio verificado
- [ ] Magic Link testado (email chega, link funciona)
- [ ] Credentials testado (se habilitado)

### Integrações
- [ ] WhatsApp Webhook URL configurada no Meta (Preview + Production)
- [ ] `WHATSAPP_TOKEN`, `WHATSAPP_APP_SECRET`, `WHATSAPP_VERIFY_TOKEN` no Vercel
- [ ] Templates WhatsApp aprovados no Meta
- [ ] Resend API Key no Vercel
- [ ] Vercel Blob token no Vercel

### Segurança
- [ ] `npm audit` → 0 high/critical
- [ ] TruffleHog scan limpo
- [ ] Semgrep scan limpo
- [ ] CSP headers configurados
- [ ] Rate limiting ativo
- [ ] Secrets NÃO no repo (apenas Vercel env)

### Monitoramento
- [ ] Health check endpoint: `GET /api/health` → 200
- [ ] Vercel Analytics ativo
- [ ] Upptime monitor configurado (GitHub Actions)
- [ ] Alertas Telegram/Email configurados
- [ ] Sentry DSN (se usando)

### Deploy
- [ ] Preview deploy funciona (PR abre → URL gerada)
- [ ] Production deploy funciona (merge main → deploy)
- [ ] Smoke test pós-deploy documentado
- [ ] Rollback testado (`vercel rollback`)

---

## Variáveis de Ambiente (Referência)

### Obrigatórias
| Variável | Descrição | Onde Gerar |
|----------|-----------|------------|
| `NEXTAUTH_SECRET` | Chave JWT/sessão | `openssl rand -base64 32` |
| `NEXTAUTH_URL` | URL canônica do app | Vercel (auto em preview) |
| `DATABASE_URL` | Neon pooled connection | Neon Dashboard |
| `DIRECT_URL` | Neon direct connection | Neon Dashboard |
| `WHATSAPP_TOKEN` | Access token permanente | Meta Developer Console |
| `WHATSAPP_APP_SECRET` | App secret para HMAC | Meta Developer Console |
| `WHATSAPP_VERIFY_TOKEN` | Token verificação webhook | Definir aleatório |
| `WHATSAPP_PHONE_NUMBER_ID` | ID do número WhatsApp | Meta Developer Console |
| `RESEND_API_KEY` | Chave API Resend | Resend Dashboard |

### Opcionais (Feature Flags)
| Variável | Default | Descrição |
|----------|---------|-----------|
| `ENABLE_SSE` | `true` | Tempo real |
| `ENABLE_PWA` | `true` | Instalável |
| `ENABLE_TEMPLATES` | `true` | Templates WhatsApp |
| `ENABLE_REPORTS` | `true` | Relatórios |
| `MAX_PEDIDOS_PAGE` | `20` | Paginação |
| `WEBHOOK_QUEUE_ENABLED` | `true` | Queue assíncrona |

### Observabilidade (Opcional)
| Variável | Descrição |
|----------|-----------|
| `NEXT_PUBLIC_SENTRY_DSN` | Sentry error tracking |
| `EDGE_CONFIG` | Vercel Edge Config (flags v1.1+) |
| `UPSTASH_REDIS_URL` | Rate limit / Cache |
| `UPSTASH_REDIS_TOKEN` | Rate limit / Cache |

---

## Workflow de Desenvolvimento (Dia a Dia)

### Iniciar Nova Feature
```bash
# 1. Atualizar main
git checkout main && git pull origin main

# 2. Criar branch
git checkout -b feat/nome-da-feature

# 3. Escrever spec/ADR se necessário
#    Editar arquivos em 01-07/ conforme necessário

# 4. Desenvolver com IA (Claude Code)
claude
# @arquivos-relevantes
# Prompt específico com constraints

# 5. Validar localmente
npm run lint && npm run typecheck && npm run test && npm run build

# 6. Commit convencional
git add -A
git commit -m "feat(scope): descrição curta"

# 7. Push + PR
git push origin feat/nome-da-feature
gh pr create --title "feat(scope): descrição" --body "Refs #issue"
```

### Code Review (Solo = Auto-review)
```markdown
## Auto-Review Checklist
- [ ] Código lido e entendido
- [ ] Testes passam (unit + e2e se user-facing)
- [ ] Lint/Typecheck limpo
- [ ] Segurança: validação Zod, auth, RLS
- [ ] Acessibilidade: labels, roles, foco
- [ ] Performance: sem N+1, bundle OK
- [ ] Documentação atualizada (CHANGELOG, README se necessário)
- [ ] ADR criado se decisão arquitetural
```

### Merge e Deploy
```bash
# Após CI verde + review
gh pr merge --squash --delete-branch

# Vercel deploya automaticamente
# Aguardar deploy + smoke test (10 min)
# Verificar alertas nas primeiras 2h
```

---

## Rotinas do Projeto

### Diário (5 min)
- [ ] Verificar alertas (Telegram/Email)
- [ ] Verificar logs erro (Vercel/Sentry)
- [ ] Verificar dead letters (webhook)

### Semanal (30 min)
- [ ] Métricas negócio (DAU, pedidos, conversão, churn)
- [ ] `npm outdated` + Dependabot PRs
- [ ] `npm audit` + security scans
- [ ] Backup verify (Neon PITR)
- [ ] Feedback usuários
- [ ] Runbook drill (simular 1 incidente)

### Mensal (2h)
- [ ] Rotacionar segredos (WhatsApp 60d, NextAuth 90d)
- [ ] Teste rollback + smoke test
- [ ] Teste restore DB (Neon PITR)
- [ ] Revisão runbook
- [ ] Análise custos
- [ ] Lighthouse CI trends
- [ ] Planejamento evolução (bugs, Should, tech debt)

### Trimestral (Meio dia)
- [ ] Pen test leve (OWASP ZAP)
- [ ] LGPD audit
- [ ] Revisão arquitetura (ADRs)
- [ ] Capacity planning
- [ ] Disaster recovery test
- [ ] Revisão contratos

---

## Templates de Arquivos do Projeto

### CHANGELOG.md
```markdown
# Changelog

Todas as mudanças notáveis documentadas aqui.
Formato: [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/)
Versionamento: [Semantic Versioning](https://semver.org/lang/pt-BR/)

## [1.0.0] - 2026-01-15
### Added
- Fluxo core: Pedido WhatsApp → Dashboard → Status → Entrega
- Auth: Magic Link + Credentials
- Tempo real via SSE
- Templates WhatsApp (preparado, pronto, entregue)
- Relatório diário/semanal
- Config: Produtos, Horários
- PWA instalável

### Security
- RLS multi-tenant no PostgreSQL
- HMAC validation webhook WhatsApp
- Rate limiting API
- CSP headers

## [0.9.0] - 2026-01-01
### Added
- MVP beta para usuários piloto
- Onboarding guiado
- Testes E2E @smoke + @core
```

### DECISIONS.md (ADR Log)
```markdown
# Decisões Arquiteturais (ADR Log)

| ADR | Título | Status | Data | Link |
|-----|--------|--------|------|------|
| 001 | Stack: Next.js + Vercel + Neon + Prisma | Accepted | 2026-01-10 | [[ADR-001]] |
| 002 | Auth: NextAuth v5 Magic Link + Credentials | Accepted | 2026-01-10 | [[ADR-002]] |
| 003 | Tempo Real: SSE (não WebSocket) | Accepted | 2026-01-11 | [[ADR-003]] |
| 004 | Multi-tenant: PostgreSQL RLS | Accepted | 2026-01-11 | [[ADR-004]] |
| 005 | Webhook Idempotência: message_id + tabela eventos | Accepted | 2026-01-12 | [[ADR-005]] |
```

### RUNBOOK.md (Resumo Operacional)
```markdown
# Runbook Operacional - Resumo Rápido

## Contatos
- On-call: [Você] - Telegram/Telefone
- Backup: [Colega] - Telegram
- Vercel: Dashboard → Support
- Neon: Dashboard → Support
- Meta: Business Help Center
- Resend: Dashboard → Support

## Incidentes Críticos (SEV-1)
| Incidente | Sintoma | Mitigação Rápida |
|-----------|---------|------------------|
| Webhook Down | Pedidos não aparecem | `vercel rollback` + verificar token |
| DB Down | Health check falha | Verificar Neon → rollback conexões |
| Auth Down | Login falha | Verificar NextAuth secret + Resend |

## Deploy
- **Produção:** Merge em `main` → Auto deploy Vercel
- **Rollback:** `vercel rollback` (< 30s)
- **Smoke Test:** 10 fluxos core (ver checklist)

## Segredos (Rotação)
- WhatsApp Token: 60 dias
- NextAuth Secret: 90 dias
- Resend Key: Conforme necessidade
```

---

## Exemplo: Projeto "PedidosSimples" (Já Criado)

```
05 - Projetos/
└── Exemplo-PedidosSimples/
    ├── 01 - Descoberta e Validação/
    ├── 02 - Requisitos e Arquitetura/
    ├── 03 - UX e UI/
    ├── 04 - Desenvolvimento/
    ├── 05 - Testes/
    ├── 06 - Segurança e Validação/
    ├── 07 - Publicação e Manutenção/
    ├── 08 - Código Fonte/          # Repo real
    ├── 09 - Documentação Viva/
    │   ├── CHANGELOG.md
    │   ├── DECISIONS.md
    │   └── RUNBOOK.md
    └── README.md
```

**ADRs do Exemplo:**
- `ADR-001-stack-tecnologica.md` — Next.js 15 + Vercel + Neon + Prisma + Tailwind + NextAuth v5
- `ADR-002-modelo-de-dados.md` — Schema Prisma com RLS, enums, índices

---

## Boas Práticas de Projeto

### Documentação Viva
- Atualize `CHANGELOG.md` a cada release
- Registre decisões em `DECISIONS.md` (formato ADR leve)
- Mantenha `RUNBOOK.md` sincronizado com runbook completo

### Versionamento
- **SemVer estrito:** Patch (fix), Minor (feature compat), Major (breaking)
- Tags Git: `v1.0.0`, `v1.1.0`, `v2.0.0`
- Changelog gerado via Changesets ou manual

### Branching
```
main                    # Production (protegida)
  └── feat/*            # Features (PR → main)
  └── fix/*             # Bug fixes (PR → main)
  └── hotfix/*          # Emergency (PR → main, deploy imediato)
  └── chore/*           # Deps, config, docs
```

### Commits (Conventional Commits)
```
feat(pedidos): adicionar filtro por período no dashboard
fix(webhook): corrigir validação HMAC para mensagens de imagem
chore(deps): atualizar next 15.0.1 → 15.0.2
docs(readme): adicionar instruções de setup local
refactor(auth): extrair validação de sessão para hook
test(e2e): adicionar cenário cancelamento com motivo
perf(db): adicionar índice composto lojaId+status+createdAt
```

---

## Encerramento do Projeto

Quando o projeto chega ao fim (sunset, venda, pivot):

1. **Exportar dados** — `pg_dump` + backup Vercel Blob
2. **Documentar lições** — `LIÇÕES_APRENDIDAS.md`
3. **Arquivar repo** — GitHub: Settings → Archive repository
4. **Cancelar serviços** — Vercel, Neon, Resend, Meta, domínios
5. **Notificar usuários** — 30 dias antecedência, export dados
6. **LGPD** — Anonimizar/excluir dados conforme solicitação
7. **Preservar conhecimento** — Mover ADRs/decisões para OS principal

---

## Referências Rápidas

- [[00 - Comece Aqui/Iniciar Novo Projeto]] — Passo a passo visual
- [[01 - Manual de Desenvolvimento/Visão Geral das Etapas]] — 17 etapas completas
- [[02 - Modelos/Índice de Modelos]] — Todos modelos disponíveis
- [[03 - Guias Práticos/Guia de Vibe Coding]] — Workflow com IA
- [[03 - Guias Práticos/Guia de Publicação]] — Deploy, monitoramento, runbook
- [[04 - Referência/Ferramentas]] — Stack completa com comandos
- [[04 - Referência/Conceitos]] — Conceitos fundamentais explicados