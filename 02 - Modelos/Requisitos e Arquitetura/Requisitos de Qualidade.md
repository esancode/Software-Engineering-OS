# Requisitos de Qualidade (Não-Funcionais)

> **Copie para:** `05 - Projetos/SeuProjeto/02 - Produto/Requisitos de Qualidade.md`
> Baseado na ETAPA 04. Requisitos não-funcionais mensuráveis e testáveis.

---

## Formato do Requisito de Qualidade
| Campo | Descrição |
|-------|-----------|
| **ID** | RNF-XXX (sequencial, único) |
| **Categoria** | Performance, Segurança, Confiabilidade, Usabilidade, Compatibilidade, Manutenibilidade, Portabilidade, Legal |
| **Descrição** | "O sistema DEVE [propriedade] [condição] [métrica mensurável]" |
| **Métrica** | Como medir (ferramenta, script, teste) |
| **Meta** | Valor alvo (ex: p95 < 500ms) |
| **Prioridade** | Must / Should / Could / Won't |
| **Critério de Teste** | Link para teste automatizado ou manual |
| **Status** | Proposto / Aprovado / Validado / Não Atendido |

---

## Lista de Requisitos de Qualidade

### Performance

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-001 | Performance | O sistema DEVE carregar a lista inicial de pedidos | Lighthouse / WebPageTest / k6 | p95 < 500ms (rede 4G) | Must | `test:perf:list-load` | Proposto |
| RNF-002 | Performance | O sistema DEVE processar webhook e exibir na UI | Log timestamp webhook → SSE → UI | < 2s (p95) | Must | `test:perf:webhook-to-ui` | Proposto |
| RNF-003 | Performance | O sistema DEVE responder ACK ao WhatsApp | Log webhook received → ACK sent | < 5s (WhatsApp exige < 20s) | Must | `test:perf:webhook-ack` | Proposto |
| RNF-004 | Performance | O sistema DEVE sustentar carga de pico | k6 / Artillery | 100 req/s webhook, 50 users SSE | Must | `test:load:peak` | Proposto |
| RNF-005 | Performance | O sistema DEVE ter First Contentful Paint | Lighthouse / Chrome DevTools | FCP < 1.5s (mobile 4G) | Should | `test:perf:fcp` | Proposto |
| RNF-006 | Performance | O sistema DEVE ter Time to Interactive | Lighthouse | TTI < 3.5s (mobile 4G) | Should | `test:perf:tti` | Proposto |
| RNF-007 | Performance | O sistema DEVE manter uso de memória estável | Monitoramento Vercel/Neon | < 80% limite plano (sem memory leak) | Should | `test:perf:memory` | Proposto |

### Segurança

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-010 | Segurança | O sistema DEVE usar HTTPS em todas as conexões | SSL Labs / observatório | A+ no SSL Labs, HSTS ativo | Must | `test:sec:https` | Proposto |
| RNF-011 | Segurança | O sistema DEVE validar HMAC em 100% webhooks WhatsApp | Log assinatura válida/inválida | 100% válidos aceitos, 100% inválidos rejeitados | Must | `test:sec:webhook-hmac` | Proposto |
| RNF-012 | Segurança | O sistema DEVE armazenar segredos apenas em variáveis de ambiente | Auditoria código + Vercel/Neon | Zero segredos no repo, zero no build log | Must | `test:sec:secrets` | Proposto |
| RNF-013 | Segurança | O sistema DEVE implementar autenticação em todas rotas privadas | Varredura rotas (middleware) | 0 rotas privadas sem auth | Must | `test:sec:auth-routes` | Proposto |
| RNF-014 | Segurança | O sistema DEVE proteger contra CSRF | Teste formulários / APIs | Tokens CSRF em forms, SameSite cookies | Must | `test:sec:csrf` | Proposto |
| RNF-015 | Segurança | O sistema DEVE sanitizar entrada de usuário (XSS) | OWASP ZAP / CodeQL | 0 vulnerabilidades XSS críticas/altas | Must | `test:sec:xss` | Proposto |
| RNF-016 | Segurança | O sistema DEVE implementar rate limiting em APIs sensíveis | Teste burst requests | Bloqueio após X req/min por IP/user | Should | `test:sec:ratelimit` | Proposto |
| RNF-017 | Segurança | O sistema DEVE criptografar dados sensíveis em repouso | Auditoria Neon/Prisma | PII criptografado (telefone, nome) | Should | `test:sec:encryption` | Proposto |
| RNF-018 | Segurança | O sistema DEVE ter headers de segurança | securityheaders.com | CSP, X-Frame-Options, Referrer-Policy | Should | `test:sec:headers` | Proposto |
| RNF-019 | Segurança | O sistema DEVE logar eventos de segurança (audit log) | Verificação logs | Login, logout, falha auth, mudança permissão | Could | `test:sec:auditlog` | Proposto |

### Confiabilidade

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-020 | Confiabilidade | O sistema DEVE garantir idempotência de webhook | Replay mesmo payload N vezes | 1 pedido criado por message_id único | Must | `test:rel:idempotency` | Proposto |
| RNF-021 | Confiabilidade | O sistema DEVE não perder dados em falha | Chaos engineering / simulação | 0 pedidos perdidos em reinício | Must | `test:rel:data-loss` | Proposto |
| RNF-022 | Confiabilidade | O sistema DEVE ter uptime | Vercel/Upptime monitor | > 99.9% (excluindo deploy) | Must | `test:rel:uptime` | Proposto |
| RNF-023 | Confiabilidade | O sistema DEVE recuperar de falha de banco | Teste kill connection | Reconexão automática < 5s | Must | `test:rel:db-recovery` | Proposto |
| RNF-024 | Confiabilidade | O sistema DEVE ter backup automatizado do banco | Neon point-in-time recovery | PITR ativo, RPO < 1h, RTO < 4h | Should | `test:rel:backup` | Proposto |
| RNF-025 | Confiabilidade | O sistema DEVE lidar com falha de serviço externo (WhatsApp, Resend) | Circuit breaker / retry | Retry com backoff, fallback gracioso | Should | `test:rel:external-failure` | Proposto |
| RNF-026 | Confiabilidade | O sistema DEVE ter health check endpoint | GET /api/health | 200 OK com status deps (db, redis) | Must | `test:rel:health` | Proposto |

### Usabilidade

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-030 | Usabilidade | O sistema DEVE ser utilizável em mobile (375px+) | Teste real dispositivos | Touch targets ≥ 44px, sem scroll horizontal | Must | `test:ux:mobile` | Proposto |
| RNF-031 | Usabilidade | O sistema DEVE ter contraste acessível | axe-core / Lighthouse | WCAG AA (4.5:1 texto, 3:1 UI) | Must | `test:ux:contrast` | Proposto |
| RNF-032 | Usabilidade | O sistema DEVE ser navegável por teclado | Teste tab order | 100% interativos alcançáveis, foco visível | Must | `test:ux:keyboard` | Proposto |
| RNF-033 | Usabilidade | O sistema DEVE ter labels em inputs/forms | axe-core | 0 violações "form label" | Must | `test:ux:labels` | Proposto |
| RNF-034 | Usabilidade | O sistema DEVE dar feedback visual em ações | Teste manual | Loading, success, error states visíveis | Must | `test:ux:feedback` | Proposto |
| RNF-035 | Usabilidade | O sistema DEVE suportar leitores de tela | NVDA / VoiceOver test | ARIA labels, roles, live regions | Should | `test:ux:screenreader` | Proposto |
| RNF-036 | Usabilidade | O sistema DEVE ter onboarding guiado (primeiro acesso) | Teste usuários novos | 80%+ completam sem ajuda | Should | `test:ux:onboarding` | Proposto |

### Compatibilidade

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-040 | Compatibilidade | O sistema DEVE funcionar no Chrome (últimas 2 versões) | BrowserStack / local | 100% features core | Must | `test:compat:chrome` | Proposto |
| RNF-041 | Compatibilidade | O sistema DEVE funcionar no Safari (últimas 2 versões) | BrowserStack / local | 100% features core | Must | `test:compat:safari` | Proposto |
| RNF-042 | Compatibilidade | O sistema DEVE funcionar no Firefox (últimas 2 versões) | BrowserStack / local | 100% features core | Must | `test:compat:firefox` | Proposto |
| RNF-043 | Compatibilidade | O sistema DEVE funcionar no iOS Safari (últimas 2) | BrowserStack / device real | 100% features core | Must | `test:compat:ios` | Proposto |
| RNF-044 | Compatibilidade | O sistema DEVE funcionar no Chrome Android (últimas 2) | BrowserStack / device real | 100% features core | Must | `test:compat:android` | Proposto |
| RNF-045 | Compatibilidade | O sistema DEVE ser PWA instalável | Lighthouse PWA audit | Score ≥ 90, install prompt | Could | `test:compat:pwa` | Proposto |

### Manutenibilidade

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-050 | Manutenibilidade | O sistema DEVE ter cobertura de testes | Jest/Vitest + Playwright | ≥ 80% linhas, ≥ 90% branches core | Must | `test:maintain:coverage` | Proposto |
| RNF-051 | Manutenibilidade | O sistema DEVE passar lint e typecheck no CI | ESLint + TypeScript strict | 0 errors, 0 warnings (exceto known) | Must | `test:maintain:lint` | Proposto |
| RNF-052 | Manutenibilidade | O sistema DEVE ter documentação de APIs (OpenAPI) | Swagger UI / scalar | 100% endpoints documentados | Should | `test:maintain:openapi` | Proposto |
| RNF-053 | Manutenibilidade | O sistema DEVE ter ADRs para decisões arquiteturais | Contagem arquivos `docs/adr/` | ≥ 1 ADR por decisão estrutural | Should | `test:maintain:adrs` | Proposto |
| RNF-054 | Manutenibilidade | O sistema DEVE ter tempo de build razoável | Vercel/CI logs | < 3 min (incl. testes) | Should | `test:maintain:build-time` | Proposto |

### Legal e Compliance

| ID | Categoria | Descrição | Métrica | Meta | Prioridade | Critério de Teste | Status |
|----|-----------|-----------|---------|------|------------|-------------------|--------|
| RNF-060 | Legal | O sistema DEVE atender LGPD (dados brasileiros) | Checklist LGPD | Consentimento, acesso, exclusão, portabilidade | Must | `test:legal:lgpd` | Proposto |
| RNF-061 | Legal | O sistema DEVE ter política de privacidade acessível | Link rodapé / onboarding | Texto claro, atualizado, em PT-BR | Must | `test:legal:privacy` | Proposto |
| RNF-062 | Legal | O sistema DEVE ter termos de uso | Link rodapé / onboarding | Texto claro, aceitação no cadastro | Must | `test:legal:terms` | Proposto |
| RNF-063 | Legal | O sistema DEVE permitir exclusão de conta/dados | Fluxo exclusão | Dados removidos em ≤ 30 dias | Must | `test:legal:deletion` | Proposto |
| RNF-064 | Legal | O sistema DEVE registrar consentimento LGPD | Banco: consent_log | Timestamp, versão política, IP | Should | `test:legal:consent` | Proposto |

---

## Matriz RNF ←→ Requisitos Funcionais

| RNF | RFs Relacionados | Validação Conjunta |
|-----|------------------|-------------------|
| RNF-001..003 | RF-010..019, RF-020..027, RF-030..034 | Testes integrados webhook→UI |
| RNF-010..019 | RF-001..005, RF-010..019, RF-040..043 | Testes segurança transversais |
| RNF-020..026 | RF-010..019, RF-030..034 | Testes confiabilidade transacionais |
| RNF-030..036 | RF-020..027, RF-030..034, RF-040..043 | Testes usabilidade fluxos core |
| RNF-040..045 | Todos RFs | Matriz compatibilidade cross-browser |
| RNF-050..054 | Todos | CI/CD pipeline |
| RNF-060..064 | RF-001..005, RF-016, RF-025, RF-070..072 | Auditoria LGPD fluxos dados |

---

## Checklist de Qualidade dos RNFs

- [ ] Cada RNF tem ID único, categoria, métrica e meta mensurável
- [ ] Meta é específica (número + unidade + percentil se aplicável)
- [ ] Critério de teste aponta para script/ferramenta real
- [ ] Prioridade MoSCoW alinhada com criticidade do negócio
- [ ] RNFs Must cobrem: segurança crítica, performance core, confiabilidade dados, usabilidade básica, legal obrigatório
- [ ] Rastreabilidade para RFs mapeada
- [ ] Pelo menos 1 teste automatizado por RNF Must

---

## Próxima Etapa
[[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/02 - Produto e Requisitos/ETAPA 04 - Organizar os Requisitos]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos Funcionais]]
- [[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]]
- [[02 - Modelos/Requisitos e Arquitetura/Regras de Negócio]]
- [[04 - Referência/Glossário/Requisito Não-Funcional]]
- [[04 - Referência/Glossário/NFR]]
- [[04 - Referência/Glossário/SLA]]
- [[04 - Referência/Glossário/LGPD]]