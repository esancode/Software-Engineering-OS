# Guia de Testes

> **Local:** `03 - Guias Práticos/Guia de Testes.md`
> **Baseado em:** [[02 - Modelos/Testes/Estratégia de Testes]], [[02 - Modelos/Testes/Plano de Testes]], [[02 - Modelos/Testes/Casos de Teste]]
> **Objetivo:** Referência prática para escrever, rodar e manter testes automatizados (unit, integration, e2e, a11y, load).

---

## Pirâmide de Testes (Target MVP)

```
        ████████
       ██  E2E  ██     ~10%  |  Playwright @smoke @core
      ████████████
     ██ Integration ██    ~20%  |  Vitest (API routes, Server Actions, DB)
    ████████████████
   ██████ Unit ██████████  ~70%  |  Vitest (utils, validators, parsers, hooks)
  ██████████████████████
```

| Camada | Ferramenta | Cobertura Alvo | Tempo Máx |
|--------|------------|----------------|-----------|
| **Unit** | Vitest | Lines ≥ 80%, Branches ≥ 70% | < 30s |
| **Integration** | Vitest + Testcontainers/Prisma | Paths críticos API/DB | < 60s |
| **E2E** | Playwright | @smoke 100%, @core 100% | < 3min |
| **A11y** | axe + Playwright | 0 critical/serious | < 30s |
| **Visual** | Playwright/Chromatic | Páginas core | < 2min |
| **Load** | k6 | p95 < targets | < 5min |

---

## Setup e Configuração

### package.json Scripts
```json
{
  "scripts": {
    "test": "vitest run --coverage",
    "test:watch": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest run --coverage",
    "test:e2e": "playwright test",
    "test:e2e:ui": "playwright test --ui",
    "test:e2e:headed": "playwright test --headed",
    "test:a11y": "playwright test --project=chromium --grep @a11y",
    "test:load": "k6 run tests/load/*.js",
    "lint": "eslint . --ext .ts,.tsx",
    "typecheck": "tsc --noEmit",
    "precommit": "npm run lint && npm run typecheck && npm run test"
  }
}
```

### vitest.config.ts
```typescript
// vitest.config.ts
import { defineConfig } from "vitest/config"
import react from "@vitejs/plugin-react"
import path from "path"

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./tests/setup.ts"],
    include: ["src/**/*.test.{ts,tsx}"],
    coverage: {
      provider: "v8",
      reporter: ["text", "json", "html", "lcov"],
      thresholds: {
        lines: 80,
        functions: 80,
        branches: 70,
        statements: 80,
      },
      exclude: [
        "node_modules/",
        "tests/",
        "*.config.*",
        "src/app/**", // Next.js app router - test via e2e
        "src/components/ui/**", // Testados visualmente
      ],
    },
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

### tests/setup.ts
```typescript
// tests/setup.ts
import "@testing-library/jest-dom"
import { vi } from "vitest"

// Mock Next.js
vi.mock("next/navigation", () => ({
  useRouter: () => ({ push: vi.fn(), back: vi.fn() }),
  usePathname: () => "/",
  useSearchParams: () => new URLSearchParams(),
}))

// Mock NextAuth
vi.mock("next-auth/react", () => ({
  useSession: () => ({ data: null, status: "unauthenticated" }),
  signIn: vi.fn(),
  signOut: vi.fn(),
}))

// Mock Prisma
vi.mock("@/lib/prisma", () => ({
  prisma: {
    pedido: { findMany: vi.fn(), findUnique: vi.fn(), create: vi.fn(), update: vi.fn() },
    loja: { findUnique: vi.fn(), update: vi.fn() },
    $transaction: vi.fn((cb) => cb(vi.fn())),
  },
}))

// Mock Resend
vi.mock("resend", () => ({
  Resend: vi.fn().mockImplementation(() => ({
    emails: { send: vi.fn().mockResolvedValue({ data: { id: "test" } }) },
  })),
}))

// Global test utils
global.ResizeObserver = vi.fn().mockImplementation(() => ({
  observe: vi.fn(),
  unobserve: vi.fn(),
  disconnect: vi.fn(),
}))
```

### playwright.config.ts
```typescript
// playwright.config.ts
import { defineConfig, devices } from "@playwright/test"

export default defineConfig({
  testDir: "./tests/e2e",
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [
    ["html", { outputFolder: "playwright-report" }],
    ["json", { outputFile: "test-results/results.json" }],
    ["junit", { outputFile: "test-results/junit.xml" }],
  ],
  use: {
    baseURL: "http://localhost:3000",
    trace: "on-first-retry",
    screenshot: "only-on-failure",
    video: "retain-on-failure",
  },
  projects: [
    { name: "chromium", use: { ...devices["Desktop Chrome"] } },
    { name: "firefox", use: { ...devices["Desktop Firefox"] } },
    { name: "webkit", use: { ...devices["Desktop Safari"] } },
    { name: "Mobile Chrome", use: { ...devices["Pixel 5"] } },
    { name: "Mobile Safari", use: { ...devices["iPhone 12"] } },
  ],
  webServer: {
    command: "npm run dev",
    url: "http://localhost:3000",
    reuseExistingServer: !process.env.CI,
    timeout: 120000,
  },
  expect: {
    toHaveScreenshot: {
      maxDiffPixels: 100,
      threshold: 0.2,
    },
  },
})
```

---

## Testes Unitários (Vitest + React Testing Library)

### Padrão AAA (Arrange, Act, Assert)
```typescript
// src/lib/validators/pedido.test.ts
import { describe, it, expect, vi } from "vitest"
import { validarPedido, PedidoInput } from "./pedido"

describe("validarPedido", () => {
  const pedidoValido: PedidoInput = {
    clienteNome: "João Silva",
    clienteTelefone: "11999999999",
    itens: [{ nome: "X-Burger", quantidade: 2, precoUnitario: 25 }],
    observacao: "Sem cebola",
  }

  it("deve retornar sucesso para pedido válido", () => {
    // Arrange - já pronto no beforeEach ou const
    
    // Act
    const resultado = validarPedido(pedidoValido)
    
    // Assert
    expect(resultado.success).toBe(true)
    expect(resultado.data).toMatchObject({
      clienteNome: "João Silva",
      total: 50,
    })
  })

  it("deve falhar se clienteNome vazio", () => {
    const input = { ...pedidoValido, clienteNome: "" }
    const resultado = validarPedido(input)
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("clienteNome")
  })

  it("deve falhar se telefone inválido (não E.164)", () => {
    const input = { ...pedidoValido, clienteTelefone: "999999999" }
    const resultado = validarPedido(input)
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("telefone")
  })

  it("deve falhar se itens vazio", () => {
    const input = { ...pedidoValido, itens: [] }
    const resultado = validarPedido(input)
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("itens")
  })

  it("deve falhar se preço negativo", () => {
    const input = { ...pedidoValido, itens: [{ nome: "X", quantidade: 1, precoUnitario: -5 }] }
    const resultado = validarPedido(input)
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("precoUnitario")
  })
})
```

### Testes de Parser (WhatsApp)
```typescript
// src/lib/parsers/whatsapp.test.ts
import { describe, it, expect } from "vitest"
import { parseWhatsAppMessage } from "./whatsapp"

describe("parseWhatsAppMessage", () => {
  const casos = [
    {
      nome: "Pedido simples texto",
      mensagem: { type: "text", text: { body: "1 x-burger 30, 2 batata 15" } },
      esperado: { itens: 2, total: 60, temPreco: true },
    },
    {
      nome: "Pedido só itens",
      mensagem: { type: "text", text: { body: "2 hamburguer, 1 refri" } },
      esperado: { itens: 2, total: 0, temPreco: false },
    },
    {
      nome: "Com observação",
      mensagem: { type: "text", text: { body: "1 x-salada sem tomate" } },
      esperado: { itens: 1, observacao: "sem tomate" },
    },
    {
      nome: "Imagem com legenda",
      mensagem: { type: "image", image: { caption: "1 x-bacon 35", mime_type: "image/jpeg" } },
      esperado: { itens: 1, total: 35, temMidia: true },
    },
    {
      nome: "Localização",
      mensagem: { type: "location", location: { latitude: -23.5, longitude: -46.6, address: "Rua A, 123" } },
      esperado: { temLocalizacao: true, endereco: "Rua A, 123" },
    },
  ]

  for (const c of casos) {
    it(`deve parsear: ${c.nome}`, () => {
      const resultado = parseWhatsAppMessage(c.mensagem)
      expect(resultado.success).toBe(true)
      if (c.esperado.itens) expect(resultado.data.itens).toHaveLength(c.esperado.itens)
      if (c.esperado.total) expect(resultado.data.total).toBe(c.esperado.total)
      if (c.esperado.observacao) expect(resultado.data.observacao).toContain(c.esperado.observacao)
      if (c.esperado.temMidia) expect(resultado.data.midia).toBeDefined()
      if (c.esperado.temLocalizacao) expect(resultado.data.localizacao).toBeDefined()
    })
  }

  it("deve falhar para tipo não suportado", () => {
    const resultado = parseWhatsAppMessage({ type: "video", video: {} })
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("não suportado")
  })
})
```

### Testes de Hooks (React)
```typescript
// src/hooks/useSSE.test.tsx
import { describe, it, expect, vi, beforeEach, afterEach } from "vitest"
import { renderHook, act } from "@testing-library/react"
import { useSSE } from "./useSSE"

// Mock EventSource global
class MockEventSource {
  static CONNECTING = 0
  static OPEN = 1
  static CLOSED = 2
  
  readyState = MockEventSource.CONNECTING
  onopen: (() => void) | null = null
  onerror: (() => void) | null = null
  onmessage: ((e: MessageEvent) => void) | null = null
  url: string
  
  constructor(url: string) {
    this.url = url
    setTimeout(() => { this.readyState = MockEventSource.OPEN; this.onopen?.() }, 10)
  }
  
  close() { this.readyState = MockEventSource.CLOSED }
  
  simulateMessage(data: string, lastEventId = "") {
    this.onmessage?.({ data, lastEventId } as MessageEvent)
  }
  
  simulateError() { this.onerror?.(); this.close() }
}

global.EventSource = MockEventSource as any

describe("useSSE", () => {
  beforeEach(() => vi.useFakeTimers())
  afterEach(() => vi.useRealTimers())

  it("deve conectar e receber mensagens", () => {
    const { result } = renderHook(() => useSSE("loja-123"))
    
    expect(result.current.status).toBe("connecting")
    
    act(() => vi.advanceTimersByTime(20))
    expect(result.current.status).toBe("open")
    
    const es = (global.EventSource as any).instances[0]
    act(() => es.simulateMessage(JSON.stringify({ type: "NEW_ORDER", payload: { id: "1" } }), "evt-1"))
    
    expect(result.current.lastEventId).toBe("evt-1")
  })

  it("deve reconectar ao erro", () => {
    const { result } = renderHook(() => useSSE("loja-123"))
    
    act(() => vi.advanceTimersByTime(20))
    expect(result.current.status).toBe("open")
    
    const es = (global.EventSource as any).instances[0]
    act(() => es.simulateError())
    
    expect(result.current.status).toBe("closed")
    
    act(() => vi.advanceTimersByTime(3100))
    expect(result.current.status).toBe("connecting")
    act(() => vi.advanceTimersByTime(20))
    expect(result.current.status).toBe("open")
  })
})
```

---

## Testes de Integração (API Routes + Server Actions)

### Testando Server Actions
```typescript
// src/actions/pedidos.test.ts
import { describe, it, expect, vi, beforeEach } from "vitest"
import { atualizarStatus } from "./pedidos"
import { prisma } from "@/lib/prisma"
import { revalidatePath } from "next/cache"
import { notificarSSE } from "@/lib/sse"

vi.mock("next/cache", () => ({ revalidatePath: vi.fn() }))
vi.mock("@/lib/sse", () => ({ notificarSSE: vi.fn() }))
vi.mock("@/lib/prisma", () => ({ prisma: { pedido: { findUnique: vi.fn(), update: vi.fn() } } }))

describe("atualizarStatus (Server Action)", () => {
  beforeEach(() => vi.clearAllMocks())

  it("deve atualizar status e notificar SSE", async () => {
    // Arrange
    const pedidoMock = { id: "ped-1", lojaId: "loja-1", status: "NOVO", numero: 1 }
    const atualizadoMock = { ...pedidoMock, status: "PREPARANDO", updatedAt: new Date() }
    
    vi.mocked(prisma.pedido.findUnique).mockResolvedValue(pedidoMock)
    vi.mocked(prisma.pedido.update).mockResolvedValue(atualizadoMock)
    vi.mocked(notificarSSE).mockResolvedValue(undefined)

    // Act
    const resultado = await atualizarStatus({
      pedidoId: "ped-1",
      status: "PREPARANDO",
      usuarioId: "loja-1",
    })

    // Assert
    expect(resultado.success).toBe(true)
    expect(prisma.pedido.update).toHaveBeenCalledWith({
      where: { id: "ped-1" },
      data: { status: "PREPARANDO", updatedAt: expect.any(Date) },
    })
    expect(revalidatePath).toHaveBeenCalledWith("/dashboard")
    expect(notificarSSE).toHaveBeenCalledWith("loja-1", {
      type: "STATUS_CHANGED",
      pedido: atualizadoMock,
    })
  })

  it("deve falhar se pedido não encontrado", async () => {
    vi.mocked(prisma.pedido.findUnique).mockResolvedValue(null)
    
    const resultado = await atualizarStatus({
      pedidoId: "inexistente",
      status: "PREPARANDO",
      usuarioId: "loja-1",
    })
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("não encontrado")
  })

  it("deve falhar se transição inválida", async () => {
    vi.mocked(prisma.pedido.findUnique).mockResolvedValue({ 
      id: "ped-1", lojaId: "loja-1", status: "ENTREGUE" 
    })
    
    const resultado = await atualizarStatus({
      pedidoId: "ped-1",
      status: "PREPARANDO", // ENTREGUE -> PREPARANDO inválido
      usuarioId: "loja-1",
    })
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("Transição inválida")
  })

  it("deve falhar se usuário não é dono do pedido", async () => {
    vi.mocked(prisma.pedido.findUnique).mockResolvedValue({ 
      id: "ped-1", lojaId: "loja-2", status: "NOVO" 
    })
    
    const resultado = await atualizarStatus({
      pedidoId: "ped-1",
      status: "PREPARANDO",
      usuarioId: "loja-1", // Diferente!
    })
    
    expect(resultado.success).toBe(false)
    expect(resultado.error).toContain("Não autorizado")
  })
})
```

### Testando API Route (Webhook)
```typescript
// src/app/api/webhook/whatsapp/route.test.ts
import { describe, it, expect, vi, beforeEach } from "vitest"
import { POST } from "./route"
import { prisma } from "@/lib/prisma"
import { parseWhatsAppMessage } from "@/lib/parsers/whatsapp"
import { processarPedido } from "@/lib/pedidos/processor"

vi.mock("@/lib/prisma")
vi.mock("@/lib/parsers/whatsapp")
vi.mock("@/lib/pedidos/processor")

describe("POST /api/webhook/whatsapp", () => {
  beforeEach(() => vi.clearAllMocks())

  const createRequest = (body: object, signature = "valid-sig") =>
    new Request("https://app.com/api/webhook/whatsapp", {
      method: "POST",
      headers: { "content-type": "application/json", "x-hub-signature-256": signature },
      body: JSON.stringify(body),
    })

  it("deve processar mensagem válida e retornar 200", async () => {
    const webhookBody = {
      entry: [{ changes: [{ value: { messages: [{ id: "msg-1", type: "text", text: { body: "1 burger" } }] } }] },
    }
    
    vi.mocked(parseWhatsAppMessage).mockReturnValue({
      success: true,
      data: { clienteNome: "Cliente", clienteTelefone: "11999999999", itens: [{ nome: "burger", quantidade: 1 }], total: 0 },
    })
    vi.mocked(processarPedido).mockResolvedValue({ success: true, pedidoId: "ped-1" })
    vi.mocked(prisma.webhookEvent.create).mockResolvedValue({})

    const req = createRequest(webhookBody)
    const response = await POST(req)
    
    expect(response.status).toBe(200)
    expect(await response.json()).toEqual({ success: true })
  })

  it("deve rejeitar assinatura inválida (401)", async () => {
    const req = createRequest({}, "invalid-sig")
    const response = await POST(req)
    
    expect(response.status).toBe(401)
  })

  it("deve retornar 200 mesmo se processamento falhar (idempotência)", async () => {
    vi.mocked(parseWhatsAppMessage).mockReturnValue({ success: false, error: "parse error" })
    vi.mocked(prisma.webhookEvent.create).mockResolvedValue({})
    
    const req = createRequest({ entry: [{ changes: [{ value: { messages: [{ id: "msg-1" }] } }] }] })
    const response = await POST(req)
    
    expect(response.status).toBe(200) // Não retenta webhook
    expect(prisma.webhookEvent.create).toHaveBeenCalledWith(
      expect.objectContaining({ status: "ERRO_PARSE" })
    )
  })

  it("deve ignorar mensagem duplicada (idempotência)", async () => {
    vi.mocked(prisma.webhookEvent.findUnique).mockResolvedValue({ id: "msg-1" }) // Já existe
    
    const req = createRequest({ entry: [{ changes: [{ value: { messages: [{ id: "msg-1" }] } }] }] })
    const response = await POST(req)
    
    expect(response.status).toBe(200)
    expect(prisma.webhookEvent.create).not.toHaveBeenCalled()
  })
})
```

---

## Testes E2E (Playwright)

### Page Objects
```typescript
// tests/e2e/pages/DashboardPage.ts
import { Page, Locator, expect } from "@playwright/test"

export class DashboardPage {
  readonly page: Page
  readonly pedidoList: Locator
  readonly filtroPeriodo: Locator
  readonly statusLIVE: Locator
  
  constructor(page: Page) {
    this.page = page
    this.pedidoList = page.locator('[role="list"]')
    this.filtroPeriodo = page.locator('select[name="periodo"]')
    this.statusLIVE = page.locator('text=LIVE')
  }
  
  async goto() {
    await this.page.goto("/dashboard")
    await expect(this.page).toHaveTitle(/Dashboard/)
  }
  
  async esperarCarregar() {
    await expect(this.pedidoList).toBeVisible()
  }
  
  async contarPedidos() {
    return await this.pedidoList.locator('[role="listitem"]').count()
  }
  
  async clicarStatus(pedidoNumero: number, status: string) {
    const card = this.pedidoList.locator(`[role="listitem"]`).nth(pedidoNumero)
    await card.locator(`button:has-text("${status}")`).click()
  }
  
  async verificarStatus(pedidoNumero: number, statusEsperado: string) {
    const card = this.pedidoList.locator(`[role="listitem"]`).nth(pedidoNumero)
    await expect(card.locator(".status-badge")).toHaveText(statusEsperado)
  }
  
  async verificarSSEAtivo() {
    await expect(this.statusLIVE).toBeVisible()
  }
}
```

### Testes @smoke (Críticos - Devem Passar Sempre)
```typescript
// tests/e2e/smoke.spec.ts
import { test, expect } from "@playwright/test"
import { DashboardPage } from "./pages/DashboardPage"
import { LoginPage } from "./pages/LoginPage"

test.describe("@smoke Fluxo Crítico", () => {
  let dashboard: DashboardPage
  
  test.beforeEach(async ({ page }) => {
    const login = new LoginPage(page)
    await login.goto()
    await login.loginMagicLink("test@example.com")
    dashboard = new DashboardPage(page)
    await dashboard.goto()
    await dashboard.esperarCarregar()
  })
  
  test("@smoke Deve carregar dashboard e mostrar lista vazia inicial", async () => {
    await expect(dashboard.pedidoList).toBeVisible()
    // Pode estar vazio ou com pedidos
  })
  
  test("@smoke Deve mostrar indicador LIVE", async () => {
    await dashboard.verificarSSEAtivo()
  })
  
  test("@smoke Login Magic Link funciona", async ({ page }) => {
    // Já testado no beforeEach
    await expect(page).toHaveURL(/\/dashboard/)
  })
})
```

### Testes @core (Funcionalidades Principais)
```typescript
// tests/e2e/core.spec.ts
import { test, expect } from "@playwright/test"
import { DashboardPage } from "./pages/DashboardPage"
import { test as setup } from "./fixtures/auth"

test.describe("@core Pedidos", () => {
  test.use({ storageState: "tests/.auth/user.json" })
  
  let dashboard: DashboardPage
  
  test.beforeEach(async ({ page }) => {
    dashboard = new DashboardPage(page)
    await dashboard.goto()
    await dashboard.esperarCarregar()
  })
  
  test("@core Deve processar fluxo completo: Novo -> Preparando -> Pronto -> Entregue", async () => {
    // Simular pedido via API (ou seed)
    await dashboard.page.request.post("/api/test/seed-pedido", { data: { itens: [{ nome: "X-Burger", qtd: 1 }] } })
    
    // Aguardar SSE
    await expect(dashboard.pedidoList.locator('[role="listitem"]').first()).toBeVisible({ timeout: 5000 })
    
    // Fluxo de status
    await dashboard.clicarStatus(0, "PREPARANDO")
    await dashboard.verificarStatus(0, "Preparando")
    
    await dashboard.clicarStatus(0, "PRONTO")
    await dashboard.verificarStatus(0, "Pronto")
    
    await dashboard.clicarStatus(0, "ENTREGUE")
    await dashboard.verificarStatus(0, "Entregue")
  })
  
  test("@core Deve filtrar pedidos por período", async () => {
    await dashboard.filtroPeriodo.selectOption("semana")
    await expect(dashboard.pedidoList).toBeVisible()
    // Verificar que filtro aplicado (URL ou estado)
  })
  
  test("@core Deve cancelar pedido com motivo", async () => {
    await dashboard.page.request.post("/api/test/seed-pedido", { data: { status: "NOVO" } })
    await dashboard.esperarCarregar()
    
    await dashboard.clicarStatus(0, "CANCELADO")
    // Modal de motivo deve abrir
    await expect(dashboard.page.locator('[role="dialog"]')).toBeVisible()
    await dashboard.page.fill('textarea[name="motivo"]', "Cliente desistiu")
    await dashboard.page.click('button:has-text("Confirmar")')
    
    await dashboard.verificarStatus(0, "Cancelado")
  })
})
```

### Testes de Acessibilidade (@a11y)
```typescript
// tests/e2e/a11y.spec.ts
import { test, expect } from "@playwright/test"
import { injectAxe, checkA11y } from "axe-playwright"

test.describe("@a11y Acessibilidade", () => {
  test.use({ storageState: "tests/.auth/user.json" })
  
  test("@a11y Dashboard - sem violações críticas", async ({ page }) => {
    await page.goto("/dashboard")
    await injectAxe(page)
    await checkA11y(page, undefined, {
      detailedReport: true,
      includedImpacts: ["critical", "serious"],
    })
  })
  
  test("@a11y Login - navegação por teclado", async ({ page }) => {
    await page.goto("/login")
    await injectAxe(page)
    
    // Tab navigation
    await page.keyboard.press("Tab")
    await expect(page.locator('input[type="email"]')).toBeFocused()
    
    await page.keyboard.press("Tab")
    await expect(page.locator('button[type="submit"]')).toBeFocused()
    
    await checkA11y(page)
  })
  
  test("@a11y PedidoCard - screen reader anuncia status", async ({ page }) => {
    await page.goto("/dashboard")
    await injectAxe(page)
    
    const card = page.locator('[role="listitem"]').first()
    const statusBadge = card.locator(".status-badge")
    
    // Verificar role e aria-label
    await expect(statusBadge).toHaveAttribute("role", "status")
    await checkA11y(card)
  })
})
```

### Testes Visuais
```typescript
// tests/e2e/visual.spec.ts
import { test, expect } from "@playwright/test"

test.describe("Visual Regression", () => {
  test("dashboard desktop", async ({ page }) => {
    await page.goto("/dashboard")
    await page.waitForLoadState("networkidle")
    await expect(page).toHaveScreenshot("dashboard-desktop.png", { fullPage: true })
  })
  
  test("dashboard mobile", async ({ page }) => {
    await page.setViewportSize({ width: 375, height: 667 })
    await page.goto("/dashboard")
    await page.waitForLoadState("networkidle")
    await expect(page).toHaveScreenshot("dashboard-mobile.png", { fullPage: true })
  })
  
  test("pedido card estados", async ({ page }) => {
    await page.goto("/dashboard")
    // Seed pedidos em diferentes estados
    await page.request.post("/api/test/seed-pedidos-estados")
    await page.waitForLoadState("networkidle")
    
    const cards = page.locator('[role="listitem"]')
    for (let i = 0; i < await cards.count(); i++) {
      await expect(cards.nth(i)).toHaveScreenshot(`pedido-card-${i}.png`)
    }
  })
})
```

---

## Testes de Carga (k6)

### Script Básico
```javascript
// tests/load/dashboard.js
import http from 'k6/http'
import { check, sleep } from 'k6'
import { Rate } from 'k6/metrics'

const errorRate = new Rate('errors')

export const options = {
  stages: [
    { duration: '30s', target: 10 },   // Ramp up
    { duration: '1m', target: 50 },    // Sustained
    { duration: '30s', target: 100 },  // Stress
    { duration: '30s', target: 0 },    // Ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],      // Dashboard p95 < 500ms
    http_req_failed: ['rate<0.01'],        // Error rate < 1%
    errors: ['rate<0.01'],
  },
}

const BASE_URL = __ENV.BASE_URL || 'http://localhost:3000'
const AUTH_TOKEN = __ENV.AUTH_TOKEN // Obter via script de login

export default function () {
  const headers = {
    'Cookie': `next-auth.session-token=${AUTH_TOKEN}`,
    'Accept': 'text/html',
  }
  
  // Dashboard
  let res = http.get(`${BASE_URL}/dashboard`, { headers })
  check(res, { 'dashboard 200': (r) => r.status === 200 }) || errorRate.add(1)
  
  // API pedidos
  res = http.get(`${BASE_URL}/api/pedidos?periodo=hoje`, { headers })
  check(res, { 'api pedidos 200': (r) => r.status === 200 }) || errorRate.add(1)
  
  sleep(1)
}

export function handleSummary(data) {
  return {
    'stdout': textSummary(data, { indent: ' ', enableColors: true }),
    'load-summary.json': JSON.stringify(data),
  }
}
```

### Rodar Load Test
```bash
# Local
AUTH_TOKEN=$(node scripts/get-auth-token.js) BASE_URL=http://localhost:3000 k6 run tests/load/dashboard.js

# CI (com token de service account)
k6 run tests/load/dashboard.js --env BASE_URL=https://app.seudominio.com --env AUTH_TOKEN=$PROD_AUTH_TOKEN
```

---

## Mocks e Fixtures

### Factory de Dados (Test Data Builders)
```typescript
// tests/factories/pedido.ts
import { Pedido, ItemPedido, StatusPedido } from "@prisma/client"

export function buildPedido(overrides: Partial<Pedido> = {}): Pedido {
  return {
    id: "ped-" + Math.random().toString(36).slice(2),
    numero: Math.floor(Math.random() * 10000),
    lojaId: "loja-1",
    clienteNome: "Cliente Teste",
    clienteTelefone: "11999999999",
    status: "NOVO",
    total: 0,
    canal: "WHATSAPP",
    observacao: null,
    createdAt: new Date(),
    updatedAt: new Date(),
    ...overrides,
  }
}

export function buildItemPedido(overrides: Partial<ItemPedido> = {}): ItemPedido {
  return {
    id: "item-" + Math.random().toString(36).slice(2),
    pedidoId: "ped-1",
    nome: "X-Burger",
    quantidade: 1,
    precoUnitario: 25,
    observacao: null,
    ...overrides,
  }
}

export function buildPedidoComItens(qtdItens = 2, overrides: Partial<Pedido> = {}) {
  const pedido = buildPedido(overrides)
  const itens = Array.from({ length: qtdItens }, (_, i) => 
    buildItemPedido({ pedidoId: pedido.id, nome: `Item ${i + 1}`, precoUnitario: 10 + i * 5 })
  )
  const total = itens.reduce((s, i) => s + i.quantidade * i.precoUnitario, 0)
  return { ...pedido, total, itens }
}
```

### Fixtures Playwright (Auth)
```typescript
// tests/e2e/fixtures/auth.ts
import { test as base, Page } from "@playwright/test"
import fs from "fs"
import path from "path"

const AUTH_FILE = path.resolve(__dirname, "../.auth/user.json")

interface AuthFixture {
  authenticatedPage: Page
}

export const test = base.extend<AuthFixture>({
  authenticatedPage: async ({ page }, use) => {
    if (fs.existsSync(AUTH_FILE)) {
      await page.context().storageState({ path: AUTH_FILE })
    } else {
      // Login real se não existe
      await page.goto("/login")
      await page.fill('input[name="email"]', process.env.TEST_EMAIL!)
      await page.click('button[type="submit"]')
      // Aguardar email mágico (ou usar credenciais se configurado)
      await page.waitForURL("/dashboard")
      await page.context().storageState({ path: AUTH_FILE })
    }
    await use(page)
  },
})

export { expect } from "@playwright/test"
```

---

## CI/CD Pipeline (GitHub Actions)

```yaml
# .github/workflows/test.yml
name: Test Suite

on: [push, pull_request]

jobs:
  unit-integration:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm run test:coverage
      - uses: codecov/codecov-action@v4
        with: { files: ./coverage/lcov.info }

  e2e:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npm run build
      - run: npm run test:e2e
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 7

  a11y:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npm run test:a11y

  load:
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: grafana/k6-action@v0.2.0
        with:
          filename: tests/load/dashboard.js
          env: BASE_URL=https://app.seudominio.com AUTH_TOKEN=${{ secrets.LOAD_TEST_TOKEN }}

  quality-gate:
    needs: [unit-integration, e2e, a11y]
    runs-on: ubuntu-latest
    steps:
      - run: echo "All tests passed - ready for deploy"
```

---

## Debugging Testes

### Vitest Debug
```bash
# VS Code launch.json
{
  "type": "node",
  "request": "launch",
  "name": "Debug Vitest",
  "program": "${workspaceFolder}/node_modules/vitest/vitest.mjs",
  "args": ["run", "--reporter=verbose"],
  "console": "integratedTerminal"
}
```

### Playwright Debug
```bash
# Modo headed + devtools
npm run test:e2e:headed

# Debug específico
npx playwright test tests/e2e/core.spec.ts --debug

# Trace viewer
npx playwright show-trace trace.zip
```

### Logs Úteis
```typescript
// Em testes: debugar estado
import { screen } from "@testing-library/react"

// Print DOM
console.log(screen.getByRole("list").outerHTML)

// Query helpers
screen.getByTestId("pedido-1") // data-testid
screen.getByRole("button", { name: /preparando/i })
screen.getByLabelText(/email/i)
screen.getByPlaceholderText(/digite/i)
screen.getByText(/erro/i)
```

---

## Checklist por PR

- [ ] Testes unitários para nova lógica (cobertura ≥ targets)
- [ ] Testes integração para Server Actions / API routes novas
- [ ] Testes E2E @smoke + @core para fluxos user-facing
- [ ] Testes a11y para novos componentes/páginas
- [ ] Testes visuais se UI mudou significativamente
- [ ] `npm run test` passa localmente
- [ ] `npm run test:e2e` passa localmente
- [ ] CI verde no PR
- [ ] Coverage não caiu (Codecov/PR comment)

---

## Métricas de Qualidade (Dashboard)

| Métrica | Target | Onde Ver |
|---------|--------|----------|
| Unit Coverage | ≥ 80% lines, ≥ 70% branches | Codecov / Vitest HTML |
| E2E Pass Rate | 100% @smoke, 100% @core | GitHub Actions / Playwright Report |
| A11y Violations | 0 critical, 0 serious | Playwright Report / axe |
| Flaky Tests | 0 | Playwright retries / History |
| Test Duration | Unit < 30s, E2E < 3min | CI Logs |
| Load Test p95 | Dashboard < 500ms, Webhook < 2s | k6 Summary |

---

## Próximos Passos
- [[03 - Guias Práticos/Guia de Segurança]] — OWASP, LGPD, secrets
- [[03 - Guias Práticos/Guia de Publicação]] — Deploy, monitoramento, runbook
- [[02 - Modelos/Testes/Estratégia de Testes]] — Estratégia completa
- [[02 - Modelos/Testes/Plano de Testes]] — Plano faseado
- [[02 - Modelos/Testes/Casos de Teste]] — 80+ casos detalhados
- [[04 - Referência/Glossário/Test Pyramid]]
- [[04 - Referência/Glossário/Vitest]]
- [[04 - Referência/Glossário/Playwright]]
- [[04 - Referência/Glossário/k6]]
- [[04 - Referência/Ferramentas/Testing Library]]