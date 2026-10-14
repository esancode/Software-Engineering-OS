# Plano de Testes

> **Copie para:** `05 - Projetos/SeuProjeto/06 - Testes e Qualidade/Plano de Testes.md`
> Baseado na ETAPA 11. **Plano detalhado de execução** por sprint, com responsáveis, critérios de saída e rastreabilidade.

---

## Visão Geral

| Fase | Atividade | Responsável | Critério de Saída |
|------|-----------|-------------|-------------------|
| **Sprint 0** | Setup testes (Vitest, Playwright, CI) | Você | CI roda `test` + `test:e2e` + coverage |
| **Sprint 1** | Unit (parser, validators) + Integration (webhook, pedido) | Você | 80% coverage core, 0 critical bugs |
| **Sprint 2** | E2E fluxos core + A11y automatizado | Você | 10 cenários passam, axe 0 critical/serious |
| **Sprint 3** | Load test, Security test, Regression completo | Você | Load test passa, Security scan limpo, All green |

---

## Sprint 0: Fundação de Testes (Dia 1-3)

### Entregáveis
- [ ] Vitest configurado + coverage thresholds
- [ ] Playwright configurado (chromium, firefox, webkit)
- [ ] Test DB (Neon branch + SQLite local)
- [ ] Factories base (Loja, Usuario, Pedido, Produto)
- [ ] CI pipeline: lint → typecheck → test → build → e2e
- [ ] Lighthouse CI configurado
- [ ] Helpers E2E: login, webhook simulation, SSE mock

### Testes a Implementar
| Teste | Tipo | Arquivo | Status |
|-------|------|---------|--------|
| Setup/teardown test DB | Integration | `tests/utils/test-db.ts` | ☐ |
| Factory Loja/Usuario/Pedido | Fixture | `tests/factories/*.ts` | ☐ |
| Helper login E2E | E2E | `tests/utils/auth-helpers.ts` | ☐ |
| Helper webhook simulation | E2E | `tests/utils/webhook-helpers.ts` | ☐ |
| CI workflow completo | CI | `.github/workflows/ci.yml` | ☐ |

### Critério de Saída Sprint 0
- [ ] `npm run test` passa (mesmo sem testes reais)
- [ ] `npm run test:e2e` roda (mesmo vazio)
- [ ] Coverage report gerado
- [ ] PR abre → CI roda completo → status checks aparecem

---

## Sprint 1: Core Domain Tests (Dia 4-10)

### Unit Tests (Parser, Validators, Utils)

| Teste | Casos | Arquivo | Target Coverage |
|-------|-------|---------|-----------------|
| `parseMessage()` | 25+ | `src/lib/parsers/parse-message.test.ts` | 100% |
| `normalizePhone()` | 10+ | `src/lib/utils/phone.test.ts` | 100% |
| `formatCurrency()` | 8+ | `src/lib/utils/format.test.ts` | 100% |
| `calculateTotal()` | 6+ | `src/lib/utils/calc.test.ts` | 100% |
| `canTransition()` (state machine) | 36 (6×6) | `src/lib/domain/status-machine.test.ts` | 100% |
| Zod schemas (todos) | 15+ | `src/lib/validators/schemas.test.ts` | 100% |
| `generateSlug()` | 5+ | `src/lib/utils/slug.test.ts` | 100% |

#### Casos de Teste: `parseMessage()`
```typescript
// Casos obrigatórios (mínimo 25)
const testCases = [
  // Básicos
  { input: 'quero 2 x-burger', expected: [{nome:'x-burger',qtd:2}] },
  { input: '1 x-salada e 1 batata', expected: [{nome:'x-salada',qtd:1},{nome:'batata',qtd:1}] },
  
  // Com preços
  { input: '2 x-burger 25 reais cada', expected: [{nome:'x-burger',qtd:2,preco:25}] },
  { input: '1 burger 30 e 1 refri 8', expected: [{nome:'burger',qtd:1,preco:30},{nome:'refri',qtd:1,preco:8}] },
  
  // Quantidades por extenso
  { input: 'dois x-burger', expected: [{nome:'x-burger',qtd:2}] },
  { input: 'três batatas', expected: [{nome:'batata',qtd:3}] },
  
  // Observações
  { input: '1 x-burger sem cebola', expected: [{nome:'x-burger',qtd:1}], obs: 'sem cebola' },
  { input: 'batata bem sequinha por favor', expected: [], obs: 'batata bem sequinha por favor' },
  
  // Edge cases
  { input: '', expected: [], needsReview: true },
  { input: 'oi tudo bem', expected: [], needsReview: true },
  { input: '   ', expected: [], needsReview: true },
  { input: '🍔🍟', expected: [], needsReview: true },
  
  // Variações linguagem natural
  { input: 'me vê 1 x-tudo', expected: [{nome:'x-tudo',qtd:1}] },
  { input: 'pode mandar 2 hamburguer', expected: [{nome:'hamburguer',qtd:2}] },
  { input: 'gostaria de 1 x-bacon e 2 coca', expected: [{nome:'x-bacon',qtd:1},{nome:'coca',qtd:2}] },
  
  // Case insensitive
  { input: 'QUERO 2 X-BURGER', expected: [{nome:'x-burger',qtd:2}] },
  { input: 'Quero 2 X-Burger', expected: [{nome:'x-burger',qtd:2}] },
  
  // Múltiplos separadores
  { input: '1 x-burger, 1 batata, 1 refri', expected: [{nome:'x-burger',qtd:1},{nome:'batata',qtd:1},{nome:'refri',qtd:1}] },
  { input: '1 x-burger; 1 batata; 1 refri', expected: [{nome:'x-burger',qtd:1},{nome:'batata',qtd:1},{nome:'refri',qtd:1}] },
  
  // Com "e" final
  { input: '1 x-burger e 1 batata e 1 refri', expected: [{nome:'x-burger',qtd:1},{nome:'batata',qtd:1},{nome:'refri',qtd:1}] },
];
```

### Integration Tests (Server Actions + DB)

| Teste | Cenários | Arquivo | Status |
|-------|----------|---------|--------|
| `createPedido()` | Sucesso, validação, idempotência, dead letter | `src/actions/pedidos.integration.test.ts` | ☐ |
| `updateStatus()` | Transições válidas, inválidas, timestamps, histórico | `src/actions/pedidos.integration.test.ts` | ☐ |
| `cancelarPedido()` | Com/sem motivo, status permitido, transição | `src/actions/pedidos.integration.test.ts` | ☐ |
| `enviarMensagem()` | Template, custom, rate limit, erro WhatsApp | `src/actions/mensagens.integration.test.ts` | ☐ |
| Webhook handler | HMAC válido/inválido, idempotência, parser falha | `src/app/api/webhook/whatsapp/route.integration.test.ts` | ☐ |
| SSE broadcast | Novo pedido, status atualizado, multi-loja | `src/lib/sse.integration.test.ts` | ☐ |
| Relatórios | Diário, semanal, métricas corretas | `src/services/relatorios.integration.test.ts` | ☐ |

### Critério de Saída Sprint 1
- [ ] Unit tests: 100% coverage parser, validators, state machine, utils
- [ ] Integration tests: Todos Server Actions + Webhook + SSE cobertos
- [ ] Coverage global ≥ 80% statements, ≥ 70% branches
- [ ] 0 testes flaky
- [ ] Tempo total testes < 60s (unit) + 120s (integration)

---

## Sprint 2: E2E + Acessibilidade (Dia 11-15)

### E2E Tests (Playwright) - Cenários Core

| # | Cenário | Arquivo | Tags | Prioridade |
|---|---------|---------|------|------------|
| 1 | Login Magic Link → Dashboard | `auth.e2e.test.ts` | @auth @smoke | Crítico |
| 2 | Login Credentials → Dashboard | `auth.e2e.test.ts` | @auth | Crítico |
| 3 | Logout + Session Refresh | `auth.e2e.test.ts` | @auth | Alto |
| 4 | Onboarding completo (4 passos) | `onboarding.e2e.test.ts` | @onboarding @smoke | Crítico |
| 5 | Pedido WhatsApp → Dashboard (SSE) | `pedido-flow.e2e.test.ts` | @core @sse @smoke | Crítico |
| 6 | Fluxo status: Novo → Preparando → Pronto → Entregue | `pedido-flow.e2e.test.ts` | @core @smoke | Crítico |
| 7 | Cancelar pedido (novo/preparando) | `pedido-flow.e2e.test.ts` | @core | Alto |
| 8 | Responder cliente: Template + Custom | `communication.e2e.test.ts` | @communication | Alto |
| 9 | Relatório diário métricas | `relatorios.e2e.test.ts` | @reports | Médio |
| 10 | Config Produtos CRUD | `config.e2e.test.ts` | @config | Médio |
| 11 | Config Horários + Presets | `config.e2e.test.ts` | @config | Médio |
| 12 | Filtros Dashboard (status + data + busca) | `dashboard.e2e.test.ts` | @core | Alto |
| 13 | Páginação + Scroll infinito | `dashboard.e2e.test.ts` | @core | Médio |
| 14 | Offline fallback (PWA) | `pwa.e2e.test.ts` | @pwa | Baixo |

### Acessibilidade (Automatizado + Manual)

| Teste | Ferramenta | Frequência | Critério |
|-------|------------|------------|----------|
| Axe automated (todas páginas) | Playwright + axe | Todo PR | 0 critical/serious |
| Navegação só teclado | Manual | Semanal | 100% fluxos core |
| Screen Reader (NVDA/VoiceOver) | Manual | Release | Fluxo core anunciado |
| Zoom 200% | Manual | Semanal | Layout não quebra |
| Contraste cores | Automated (axe) + Manual | Todo PR | 4.5:1 texto, 3:1 UI |
| Reduced motion | Manual | Semanal | Animações desabilitadas |

### Critério de Saída Sprint 2
- [ ] 14 cenários E2E passando (chromium + firefox)
- [ ] Axe: 0 violações critical/serious em todas páginas
- [ ] Testes manuais a11y: teclado, screen reader, zoom, contraste OK
- [ ] Tempo total E2E < 10 min (CI)
- [ ] Screenshots/trace em falhas para debug

---

## Sprint 3: Qualidade Final + Não-Funcionais (Dia 16-20)

### Load Test (k6)

| Cenário | Configuração | Target | Status |
|---------|--------------|--------|--------|
| Webhook burst | 100 VUs, 30s, ramp-up 10s | p95 < 2s, 0 erros | ☐ |
| SSE conexões | 50 VUs, conexão SSE 60s | 99% conectadas, < 100ms msg | ☐ |
| Dashboard carga | 20 VUs, 5 min, lista + filtros | p95 < 500ms | ☐ |
| APIs autenticadas | 30 VUs, mixed read/write | p95 < 800ms | ☐ |
| Stress | 200 VUs webhook 60s | Degradação graciosa, sem crash | ☐ |

```javascript
// k6-script.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '30s', target: 100 },  // Ramp up
    { duration: '1m', target: 100 },   // Stay
    { duration: '30s', target: 0 },    // Ramp down
  ],
  thresholds: {
    'http_req_duration': ['p(95)<2000'],
    'http_req_failed': ['rate<0.01'],
    'checks': ['rate>0.99'],
  },
};

export default function () {
  // Webhook simulation
  const payload = JSON.stringify({ /* webhook body */ });
  const params = {
    headers: {
      'Content-Type': 'application/json',
      'X-Hub-Signature-256': `sha256=${hmac(payload)}`,
    },
  };
  const res = http.post('https://app.vercel.app/api/webhook/whatsapp', payload, params);
  check(res, { 'status 200': (r) => r.status === 200, 'ack < 5s': (r) => r.timings.duration < 5000 });
  sleep(1);
}
```

### Security Tests

| Teste | Ferramenta | Critério | Status |
|-------|------------|----------|--------|
| SAST | CodeQL / Semgrep | 0 high/critical | ☐ |
| Dependencies | `npm audit` / Snyk | 0 high/critical | ☐ |
| Secrets | TruffleHog / GitLeaks | 0 secrets | ☐ |
| CSP/Eval | Lighthouse + Manual | CSP válido, sem 'unsafe-eval' desnecessário | ☐ |
| Rate Limit | k6 custom | Bloqueia após limite | ☐ |
| Auth Bypass | Manual + Playwright | Rotas protegidas retornam 401/403 | ☐ |
| LGPD Compliance | Checklist manual | Consent, export, delete, retention | ☐ |

### Regression Test (Smoke Suite)

```typescript
// tests/e2e/regression.e2e.test.ts
// Roda TODOS cenários core em sequência
test.describe('Regression Suite @regression', () => {
  test('Login → Onboarding → Pedido → Entrega → Relatório → Config', async ({ page }) => {
    // Fluxo completo end-to-end
  });
});
```

### Critério de Saída Sprint 3 (MVP Ready)
- [ ] Load test: Todos targets atingidos
- [ ] Security: 0 high/critical findings
- [ ] Regression suite: 100% pass
- [ ] Lighthouse CI: Perf ≥ 90, A11y ≥ 95, Best Practices ≥ 90
- [ ] Bundle size: < 200KB gzipped first load
- [ ] Zero known bugs critical/blocker

---

## Rastreabilidade: Requisito → Caso de Teste

| RF/RNF | Casos de Teste | Sprint |
|--------|----------------|--------|
| RF-001..005 | Auth unit + integration + E2E 1-3 | 0, 2 |
| RF-010..019 | Webhook unit + integration + E2E 5 | 1, 2 |
| RF-015 | Parser unit (25 casos) | 1 |
| RF-016 | createPedido integration | 1 |
| RF-020..027 | Dashboard integration + E2E 12,13 | 1, 2 |
| RF-030..034 | updateStatus integration + E2E 6,7 | 1, 2 |
| RF-040..043 | enviarMensagem integration + E2E 8 | 1, 2 |
| RF-050..053 | SSE integration + E2E 5 | 1, 2 |
| RF-060..062 | Relatórios integration + E2E 9 | 1, 2 |
| RF-070..081 | Config integration + E2E 10,11 | 1, 2 |
| RNF-001..003 | Load test dashboard/webhook | 3 |
| RNF-010..019 | Security tests + integration | 3 |
| RNF-030..036 | A11y automated + manual | 2, 3 |

---

## Definição de Pronto por Nível

### Unit Test Pronto
- [ ] Testa uma unidade isolada (função, hook, componente puro)
- [ ] Mocks mínimos (apenas dependências externas)
- [ ] Nomes descritivos: `should do X when Y`
- [ ] AAA pattern claro
- [ ] Executa < 100ms

### Integration Test Pronto
- [ ] Testa integração real (DB, API externa mockada, SSE)
- [ ] Test DB isolada (setup/teardown por arquivo)
- [ ] Dados via factories
- [ ] Verifica side effects (DB writes, eventos emitidos)
- [ ] Executa < 5s

### E2E Test Pronto
- [ ] Testa fluxo real do usuário
- [ ] Seletores `data-testid` (não CSS classes)
- [ ] Auto-wait do Playwright (sem sleeps arbitrários)
- [ ] Independente (não depende de outros testes)
- [ ] Tags para filtragem (@smoke, @core, @a11y)
- [ ] Screenshot/trace em falha
- [ ] Executa < 60s

---

## Métricas de Qualidade (Dashboard)

| Métrica | Target | Frequência | Ação se Falhar |
|---------|--------|------------|----------------|
| Unit coverage | ≥ 80% | Todo PR | Bloquear merge |
| Branch coverage | ≥ 70% | Todo PR | Bloquear merge |
| E2E pass rate | 100% @smoke | Todo PR | Bloquear merge |
| A11y violations | 0 critical/serious | Todo PR | Bloquear merge |
| Lighthouse Perf | ≥ 90 | Todo PR | Warn |
| Lighthouse A11y | ≥ 95 | Todo PR | Error |
| Load test p95 | < targets | Sprint 3 | Investigar + fix |
| Security findings | 0 high/critical | Sprint 3 | Bloquear release |
| Flaky tests | 0 | Contínuo | Fix ou quarentena |

---

## Checklist Plano (Validação ETAPA 11)

- [ ] Testes mapeados por sprint com responsáveis
- [ ] Casos de teste unit detalhados (parser 25+ casos)
- [ ] Integration tests cobrem todos Server Actions + Webhook + SSE
- [ ] E2E: 14 cenários core + regression suite
- [ ] A11y: Automatizado (axe) + Manual (teclado, SR, zoom, contraste)
- [ ] Load test: 5 cenários com targets mensuráveis
- [ ] Security: SAST, deps, secrets, rate limit, auth bypass, LGPD
- [ ] CI/CD: Pipeline completo com gates de qualidade
- [ ] Rastreabilidade: Cada RF/RNF → casos de teste
- [ ] Definition of Done por nível (unit, integration, e2e)

---

## Próxima Etapa
[[02 - Modelos/Testes/Casos de Teste]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/06 - Testes e Qualidade/ETAPA 11 - Executar Testes e Garantir a Qualidade]]
- [[02 - Modelos/Testes/Estratégia de Testes]]
- [[02 - Modelos/Testes/Casos de Teste]]
- [[02 - Modelos/Desenvolvimento/Checklist de Código]]
- [[04 - Referência/Glossário/Test Plan]]
- [[04 - Referência/Glossário/Test Case]]
- [[04 - Referência/Glossário/k6]]
- [[04 - Referência/Glossário/Load Testing]]