# Guia de UI/UX

> **Local:** `03 - Guias Práticos/Guia de UI/UX.md`
> **Baseado em:** [[02 - Modelos/UX e UI/Design System - Tokens]], [[02 - Modelos/UX e UI/Especificação de Componentes]], [[02 - Modelos/UX e UI/Acessibilidade e Checklist UI]]
> **Objetivo:** Referência prática para implementar interfaces consistentes, acessíveis e usáveis.

---

## Design System - Tokens (Fonte da Verdade)

> **Arquivo:** `src/styles/tokens.css` (importado em `globals.css`)

```css
:root {
  /* === CORES === */
  /* Primária - Brand */
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-200: #bfdbfe;
  --color-primary-300: #93c5fd;
  --color-primary-400: #60a5fa;
  --color-primary-500: #3b82f6;  /* Main */
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
  --color-primary-800: #1e40af;
  --color-primary-900: #1e3a8a;
  --color-primary-950: #172554;

  /* Neutra - UI Base */
  --color-neutral-0: #ffffff;
  --color-neutral-50: #fafafa;
  --color-neutral-100: #f5f5f5;
  --color-neutral-200: #e5e5e5;
  --color-neutral-300: #d4d4d4;
  --color-neutral-400: #a3a3a3;
  --color-neutral-500: #737373;
  --color-neutral-600: #525252;
  --color-neutral-700: #404040;
  --color-neutral-800: #262626;
  --color-neutral-900: #171717;
  --color-neutral-950: #0a0a0a;

  /* Semânticas */
  --color-success: #16a34a;
  --color-success-bg: #dcfce7;
  --color-warning: #f59e0b;
  --color-warning-bg: #fef3c7;
  --color-error: #dc2626;
  --color-error-bg: #fee2e2;
  --color-info: #3b82f6;
  --color-info-bg: #dbeafe;

  /* === ESPAÇAMENTO (base 4px) === */
  --space-0: 0;
  --space-1: 4px;   /* 0.25rem */
  --space-2: 8px;   /* 0.5rem */
  --space-3: 12px;  /* 0.75rem */
  --space-4: 16px;  /* 1rem */
  --space-5: 20px;  /* 1.25rem */
  --space-6: 24px;  /* 1.5rem */
  --space-8: 32px;  /* 2rem */
  --space-10: 40px; /* 2.5rem */
  --space-12: 48px; /* 3rem */
  --space-16: 64px; /* 4rem */

  /* === TIPOGRAFIA === */
  --font-sans: "Inter", system-ui, sans-serif;
  --font-mono: "JetBrains Mono", monospace;

  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.125rem;   /* 18px */
  --text-xl: 1.25rem;    /* 20px */
  --text-2xl: 1.5rem;    /* 24px */
  --text-3xl: 1.875rem;  /* 30px */
  --text-4xl: 2.25rem;   /* 36px */

  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;

  --leading-tight: 1.25;
  --leading-normal: 1.5;
  --leading-relaxed: 1.75;

  /* === SOMBRAS === */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-base: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);

  /* === RAIOS === */
  --radius-none: 0;
  --radius-sm: 4px;
  --radius-base: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-xl: 24px;
  --radius-full: 9999px;

  /* === MOTION === */
  --duration-fast: 150ms;
  --duration-base: 200ms;
  --duration-slow: 300ms;
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);

  /* === Z-INDEX === */
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-modal: 300;
  --z-popover: 400;
  --z-tooltip: 500;
  --z-toast: 600;

  /* === BREAKPOINTS (Tailwind) === */
  --bp-sm: 640px;
  --bp-md: 768px;
  --bp-lg: 1024px;
  --bp-xl: 1280px;
  --bp-2xl: 1536px;
}

/* Dark Mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg-primary: var(--color-neutral-950);
    --color-bg-secondary: var(--color-neutral-900);
    --color-bg-tertiary: var(--color-neutral-800);
    --color-text-primary: var(--color-neutral-50);
    --color-text-secondary: var(--color-neutral-400);
    --color-text-muted: var(--color-neutral-500);
    --color-border: var(--color-neutral-700);
  }
}

:root[data-theme="light"] {
  --color-bg-primary: var(--color-neutral-0);
  --color-bg-secondary: var(--color-neutral-50);
  --color-bg-tertiary: var(--color-neutral-100);
  --color-text-primary: var(--color-neutral-950);
  --color-text-secondary: var(--color-neutral-600);
  --color-text-muted: var(--color-neutral-400);
  --color-border: var(--color-neutral-200);
}
```

### Uso nos Componentes (Tailwind + CSS Variables)

```tsx
// Bom: Usar tokens semânticos
<div className="bg-[var(--color-bg-primary)] text-[var(--color-text-primary)]">
  <button className="bg-[var(--color-primary-500)] hover:bg-[var(--color-primary-600)] 
                       text-white px-4 py-2 rounded-[var(--radius-base)] 
                       transition-colors duration-[var(--duration-fast)]">
    Ação Principal
  </button>
</div>

// Ruim: Hardcoded values
<div className="bg-white text-gray-900">
  <button className="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg">
    Ação Principal
  </button>
</div>
```

---

## Componentes Atômicos (Base)

### Button
```tsx
// src/components/ui/Button.tsx
"use client"
import { forwardRef, ButtonHTMLAttributes } from "react"
import { cn } from "@/lib/utils"

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: "primary" | "secondary" | "outline" | "ghost" | "danger"
  size?: "sm" | "md" | "lg"
  loading?: boolean
}

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant = "primary", size = "md", loading, disabled, children, ...props }, ref) => {
    const base = "inline-flex items-center justify-center font-medium transition-colors duration-[var(--duration-fast)] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[var(--color-primary-500)] focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none"
    
    const variants = {
      primary: "bg-[var(--color-primary-500)] text-white hover:bg-[var(--color-primary-600)] active:bg-[var(--color-primary-700)]",
      secondary: "bg-[var(--color-neutral-100)] text-[var(--color-neutral-900)] hover:bg-[var(--color-neutral-200)] active:bg-[var(--color-neutral-300)] dark:bg-[var(--color-neutral-800)] dark:text-[var(--color-neutral-100)] dark:hover:bg-[var(--color-neutral-700)]",
      outline: "border border-[var(--color-border)] bg-transparent hover:bg-[var(--color-bg-tertiary)]",
      ghost: "bg-transparent hover:bg-[var(--color-bg-tertiary)]",
      danger: "bg-[var(--color-error)] text-white hover:bg-[var(--color-error)]/90",
    }
    
    const sizes = {
      sm: "h-8 px-3 text-sm rounded-[var(--radius-sm)] gap-1.5",
      md: "h-10 px-4 text-sm rounded-[var(--radius-base)] gap-2",
      lg: "h-12 px-6 text-base rounded-[var(--radius-md)] gap-2",
    }

    return (
      <button
        ref={ref}
        className={cn(base, variants[variant], sizes[size], className)}
        disabled={disabled || loading}
        {...props}
      >
        {loading && <Spinner className="h-4 w-4" />}
        {children}
      </button>
    )
  }
)
Button.displayName = "Button"
```

### Input
```tsx
// src/components/ui/Input.tsx
"use client"
import { forwardRef, InputHTMLAttributes, LabelHTMLAttributes } from "react"
import { cn } from "@/lib/utils"

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label?: string
  error?: string
  hint?: string
}

export const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ className, label, error, hint, id, ...props }, ref) => {
    const inputId = id || label?.toLowerCase().replace(/\s+/g, "-")
    
    return (
      <div className="space-y-1.5 w-full">
        {label && (
          <label htmlFor={inputId} className="block text-sm font-medium text-[var(--color-text-primary)]">
            {label}
          </label>
        )}
        <input
          ref={ref}
          id={inputId}
          className={cn(
            "w-full h-10 px-3 text-sm bg-[var(--color-bg-primary)] text-[var(--color-text-primary)]",
            "border border-[var(--color-border)] rounded-[var(--radius-base)]",
            "placeholder:text-[var(--color-text-muted)]",
            "focus:outline-none focus:ring-2 focus:ring-[var(--color-primary-500)] focus:border-transparent",
            "disabled:opacity-50 disabled:cursor-not-allowed",
            "transition-colors duration-[var(--duration-fast)]",
            error && "border-[var(--color-error)] focus:ring-[var(--color-error)]",
            className
          )}
          aria-invalid={error ? "true" : "false"}
          aria-describedby={error ? `${inputId}-error` : hint ? `${inputId}-hint` : undefined}
          {...props}
        />
        {error && (
          <p id={`${inputId}-error`} className="text-sm text-[var(--color-error)]" role="alert">
            {error}
          </p>
        )}
        {hint && !error && (
          <p id={`${inputId}-hint`} className="text-sm text-[var(--color-text-muted)]">
            {hint}
          </p>
        )}
      </div>
    )
  }
)
Input.displayName = "Input"
```

### Card
```tsx
// src/components/ui/Card.tsx
import { HTMLAttributes, forwardRef } from "react"
import { cn } from "@/lib/utils"

interface CardProps extends HTMLAttributes<HTMLDivElement> {
  variant?: "default" | "outlined" | "elevated"
  padding?: "none" | "sm" | "md" | "lg"
}

export const Card = forwardRef<HTMLDivElement, CardProps>(
  ({ className, variant = "default", padding = "md", children, ...props }, ref) => {
    const variants = {
      default: "bg-[var(--color-bg-secondary)] border border-[var(--color-border)]",
      outlined: "bg-transparent border-2 border-[var(--color-border)]",
      elevated: "bg-[var(--color-bg-primary)] shadow-[var(--shadow-lg)] border-none",
    }
    
    const paddings = {
      none: "",
      sm: "p-3",
      md: "p-4",
      lg: "p-6",
    }

    return (
      <div
        ref={ref}
        className={cn("rounded-[var(--radius-lg)]", variants[variant], paddings[padding], className)}
        {...props}
      >
        {children}
      </div>
    )
  }
)
Card.displayName = "Card"

export const CardHeader = forwardRef<HTMLDivElement, HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn("mb-3", className)} {...props} />
  )
)
CardHeader.displayName = "CardHeader"

export const CardTitle = forwardRef<HTMLHeadingElement, HTMLAttributes<HTMLHeadingElement>>(
  ({ className, ...props }, ref) => (
    <h3 ref={ref} className={cn("text-lg font-semibold text-[var(--color-text-primary)]", className)} {...props} />
  )
)
CardTitle.displayName = "CardTitle"

export const CardContent = forwardRef<HTMLDivElement, HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn("text-[var(--color-text-secondary)]", className)} {...props} />
  )
)
CardContent.displayName = "CardContent"
```

---

## Componentes Compostos (Molecules)

### StatusBadge (Pedidos)
```tsx
// src/components/pedidos/StatusBadge.tsx
type Status = "NOVO" | "PREPARANDO" | "PRONTO" | "ENTREGUE" | "CANCELADO"

const STATUS_CONFIG: Record<Status, { label: string; className: string }> = {
  NOVO: { label: "Novo", className: "bg-[var(--color-info-bg)] text-[var(--color-info)]" },
  PREPARANDO: { label: "Preparando", className: "bg-[var(--color-warning-bg)] text-[var(--color-warning)]" },
  PRONTO: { label: "Pronto", className: "bg-[var(--color-success-bg)] text-[var(--color-success)]" },
  ENTREGUE: { label: "Entregue", className: "bg-[var(--color-neutral-100)] text-[var(--color-neutral-600)] dark:bg-[var(--color-neutral-800)] dark:text-[var(--color-neutral-400)]" },
  CANCELADO: { label: "Cancelado", className: "bg-[var(--color-error-bg)] text-[var(--color-error)]" },
}

export function StatusBadge({ status, size = "md" }: { status: Status; size?: "sm" | "md" }) {
  const config = STATUS_CONFIG[status]
  const sizes = {
    sm: "px-2 py-0.5 text-xs",
    md: "px-2.5 py-1 text-sm",
  }
  
  return (
    <span className={cn(
      "inline-flex items-center font-medium rounded-full",
      config.className,
      sizes[size]
    )}>
      {config.label}
    </span>
  )
}
```

### PedidoCard (Organism)
```tsx
// src/components/pedidos/PedidoCard.tsx
import { Card, CardContent } from "@/components/ui/Card"
import { Button } from "@/components/ui/Button"
import { StatusBadge } from "./StatusBadge"
import { formatarMoeda, formatarHora } from "@/lib/utils"
import { TRANSICOES_VALIDAS } from "@/lib/regras-negocio"

interface Props {
  pedido: PedidoComItens
  onAtualizar: (id: string, status: Status) => Promise<void>
  loading?: Set<string>
}

export function PedidoCard({ pedido, onAtualizar, loading }: Props) {
  const proximosStatus = TRANSICOES_VALIDAS[pedido.status] || []
  const isLoading = loading?.has(pedido.id)

  return (
    <Card variant="default" padding="md" className="relative">
      {/* Indicador LIVE SSE */}
      <div className="absolute top-3 right-3 flex items-center gap-1 text-xs text-[var(--color-success)]">
        <span className="w-1.5 h-1.5 rounded-full bg-current animate-pulse" />
        <span>LIVE</span>
      </div>

      <div className="flex justify-between items-start mb-3">
        <div>
          <p className="text-sm text-[var(--color-text-muted)]">Pedido #{pedido.numero}</p>
          <h3 className="font-medium text-[var(--color-text-primary)]">{pedido.clienteNome}</h3>
        </div>
        <StatusBadge status={pedido.status} />
      </div>

      <div className="grid grid-cols-2 gap-3 mb-3 text-sm">
        <div><span className="text-[var(--color-text-muted)]">Total</span><p className="font-medium">{formatarMoeda(pedido.total)}</p></div>
        <div><span className="text-[var(--color-text-muted)]">Itens</span><p className="font-medium">{pedido.itens.length}</p></div>
        <div><span className="text-[var(--color-text-muted)]">Hora</span><p>{formatarHora(pedido.createdAt)}</p></div>
        <div><span className="text-[var(--color-text-muted)]">Canal</span><p className="capitalize">{pedido.canal}</p></div>
      </div>

      {pedido.observacao && (
        <p className="text-sm text-[var(--color-text-secondary)] bg-[var(--color-bg-tertiary)] p-2 rounded-[var(--radius-sm)] mb-3">
          {pedido.observacao}
        </p>
      )}

      <div className="flex gap-2">
        {proximosStatus.map((s) => (
          <Button
            key={s}
            variant={s === "ENTREGUE" ? "primary" : s === "CANCELADO" ? "danger" : "outline"}
            size="sm"
            className="flex-1"
            onClick={() => onAtualizar(pedido.id, s)}
            disabled={isLoading}
          >
            {s}
          </Button>
        ))}
      </div>
    </Card>
  )
}
```

---

## Layouts e Páginas (Templates)

### Dashboard Layout
```tsx
// src/app/dashboard/page.tsx (Server Component)
import { Suspense } from "react"
import { PedidoListSkeleton } from "@/components/pedidos/PedidoListSkeleton"
import { PedidoListClient } from "./PedidoListClient"
import { getPedidosDashboard } from "@/actions/pedidos"

export default async function DashboardPage() {
  const pedidos = await getPedidosDashboard()
  
  return (
    <div className="container mx-auto px-4 py-6 space-y-6">
      <header className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
        <div>
          <h1 className="text-2xl font-bold text-[var(--color-text-primary)]">Dashboard</h1>
          <p className="text-[var(--color-text-secondary)]">Gerencie seus pedidos em tempo real</p>
        </div>
        <div className="flex gap-2">
          <FiltroPeriodo />
          <Button variant="outline">Exportar</Button>
        </div>
      </header>

      <Suspense fallback={<PedidoListSkeleton />}>
        <PedidoListClient initialPedidos={pedidos} />
      </Suspense>
    </div>
  )
}
```

### Client Component com SSE
```tsx
// src/app/dashboard/PedidoListClient.tsx
"use client"
import { useState, useEffect } from "react"
import { useSSE } from "@/hooks/useSSE"
import { PedidoCard } from "@/components/pedidos/PedidoCard"
import { atualizarStatus } from "@/actions/pedidos"
import { useSession } from "next-auth/react"

interface Props {
  initialPedidos: PedidoComItens[]
}

export function PedidoListClient({ initialPedidos }: Props) {
  const { data: session } = useSession()
  const [pedidos, setPedidos] = useState(initialPedidos)
  const [loading, setLoading] = useState<Set<string>>(new Set())
  const { status: sseStatus } = useSSE(session?.user.lojaId || "")

  useEffect(() => {
    const handleMessage = (e: CustomEvent) => {
      const { type, payload } = e.detail
      if (type === "NEW_ORDER") {
        setPedidos((prev) => [payload as PedidoComItens, ...prev])
      } else if (type === "STATUS_CHANGED") {
        setPedidos((prev) => prev.map((p) => (p.id === payload.id ? payload : p)))
      }
    }
    window.addEventListener("sse-message", handleMessage as EventListener)
    return () => window.removeEventListener("sse-message", handleMessage as EventListener)
  }, [])

  const handleAtualizar = async (id: string, status: Status) => {
    setLoading((prev) => new Set(prev).add(id))
    try {
      await atualizarStatus({ pedidoId: id, status, usuarioId: session!.user.id })
    } finally {
      setLoading((prev) => { const n = new Set(prev); n.delete(id); return n })
    }
  }

  if (pedidos.length === 0) {
    return (
      <div className="text-center py-12 text-[var(--color-text-muted)]">
        <svg className="mx-auto h-12 w-12 mb-4 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={1.5} d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
        </svg>
        <p className="text-lg">Nenhum pedido ainda</p>
        <p className="text-sm">Pedidos aparecerão aqui em tempo real</p>
      </div>
    )
  }

  return (
    <div className="space-y-4" role="list" aria-label="Lista de pedidos">
      {pedidos.map((p) => (
        <PedidoCard
          key={p.id}
          pedido={p}
          onAtualizar={handleAtualizar}
          loading={loading}
        />
      ))}
      {sseStatus !== "open" && (
        <div className="text-center text-sm text-[var(--color-warning)] p-3" role="status">
          ⚠️ Conexão em tempo real perdida. Tentando reconectar...
        </div>
      )}
    </div>
  )
}
```

---

## Acessibilidade (WCAG 2.1 AA) - Checklist Rápido

### Por Componente
| Componente | Requisitos Mínimos |
|------------|-------------------|
| **Button** | `type="button"`, foco visível, `aria-disabled`, loading = `aria-busy` |
| **Input** | `<label htmlFor>`, `aria-invalid`, `aria-describedby` (erro/hint) |
| **Select** | `<label>`, `aria-invalid`, opções com `role="option"` |
| **Modal/Dialog** | `role="dialog" aria-modal="true"`, foco trap, `Esc` fecha, restore focus |
| **Toast** | `role="status" aria-live="polite"`, auto-dismiss ≥ 5s, pause on hover |
| **Table** | `<thead><th scope="col">`, `<tbody><th scope="row">` |
| **Navigation** | `<nav aria-label>`, link ativo `aria-current="page"` |
| **Icon Button** | `aria-label` ou `<span className="sr-only">` |

### Utilitários A11y
```tsx
// src/components/ui/VisuallyHidden.tsx
export function VisuallyHidden({ children }: { children: React.ReactNode }) {
  return (
    <span className="sr-only">
      {children}
    </span>
  )
}

// src/hooks/useFocusTrap.ts
export function useFocusTrap(active: boolean) {
  const containerRef = useRef<HTMLDivElement>(null)
  useEffect(() => {
    if (!active) return
    const container = containerRef.current
    if (!container) return
    const focusable = container.querySelectorAll<HTMLElement>(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    )
    const first = focusable[0]
    const last = focusable[focusable.length - 1]
    const handleTab = (e: KeyboardEvent) => {
      if (e.key !== "Tab") return
      if (e.shiftKey && document.activeElement === first) { e.preventDefault(); last.focus() }
      if (!e.shiftKey && document.activeElement === last) { e.preventDefault(); first.focus() }
    }
    container.addEventListener("keydown", handleTab)
    first?.focus()
    return () => container.removeEventListener("keydown", handleTab)
  }, [active])
  return containerRef
}
```

---

## Responsividade (Mobile-First)

### Breakpoints Tailwind
```tsx
// Mobile first: classes base = mobile, sm: md: lg: xl: 2xl: para maiores
<div className="
  grid 
  grid-cols-1           /* Mobile: 1 coluna */
  sm:grid-cols-2        /* ≥640px: 2 colunas */
  lg:grid-cols-3        /* ≥1024px: 3 colunas */
  xl:grid-cols-4        /* ≥1280px: 4 colunas */
  gap-4
">
  {items.map(item => <Card key={item.id}>{item.content}</Card>)}
</div>
```

### Container Queries (Componentes que se adaptam ao pai)
```css
/* src/styles/container-queries.css */
.card-container {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 400px) {
  .card-grid { grid-template-columns: 1fr 1fr; }
}
@container card (min-width: 600px) {
  .card-grid { grid-template-columns: repeat(3, 1fr); }
}
```

---

## PWA - Install + Offline

### Manifest
```json
// public/manifest.json
{
  "name": "PedidosSimples",
  "short_name": "Pedidos",
  "description": "Gerencie pedidos do WhatsApp no dashboard",
  "start_url": "/dashboard",
  "display": "standalone",
  "background_color": "#fafafa",
  "theme_color": "#3b82f6",
  "orientation": "portrait-primary",
  "icons": [
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png", "purpose": "any maskable" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png", "purpose": "any maskable" }
  ],
  "categories": ["business", "productivity"],
  "shortcuts": [
    { "name": "Novo Pedido", "url": "/dashboard?new=1", "icons": [{ "src": "/icons/plus.png", "sizes": "96x96" }] }
  ]
}
```

### Service Worker (next-pwa)
```typescript
// next.config.js
const withPWA = require("next-pwa")({
  dest: "public",
  register: true,
  skipWaiting: true,
  disable: process.env.NODE_ENV === "development",
  runtimeCaching: [
    {
      urlPattern: /^https:\/\/fonts\.googleapis\.com\/.*/i,
      handler: "CacheFirst",
      options: { cacheName: "google-fonts", expiration: { maxEntries: 10, maxAgeSeconds: 60 * 60 * 24 * 365 } }
    },
    {
      urlPattern: /\/api\/pedidos/,
      handler: "NetworkFirst",
      options: { cacheName: "pedidos-api", networkTimeoutSeconds: 5, expiration: { maxEntries: 100, maxAgeSeconds: 60 * 60 * 24 } }
    }
  ]
})
```

### Install Prompt
```tsx
// src/components/ui/PWAInstallPrompt.tsx
"use client"
import { useState, useEffect, useRef } from "react"
import { Button } from "@/components/ui/Button"

export function PWAInstallPrompt() {
  const [show, setShow] = useState(false)
  const deferredPromptRef = useRef<BeforeInstallPromptEvent>()

  useEffect(() => {
    const handler = (e: BeforeInstallPromptEvent) => {
      e.preventDefault()
      deferredPromptRef.current = e
      setShow(true)
    }
    window.addEventListener("beforeinstallprompt", handler as EventListener)
    return () => window.removeEventListener("beforeinstallprompt", handler as EventListener)
  }, [])

  if (!show) return null

  const handleInstall = async () => {
    const prompt = deferredPromptRef.current
    if (!prompt) return
    await prompt.prompt()
    const { outcome } = await prompt.userChoice
    if (outcome === "accepted") setShow(false)
    deferredPromptRef.current = undefined
  }

  return (
    <div className="fixed bottom-4 left-4 right-4 md:left-auto md:right-4 md:bottom-4 md:w-80 z-[var(--z-toast)] animate-slide-up">
      <Card variant="elevated" padding="md" className="flex flex-col gap-3">
        <div>
          <p className="font-medium">Instalar PedidosSimples</p>
          <p className="text-sm text-[var(--color-text-secondary)]">Acesse rápido na tela inicial, funciona offline</p>
        </div>
        <div className="flex gap-2">
          <Button variant="outline" onClick={() => setShow(false)}>Agora não</Button>
          <Button onClick={handleInstall}>Instalar</Button>
        </div>
      </Card>
    </div>
  )
}
```

---

## Testes Visuais e de Regressão

### Storybook (Opcional - v1.1+)
```bash
npx storybook@latest init
```

### Chromatic / Playwright Visual
```typescript
// tests/visual/dashboard.spec.ts
import { test, expect } from "@playwright/test"

test("dashboard visual regression", async ({ page }) => {
  await page.goto("/dashboard")
  await expect(page).toHaveScreenshot("dashboard-desktop.png", {
    fullPage: true,
    animations: "disabled",
  })
})

test("dashboard mobile", async ({ page }) => {
  await page.setViewportSize({ width: 375, height: 667 })
  await page.goto("/dashboard")
  await expect(page).toHaveScreenshot("dashboard-mobile.png", { fullPage: true })
})
```

---

## Checklist de UI por Feature

- [ ] Usa tokens do Design System (não hardcoded)
- [ ] Componentes atômicos reutilizados (Button, Input, Card, Badge)
- [ ] Estados: default, hover, focus, active, disabled, loading, error
- [ ] Acessível: labels, roles, aria-*, foco visível, ordem tab lógica
- [ ] Responsivo: mobile-first, testado 375px / 768px / 1024px / 1440px
- [ ] Dark mode: funciona sem flash, cores semânticas
- [ ] Performance: sem layout shift (CLS), imagens otimizadas, lazy load
- [ ] Internacionalização ready: strings externalizadas (v1.1+)
- [ ] Testes: unit (component), visual (Storybook/Chromatic), e2e (fluxo)

---

## Próximos Passos
- [[03 - Guias Práticos/Guia de Testes]] — Estratégia, Vitest, Playwright, axe
- [[03 - Guias Práticos/Guia de Segurança]] — OWASP, LGPD, secrets
- [[03 - Guias Práticos/Guia de Publicação]] — Deploy, monitoramento, runbook
- [[02 - Modelos/UX e UI/Design System - Tokens]] — Tokens completos
- [[02 - Modelos/UX e UI/Especificação de Componentes]] — Catálogo completo
- [[02 - Modelos/UX e UI/Acessibilidade e Checklist UI]] — Checklist detalhado
- [[04 - Referência/Glossário/Design Tokens]]
- [[04 - Referência/Glossário/Atomic Design]]
- [[04 - Referência/Glossário/WCAG]]
- [[04 - Referência/Glossário/PWA]]