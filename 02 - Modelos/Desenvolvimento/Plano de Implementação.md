# Plano de Implementação

> **Copie para:** `05 - Projetos/SeuProjeto/05 - Preparação e Desenvolvimento/Plano de Implementação.md`
> Baseado na ETAPA 09. **Plano detalhado de sprints, tarefas, estimativas e dependências** para o MVP.

---

## Visão Geral do MVP

| Métrica | Valor |
|---------|-------|
| **Prazo total** | 4 semanas (20 dias úteis) |
| **Time** | 1 pessoa (full-stack solo) |
| **Stack** | Next.js 14 + TypeScript + Prisma + Neon + Vercel + NextAuth + Tailwind + shadcn/ui |
| **Deploy** | Vercel (preview em PRs, production no merge main) |
| **Definition of Done** | Código + Testes + Lint + TypeCheck + Build + Deploy Preview + Aprovação manual |

---

## Estratégia de Sprints (1 semana = 1 sprint)

| Sprint | Foco | Entregável Principal | Dias |
|--------|------|---------------------|------|
| **Sprint 0** | Setup + Fundação | Repo, CI/CD, Auth, DB, Design System | 3 |
| **Sprint 1** | Core: Pedidos + WhatsApp | Webhook, Parser, Lista, Detalhe, Status, SSE | 7 |
| **Sprint 2** | Comunicação + UX | Templates WhatsApp, Resposta custom, Onboarding, PWA | 5 |
| **Sprint 3** | Qualidade + Lançamento | Testes, Relatórios, Config, Segurança, Deploy prod | 5 |

---

## Sprint 0: Setup + Fundação (3 dias)

### Dia 1: Repositório + CI/CD + Infra
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Init repo Git + GitHub | 30min | - | ☐ |
| Next.js 14 + TypeScript + ESLint + Prettier | 1h | - | ☐ |
| Tailwind CSS + shadcn/ui setup | 1h | - | ☐ |
| Prisma + Neon (schema inicial: Loja, Usuario, Pedido, PedidoItem) | 2h | - | ☐ |
| Vercel project + preview deployments | 1h | Repo | ☐ |
| GitHub Actions CI (lint, typecheck, test, build) | 2h | - | ☐ |
| Environment variables (Vercel + .env.example) | 30min | - | ☐ |

### Dia 2: Auth + Design System
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| NextAuth v5 config (Magic Link + Credentials) | 3h | Prisma User/Account/Session | ☐ |
| Resend integration (Magic Link email) | 1h | Resend account | ☐ |
| Login page + Callback page | 1h | NextAuth | ☐ |
| Middleware auth protection + rate limit | 1h | NextAuth | ☐ |
| Design tokens CSS + Tailwind config | 2h | - | ☐ |
| Componentes base: Button, Input, Badge, Card, Modal | 3h | Tokens | ☐ |

### Dia 3: Dashboard Shell + SSE
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Dashboard layout (Header, BottomNav, SSE Provider) | 2h | Auth, Components | ☐ |
| SSE endpoint (`/api/events`) + broadcast | 2h | Next.js Route Handlers | ☐ |
| SSE client hook (EventSource + reconnect) | 1h | - | ☐ |
| Empty state + Loading skeletons | 1h | Components | ☐ |
| Health check endpoint (`/api/health`) | 30min | - | ☐ |
| Deploy preview + smoke test | 30min | CI/CD | ☐ |

**Definition of Done Sprint 0:**
- [ ] `main` branch deploya automaticamente no Vercel
- [ ] PRs criam preview deployments
- [ ] Login Magic Link funciona (email chega, link valida, redireciona dashboard)
- [ ] Login Credentials funciona (hash senha, JWT, refresh token)
- [ ] Middleware protege `/dashboard` e `/api/*`
- [ ] Design tokens aplicados (light/dark mode)
- [ ] Componentes base renderizam sem erros
- [ ] SSE conecta e recebe eventos de teste
- [ ] Health check retorna 200 + deps status

---

## Sprint 1: Core - Pedidos + WhatsApp (7 dias)

### Dia 4-5: Webhook WhatsApp + Parser
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| WhatsApp Cloud API setup (Meta Business) | 1h | Conta Meta | ☐ |
| Webhook endpoint `/api/webhook/whatsapp` | 2h | - | ☐ |
| HMAC validation (X-Hub-Signature-256) | 1h | - | ☐ |
| Idempotência (whatsappMessageId unique) | 1h | Prisma schema | ☐ |
| Parser de mensagem texto livre (regex + heurísticas) | 3h | - | ☐ |
| Dead letter queue (tabela + retry logic) | 1h | - | ☐ |
| Testes unitários parser (10+ casos) | 2h | - | ☐ |
| Testes integração webhook (mock WhatsApp) | 2h | - | ☐ |

### Dia 6: Criação de Pedido + Banco
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Prisma schema completo (Pedido, Item, Historico, Mensagem) | 1h | - | ☐ |
| Server Action `createPedido` (transação: pedido + itens + historico) | 2h | Parser | ☐ |
| Broadcast SSE `novo_pedido` após criação | 1h | SSE | ☐ |
| Índices otimizados (loja+status+data, telefone+status) | 30min | - | ☐ |
| Seed desenvolvimento (loja demo, produtos, horários) | 30min | - | ☐ |

### Dia 7-8: Lista de Pedidos (Dashboard)
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Server Component lista paginada (20/page) | 2h | Prisma | ☐ |
| Filtros: status chips + data (hoje/semana/mês/custom) | 2h | - | ☐ |
| Busca por cliente (nome/telefone) | 1h | - | ☐ |
| Badges contadores por status (atualizados via SSE) | 1h | SSE | ☐ |
| PedidoCard componente + estados (loading, empty, error) | 2h | Components | ☐ |
| Otimistic UI: atualização imediata + confirmação SSE | 2h | SSE | ☐ |
| Testes E2E: lista carrega, filtra, busca, paginação | 2h | - | ☐ |

### Dia 9: Detalhe do Pedido + Ações
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Página `/pedidos/[id]` (Server Component + Client actions) | 2h | - | ☐ |
| Timeline de status (PedidoStatusHistorico) | 1h | - | ☐ |
| Server Actions: `updateStatus`, `cancelarPedido` | 2h | Prisma | ☐ |
| Validação transições (state machine RN-003) | 1h | - | ☐ |
| Broadcast SSE `status_atualizado` | 1h | SSE | ☐ |
| Modal cancelar (confirmação + motivo) | 1h | Modal component | ☐ |
| Testes E2E: fluxo completo novo → entregue | 2h | - | ☐ |

### Dia 10: Tempo Real (SSE) Polish
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Reconexão automática + last-event-id | 1h | - | ☐ |
| Heartbeat 30s + cleanup aba hidden > 5min | 1h | - | ☐ |
| Indicador visual "LIVE" pulsando no header | 30min | - | ☐ |
| Toast "Reconectando..." se > 5s | 30min | Toast | ☐ |
| Teste carga: 50 conexões SSE simultâneas | 1h | - | ☐ |

**Definition of Done Sprint 1:**
- [ ] Webhook recebe mensagem WhatsApp → ACK < 5s
- [ ] Parser extrai itços corretamente (80%+ casos teste)
- [ ] Pedido criado com status NOVO, itens, total, histórico
- [ ] Dashboard lista pedidos < 500ms (p95)
- [ ] Filtros status + data funcionam
- [ ] SSE: novo pedido aparece em < 1s sem reload
- [ ] Detalhe mostra itens, obs, timeline, ações
- [ ] Ações status: novo→preparando→pronto→entregue (1 clique cada)
- [ ] Cancelar com confirmação funciona
- [ ] Transições inválidas rejeitadas
- [ ] SSE reconecta automaticamente em queda rede

---

## Sprint 2: Comunicação + UX (5 dias)

### Dia 11: Templates WhatsApp + Envio
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| WhatsApp Service (templates, texto livre, mídia) | 2h | Meta templates aprovados | ☐ |
| Server Action `enviarMensagem` (template + custom) | 2h | WhatsApp Service | ☐ |
| Templates pré-definidos (6 templates MVP) | 1h | - | ☐ |
| Modal Responder (templates cards + textarea custom) | 2h | Modal, Components | ☐ |
| Histórico mensagens no detalhe | 1h | - | ☐ |
| Rate limit + retry + circuit breaker WhatsApp | 1h | - | ☐ |
| Testes integração envio (mock API) | 1h | - | ☐ |

### Dia 12: Onboarding Completo
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Página `/onboarding/1-loja` (nome + telefone) | 1h | - | ☐ |
| Página `/onboarding/2-whatsapp` (instruções + validação token) | 2h | WhatsApp Service | ☐ |
| Página `/onboarding/3-horario` (presets + grid 7 dias) | 2h | ScheduleGrid component | ☐ |
| Página `/onboarding/4-teste` (envia pedido fake) | 1h | Webhook | ☐ |
| Fluxo completo: login → onboarding → dashboard | 1h | - | ☐ |
| Teste usabilidade: 3 usuários reais completam < 5min | 2h | Usuários | ☐ |

### Dia 13: PWA + Offline Básico
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| `next-pwa` config (manifest, service worker) | 1h | - | ☐ |
| Service Worker: cache static + API GET (stale-while-revalidate) | 2h | - | ☐ |
| Offline fallback: mostra última lista cached | 1h | SW | ☐ |
| Install prompt + "Adicionar à tela inicial" | 1h | - | ☐ |
| Ícones PWA (192, 512, maskable) | 30min | - | ☐ |
| Teste Lighthouse PWA score ≥ 90 | 30min | - | ☐ |

### Dia 14: Relatórios + Configurações
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Página `/relatorios` (Hoje, Semana, Mês, Custom) | 2h | - | ☐ |
| Cards métricas + gráfico barras por hora | 1h | ReportChart | ☐ |
| Tabela pedidos entregues + export CSV | 1h | - | ☐ |
| `/configuracoes/produtos` (CRUD + autocomplete categoria) | 2h | ProductCard | ☐ |
| `/configuracoes/horarios` (grid + presets + mensagem fechado) | 1h | ScheduleGrid | ☐ |
| Sugestão produtos ao criar pedido manual (Could) | 1h | - | ☐ |

### Dia 15: Polish UX + Acessibilidade
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Revisão acessibilidade completa (checklist) | 2h | A11y Checklist | ☐ |
| Touch targets 44px, focus visible, ARIA labels | 1h | - | ☐ |
| Reduced motion respeitado | 30min | - | ☐ |
| Dark mode polish | 30min | - | ☐ |
| Toasts para todas ações (sucesso/erro) | 1h | Toast | ☐ |
| Loading states em todos botões/actions | 1h | - | ☐ |
| Error boundaries + fallback UI | 1h | - | ☐ |

**Definition of Done Sprint 2:**
- [ ] Templates WhatsApp aprovados no Meta funcionando
- [ ] Responder cliente: template (1 clique) + custom funciona
- [ ] Onboarding 4 passos < 5 min, 80%+ completam sem ajuda
- [ ] PWA instalável, funciona offline (última lista cached)
- [ ] Relatórios: hoje/semana/mês com métricas corretas
- [ ] Config: produtos CRUD, horários grid + presets
- [ ] Acessibilidade: 0 violações axe critical/serious
- [ ] Lighthouse PWA ≥ 90, A11y ≥ 95

---

## Sprint 3: Qualidade + Lançamento (5 dias)

### Dia 16: Testes Automatizados
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Testes unitários: parser, validadores, utils (≥80% coverage) | 2h | - | ☐ |
| Testes integração: Server Actions (Prisma test DB) | 2h | - | ☐ |
| Testes E2E Playwright: fluxos core (5 cenários) | 3h | - | ☐ |
| Testes E2E: auth (login, logout, session refresh) | 1h | - | ☐ |
| Testes E2E: acessibilidade (axe no Playwright) | 1h | - | ☐ |
| CI: roda testes em every PR + build | 1h | GitHub Actions | ☐ |

### Dia 17: Segurança + LGPD
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Security headers (CSP, HSTS, etc) no next.config.js | 1h | - | ☐ |
| Rate limit auth (30/min/IP) + APIs (100/min/user) | 1h | - | ☐ |
| CSRF protection (NextAuth built-in + forms) | 30min | - | ☐ |
| LGPD: Consentimento no onboarding + ConsentimentoLGPD table | 1h | - | ☐ |
| LGPD: Exportar dados usuário (`/api/user/data`) | 1h | - | ☐ |
| LGPD: Excluir conta (anonimiza + revoga tokens) | 1h | - | ☐ |
| Política privacidade + Termos (páginas estáticas) | 1h | - | ☐ |
| Auditoria segredos (nenhum no repo, apenas env) | 30min | - | ☐ |

### Dia 18: Observabilidade + Performance
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Logs estruturados (pino/console JSON) | 1h | - | ☐ |
| Vercel Analytics + Web Vitals | 30min | - | ☐ |
| Custom metrics: webhook latency, SSE latency, error rate | 1h | - | ☐ |
| Load test k6: 100 req/s webhook + 50 SSE | 2h | - | ☐ |
| Otimização queries (explain analyze, índices) | 1h | - | ☐ |
| Bundle analysis (next-bundle-analyzer) | 30min | - | ☐ |

### Dia 19: Deploy Produção + Smoke Test
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| Domínio customizado + SSL (Vercel) | 1h | Domínio | ☐ |
| Variáveis produção (tokens long-lived, URLs prod) | 30min | - | ☐ |
| Webhook URL produção registrada no Meta | 30min | - | ☐ |
| Deploy production (`main` branch) | 30min | CI passa | ☐ |
| Smoke test produção (fluxo completo real) | 1h | - | ☐ |
| Monitoramento 1h pós-deploy (logs, erros, métricas) | 1h | - | ☐ |

### Dia 20: Documentação + Handoff
| Tarefa | Estimativa | Dependência | Status |
|--------|------------|-------------|--------|
| README projeto (setup, deploy, comandos) | 1h | - | ☐ |
| ADRs finalizados (stack, data model, auth, SSE, async) | 1h | - | ☐ |
| Runbook incidentes (webhook down, SSE down, DB down) | 1h | - | ☐ |
| Changelog v1.0.0 | 30min | - | ☐ |
| Retrospectiva + lições aprendidas | 1h | - | ☐ |
| Comemoração! 🎉 | - | - | ☐ |

**Definition of Done Sprint 3 (MVP COMPLETO):**
- [ ] Testes: Unit ≥80%, Integration core, E2E 5 fluxos, A11y automatizado
- [ ] Segurança: Headers, rate limit, CSRF, segredos em env, LGPD compliance
- [ ] Observabilidade: Logs, métricas, alertas básicos
- [ ] Performance: Load test passa, bundle < 200KB gzipped
- [ ] Produção: Deploy funcionando, domínio custom, webhook real ativo
- [ ] Documentação: README, ADRs, Runbook, Changelog
- [ ] Usuário real processa 10 pedidos sem ajuda (critério ETAPA 03)

---

## Estimativas por Épico (Story Points / Dias)

| Épico | Pontos | Dias | Sprint |
|-------|--------|------|--------|
| Setup + CI/CD + Infra | 8 | 1 | 0 |
| Auth (Magic Link + Credentials) | 13 | 2 | 0 |
| Design System + Componentes Base | 13 | 2 | 0 |
| Dashboard Shell + SSE | 8 | 1 | 0 |
| WhatsApp Webhook + Parser | 21 | 3 | 1 |
| Criação Pedido + Banco | 8 | 1 | 1 |
| Lista Dashboard + Filtros | 13 | 2 | 1 |
| Detalhe + Ações Status | 13 | 2 | 1 |
| SSE Polish | 5 | 1 | 1 |
| WhatsApp Templates + Envio | 13 | 2 | 2 |
| Onboarding (4 passos) | 13 | 2 | 2 |
| PWA + Offline | 8 | 1 | 2 |
| Relatórios + Config | 13 | 2 | 2 |
| Polish UX + A11y | 8 | 1 | 2 |
| Testes Automatizados | 13 | 2 | 3 |
| Segurança + LGPD | 8 | 1 | 3 |
| Observabilidade + Performance | 8 | 1 | 3 |
| Deploy Produção | 5 | 1 | 3 |
| Documentação | 5 | 1 | 3 |
| **TOTAL** | **~190** | **~25** | **4 semanas** |

> **Buffer:** 5 dias (20% do total) para imprevistos, code review, ajustes.

---

## Rastreabilidade: Requisitos → Tarefas

| Requisito | Tarefa Sprint | Critério Aceitação |
|-----------|---------------|-------------------|
| RF-001..005 (Auth) | Sprint 0 Dia 2 | Login Magic Link + Credentials |
| RF-010..019 (Webhook) | Sprint 1 Dia 4-5 | Webhook HMAC + Idempotência |
| RF-015 (Parser) | Sprint 1 Dia 4-5 | Extração itens 80%+ |
| RF-016 (Criar Pedido) | Sprint 1 Dia 6 | Pedido NOVO persistido |
| RF-020..027 (Lista) | Sprint 1 Dia 7-8 | Lista < 500ms, filtros |
| RF-030..034 (Status) | Sprint 1 Dia 9 | Fluxo completo + SSE |
| RF-040..043 (Mensagens) | Sprint 2 Dia 11 | Templates + Custom |
| RF-050..053 (SSE) | Sprint 0-1 | Tempo real < 1s |
| RF-060..062 (Relatórios) | Sprint 2 Dia 14 | Métricas corretas |
| RF-070..081 (Config) | Sprint 2 Dia 14 | Produtos + Horários |
| RNF-001..007 (Perf) | Sprint 1-3 | Load test passa |
| RNF-010..019 (Seg) | Sprint 3 Dia 17 | Headers, rate limit, LGPD |
| RNF-030..036 (UX) | Sprint 2 Dia 15 | A11y 0 critical/serious |

---

## Riscos e Mitigações (Atualizado)

| Risco | Prob. | Impacto | Mitigação | Dono | Status |
|-------|-------|---------|-----------|------|--------|
| WhatsApp template rejection | Média | Alto | Submeter templates Dia 1, ter fallback texto livre | Você | ⏳ |
| Parser falha pedidos complexos | Média | Médio | Dead letter + alerta + revisão manual + melhoria iterativa | Você | 📝 |
| Vercel 10s timeout webhook | Alta | Alto | ACK síncrono < 5s, processamento assíncrono (Inngest) | Você | 📝 |
| Neon cold start > 500ms | Média | Médio | Keep-warm cron 5min, connection pooling | Você | 📝 |
| SSE connections limit Vercel | Baixa | Médio | Heartbeat + cleanup, monitorar, upgrade Pro se necessário | Você | 📝 |
| Escopo creep (features "Could") | Alta | Médio | **Congelar escopo Sprint 1**, revisar só no planejamento Sprint 2 | Você | ✅ |
| Doença/imprevisto pessoal | Baixa | Alto | Buffer 5 dias, priorizar Must, cortar Could | Você | 📝 |

---

## Checklist Planejamento (Validação ETAPA 09)

- [ ] Sprints definidos com foco claro e entregável
- [ ] Tarefas granulares (máx 3h cada) com estimativas
- [ ] Dependências entre tarefas mapeadas
- [ ] Definition of Done por sprint + MVP
- [ ] Rastreabilidade Requisitos → Tarefas → Critérios
- [ ] Riscos identificados com mitigações
- [ ] Buffer realista (20%)
- [ ] Capacidade: 1 pessoa × 6h/dia focadas = 30h/semana
- [ ] Ferramentas: GitHub Projects / Linear / Notion para tracking
- [ ] Daily standup (solo: revisão diária 15min: feito, fazendo, bloqueios)

---

## Próxima Etapa
[[02 - Modelos/Desenvolvimento/Guia de Vibe Coding com IA]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/05 - Preparação e Desenvolvimento/ETAPA 09 - Preparar o Ambiente e o Plano de Implementação]]
- [[01 - Manual de Desenvolvimento/05 - Preparação e Desenvolvimento/ETAPA 10 - Desenvolver o Software por Partes]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos Funcionais]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]]
- [[02 - Modelos/Desenvolvimento/Guia de Vibe Coding com IA]]
- [[02 - Modelos/Desenvolvimento/Checklist de Código]]
- [[04 - Referência/Glossário/Sprint]]
- [[04 - Referência/Glossário/Definition of Done]]
- [[04 - Referência/Glossário/Vibe Coding]]