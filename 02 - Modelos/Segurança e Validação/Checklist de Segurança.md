# Checklist de Segurança

> **Copie para:** `05 - Projetos/SeuProjeto/07 - Segurança e Validação/Checklist de Segurança.md`
> Baseado na ETAPA 12. **Checklist obrigatório de segurança** para revisão pré-produção e auditoria contínua.

---

## Como Usar

1. **Pré-Produção:** Complete todo checklist antes do primeiro deploy production
2. **Contínuo:** Revisar itens marcados "Contínuo" a cada sprint
3. **Incidente:** Usar como guia de investigação se houver breach
4. **Auditoria:** Entregar para auditor externo se necessário

---

## 1. Autenticação e Autorização

### 1.1 Autenticação
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| A1 | Magic Link expira em 15 min e uso único | ☐ | Testes: TC-AUTH-002, 003 | Contínuo |
| A2 | Credentials: hash Argon2id/bcrypt, rate limit 5/15min | ☐ | Código + TC-AUTH-005 | Contínuo |
| A3 | JWT access token: 1h, HS256, segredo 32+ chars | ☐ | Config NextAuth | Contínuo |
| A4 | Refresh token: rotação a cada uso, 30d, max 5 sessões | ☐ | TC-AUTH-006, RN-021 | Contínuo |
| A5 | Logout invalida refresh token no DB (não só cookie) | ☐ | TC-AUTH-007 | Contínuo |
| A6 | Middleware protege TODAS rotas `/dashboard/*` e `/api/*` | ☐ | TC-AUTH-008 | Contínuo |
| A7 | Rate limit auth endpoints: 30 req/min/IP | ☐ | TC-AUTH-009 | Contínuo |
| A8 | CSRF protection: NextAuth built-in + forms customizados | ☐ | Código middleware | Contínuo |
| A9 | Prevenção enumeração usuário: mensagem genérica "Email ou senha incorretos" | ☐ | TC-AUTH-005 | Contínuo |

### 1.2 Autorização (RBAC)
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| A10 | Roles: OWNER (total) vs FUNCIONARIO (pedidos, relatórios) | ☐ | Código `requireRole` | Contínuo |
| A11 | Server Actions validam role antes de mutação | ☐ | Todas actions | Contínuo |
| A12 | API Routes validam role (se houver) | ☐ | Middleware/API | Contínuo |
| A13 | UI esconde ações não autorizadas (não só backend) | ☐ | Componentes condicionais | Contínuo |

---

## 2. Proteção de Dados

### 2.1 Em Trânsito
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| D1 | HTTPS obrigatório em todas conexões (HSTS) | ☐ | `next.config.js` headers | Contínuo |
| D2 | TLS 1.2+ (Vercel/Neon/Resend gerenciam) | ☐ | Vercel/Neon dashboard | Contínuo |
| D3 | Cookies: `Secure; HttpOnly; SameSite=Lax` | ☐ | NextAuth config | Contínuo |
| D4 | Webhook WhatsApp: HMAC SHA256 validado 100% | ☐ | TC-WHATS-001, 002, 003 | Contínuo |

### 2.2 Em Repouso
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| D5 | Segredos APENAS em variáveis de ambiente (Vercel/Neon) | ☐ | Auditoria repo + env | Contínuo |
| D6 | Zero segredos no código, build logs, Dockerfiles | ☐ | `git log --all --full-history --oneline -- "**/*secret*"` | Contínuo |
| D7 | Banco Neon: criptografia em repouso (default) | ☐ | Neon dashboard | Contínuo |
| D8 | PII (nome, telefone) - considerar criptografia campo a campo | ☐ | LGPD assessment | Pré-prod |
| D9 | Backups Neon: PITR ativo, retenção 7d (free) / 30d (paid) | ☐ | Neon dashboard | Contínuo |

### 2.3 Minimização e Retenção
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| D10 | Coleta apenas dados necessários (email, telefone, nome opcional) | ☐ | Schema Prisma | Contínuo |
| D11 | Logs de auditoria: retenção 2 anos (status, mensagens) | ☐ | RN-032 | Contínuo |
| D12 | Logs de acesso (login): retenção 1 ano | ☐ | RN-032 | Contínuo |
| D13 | Dead letters: retenção 30 dias, depois purge | ☐ | Cleanup cron | Contínuo |

---

## 3. Segurança de Aplicação

### 3.1 Input Validation & Sanitization
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| S1 | Zod schema em TODOS inputs externos (webhook, actions, forms, API) | ☐ | Grep `z.object` | Contínuo |
| S2 | Validação server-side **sempre** (client-side só UX) | ☐ | Server Actions | Contínuo |
| S3 | XSS: sem `dangerouslySetInnerHTML` sem sanitização | ☐ | Grep `dangerouslySetInnerHTML` | Contínuo |
| S4 | SQL Injection: Prisma ORM (parametrizado), sem raw SQL não sanitizado | ☐ | Código review | Contínuo |
| S5 | Upload de arquivos: validação tipo, tamanho, armazenamento seguro (Vercel Blob) | ☐ | Could v1.1 | Pré-prod |

### 3.2 Headers de Segurança
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| S6 | CSP: `default-src 'self'; script-src 'self' 'unsafe-eval' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data: blob: https://*.vercel.app https://*.public.blob.vercel-storage.com; connect-src 'self' https://*.vercel.app wss://*.vercel.app https://graph.facebook.com https://api.resend.com` | ☐ | `next.config.js` | Contínuo |
| S7 | HSTS: `max-age=31536000; includeSubDomains; preload` | ☐ | `next.config.js` | Contínuo |
| S8 | X-Frame-Options: `DENY` | ☐ | `next.config.js` | Contínuo |
| S9 | X-Content-Type-Options: `nosniff` | ☐ | `next.config.js` | Contínuo |
| S10 | Referrer-Policy: `strict-origin-when-cross-origin` | ☐ | `next.config.js` | Contínuo |
| S11 | Permissions-Policy: `camera=(), microphone=(), geolocation=()` | ☐ | `next.config.js` | Contínuo |

### 3.3 Rate Limiting & DoS Protection
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| S12 | Auth endpoints: 30 req/min/IP | ☐ | TC-AUTH-009 | Contínuo |
| S13 | APIs autenticadas: 100 req/min/usuário | ☐ | Middleware | Contínuo |
| S14 | Webhook: ACK < 5s, processamento assíncrono (queue) | ☐ | Arquitetura SSE | Contínuo |
| S15 | SSE: heartbeat 30s, cleanup aba hidden > 5min, max conexões monitorado | ☐ | Código SSE | Contínuo |

---

## 4. Integrações Externas

### 4.1 WhatsApp Cloud API
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| I1 | Token WhatsApp: long-lived (60 dias), armazenado em env, rotação monitorada | ☐ | Meta dashboard | Contínuo |
| I2 | Templates aprovados no Meta antes de usar | ☐ | Meta Business Manager | Contínuo |
| I3 | Respeita janela 24h: texto livre só dentro, template fora | ☐ | TC-MSG-003, RN-012 | Contínuo |
| I4 | Rate limit Meta: 100 msg/s, queue local + backoff | ☐ | Código WhatsApp service | Contínuo |
| I5 | Webhook URL: HTTPS, domínio verificado, challenge token | ☐ | Meta config | Contínuo |

### 4.2 Resend (Email)
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| I6 | API Key Resend em env, domínio verificado | ☐ | Resend dashboard | Contínuo |
| I7 | Rate limit: 3k/dia free, queue se burst | ☐ | Código email service | Contínuo |
| I8 | Templates email: sem PII desnecessário, unsubscribe se marketing | ☐ | LGPD | Contínuo |

### 4.3 Neon (Database)
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| I8 | Connection string com `pgbouncer=true` + `sslmode=require` | ☐ | `.env` | Contínuo |
| I9 | Branch `main` = production, `preview-*` = PRs (isolamento) | ☐ | Neon branching | Contínuo |
| I10 | PITR ativo, RPO < 1h, RTO < 4h | ☐ | Neon dashboard | Contínuo |

---

## 5. LGPD / GDPR Compliance

| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| L1 | Base legal documentada: Legítimo interesse (pedido) + Consentimento (marketing) | ☐ | RN-030 | Contínuo |
| L2 | Consentimento granular por finalidade (NECESSARIO, FUNCIONAL, MARKETING) | ☐ | Tabela `ConsentimentoLGPD` | Contínuo |
| L3 | Política de Privacidade acessível, clara, em PT-BR, versão datada | ☐ | Página `/privacidade` | Contínuo |
| L4 | Termos de Uso acessíveis, aceitação no onboarding | ☐ | Página `/termos` | Contínuo |
| L5 | Direito de Acesso: endpoint `/api/user/data` exporta tudo (JSON) | ☐ | TC-LGPD-001 | Contínuo |
| L6 | Direito de Exclusão: anonimiza PII, mantém dados fiscais (pedidos entregues) | ☐ | TC-LGPD-002, RN-031 | Contínuo |
| L7 | Direito de Portabilidade: export JSON estruturado | ☐ | Mesmo endpoint acesso | Contínuo |
| L8 | Direito de Retificação: editar perfil / solicitar correção | ☐ | Perfil usuário | Contínuo |
| L9 | DPO/Encarregado definido (mesmo que seja você) | ☐ | Documentado | Contínuo |
| L10 | Registro de atividades de tratamento (ROPA) - simplified para micro | ☐ | Planilha/documento | Contínuo |
| L11 | DPIA (Relatório de Impacto) - se alto risco (provavelmente não para MVP) | ☐ | Avaliação | Pré-prod |
| L12 | Data Breach Plan: notificação ANPD em 48h, usuários afetados | ☐ | Runbook incidente | Contínuo |
| L13 | Cookies: consentimento para não-essenciais (analytics) | ☐ | Banner cookies | Contínuo |

---

## 6. Infraestrutura e Operações

### 6.1 Vercel
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| O1 | Deploy apenas via `main` branch (preview em PRs) | ☐ | Vercel git integration | Contínuo |
| O2 | Variáveis de ambiente: Production vs Preview separadas | ☐ | Vercel dashboard | Contínuo |
| O3 | Function timeout: 10s Hobby / 60s Pro (monitorar) | ☐ | Vercel limits | Contínuo |
| O4 | Edge Functions: apenas onde necessário (middleware) | ☐ | Arquitetura | Contínuo |
| O5 | Logs: Vercel Logs retidos 30d (Hobby) / 1y (Pro) | ☐ | Vercel plan | Contínuo |

### 6.2 Monitoramento e Alertas
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| O6 | Health check: `/api/health` retorna 200 + deps status (DB, Redis) | ☐ | TC-REL-001 | Contínuo |
| O7 | Uptime monitoring: Upptime / UptimeRobot (5 min) | ☐ | Externo | Contínuo |
| O8 | Alertas: Error rate > 1%, Webhook latency > 5s, SSE desconexões > 10% | ☐ | Vercel/Logs | Contínuo |
| O9 | Logs estruturados (JSON): requestId, userId, timestamp, level, message | ☐ | Código logging | Contínuo |
| O10 | Correlação: requestId propagado (middleware → actions → DB) | ☐ | Middleware | Contínuo |

### 6.3 Gestão de Incidentes
| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| O11 | Runbook incidentes: Webhook down, SSE down, DB down, Auth down | ☐ | Documento `RUNBOOK.md` | Contínuo |
| O12 | Rollback: `vercel rollback` < 2 min para deployment anterior | ☐ | Vercel CLI | Contínuo |
| O13 | Comunicação: Status page / canal aviso usuários se downtime > 10 min | ☐ | Plano comunicação | Contínuo |

---

## 7. Desenvolvimento Seguro (DevSecOps)

| # | Item | Status | Evidência | Frequência |
|---|------|--------|-----------|------------|
| D1 | SAST: CodeQL / Semgrep no CI (0 high/critical) | ☐ | GitHub Actions | Todo PR |
| D2 | Dependencies: `npm audit` + Snyk/Dependabot (0 high/critical) | ☐ | CI + Dependabot | Contínuo |
| D3 | Secrets scanning: TruffleHog / GitLeaks no CI | ☐ | GitHub Actions | Todo PR |
| D4 | Container/Base image: Node 20 LTS slim, non-root user | ☐ | Dockerfile (se usar) | Contínuo |
| D5 | Commit assinado (GPG) - opcional para solo | ☐ | Git config | Contínuo |
| D6 | Branch protection: `main` requer CI pass + review (mesmo solo: auto-review) | ☐ | GitHub branch rules | Contínuo |

---

## 8. Testes de Segurança (Validação)

| # | Teste | Ferramenta | Critério Pass | Frequência |
|---|-------|------------|---------------|------------|
| T1 | Auth bypass: rotas protegidas retornam 401/403 | Playwright manual | 100% rotas | Sprint 3 |
| T2 | Rate limit: burst > limite → 429 | k6 custom | Bloqueia | Sprint 3 |
| T3 | HMAC webhook: payloads válidos/inválidos | Integration tests | 100% | Contínuo |
| T4 | XSS: tentativas injeção em inputs | Playwright + payloads | Sanitizado/Rejeitado | Sprint 3 |
| T5 | CSP: `eval` bloqueado, inline só permitido onde necessário | Lighthouse + Manual | CSP válido | Sprint 3 |
| T6 | LGPD: fluxo exportar/excluir dados | E2E | Funciona | Sprint 3 |
| T7 | Secrets: zero no repo, build, artifacts | TruffleHog | 0 findings | Todo PR |

---

## 9. Checklist de Prontidão para Produção (Go/No-Go)

> **Todos itens abaixo devem ser ✅ para deploy production**

| Categoria | Item | ✅ |
|-----------|------|----|
| **Auth** | Magic Link + Credentials funcionando, rate limit, RBAC | ☐ |
| **Auth** | Refresh rotation, logout invalida, session max 5 | ☐ |
| **Dados** | HTTPS+HSTS, cookies secure, segredos só env, PII proteção | ☐ |
| **Dados** | Backups PITR, retenção logs definida | ☐ |
| **App** | Zod em todos inputs, CSP+HSTS+headers, rate limits | ☐ |
| **App** | XSS/SQLi prevenidos, HMAC webhook 100% | ☐ |
| **Integrações** | WhatsApp tokens rotacionados, templates aprovados, janela 24h | ☐ |
| **Integrações** | Resend domínio verificado, Neon branching + PITR | ☐ |
| **LGPD** | Consentimento, privacidade, termos, acesso/exclusão/portabilidade | ☐ |
| **LGPD** | Breach plan, DPO, retenção documentada | ☐ |
| **Infra** | Vercel prod config, health check, uptime monitoring, alertas | ☐ |
| **Infra** | Runbook incidentes, rollback testado, comunicação planejada | ☐ |
| **DevSecOps** | SAST, deps, secrets scan no CI, branch protection | ☐ |
| **Testes** | Load test passa, security tests passam, regression suite verde | ☐ |
| **Observabilidade** | Logs estruturados, requestId, métricas core, dashboards | ☐ |

---

## 10. Auditoria Contínua (Mensal)

- [ ] Revisar logs de acesso suspeitos (múltiplas falhas login, IPs estranhos)
- [ ] Verificar alertas de segurança (Vercel, Neon, GitHub Dependabot)
- [ ] Atualizar dependências (patch/minor sem breaking)
- [ ] Rotacionar segredos se suspeita de vazamento (WhatsApp token, NextAuth secret)
- [ ] Revisar permissões: quem tem acesso Vercel/Neon/GitHub/Resend
- [ ] Testar restore backup (point-in-time recovery) - trimestral
- [ ] Revisar runbook incidentes - atualizar se arquitetura mudou
- [ ] Verificar conformidade LGPD: consentimentos ativos, retenção respeitada

---

## Próxima Etapa
[[02 - Modelos/Segurança e Validação/Plano de Validação]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/07 - Segurança e Validação/ETAPA 12 - Revisar Segurança, Privacidade e Confiabilidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Regras de Negócio]]
- [[02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth]]
- [[02 - Modelos/Testes/Casos de Teste]]
- [[02 - Modelos/Segurança e Validação/Plano de Validação]]
- [[04 - Referência/Glossário/LGPD]]
- [[04 - Referência/Glossário/GDPR]]
- [[04 - Referência/Glossário/OWASP]]
- [[04 - Referência/Glossário/CSP]]