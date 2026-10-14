# Especificação de Componentes

> **Copie para:** `05 - Projetos/SeuProjeto/04 - UX e UI/Especificação de Componentes.md`
> Baseado na ETAPA 08. **Catálogo de componentes atômicos/moleculares/organismos** com props, estados, acessibilidade e exemplos de uso.

---

## Arquitetura de Componentes (Atomic Design)

```
src/components/
├── ui/                    # Átomos - Primitivos reutilizáveis
│   ├── button.tsx
│   ├── input.tsx
│   ├── textarea.tsx
│   ├── select.tsx
│   ├── checkbox.tsx
│   ├── radio-group.tsx
│   ├── switch.tsx
│   ├── label.tsx
│   ├── badge.tsx
│   ├── avatar.tsx
│   ├── separator.tsx
│   ├── skeleton.tsx
│   ├── spinner.tsx
│   ├── tooltip.tsx
│   └── scroll-area.tsx
├── molecules/             # Moléculas - Combinações de átomos
│   ├── form-field.tsx     # Label + Input + Error + Hint
│   ├── search-input.tsx   # Input + Icon + Clear
│   ├── filter-chips.tsx   # Chip group com seleção múltipla
│   ├── status-badge.tsx   # Badge semântico por status
│   ├── pagination.tsx     # Prev/Next + Page numbers
│   ├── empty-state.tsx    # Icon + Title + Description + Action
│   ├── loading-card.tsx   # Skeleton do card pedido
│   ├── toast.tsx          # Notificação temporária
│   └── modal.tsx          # Dialog acessível (Radix)
├── organisms/             # Organismos - Seções complexas
│   ├── header.tsx         # Logo + User menu + SSE indicator
│   ├── bottom-nav.tsx     # Navegação principal PWA
│   ├── pedido-card.tsx    # Card principal da lista
│   ├── pedido-detail.tsx  # Detalhe completo + actions
│   ├── pedido-timeline.tsx # Timeline de status
│   ├── reply-modal.tsx    # Modal templates + custom
│   ├── report-cards.tsx   # Grid métricas relatório
│   ├── report-chart.tsx   # Barra horizontal simples
│   ├── product-card.tsx   # Card config produtos
│   ├── schedule-grid.tsx  # Grid 7 dias horários
│   └── onboarding-step.tsx # Stepper onboarding
└── templates/             # Templates - Layouts de página
    ├── dashboard-layout.tsx
    ├── auth-layout.tsx
    ├── settings-layout.tsx
    └── report-layout.tsx
```

---

## Átomos (Primitivos)

### Button
```typescript
// components/ui/button.tsx
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'default' | 'destructive' | 'outline' | 'secondary' | 'ghost' | 'link';
  size?: 'default' | 'sm' | 'lg' | 'xl' | 'icon';
  loading?: boolean;
  iconLeft?: React.ReactNode;
  iconRight?: React.ReactNode;
  fullWidth?: boolean;
}

// Estados visuais (via cva + tokens)
/*
Default:      bg-primary text-primary-foreground hover:bg-primary/90
Destructive:  bg-destructive text-destructive-foreground hover:bg-destructive/90
Outline:      border border-input bg-background hover:bg-accent
Secondary:    bg-secondary text-secondary-foreground hover:bg-secondary/80
Ghost:        hover:bg-accent hover:text-accent-foreground
Link:         text-primary underline-offset-4 hover:underline

Sizes:
sm:  h-9 px-3 text-xs
default: h-10 px-4 py-2 text-sm
lg:  h-11 px-8 text-base
xl:  h-12 px-10 text-lg  (mobile touch-friendly)
icon: h-10 w-10
*/

// Acessibilidade
// - type="button" padrão (evita submit acidental)
// - disabled: aria-disabled, pointer-events-none, opacity-50
// - loading: aria-busy="true", spinner, mantém largura
// - focus-visible: ring-2 ring-primary ring-offset-2
```

### Input / Textarea / Select
```typescript
// components/ui/input.tsx
interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  error?: string;
  hint?: string;
  iconLeft?: React.ReactNode;
  iconRight?: React.ReactNode;
  clearable?: boolean;
}

// Estados:
// Default: border-input bg-background
// Focus: border-primary ring-2 ring-primary/20
// Error: border-destructive + aria-invalid="true" + error message (id linkado via aria-describedby)
// Disabled: opacity-50 cursor-not-allowed
// Com valor + clearable: mostra X limpar (exceto type=password)

// Textarea: mesmas props, add rows, auto-resize opcional
// Select: usa Radix SelectPrimitive, mesmo pattern de estados
```

### Badge
```typescript
// components/ui/badge.tsx
interface BadgeProps extends React.HTMLAttributes<HTMLSpanElement> {
  variant?: 'default' | 'secondary' | 'destructive' | 'outline' | 'status';
  status?: 'novo' | 'preparando' | 'pronto' | 'entregue' | 'cancelado' | 'aguardando_abertura';
  size?: 'default' | 'sm' | 'lg';
  dot?: boolean; // Indicador redondo antes do texto
}

// Status variants usam tokens semânticos (ver Design System - Tokens)
// size sm: px-2 py-0.5 text-xs
// size default: px-2.5 py-0.5 text-xs
// size lg: px-3 py-1 text-sm
```

### Switch / Checkbox / Radio
```typescript
// Baseados em Radix Primitives + tokens
// Switch: role="switch", aria-checked, focus-visible ring
// Checkbox: indeterminate support, label clickável
// RadioGroup: roving tabindex, orientação vertical/horizontal
```

### Avatar
```typescript
// components/ui/avatar.tsx
interface AvatarProps {
  src?: string;
  alt?: string;
  fallback?: string; // Iniciais do nome
  size?: 'sm' | 'default' | 'lg' | 'xl'; // 32, 40, 48, 64px
  shape?: 'circle' | 'square';
}
```

### Tooltip
```typescript
// Radix Tooltip + tokens
// Side: top/bottom/left/right, align: start/center/end
// Delay: 200ms show, 100ms hide
// Acessibilidade: role="tooltip", id referenciado via aria-describedby
```

---

## Moléculas

### FormField (Composição Padrão)
```tsx
// components/molecules/form-field.tsx
interface FormFieldProps {
  label: string;
  name: string;           // Para hook form / htmlFor
  error?: string;
  hint?: string;
  required?: boolean;
  children: React.ReactNode; // Input, Select, Textarea, etc.
  // Render prop para flexibilidade
  render?: (props: { 
    id: string; 
    errorId?: string; 
    hintId?: string;
    describedBy: string;
  }) => React.ReactNode;
}

/*
Estrutura:
<div className="space-y-1.5">
  <Label htmlFor={id} className="text-sm font-medium">
    {label} {required && <span className="text-destructive" aria-hidden="true">*</span>}
  </Label>
  {render({ id, errorId, hintId, describedBy })}
  {error && <p id={errorId} className="text-sm text-destructive" role="alert">{error}</p>}
  {hint && !error && <p id={hintId} className="text-sm text-muted-foreground">{hint}</p>}
</div>

Acessibilidade:
- Input: aria-describedBy={describedBy} (concatena errorId + hintId)
- Error: role="alert" para anúncio imediato
- Label: htmlFor = input id
*/
```

### FilterChips (Filtros Dashboard)
```tsx
// components/molecules/filter-chips.tsx
interface FilterChipProps {
  label: string;
  value: string;
  count?: number;
  selected: boolean;
  onToggle: (value: string) => void;
  variant?: 'default' | 'status'; // status usa cores semânticas
}

interface FilterChipsProps {
  filters: FilterChipProps[];
  allowMultiple?: boolean;
  scrollable?: boolean; // Horizontal scroll no mobile
  ariaLabel: string; // "Filtrar pedidos por status"
}

/*
Visual:
- Chip selecionado: bg-primary text-primary-foreground (ou cor status)
- Chip não selecionado: bg-secondary text-secondary-foreground
- Badge count: bg-background/50 text-xs (se count > 0)
- Scroll horizontal: snap-x, hide scrollbar, touch-action: pan-x
*/
```

### StatusBadge (Específico do Domínio)
```tsx
// components/molecules/status-badge.tsx
interface StatusBadgeProps {
  status: PedidoStatus;
  size?: 'sm' | 'default' | 'lg';
  showIcon?: boolean;
  showLabel?: boolean; // Se false, só ícone + tooltip
}

/*
Mapeamento status → tokens (ver Design System - Tokens):
NOVO:           bg-status-novo-bg text-status-novo border-status-novo-border
PREPARANDO:     bg-status-preparando-bg text-status-preparando border-status-preparando-border
PRONTO:         bg-status-pronto-bg text-status-pronto border-status-pronto-border
ENTREGUE:       bg-status-entregue-bg text-status-entregue border-status-entregue-border
CANCELADO:      bg-status-cancelado-bg text-status-cancelado border-status-cancelado-border
AGUARDANDO_ABERTURA: bg-status-aguardando-bg text-status-aguardando border-status-aguardando-border

Labels: "Novo", "Preparando", "Pronto", "Entregue", "Cancelado", "Aguardando"
Ícones: Bell, Utensils, CheckCircle, Package, XCircle, Clock
*/
```

### EmptyState
```tsx
// components/molecules/empty-state.tsx
interface EmptyStateProps {
  icon: React.ReactNode;
  title: string;
  description: string;
  action?: { label: string; onClick: () => void; variant?: ButtonProps['variant'] };
  illustration?: 'none' | 'small' | 'large'; // Espaço para ilustração futura
}

/*
Layout:
<div className="flex flex-col items-center text-center py-12 px-4">
  <div className="mb-4 text-muted-foreground">{icon}</div>
  <h3 className="text-lg font-semibold text-foreground">{title}</h3>
  <p className="mt-1 text-sm text-muted-foreground max-w-xs">{description}</p>
  {action && <Button onClick={action.onClick} variant={action.variant} className="mt-4">{action.label}</Button>}
</div>
*/
```

### Toast (Notificações)
```tsx
// components/molecules/toast.tsx
// Usa Sonner ou implementação custom com Radix Toast
// Tipos: success, error, warning, info, loading, promise
// Posição: bottom-right (mobile: bottom-center full width)
// Duração: 4s (success/info), 6s (error/warning), persist (loading/promise)
// Ação: undo, dismiss, custom
// Acessibilidade: role="status" (polite), aria-live="polite"
```

### Modal (Dialog Acessível)
```tsx
// components/molecules/modal.tsx
// Base: Radix DialogPrimitive
// Props: open, onOpenChange, title, description, children, size (sm/md/lg/full)
// Focus trap automático, ESC para fechar, click backdrop para fechar (configurável)
// Restore focus ao fechar
// Portal para body (evita z-index issues)
```

---

## Organismos

### Header (Topo App)
```tsx
// components/organisms/header.tsx
interface HeaderProps {
  title: string;           // Nome da loja
  user: { name?: string; email: string; role: string };
  sseConnected: boolean;
  onMenuOpen: () => void;
  onProfileClick: () => void;
  onLogout: () => void;
}

/*
Layout mobile:
┌─────────────────────────────────────┐
│ ◀ Título da Página      👤  ⚙  ●   │  ← ● = SSE indicator (verde pulsando)
└─────────────────────────────────────┘

Menu do usuário (dropdown):
┌─────────────────────────┐
│ João Silva              │
│ joao@loja.com           │
│ ─────────────────────   │
│ 👤 Perfil               │
│ ⚙ Configurações         │
│ ─────────────────────   │
│ 🚪 Sair                 │
└─────────────────────────┘

Acessibilidade:
- Landmark: <header role="banner">
- Botão menu: aria-expanded, aria-controls, aria-haspopup="menu"
- Menu: role="menu", itens role="menuitem"
- SSE indicator: aria-live="polite" (anuncia "Tempo real conectado/desconectado")
*/
```

### BottomNav (Navegação Principal PWA)
```tsx
// components/organisms/bottom-nav.tsx
interface NavItem {
  href: string;
  label: string;
  icon: React.ReactNode;
  badge?: number;
  active?: boolean;
}

interface BottomNavProps {
  items: NavItem[];
  currentPath: string;
}

/*
Layout (fixed bottom, safe-area-inset-bottom):
┌─────────────────────────────────────┐
│ 🏠 Pedidos    📊 Relatórios   ⚙ Config │  ← 3 itens, equal width
│    ●3                          │       │  ← Badge no item ativo/inativo
└─────────────────────────────────────┘

Mobile: labels ocultos se espaço apertado (só ícones)
Tablet+: labels visíveis
PWA: safe-area-inset-bottom para notch/home indicator
*/
```

### PedidoCard (Core Component)
```tsx
// components/organisms/pedido-card.tsx
interface PedidoCardProps {
  pedido: Pedido;
  onClick: () => void;
  onStatusChange?: (status: PedidoStatus) => void; // Quick actions no card (tablet+)
  selected?: boolean;
  variant?: 'default' | 'compact'; // compact para side-by-side tablet
}

/*
Estrutura (ver Wireframes):
- Header: #ID + StatusBadge + Total
- Body: Cliente + Itens (line-clamp-2)
- Footer: Timestamp + Origem (WhatsApp)
- Border-left colorido se NOVO (border-l-4 border-status-novo)
- Hover: bg-accent/50
- Selected (tablet): ring-2 ring-primary
- Touch: touch-manipulation (remove 300ms delay)
- Acessibilidade: article, button role se onClick, aria-pressed se selected
*/
```

### PedidoDetail (Tela Completa)
```tsx
// components/organisms/pedido-detail.tsx
interface PedidoDetailProps {
  pedido: Pedido;
  onStatusChange: (status: PedidoStatus) => Promise<void>;
  onCancel: (motivo?: string) => Promise<void>;
  onReply: (message: string, template?: string) => Promise<void>;
  onClose: () => void;
}

/*
Seções (ver Wireframes):
1. Header fixo: #ID + StatusBadge grande + Cliente + Telefone
2. Itens: Lista com qtd, nome, preço, obs
3. Total: Destaque
4. Observações: Texto livre
5. Timeline: PedidoStatusHistorico (ícone + label + timestamp + usuário)
6. Actions sticky bottom: Botão principal contextual + Cancelar + Responder

Estados:
- Loading: Skeleton por seção
- Error: Toast + retry
- Optimistic UI: Atualiza status localmente, confirma via SSE
*/
```

### PedidoTimeline
```tsx
// components/organisms/pedido-timeline.tsx
interface TimelineEvent {
  status: PedidoStatus;
  label: string;
  timestamp: Date;
  user?: string;
  isCurrent: boolean;
  isCompleted: boolean;
}

/*
Visual: Linha vertical conectando pontos
- Ponto atual: Maior, pulsando (se status ativo)
- Pontos completados: Check + cor status
- Pontos futuros: Outline + muted
- Timestamp: formatTime (HH:MM)
- Usuário: "(você)" se token.user.id === event.userId
*/
```

### ReplyModal
```tsx
// components/organisms/reply-modal.tsx
interface ReplyModalProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  pedidoId: string;
  clienteTelefone: string;
  onSend: (message: string, templateName?: string) => Promise<void>;
  templates: Template[]; // { name, label, preview, variables }
}

/*
Estrutura (ver Wireframes):
- Header: "Responder cliente" + Close
- Templates: Cards clicáveis com preview
- Divider: "ou"
- Textarea: Expansível, contador chars (1000 max)
- Footer: Cancelar + Enviar (loading state)
- Acessibilidade: Dialog, focus trap, focus primeiro elemento ao abrir
*/
```

---

## Templates (Layouts de Página)

### DashboardLayout
```tsx
// components/templates/dashboard-layout.tsx
interface DashboardLayoutProps {
  children: React.ReactNode;
  headerTitle: string;
  headerActions?: React.ReactNode;
}

/*
Estrutura:
<header className="fixed top-0 left-0 right-0 z-sticky bg-background/95 backdrop-blur">
  <Header ... />
</header>
<main className="pt-16 pb-20 px-4 max-w-screen-xl mx-auto">  {/* pt-16 = header height */}
  {children}
</main>
<BottomNav ... />
<Toaster position="bottom-right" /> {/* Sonner */}

/* Mobile: pt-16, pb-20 (bottom nav) */
/* Tablet+: pt-16, pb-0 (sem bottom nav, sidebar lateral) */
```

### AuthLayout
```tsx
// components/templates/auth-layout.tsx
/*
Centralizado vertical/horizontal, max-w-sm, padding 16px
Logo + tagline no topo
Card branco com sombra para formulário
Footer: links termos/privacidade
*/
```

---

## Props Comuns (Padrão do Projeto)

```typescript
// types/component-props.ts
export interface BaseComponentProps {
  className?: string;      // Sempre aceitar className para override
  testId?: string;         // data-testid para testes E2E
  id?: string;             // ID HTML (acessibilidade)
}

export interface InteractiveProps extends BaseComponentProps {
  disabled?: boolean;
  loading?: boolean;
  onClick?: () => void;
}

export interface FormComponentProps extends BaseComponentProps {
  disabled?: boolean;
  required?: boolean;
  error?: string;
  hint?: string;
  label?: string;
  name: string;            // Obrigatório para htmlFor / hook form
}
```

---

## Convenções de Nomenclatura

| Tipo | Padrão | Exemplo |
|------|--------|---------|
| Componente | PascalCase | `PedidoCard`, `StatusBadge` |
| Props Interface | `ComponentNameProps` | `PedidoCardProps` |
| Variants (cva) | `componentVariants` | `buttonVariants`, `badgeVariants` |
| CSS Classes | kebab-case (Tailwind) | `bg-primary`, `rounded-card` |
| Tokens CSS | `--category-property` | `--color-brand-500`, `--space-4` |
| Event Handlers | `on` + Action | `onStatusChange`, `onReply` |
| Boolean Props | `is`/`has`/`should`/`disabled` | `isOpen`, `hasError`, `disabled` |

---

## Checklist por Componente

- [ ] TypeScript types estritos (sem `any`)
- [ ] Props documentadas com JSDoc
- [ ] Estados visuais: default, hover, focus, active, disabled, loading, error
- [ ] Acessibilidade: ARIA, focus management, keyboard, screen reader
- [ ] Responsivo: mobile-first, breakpoints tablet/desktop
- [ ] Dark mode: tokens semânticos funcionam
- [ ] Testes: unit (render, props), integration (interação), a11y (axe)
- [ ] Storybook: stories para cada variante/estado
- [ ] Bundle size: tree-shaking friendly, sem dependências pesadas desnecessárias

---

## Próxima Etapa
[[02 - Modelos/UX e UI/Acessibilidade e Checklist UI]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 08 - Estruturar a Interface (UI)]]
- [[02 - Modelos/UX e UI/Jornada do Usuário]]
- [[02 - Modelos/UX e UI/Wireframes e Fluxos]]
- [[02 - Modelos/UX e UI/Design System - Tokens]]
- [[02 - Modelos/UX e UI/Acessibilidade e Checklist UI]]
- [[04 - Referência/Glossário/Atomic Design]]
- [[04 - Referência/Glossário/Component Library]]
- [[04 - Referência/Glossário/Radix UI]]