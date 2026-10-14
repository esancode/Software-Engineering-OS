# Estratégia de Testes

> **Copie para:** `05 - Projetos/SeuProjeto/06 - Testes e Qualidade/Estratégia de Testes.md`
> Baseado na ETAPA 11. **Pirâmide de testes, ferramentas, cobertura, CI/CD** para o MVP.

---

## Pirâmide de Testes (Adaptada para Solo Dev)

```
        ┌─────────────┐
        │   E2E       │  ← 5-10 cenários core (Playwright)
        │  (Poucos)   │
       ┌┴─────────────┴┐
       │  Integração   │  ← Server Actions, API, DB, SSE (Vitest)
       │  (Médios)     │
      ┌┴────────────────┴┐
      │     Unitários    │  ← Funções puras, parsers, validadores, utils (Vitest)
      │    (Muitos)      │
      └──────────────────┘
```

**Princípio:** Testar **comportamento**, não implementação. Usuário não liga para "função X foi chamada", liga para "pedido aparece na tela".

---

## Ferramentas

| Camada | Ferramenta | Por que |
|--------|------------|---------|
| Unit | **Vitest** | Rápido, API compatível Jest, TypeScript nativo, coverage built-in |
| Integração | **Vitest + Test DB** | Mesma API, roda contra Neon branch / SQLite local |
| E2E | **Playwright** | Cross-browser, auto-wait, trace viewer, codegen, CI nativo |
| A11y | **axe-core + Playwright** | Automatizado no CI |
| Visual | **(Opcional) Playwright screenshots** | Regression visual se necessário |

---

## Convenções de Nomenclatura

| Tipo | Padrão | Exemplo |
|------|--------|---------|
| Unit | `*.test.ts` | `parse-message.test.ts` |
| Integration | `*.integration.test.ts` | `create-pedido.integration.test.ts` |
| E2E | `*.e2e.test.ts` | `pedido-flow.e2e.test.ts` |
| Fixtures | `*.fixture.ts` | `pedido.fixture.ts` |
| Factories | `*.factory.ts` | `pedido.factory.ts` |

---

## Estrutura de Pastas

```
src/
├── lib/
│   ├── parsers/
│   │   ├── parse-message.ts
│   │   └── parse-message.test.ts           ← Unit
│   ├── validators/
│   │   ├── schemas.ts
│   │   └── schemas.test.ts                 ← Unit
│   └── utils/
│       ├── format.ts
│       └── format.test.ts                  ← Unit
├── actions/
│   ├── pedidos.ts
│   ├── pedidos.integration.test.ts         ← Integration (test DB)
│   └── pedidos.test.ts                     ← Unit (mocks)
├── services/
│   ├── whatsapp.ts
│   └── whatsapp.test.ts                    ← Unit (mocks)
└── components/
    └── organisms/
        ├── pedido-card.tsx
        └── pedido-card.test.tsx            ← Unit (React Testing Library)
```

```
tests/
├── e2e/
│   ├── auth.e2e.test.ts
│   ├── onboarding.e2e.test.ts
│   ├── pedido-flow.e2e.test.ts
│   ├── relatorios.e2e.test.ts
│   └── a11y.e2e.test.ts
├── fixtures/
│   ├── users.ts
│   ├── pedidos.ts
│   └── lojas.ts
├── factories/
│   ├── user.factory.ts
│   ├── pedido.factory.ts
│   └── loja.factory.ts
└── utils/
    ├── test-db.ts          ← Setup/teardown test database
    ├── auth-helpers.ts     ← Login helpers para E2E
    └── sse-helpers.ts      ← SSE mock helpers
```

---

## Banco de Dados de Teste

### Opção 1: Neon Branch (Recomendado - Mais Real)
```typescript
// tests/utils/test-db.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient({
  datasources: {
    db: { url: process.env.TEST_DATABASE_URL } // Neon branch "test"
  }
});

export async function setupTestDB() {
  // Limpar tabelas na ordem correta (FK)
  await prisma.mensagem.deleteMany();
  await prisma.pedidoStatusHistorico.deleteMany();
  await prisma.pedidoItem.deleteMany();
  await prisma.pedido.deleteMany();
  await prisma.produto.deleteMany();
  await prisma.horarioFuncionamento.deleteMany();
  await prisma.configuracaoLoja.deleteMany();
  await prisma.consentimentoLGPD.deleteMany();
  await prisma.usuario.deleteMany();
  await prisma.loja.deleteMany();
}

export async function seedTestDB() {
  // Criar loja, usuário, produtos, horários padrão
  const loja = await prisma.loja.create({ data: { ... } });
  const usuario = await prisma.usuario.create({ data: { ... } });
  return { loja, usuario };
}

export { prisma };
```

### Opção 2: SQLite (Mais Rápido Local)
```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'node',
    setupFiles: ['./tests/setup.ts'],
    pool: 'forks', // Isolamento entre arquivos
  },
});

// tests/setup.ts
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient({
  datasources: { db: { url: 'file:./test.db' } }
});
// ... migrations via prisma migrate deploy
```

> **Decisão:** Usar **Neon branch** para integração (mais real) + **SQLite** para unit (mais rápido). Configurar no CI.

---

## Factories (Test Data Builders)

```typescript
// tests/factories/pedido.factory.ts
import { Prisma, PedidoStatus } from '@prisma/client';

export function buildPedido(overrides: Partial<Prisma.PedidoCreateInput> = {}) {
  return {
    lojaId: 'loja-test-1',
    clienteTelefone: '5511999999999',
    clienteNome: 'Cliente Teste',
    status: 'NOVO' as PedidoStatus,
    itensJson: [{ nome: 'X-Burger', quantidade: 2, preco: 25 }],
    total: new Prisma.Decimal(50),
    observacoes: null,
    whatsappMessageId: `wamid.test-${Date.now()}`,
    ...overrides,
  };
}

export function buildPedidoItem(overrides: Partial<Prisma.PedidoItemCreateInput> = {}) {
  return {
    pedidoId: 'pedido-test-1',
    nome: 'X-Burger',
    quantidade: 2,
    precoUnitario: new Prisma.Decimal(25),
    ...overrides,
  };
}

// Uso no teste:
const pedido = buildPedido({ status: 'PREPARANDO', total: new Prisma.Decimal(75) });
```

---

## Cobertura Mínima (Targets)

| Métrica | Target | O que mede |
|---------|--------|------------|
| **Statements** | ≥ 80% | Linhas executadas |
| **Branches** | ≥ 70% | If/else, switch cases |
| **Functions** | ≥ 80% | Funções chamadas |
| **Lines** | ≥ 80% | Linhas cobertas |
| **Core Domain** | 100% | Parser, validadores, state machine |

> **Exceções:** Arquivos de config, types, barrel exports, `main.tsx`, `layout.tsx` - excluídos do coverage.

```typescript
// vitest.config.ts - coverage config
export default defineConfig({
  test: {
    coverage: {
      provider: 'v8',
      reporter: ['text', 'json', 'html'],
      exclude: [
        'node_modules/',
        '**/*.d.ts',
        '**/*.config.*',
        '**/types/**',
        '**/layout.tsx',
        '**/page.tsx', // Server Components (testar via E2E)
        '**/*.stories.tsx',
      ],
      thresholds: {
        statements: 80,
        branches: 70,
        functions: 80,
        lines: 80,
      },
    },
  },
});
```

---

## O Que Testar em Cada Camada

### Unit (Vitest) - Rápido, Isolado, Determinístico

| Alvo | Exemplos |
|------|----------|
| **Parsers** | `parseMessage()` - 20+ casos: texto simples, com preços, quantidades, obs, vazio, emoji |
| **Validadores** | Zod schemas - input válido, inválido, edge cases |
| **Utils** | `formatCurrency()`, `normalizePhone()`, `calculateTotal()`, `generateSlug()` |
| **State Machine** | `canTransition(from, to)` - todas combinações válidas/inválidas |
| **Helpers Auth** | `requireRole()`, `getSession()`, token utils |
| **Componentes Puros** | `StatusBadge`, `Button`, `formatDate` (React Testing Library) |

**Não testar:** Implementação privada, componentes com side effects (useEffect, fetch).

### Integração (Vitest + Test DB) - Real DB, Sem Browser

| Alvo | Exemplos |
|------|----------|
| **Server Actions** | `createPedido()`, `updateStatus()`, `cancelarPedido()`, `enviarMensagem()` |
| **Prisma Middleware** | Soft delete, timestamps auto, JSON sync |
| **SSE Broadcast** | `broadcastSSE()` - evento emitido para loja correta |
| **Webhook Handler** | HMAC validation, idempotência, dead letter |
| **Relatórios** | `getRelatorioDiario()` - agregações corretas |
| **Auth** | Magic Link flow, Credentials, Refresh rotation |

**Padrão AAA:**
```typescript
// create-pedido.integration.test.ts
test('cria pedido com itens e histórico', async () => {
  // Arrange
  const { loja, usuario } = await seedTestDB();
  const input = buildWebhookPayload({ message: '2 x-burger' });
  
  // Act
  const result = await createPedido(input, loja.id);
  
  // Assert
  expect(result.success).toBe(true);
  const pedido = await prisma.pedido.findUnique({ where: { id: result.pedido.id } });
  expect(pedido?.status).toBe('NOVO');
  expect(pedido?.itensJson).toHaveLength(1);
  const historico = await prisma.pedidoStatusHistorico.findMany({ where: { pedidoId: pedido.id } });
  expect(historico).toHaveLength(1);
  expect(historico[0].statusNovo).toBe('NOVO');
});
```

### E2E (Playwright) - Fluxos Reais do Usuário

| Cenário | Prioridade | Tags |
|---------|------------|------|
| Login Magic Link → Dashboard | **Crítico** | @auth @smoke |
| Login Credentials → Dashboard | **Crítico** | @auth |
| Onboarding completo (4 passos) | **Crítico** | @onboarding @smoke |
| Pedido WhatsApp → aparece no dashboard (SSE) | **Crítico** | @core @sse |
| Fluxo status: Novo → Preparando → Pronto → Entregue | **Crítico** | @core @smoke |
| Cancelar pedido com confirmação | **Alto** | @core |
| Responder cliente (template + custom) | **Alto** | @communication |
| Relatório diário mostra métricas corretas | **Médio** | @reports |
| Config produtos CRUD | **Médio** | @config |
| Config horários + preset | **Médio** | @config |
| Acessibilidade: teclado, screen reader, contraste | **Crítico** | @a11y |
| PWA: install, offline fallback | **Baixo** | @pwa |

**Exemplo E2E:**
```typescript
// tests/e2e/pedido-flow.e2e.test.ts
import { test, expect } from '@playwright/test';

test.describe('Fluxo Pedido → Entrega', () => {
  test.beforeEach(async ({ page }) => {
    await loginAsOwner(page); // Helper
  });

  test('pedido chega via webhook e flui até entregue @core @smoke', async ({ page }) => {
    // 1. Simula webhook WhatsApp
    await simulateWebhook(page, { message: '2 x-burger 1 batata' });
    
    // 2. Dashboard: badge Novo incrementa, card aparece
    await expect(page.locator('[data-badge=novo]')).toContainText('1');
    await expect(page.locator('article:has-text("x-burger")')).toBeVisible();
    
    // 3. Abre detalhe
    await page.click('article:has-text("x-burger") >> [data-testid=ver]');
    await expect(page).toHaveURL(/\/pedidos\//);
    
    // 4. Inicia preparo
    await page.click('[data-testid=iniciar-preparo]');
    await expect(page.locator('[data-status]')).toContainText('PREPARANDO');
    
    // 5. Marca pronto
    await page.click('[data-testid=marcar-pronto]');
    await expect(page.locator('[data-status]')).toContainText('PRONTO');
    
    // 6. Avisa cliente (template)
    await page.click('[data-testid=responder]');
    await page.click('[data-template=pronto]');
    await page.click('[data-testid=enviar]');
    await expect(page.locator('[data-toast]')).toContainText('Enviado');
    
    // 7. Entrega
    await page.click('[data-testid=entregar]');
    await expect(page.locator('[data-status]')).toContainText('ENTREGUE');
    
    // 8. Sai da lista ativa
    await page.goto('/dashboard');
    await expect(page.locator('article:has-text("x-burger")')).not.toBeVisible();
  });
});
```

---

## Testes de Acessibilidade (Automatizados)

```typescript
// tests/e2e/a11y.e2e.test.ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

const PAGES = ['/dashboard', '/pedidos/123', '/relatorios', '/configuracoes/produtos'];

for (const pagePath of PAGES) {
  test(`A11y: ${pagePath} - sem violações critical/serious`, async ({ page }) => {
    await loginAsOwner(page);
    await page.goto(pagePath);
    await page.waitForLoadState('networkidle');
    
    const results = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa'])
      .analyze();
    
    const criticalSerious = results.violations.filter(
      v => v.impact === 'critical' || v.impact === 'serious'
    );
    
    expect(criticalSerious).toHaveLength(0);
  });
}

// Testes específicos teclado
test('Navegação só teclado: Dashboard → Detalhe → Ações', async ({ page }) => {
  await loginAsOwner(page);
  await page.goto('/dashboard');
  
  // Tab para primeiro card
  await page.keyboard.press('Tab');
  await expect(page.locator('article:first-child')).toBeFocused();
  
  // Enter abre detalhe
  await page.keyboard.press('Enter');
  await expect(page).toHaveURL(/\/pedidos\//);
  
  // Tab nas ações
  await page.keyboard.press('Tab');
  await expect(page.locator('[data-testid=iniciar-preparo]')).toBeFocused();
  
  // Enter executa
  await page.keyboard.press('Enter');
  // ... valida status mudou
});
```

---

## CI/CD Pipeline de Testes

```yaml
# .github/workflows/test.yml
name: Test
on: [push, pull_request]

jobs:
  unit:
    name: Unit + Integration
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_DB: test
          POSTGRES_PASSWORD: test
        ports: [5432:5432]
        options: >-
          --health-cmd="pg_isready"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=5
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci
      - run: npx prisma migrate deploy
        env: { DATABASE_URL: postgresql://postgres:test@localhost:5432/test }
      - run: npm run test -- --coverage
      - uses: codecov/codecov-action@v3
        if: github.event_name == 'push'

  e2e:
    name: E2E (Playwright)
    runs-on: ubuntu-latest
    timeout-minutes: 30
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npm run build
      - run: npx playwright test --project=chromium
        env:
          TEST_BASE_URL: http://localhost:3000
          # Preview URL se disponível
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: playwright-trace
          path: test-results/
          retention-days: 7

  lighthouse:
    name: Lighthouse CI
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: treosh/lighthouse-ci-action@v11
        with:
          urls: |
            https://preview-url.vercel.app/dashboard
            https://preview-url.vercel.app/pedidos/123
          budgetPath: ./lighthouse-budget.json
          uploadArtifacts: true
```

---

## Lighthouse Budget (Performance + A11y)

```json
// lighthouse-budget.json
{
  "ci": {
    "assert": {
      "assertions": {
        "categories:performance": ["error", { "minScore": 0.9 }],
        "categories:accessibility": ["error", { "minScore": 0.95 }],
        "categories:best-practices": ["warn", { "minScore": 0.9 }],
        "categories:seo": ["warn", { "minScore": 0.9 }],
        "first-contentful-paint": ["error", { "maxNumericValue": 1800 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 2500 }],
        "total-blocking-time": ["error", { "maxNumericValue": 200 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }],
        "interactive": ["error", { "maxNumericValue": 3500 }]
      }
    }
  }
}
```

---

## Dados de Teste (Fixtures)

```typescript
// tests/fixtures/pedidos.ts
export const pedidoFixtures = {
  novoSimples: {
    whatsappMessageId: 'wamid.fixture1',
    message: 'Quero 1 x-burger',
    expected: { items: [{ nome: 'x-burger', qtd: 1 }], total: null },
  },
  novoComPrecos: {
    whatsappMessageId: 'wamid.fixture2',
    message: '2 x-salada 30 reais cada e 1 suco 8 reais',
    expected: { 
      items: [
        { nome: 'x-salada', qtd: 2, preco: 30 },
        { nome: 'suco', qtd: 1, preco: 8 }
      ],
      total: 68
    },
  },
  novoComObservacao: {
    whatsappMessageId: 'wamid.fixture3',
    message: '1 x-burger sem cebola, batata bem sequinha',
    expected: { 
      items: [{ nome: 'x-burger', qtd: 1 }],
      observacoes: 'sem cebola, batata bem sequinha'
    },
  },
  // ... mais 10+ fixtures
};
```

---

## Debugging Testes (Dicas)

| Problema | Solução |
|----------|---------|
| Test flaky (passa/falha aleatório) | `test.retry(2)` temporário, investigar race condition / timing |
| DB sujo entre testes | `beforeEach` limpa tabelas, `afterAll` disconnect |
| Playwright timeout | `test.setTimeout(30000)`, `page.waitForLoadState('networkidle')` |
| Coverage não bate | Verificar `exclude` no config, arquivos não testados |
| E2E falha no CI mas passa local | Diff screenshots, trace viewer, logs CI |

---

## Checklist Estratégia (Validação ETAPA 11)

- [ ] Pirâmide definida com targets por camada
- [ ] Ferramentas escolhidas e configuradas (Vitest, Playwright, axe)
- [ ] Test DB strategy (Neon branch + SQLite local)
- [ ] Factories/fixtures para dados consistentes
- [ ] Cobertura mínima: 80% statements, 70% branches
- [ ] Core domain: 100% coverage (parser, state machine, validadores)
- [ ] E2E: 10+ cenários core + a11y automatizado
- [ ] CI: roda unit + integration + e2e + lighthouse em todo PR
- [ ] Budgets: Performance ≥ 90, A11y ≥ 95
- [ ] Documentado: como rodar local, como debug, como adicionar teste

---

## Próxima Etapa
[[02 - Modelos/Testes/Plano de Testes]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/06 - Testes e Qualidade/ETAPA 11 - Executar Testes e Garantir a Qualidade]]
- [[02 - Modelos/Desenvolvimento/Checklist de Código]]
- [[02 - Modelos/Testes/Plano de Testes]]
- [[02 - Modelos/Testes/Casos de Teste]]
- [[04 - Referência/Glossário/Test Pyramid]]
- [[04 - Referência/Glossário/Vitest]]
- [[04 - Referência/Glossário/Playwright]]
- [[04 - Referência/Glossário/axe-core]]