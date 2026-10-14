# ETAPA 12 — Revisar Segurança, Privacidade e Confiabilidade

## Objetivo

Realizar verificações **obrigatórias** de segurança, privacidade e confiabilidade antes de disponibilizar o software para usuários reais.

## Por que esta etapa existe?

Segurança não é "feature" — é **requisito de base**. Falhas aqui causam vazamento de dados, prejuízo financeiro, perda de confiança e problemas legais. IA NÃO substitui revisão humana consciente.

## Quando começar

- ETAPA 11 concluída (Quality Gates atendidos, suite de testes passando)
- Código "feature complete" e estável
- Pronto para validação final e deploy

## O que você precisa fazer

1. **Proteção de credenciais e segredos** — variáveis de ambiente, keys, tokens, rotation
2. **Tratamento de dados pessoais** — LGPD/GDPR, consentimento, minimização, retenção, exclusão
3. **Autenticação e autorização** — senhas, MFA, sessões, tokens, RBAC/ABAC, testes de bypass
4. **Validação e sanitização de entradas** — Zod schemas, XSS, SQL injection, upload, rate limiting
5. **Exposição de informações** — error messages, logs, headers, debug endpoints, source maps
6. **Dependências vulneráveis** — `pnpm audit`, Snyk, dependabot, lockfile, versões fixas
7. **Configurações inseguras** — HTTPS, CSP, HSTS, cookies secure, CORS, headers de segurança
8. **Proteção de operações sensíveis** — CSRF, idempotência, auditoria, confirmações
9. **Tratamento de erros e logging** — não vazar stack traces, logs estruturados, PII nos logs
10. **Registros e monitoramento** — error tracking, uptime, alertas, auditoria de ações sensíveis
11. **Recuperação de falhas** — backup, restore, RPO/RTO, disaster recovery, chaos testing
12. **Cópias de segurança** — estratégia, frequência, teste de restore, criptografia
13. **Requisitos legais e regulatórios** — LGPD, PCI-DSS (se pagamentos), setoriais
14. **Produza o documento: Revisão de Segurança + Checklist de Conformidade**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Verificar segredos (nenhum no git, rotation) | ✅ VALIDAÇÃO (você executa checks) |
| Verificar dados pessoais (LGPD) | 👤 USUÁRIO (decisão legal) + 🤖 IA (checklist) |
| Testar auth/autorização (bypass, RBAC) | 🤖 IA (tenta burlar) + ✅ VALIDAÇÃO (confirma) |
| Validar entradas (XSS, SQLi, upload) | 🤖 IA (testes automatizados) + ✅ VALIDAÇÃO |
| Verificar exposição de info (erros, logs, headers) | 🤖 IA (scanners) + ✅ VALIDAÇÃO |
| Auditar dependências (`pnpm audit`, Snyk) | ✅ VALIDAÇÃO (você roda) |
| Verificar configurações (HTTPS, CSP, headers) | 🤖 IA (checklist) + ✅ VALIDAÇÃO |
| Verificar operações sensíveis (CSRF, idempotência) | 🤖 IA (testes) + ✅ VALIDAÇÃO |
| Verificar logging/monitoramento | 👤 USUÁRIO (configura) + 🤖 IA (valida) |
| Verificar backup/restore | 👤 USUÁRIO (testa restore) |
| Verificar conformidade legal | 👤 USUÁRIO (responsabilidade legal) |
| Aprovar revisão final | 👤 USUÁRIO (você assina) |

## Ferramentas recomendadas

| Verificação | Ferramentas |
|-------------|-------------|
| Segredos no código | `git-secrets`, `truffleHog`, `ggshield`, `git log --all --full-history -- "**/.env*"` |
| Dependências | `pnpm audit`, `pnpm audit --prod`, Snyk, GitHub Dependabot, `npm outdated` |
| Headers segurança | `securityheaders.com`, `observatory.mozilla.org`, `curl -I` |
| CSP | `helmet` (Express) ou `next-secure-headers` (Next.js), report-only mode |
| Rate limiting | `arcjet`, `upstash/ratelimit`, middleware Next.js |
| Validação entrada | Zod (já no projeto), testes de fuzzing |
| Auth/Session | NextAuth.js config review, testes de bypass |
| Logs PII | Grep por email, phone, CPF, token em logs |
| Backup/Restore | Teste real de restore em staging |
| LGPD | Checklist ANPD, DPIA se needed |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Revisão de Segurança | `02 - Modelos/Segurança/Revisão de Segurança.md` | `05 - Projetos/SeuProjeto/07 - Segurança/` |
| Checklist de Conformidade | `02 - Modelos/Segurança/Checklist de Conformidade.md` | `05 - Projetos/SeuProjeto/07 - Segurança/` |

### Condicionais
| Documento | Quando | Modelo |
|-----------|--------|--------|
| DPIA (Data Protection Impact Assessment) | Dados sensíveis/alto risco LGPD | `02 - Modelos/Segurança/DPIA.md` |
| PCI-DSS Self-Assessment | Se processa pagamentos | `02 - Modelos/Segurança/PCI-DSS.md` |
| Plano de Resposta a Incidentes | Produção com usuários reais | `02 - Modelos/Segurança/Plano de Resposta a Incidentes.md` |
| Relatório de Teste de Restore | Se backup crítico | `02 - Modelos/Segurança/Relatório de Restore.md` |

### Opcionais
- Penetration Test Report (se contratar terceiro)
- Threat Modeling (STRIDE) documentado

## Como produzir os documentos

### Revisão de Segurança

```markdown
# Revisão de Segurança — PedidosSimples v1

## Resumo Executivo
**Data:** 2026-10-15
**Revisor:** [Seu Nome]
**Versão:** v1.0.0-rc1
**Status:** [APROVADO / CONDICIONAL / REPROVADO]
**Riscos Residuais Aceitos:** [Liste se houver]

---

## 1. Proteção de Credenciais e Segredos

| Item | Verificação | Resultado | Evidência |
|------|-------------|-----------|-----------|
| Nenhum segredo no git | `git-secrets` scan + `git log --all --full-history -- "**/.env*"` | ✅ PASS | Log limpo |
| `.env.local` no `.gitignore` | Verificado | ✅ PASS | `.gitignore` contém `.env*` |
| `.env.example` sem valores reais | Verificado | ✅ PASS | Apenas placeholders |
| Segredos staging/prod apenas na plataforma | Vercel Environment Variables | ✅ PASS | Console Vercel verificado |
| `NEXTAUTH_SECRET` único por ambiente | Gerado com `openssl rand -base64 32` | ✅ PASS | 3 secrets distintos |
| Rotação de segredos documentada | Procedimento no Runbook | ⚠️ PENDENTE | Adicionar no Runbook v2 |

---

## 2. Dados Pessoais e LGPD

| Item | Verificação | Resultado | Evidência |
|------|-------------|-----------|-----------|
| Dados coletados mapeados | User: email, name. Order: customerName, customerPhone | ✅ PASS | Modelo de dados ETAPA 05 |
| Base legal identificada | Legítimo interesse (pedidos) + Consentimento (marketing) | ✅ PASS | Termos de uso |
| Minimização aplicada | Só coleta necessário para funcionamento | ✅ PASS | Schema Prisma |
| Consentimento granular | Checkbox separado para marketing (opcional) | ✅ PASS | Formulário onboarding |
| Direito de exclusão | API `DELETE /api/user/data` + cron job 30d | ✅ PASS | Implementado T3.x |
| Retenção definida | Pedidos: 5 anos (fiscal). User: até exclusão. | ✅ PASS | Política documentada |
| DPIA necessária? | Não (baixo risco, dados não sensíveis) | ✅ N/A | Auto-avaliação ANPD |

---

## 3. Autenticação e Autorização

| Item | Verificação | Resultado | Evidência |
|------|-------------|-----------|-----------|
| Senhas: hash bcrypt (NextAuth) | Config `credentials` provider | ✅ PASS | `auth.ts` verificado |
| Magic Link: token seguro, expiração 1h | `EmailProvider` NextAuth | ✅ PASS | Código verificado |
| Sessão: JWT stateless, expiração 24h | `session.maxAge = 24h` | ✅ PASS | `auth.ts` |
| Refresh token rotation | NextAuth handle automaticamente | ✅ PASS | Docs NextAuth |
| RBAC: OWNER vs STAFF | `role` enum no User, middleware check | ✅ PASS | `middleware.ts` |
| Testes de bypass auth | Tentativa acessar `/dashboard` sem login → 401 | ✅ PASS | Playwright test |
| Testes de escalação privilégio | STAFF não acessa `/configuracoes/integracoes` | ✅ PASS | Playwright test |
| Proteção CSRF | NextAuth CSRF token automático | ✅ PASS | Default NextAuth |
| Rate limit auth endpoints | Arcjet/Upstash em `/api/auth/*` | ⚠️ PENDENTE | Implementar T0.x |

---

## 4. Validação e Sanitização de Entradas

| Vetor | Proteção | Teste | Resultado |
|-------|----------|-------|-----------|
| API Body | Zod schema em TODAS routes | Vitest + fuzzing | ✅ PASS |
| Query Params | Zod schema | Vitest | ✅ PASS |
| Path Params | Zod schema (cuid regex) | Vitest | ✅ PASS |
| Webhook WhatsApp | Verificação assinatura HMAC | Unit test + manual | ✅ PASS |
| Upload arquivos | **Não há upload no MVP** | N/A | N/A |
| XSS (React) | Auto-escape JSX + `dangerouslySetInnerHTML` evitado | Code review + ESLint | ✅ PASS |
| SQL Injection | Prisma ORM (parameterized queries) | Code review | ✅ PASS |
| Rate Limiting API | Arcjet em `/api/pedidos/*` | Manual + load test | ⚠️ PENDENTE |

---

## 5. Exposição de Informações

| Vetor | Verificação | Resultado |
|-------|-------------|-----------|
| Error messages (produção) | Mensagens genéricas ("Erro interno"), detalhes só no log | ✅ PASS |
| Stack traces no cliente | `NODE_ENV=production` oculta | ✅ PASS |
| Source maps em produção | `productionBrowserSourceMaps: false` (default Next.js) | ✅ PASS |
| Logs com PII | Grep: email, phone, CPF, token → 0 ocorrências | ✅ PASS |
| Debug endpoints | Nenhum `/debug`, `/health` exposto sem auth | ✅ PASS |
| Headers reveladores | `X-Powered-By` removido (`next.config.js`) | ✅ PASS |
| API docs (Swagger) | Não exposto em produção | ✅ PASS |

---

## 6. Dependências Vulneráveis

| Check | Comando | Resultado | Ação |
|-------|---------|-----------|------|
| `pnpm audit` | `pnpm audit --prod` | 0 critical, 0 high, 3 moderate | Moderates: `esbuild` (dev only), `webpack` (dev only) — aceito |
| Snyk scan | `snyk test` | 0 vulns em prod deps | ✅ PASS |
| Lockfile commitado | `pnpm-lock.yaml` no git | ✅ PASS | Verificado |
| Versões fixas (sem `^` `~` em prod) | `package.json` dependencies | ✅ PASS | Apenas `^` em devDependencies |
| Dependabot configurado | `.github/dependabot.yml` | ✅ PASS | Weekly, auto-PR |

---

## 7. Configurações de Segurança (Headers, HTTPS, Cookies)

| Configuração | Valor | Verificado |
|--------------|-------|------------|
| HTTPS everywhere | Vercel (auto) + HSTS | ✅ PASS |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains; preload` | ✅ PASS |
| `Content-Security-Policy` | `default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self' data:; connect-src 'self' https://graph.facebook.com wss://*.pusher.com;` | ✅ PASS (report-only testado) |
| `X-Frame-Options` | `DENY` | ✅ PASS |
| `X-Content-Type-Options` | `nosniff` | ✅ PASS |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | ✅ PASS |
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=()` | ✅ PASS |
| Cookies: `Secure` | `true` (produção) | ✅ PASS |
| Cookies: `HttpOnly` | `true` (session) | ✅ PASS |
| Cookies: `SameSite` | `lax` (auth), `strict` (csrf) | ✅ PASS |
| CORS | Restrito a domínios conhecidos (NextAuth config) | ✅ PASS |

---

## 8. Operações Sensíveis

| Operação | Proteção | Teste |
|----------|----------|-------|
| Atualizar status pedido | Idempotência (chave `message_id` única) | Unit test: duplicate webhook → 1 pedido |
| Responder cliente | Confirmação modal + rate limit | Manual: spam click → 1 envio |
| Excluir dados usuário | Confirmação dupla + log auditoria | E2E test |
| Configurar WhatsApp token | Apenas OWNER + validação token formato | Playwright test |

---

## 9. Logging, Monitoramento e Auditoria

| Item | Implementação | Verificado |
|------|---------------|------------|
| Error tracking | Sentry (DSN em env) | ✅ PASS (staging testado) |
| Uptime monitoring | Vercel Analytics + UptimeRobot | ✅ PASS |
| Alertas | Sentry alerts (error rate > 1%) + UptimeRobot (downtime > 5min) | ✅ PASS |
| Logs estruturados | `pino` JSON logs (requestId, userId, action, duration) | ✅ PASS |
| Auditoria ações sensíveis | Tabela `AuditLog` (userId, action, resource, timestamp, metadata) | ✅ PASS (implementado T3.x) |
| PII em logs | Zero (verificado por grep) | ✅ PASS |

---

## 10. Backup, Recuperação e Confiabilidade

| Item | Estratégia | Testado |
|------|------------|---------|
| Banco de dados | Neon: Point-in-time recovery (7 dias) + branch backup diário | ✅ Restore testado em staging |
| Código | GitHub (repo) + Vercel (deploy history) | ✅ Rollback Vercel testado |
| Segredos | 1Password / gerenciador de senhas | ✅ Documentado |
| RPO (Recovery Point Objective) | < 24h (Neon PITR) | ✅ |
| RTO (Recovery Time Objective) | < 1h (Vercel redeploy + Neon restore) | ✅ Testado |
| Disaster Recovery Runbook | Documentado em `RUNBOOK.md` | ⚠️ PENDENTE v2 |

---

## 11. Requisitos Legais e Regulatórios

| Regulamento | Aplicável? | Status | Evidência |
|-------------|------------|--------|-----------|
| LGPD (Brasil) | Sim | ✅ CONFORME | Checklist ANPD atendido |
| Marco Civil da Internet | Sim | ✅ CONFORME | Logs de acesso, neutralidade |
| PCI-DSS | Não (não processa pagamento) | N/A | - |
| Lei do E-commerce (7.962/2013) | Parcial (não é e-commerce direto) | ✅ CONFORME | Info estabelecimento visível |

---

## Riscos Residuais Aceitos

| Risco | Probabilidade | Impacto | Justificativa de Aceitação | Mitigação Futura |
|-------|---------------|---------|---------------------------|------------------|
| Rate limiting API não implementado no MVP | Alta | Médio | MVP foco validação produto. WhatsApp API tem rate limit próprio. | Implementar Arcjet/Upstash v1.1 |
| Runbook DR não documentado | Média | Alto | Tempo limitado. Neon/Vercel têm SLAs. | Documentar v1.1 |
| Rate limit auth endpoints | Média | Médio | NextAuth tem proteção básica. | Arcjet v1.1 |

---

## Aprovação

**Revisor:** [Seu Nome]
**Data:** 2026-10-15
**Assinatura:** [Digital/Manual]

> [!important]
> **Esta revisão deve ser assinada por você.** Não delegue a aprovação final de segurança.
```

### Checklist de Conformidade (Resumo Rápido)

```markdown
# Checklist de Conformidade Rápido — PedidosSimples v1

## Pré-Deploy (Obrigatório)
- [ ] `pnpm audit --prod` → 0 critical/high
- [ ] `git-secrets` scan → limpo
- [ ] `pnpm build` → passa sem warnings segurança
- [ ] Headers de segurança (CSP, HSTS, etc.) → configurados
- [ ] Cookies Secure/HttpOnly/SameSite → corretos
- [ ] Variáveis de ambiente staging/prod → apenas na plataforma
- [ ] `.env.local` no `.gitignore` → confirmado
- [ ] Testes auth bypass → passam
- [ ] Testes RBAC → passam
- [ ] Validação Zod em TODAS APIs → confirmado
- [ ] Webhook WhatsApp HMAC → verificado
- [ ] Logs sem PII → grep confirmado
- [ ] Error messages genéricas em prod → confirmado
- [ ] Source maps desabilitados em prod → confirmado
- [ ] Backup/restore testado → confirmado (staging)
- [ ] LGPD checklist → atendido
- [ ] DPIA necessária? → Não (baixo risco)
- [ ] Riscos residuais documentados e aceitos → sim

## Pós-Deploy (Primeiras 24h)
- [ ] Sentry recebendo erros → testado
- [ ] UptimeRobot monitorando → ativo
- [ ] Alertas disparando → testado (erro forçado)
- [ ] Logs estruturados → visíveis no Vercel/Sentry
- [ ] Auditoria de ações sensíveis → registrando
- [ ] Performance (Lighthouse) → > 80
- [ ] Nenhum erro 500 não tratado → monitorado

## Contínuo (Semanal/Mensal)
- [ ] `pnpm audit` semanal → 0 critical/high
- [ ] Dependabot PRs → revisados e mergeados
- [ ] Sentry alerts → investigados
- [ ] Backup restore testado → mensal
- [ ] Runbook DR atualizado → conforme mudanças
```

## Ramificações Condicionais (Aplicam se...)

| Situação | Verificação Adicional Obrigatória |
|----------|-----------------------------------|
| **Pagamentos** | PCI-DSS SAQ, tokenização, não armazenar PAN, 3DS, logs auditoria completos |
| **Dados sensíveis (saúde, financeiro, biométrico)** | DPIA obrigatória, criptografia em repouso/transito, acesso restrito, retenção mínima |
| **Múltiplos perfis/permissões complexas** | Testes de autorização exaustivos (matriz RBAC/ABAC), penetration test authz |
| **APIs públicas/terceiros** | Rate limiting rigoroso, API keys rotation, contratos versionados, monitoring |
| **Armazenamento de arquivos** | Validação tipo/tamanho, antivírus scan, assinatura URL temporária, CDN privado |
| **IA/ML no produto** | Viés, explicabilidade, fallback determinístico, data lineage, model versioning |
| **Tempo real/WebSocket** | Auth no handshake, rate limit por conexão, message validation, disconnect handling |
| **Automações/Jobs** | Idempotência, dead letter queue, retry com backoff, observabilidade, timeout |

---

## Como trabalhar com IA nesta etapa

**Prompt para auditoria de segurança automatizada:**
```
Analise este códigobase para problemas de segurança:
- Arquivos: [liste pastas principais: app/api, lib, components, middleware]
- Stack: Next.js 14, Prisma, NextAuth, Zod
Verifique:
1. Validação Zod em TODAS APIs (body, query, params)
2. Auth check em TODAS rotas protegidas (getServerSession)
3. Nenhum `dangerouslySetInnerHTML` sem sanitização
4. Nenhum `eval`, `Function`, `innerHTML` direto
5. Prisma: apenas ORM methods (sem raw query sem parametrização)
6. Logs: grep por PII patterns (email, phone, cpf, token)
7. Error handling: mensagens genéricas em prod
8. Headers: next.config.js tem security headers?
Formato: Relatório com achados, severidade (P0/P1/P2), arquivo:linha, sugestão correção.
```

**Prompt para checklist LGPD:**
```
Meu modelo de dados: [cole schema Prisma relevante].
Features: [liste features que tocam dados pessoais].
Gere checklist LGPD/ANPD com:
- Base legal por dado
- Finalidade específica
- Compartilhamento (terceiros: WhatsApp Meta)
- Retenção
- Direitos do titular (acesso, correção, exclusão, portabilidade, oposição)
- Medidas técnicas (criptografia, pseudonimização, acesso restrito)
- Medidas organizacionais (política, treinamento, DPO se needed)
- DPIA: critérios ANPD → sim/não + justificativa
Formato: Tabela Markdown pronta para Checklist de Conformidade.
```

**Prompt para testar auth/autorização:**
```
Gere testes Playwright/Vitest para tentar burlar:
1. Acessar rota protegida sem login → 401
2. Acessar rota de outro usuário (ID manipulation) → 403
3. Escalar privilégio (STAFF → OWNER actions) → 403
4. Session fixation / replay → falha
5. CSRF em mutações → falha (NextAuth protege)
6. Rate limit brute force login → bloqueia
Use padrões do projeto (.claude/context/testing.md).
```

**Contexto a fornecer:** Código completo do projeto + Requisitos (ETAPA 04) + Arquitetura (ETAPA 05) + LGPD/Regulatório aplicável.
**Resultado esperado:** Revisão de Segurança completa + Checklist de Conformidade assinado por você.
**Como verificar:** Cada item do checklist verificado com evidência (comando, log, screenshot, link). Riscos residuais explicitamente aceitos por você.

## Checklist de execução

- [ ] Segredos: scan completo (git-secrets, truffleHog, git history) → limpo
- [ ] Dados pessoais: mapeados, base legal, minimização, consentimento, exclusão, retenção
- [ ] Auth: senhas hash, magic link seguro, sessão JWT, RBAC testado, bypass tentado
- [ ] Entradas: Zod em todas APIs, webhook HMAC, XSS/SQLi protegidos, rate limit planejado
- [ ] Exposição: erros genéricos, sem stack traces client, sem source maps prod, sem debug endpoints, headers ok
- [ ] Dependências: `pnpm audit --prod` 0 critical/high, Snyk limpo, lockfile commitado, dependabot on
- [ ] Configurações: HTTPS, HSTS, CSP, cookies secure/httponly/samesite, CORS restrito
- [ ] Operações sensíveis: idempotência, confirmações, auditoria, CSRF protegido
- [ ] Logging/Monitoramento: Sentry, uptime, alertas, logs JSON, auditoria ações sensíveis, sem PII
- [ ] Backup/Restore: Neon PITR, Vercel rollback, RPO/RTO definidos, restore testado staging
- [ ] Legal: LGPD conforme, PCI-DSS N/A, setoriais N/A
- [ ] Riscos residuais: documentados, probabilidade/impacto, justificativa, mitigação futura
- [ ] Revisão assinada por você (USUÁRIO)
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Revisão de Segurança completa com evidências
- [ ] Checklist de Conformidade 100% verificado (ou itens pendentes documentados com prazo)
- [ ] Riscos residuais aceitos explicitamente por você
- [ ] Zero vulnerabilidades critical/high em produção
- [ ] Auth/autorização testada contra bypass
- [ ] LGPD atendido (ou DPIA feita se needed)
- [ ] Backup/restore testado com sucesso
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste do segredo:** `git log --all --full-history -- "**/.env*"` → vazio?
- **Teste da dependência:** `pnpm audit --prod` → 0 critical/high?
- **Teste do auth:** Playwright testa 5 cenários de bypass → todos 401/403?
- **Teste do header:** `curl -I https://staging...` → CSP, HSTS, etc. presentes?
- **Teste do log:** `grep -r "email\|phone\|cpf\|token" logs/` → 0 resultados?
- **Teste do restore:** Neon branch restore → dados íntegros em < 1h?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Confiar só em `pnpm audit` | Use Snyk + Dependabot + revisão manual de deps críticas |
| Não testar restore de backup | **Teste real em staging** — restore point-in-time, valide dados |
| Esquecer rate limit em auth | NextAuth não tem rate limit nativo forte — adicione Arcjet/Upstash |
| Logs com PII em produção | Grep obrigatório no código E nos logs de staging |
| CSP quebrando app | Teste em `report-only` mode primeiro, ajuste, depois `enforce` |
| Não documentar riscos aceitos | Todo risco residual = linha no documento com sua assinatura |
| LGPD "depois eu vejo" | **Antes do deploy** — responsabilidade legal é sua |

## Se algo der errado

| Situação | Ação Imediata |
|----------|---------------|
| Segredo exposto no git | 1. Rotacionar segredo IMEDIATAMENTE 2. `git filter-repo` ou BFG para limpar history 3. Forçar push 4. Auditar acessos |
| Vulnerabilidade critical em dep | 1. Atualizar dep (patch) 2. Se breaking, workaround (fork/patch) 3. Deploy hotfix |
| Bypass auth encontrado em prod | 1. Hotfix imediato 2. Revogar sessões afetadas 3. Auditoria logs 4. Comunicar usuários se dados expostos |
| Dados vazados (LGPD) | 1. Contenção 2. Avaliação risco (ANPD 48h) 3. Notificação titulares se alto risco 4. Medidas corretivas |
| Backup restore falha | 1. Escalar para provedor (Neon/Vercel support) 2. Runbook DR 3. Comunicar stakeholders |

## Condições para avançar

- ✅ Revisão de Segurança assinada por você
- ✅ Checklist de Conformidade 100% verificado (ou pendentes com prazo v1.1)
- ✅ Riscos residuais explicitamente aceitos
- ✅ Zero critical/high em prod deps
- ✅ Auth/autorização robusta testada
- ✅ LGPD conforme
- ✅ Backup/restore validado

## Próxima etapa

[[ETAPA 13 - Validar o Produto Completo]]

## Materiais relacionados

- [[02 - Modelos/Segurança/Revisão de Segurança]]
- [[02 - Modelos/Segurança/Checklist de Conformidade]]
- [[02 - Modelos/Segurança/DPIA]]
- [[02 - Modelos/Segurança/Plano de Resposta a Incidentes]]
- [[03 - Guias Práticos/Segurança/Checklist de Segurança Básica]]
- [[03 - Guias Práticos/Segurança/LGPD para Desenvolvedores]]
- [[04 - Referência/Glossário/LGPD]]
- [[04 - Referência/Glossário/DPIA]]
- [[04 - Referência/Glossário/RBAC]]
- [[04 - Referência/Glossário/CSP]]