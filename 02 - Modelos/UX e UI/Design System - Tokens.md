# Design System - Tokens

> **Copie para:** `05 - Projetos/SeuProjeto/04 - UX e UI/Design System - Tokens.md`
> Baseado na ETAPA 07-08. **Design tokens semânticos** (cores, espaçamento, tipografia, sombras, raios, transições) para consistência visual e manutenção.

---

## Visão Geral

Tokens são **valores brutos** (não decisões de design). Decisões de design = **como** combinar tokens.
Implementados como **CSS Custom Properties** (variáveis CSS) para:
- Theming (light/dark) nativo
- Runtime switching sem rebuild
- Acesso via JS se necessário
- Fallback automático no navegador

---

## Estrutura de Arquivos

```
src/
├── app/
│   └── globals.css          # Tokens globais + @tailwind base
├── lib/
│   └── design-tokens.ts     # TypeScript types + helpers (opcional)
└── components/
    └── ui/                  # Componentes shadcn/ui usam tokens via Tailwind
```

---

## 1. Cores (Color Tokens)

### Paleta Base (Primitive Tokens)
```css
/* src/app/globals.css - :root (light mode) */
:root {
  /* Brand - Identidade visual */
  --color-brand-50:  #fef7ee;
  --color-brand-100: #fdedd6;
  --color-brand-200: #fad9ad;
  --color-brand-300: #f6bf77;
  --color-brand-400: #f1a03d;
  --color-brand-500: #ed8914;    /* Primary brand */
  --color-brand-600: #dc6d0d;
  --color-brand-700: #b8510b;
  --color-brand-800: #93400f;
  --color-brand-900: #7a3411;
  --color-brand-950: #411706;

  /* Neutral - Cinzas */
  --color-neutral-0:   #ffffff;
  --color-neutral-50:  #fafafa;
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

  /* Semantic Status - Estados do negócio */
  --color-status-novo:           #3b82f6;  /* Blue-500 */
  --color-status-novo-bg:        #eff6ff;  /* Blue-50 */
  --color-status-novo-border:    #bfdbfe;  /* Blue-200 */
  --color-status-preparando:     #f97316;  /* Orange-500 */
  --color-status-preparando-bg:  #fff7ed;  /* Orange-50 */
  --color-status-preparando-border:#fed7aa; /* Orange-200 */
  --color-status-pronto:         #22c55e;  /* Green-500 */
  --color-status-pronto-bg:      #f0fdf4;  /* Green-50 */
  --color-status-pronto-border:  #bbf7d0;  /* Green-200 */
  --color-status-entregue:       #6b7280;  /* Gray-500 */
  --color-status-entregue-bg:    #f9fafb;  /* Gray-50 */
  --color-status-entregue-border:#e5e7eb;  /* Gray-200 */
  --color-status-cancelado:      #ef4444;  /* Red-500 */
  --color-status-cancelado-bg:   #fef2f2;  /* Red-50 */
  --color-status-cancelado-border:#fecaca;  /* Red-200 */
  --color-status-aguardando:     #eab308;  /* Yellow-500 */
  --color-status-aguardando-bg:  #fefce8;  /* Yellow-50 */
  --color-status-aguardando-border:#fef08a; /* Yellow-200 */

  /* Semantic Feedback - Sistema */
  --color-success:   #22c55e;  /* Green-500 */
  --color-success-bg: #f0fdf4;
  --color-warning:   #eab308;  /* Yellow-500 */
  --color-warning-bg: #fefce8;
  --color-error:     #ef4444;  /* Red-500 */
  --color-error-bg:  #fef2f2;
  --color-info:      #3b82f6;  /* Blue-500 */
  --color-info-bg:   #eff6ff;
}
```

### Dark Mode (Override)
```css
/* src/app/globals.css - .dark */
.dark {
  /* Brand - Ajustado para contraste dark */
  --color-brand-50:  #411706;
  --color-brand-100: #7a3411;
  --color-brand-200: #93400f;
  --color-brand-300: #b8510b;
  --color-brand-400: #dc6d0d;
  --color-brand-500: #ed8914;    /* Mantém primary */
  --color-brand-600: #f1a03d;
  --color-brand-700: #f6bf77;
  --color-brand-800: #fad9ad;
  --color-brand-900: #fdedd6;
  --color-brand-950: #fef7ee;

  /* Neutral - Invertido */
  --color-neutral-0:   #0a0a0a;
  --color-neutral-50:  #171717;
  --color-neutral-100: #262626;
  --color-neutral-200: #404040;
  --color-neutral-300: #525252;
  --color-neutral-400: #737373;
  --color-neutral-500: #a3a3a3;
  --color-neutral-600: #d4d4d4;
  --color-neutral-700: #e5e5e5;
  --color-neutral-800: #f5f5f5;
  --color-neutral-900: #fafafa;
  --color-neutral-950: #ffffff;

  /* Status - Ajustados para dark (mais vibrantes) */
  --color-status-novo:           #60a5fa;  /* Blue-400 */
  --color-status-novo-bg:        #1e3a5f;  /* Blue-900/800 */
  --color-status-novo-border:    #1e40af;  /* Blue-800 */
  --color-status-preparando:     #fb923c;  /* Orange-400 */
  --color-status-preparando-bg:  #7c2d12;  /* Orange-900 */
  --color-status-preparando-border:#9a3412; /* Orange-800 */
  --color-status-pronto:         #4ade80;  /* Green-400 */
  --color-status-pronto-bg:      #14532d;  /* Green-900 */
  --color-status-pronto-border:  #166534;  /* Green-800 */
  --color-status-entregue:       #9ca3af;  /* Gray-400 */
  --color-status-entregue-bg:    #374151;  /* Gray-700 */
  --color-status-entregue-border:#4b5563;  /* Gray-600 */
  --color-status-cancelado:      #f87171;  /* Red-400 */
  --color-status-cancelado-bg:   #7f1d1d;  /* Red-900 */
  --color-status-cancelado-border:#991b1b;  /* Red-800 */
  --color-status-aguardando:     #facc15;  /* Yellow-400 */
  --color-status-aguardando-bg:  #713f12;  /* Yellow-900 */
  --color-status-aguardando-border:#854d0e; /* Yellow-800 */

  /* Feedback */
  --color-success:   #4ade80;
  --color-success-bg: #14532d;
  --color-warning:   #facc15;
  --color-warning-bg: #713f12;
  --color-error:     #f87171;
  --color-error-bg:  #7f1d1d;
  --color-info:      #60a5fa;
  --color-info-bg:   #1e3a5f;
}
```

### Tokens Semânticos (Alias - O que componentes usam)
```css
:root {
  /* Backgrounds */
  --bg-primary:        var(--color-neutral-0);
  --bg-secondary:      var(--color-neutral-50);
  --bg-tertiary:       var(--color-neutral-100);
  --bg-inverse:        var(--color-neutral-900);
  --bg-brand:          var(--color-brand-500);
  --bg-brand-hover:    var(--color-brand-600);
  --bg-brand-subtle:   var(--color-brand-50);

  /* Text */
  --text-primary:      var(--color-neutral-900);
  --text-secondary:    var(--color-neutral-600);
  --text-tertiary:     var(--color-neutral-400);
  --text-inverse:      var(--color-neutral-0);
  --text-brand:        var(--color-brand-700);
  --text-link:         var(--color-brand-600);
  --text-link-hover:   var(--color-brand-700);

  /* Border */
  --border-primary:    var(--color-neutral-200);
  --border-secondary:  var(--color-neutral-300);
  --border-focus:      var(--color-brand-500);
  --border-error:      var(--color-error);

  /* Component Specific */
  --card-bg:           var(--color-neutral-0);
  --card-border:       var(--color-neutral-200);
  --card-shadow:       0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  
  --input-bg:          var(--color-neutral-0);
  --input-border:      var(--color-neutral-300);
  --input-border-focus: var(--color-brand-500);
  --input-placeholder: var(--color-neutral-400);
  
  --button-primary-bg: var(--color-brand-500);
  --button-primary-text: var(--color-neutral-0);
  --button-primary-hover: var(--color-brand-600);
  --button-secondary-bg: var(--color-neutral-100);
  --button-secondary-text: var(--color-neutral-900);
  --button-secondary-hover: var(--color-neutral-200);
  --button-destructive-bg: var(--color-error);
  --button-destructive-text: var(--color-neutral-0);
  --button-ghost-bg: transparent;
  --button-ghost-hover: var(--color-neutral-100);
  
  --badge-novo-bg: var(--color-status-novo-bg);
  --badge-novo-text: var(--color-status-novo);
  --badge-novo-border: var(--color-status-novo-border);
  --badge-preparando-bg: var(--color-status-preparando-bg);
  --badge-preparando-text: var(--color-status-preparando);
  --badge-preparando-border: var(--color-status-preparando-border);
  --badge-pronto-bg: var(--color-status-pronto-bg);
  --badge-pronto-text: var(--color-status-pronto);
  --badge-pronto-border: var(--color-status-pronto-border);
  --badge-entregue-bg: var(--color-status-entregue-bg);
  --badge-entregue-text: var(--color-status-entregue);
  --badge-entregue-border: var(--color-status-entregue-border);
  --badge-cancelado-bg: var(--color-status-cancelado-bg);
  --badge-cancelado-text: var(--color-status-cancelado);
  --badge-cancelado-border: var(--color-status-cancelado-border);
  --badge-aguardando-bg: var(--color-status-aguardando-bg);
  --badge-aguardando-text: var(--color-status-aguardando);
  --badge-aguardando-border: var(--color-status-aguardando-border);

  /* SSE Indicator */
  --sse-live-color: var(--color-success);
  --sse-live-bg: var(--color-success-bg);
}
```

---

## 2. Espaçamento (Spacing Tokens)

```css
:root {
  /* Base unit = 4px */
  --space-0:   0;
  --space-1:   0.25rem;  /* 4px */
  --space-2:   0.5rem;   /* 8px */
  --space-3:   0.75rem;  /* 12px */
  --space-4:   1rem;     /* 16px */
  --space-5:   1.25rem;  /* 20px */
  --space-6:   1.5rem;   /* 24px */
  --space-8:   2rem;     /* 32px */
  --space-10:  2.5rem;   /* 40px */
  --space-12:  3rem;     /* 48px */
  --space-16:  4rem;     /* 64px */
  --space-20:  5rem;     /* 80px */
  --space-24:  6rem;     /* 96px */

  /* Semantic Spacing */
  --gap-inline:     var(--space-4);   /* 16px - gap horizontal padrão */
  --gap-stack:      var(--space-4);   /* 16px - gap vertical padrão */
  --gap-stack-sm:   var(--space-2);   /* 8px - gap vertical compacto */
  --gap-stack-lg:   var(--space-6);   /* 24px - gap vertical amplo */
  --padding-page:   var(--space-4);   /* 16px - padding páginas mobile */
  --padding-page-lg: var(--space-6);  /* 24px - padding páginas desktop */
  --padding-card:   var(--space-4);   /* 16px - padding interno cards */
  --padding-button: var(--space-2) var(--space-4); /* 8px 16px */
  --padding-input:  var(--space-3) var(--space-4); /* 12px 16px */
}
```

---

## 3. Tipografia (Typography Tokens)

```css
:root {
  /* Font Families */
  --font-sans:    'Inter', system-ui, -apple-system, sans-serif;
  --font-mono:    'JetBrains Mono', 'Fira Code', monospace;
  --font-display: 'Inter', system-ui, -apple-system, sans-serif; /* Same, different weight */

  /* Font Sizes (clamp para fluido) */
  --text-xs:   0.75rem;    /* 12px */
  --text-sm:   0.875rem;   /* 14px */
  --text-base: 1rem;       /* 16px */
  --text-lg:   1.125rem;   /* 18px */
  --text-xl:   1.25rem;    /* 20px */
  --text-2xl:  1.5rem;     /* 24px */
  --text-3xl:  1.875rem;   /* 30px */
  --text-4xl:  2.25rem;    /* 36px */

  /* Fluid Type Scale (mobile → desktop) */
  --text-display: clamp(1.875rem, 1.5rem + 2vw, 3rem);      /* 30px → 48px */
  --text-h1:      clamp(1.5rem, 1.25rem + 1.5vw, 2.25rem);  /* 24px → 36px */
  --text-h2:      clamp(1.25rem, 1.125rem + 1vw, 1.875rem); /* 20px → 30px */
  --text-h3:      clamp(1.125rem, 1rem + 0.5vw, 1.5rem);    /* 18px → 24px */
  --text-body:    var(--text-base);                           /* 16px */
  --text-body-sm: var(--text-sm);                             /* 14px */
  --text-caption: var(--text-xs);                             /* 12px */

  /* Font Weights */
  --font-normal:  400;
  --font-medium:  500;
  --font-semibold: 600;
  --font-bold:    700;

  /* Line Heights */
  --leading-none:    1;
  --leading-tight:   1.25;
  --leading-snug:    1.375;
  --leading-normal:  1.5;
  --leading-relaxed: 1.625;
  --leading-loose:   2;

  /* Letter Spacing */
  --tracking-tight:  -0.02em;
  --tracking-normal: 0;
  --tracking-wide:   0.02em;

  /* Semantic Typography */
  --font-heading: var(--font-display);
  --font-body:    var(--font-sans);
  --font-code:    var(--font-mono);
}
```

---

## 4. Sombras (Shadow Tokens)

```css
:root {
  /* Elevation Levels */
  --shadow-xs:   0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-sm:   0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --shadow-md:   0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg:   0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl:   0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
  --shadow-2xl:  0 25px 50px -12px rgb(0 0 0 / 0.25);

  /* Semantic Shadows */
  --shadow-card:       var(--shadow-sm);
  --shadow-card-hover: var(--shadow-md);
  --shadow-dropdown:   var(--shadow-lg);
  --shadow-modal:      var(--shadow-xl);
  --shadow-toast:      var(--shadow-lg);
  --shadow-focus:      0 0 0 2px var(--color-brand-500);
  
  /* Colored Shadows (brand) */
  --shadow-brand-sm: 0 1px 3px 0 var(--color-brand-500 / 0.3);
  --shadow-brand-md: 0 4px 6px -1px var(--color-brand-500 / 0.3);
}
```

---

## 5. Raios de Borda (Border Radius Tokens)

```css
:root {
  --radius-none:  0;
  --radius-sm:    0.25rem;  /* 4px */
  --radius-md:    0.375rem; /* 6px */
  --radius-lg:    0.5rem;   /* 8px */
  --radius-xl:    0.75rem;  /* 12px */
  --radius-2xl:   1rem;     /* 16px */
  --radius-full:  9999px;

  /* Semantic */
  --radius-button:  var(--radius-md);   /* 6px */
  --radius-input:   var(--radius-md);   /* 6px */
  --radius-card:    var(--radius-lg);   /* 8px */
  --radius-badge:   var(--radius-full); /* Pill */
  --radius-modal:   var(--radius-xl);   /* 12px */
  --radius-tooltip: var(--radius-md);   /* 6px */
}
```

---

## 6. Transições e Animação (Motion Tokens)

```css
:root {
  /* Durations */
  --duration-instant: 0ms;
  --duration-fast:    100ms;
  --duration-normal:  200ms;
  --duration-slow:    300ms;
  --duration-slower:  500ms;

  /* Easings */
  --ease-linear:  linear;
  --ease-in:      cubic-bezier(0.4, 0, 1, 1);
  --ease-out:     cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out:  cubic-bezier(0.4, 0, 0.2, 1);
  --ease-spring:  cubic-bezier(0.34, 1.56, 0.64, 1); /* Bounce sutil */

  /* Semantic Transitions */
  --transition-fast:    var(--duration-fast) var(--ease-out);
  --transition-normal:  var(--duration-normal) var(--ease-in-out);
  --transition-slow:    var(--duration-slow) var(--ease-in-out);
  --transition-colors:  var(--duration-fast) var(--ease-in-out);
  --transition-transform: var(--duration-normal) var(--ease-spring);
  --transition-shadow:  var(--duration-normal) var(--ease-out);
  --transition-all:     var(--duration-normal) var(--ease-in-out);

  /* Reduced Motion (respecta prefers-reduced-motion) */
  --transition-reduced: 0.01ms;
}
```

```css
/* Aplicação global respeitando prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  :root {
    --transition-fast:    var(--transition-reduced);
    --transition-normal:  var(--transition-reduced);
    --transition-slow:    var(--transition-reduced);
    --transition-colors:  var(--transition-reduced);
    --transition-transform: var(--transition-reduced);
    --transition-shadow:  var(--transition-reduced);
    --transition-all:     var(--transition-reduced);
  }
  
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 7. Z-Index (Layering Tokens)

```css
:root {
  --z-index-base:      0;
  --z-index-dropdown:  100;
  --z-index-sticky:    200;
  --z-index-fixed:     300;
  --z-index-modal-backdrop: 400;
  --z-index-modal:     500;
  --z-index-popover:   600;
  --z-index-tooltip:   700;
  --z-index-toast:     800;
  --z-index-max:       9999;
}
```

---

## 8. Breakpoints (Responsive Tokens)

```css
:root {
  /* Mobile First */
  --breakpoint-sm:  640px;   /* Tablet portrait */
  --breakpoint-md:  768px;   /* Tablet landscape */
  --breakpoint-lg:  1024px;  /* Desktop */
  --breakpoint-xl:  1280px;  /* Large desktop */
  --breakpoint-2xl: 1536px;  /* Ultra wide */

  /* Container Max Widths */
  --container-sm:  640px;
  --container-md:  768px;
  --container-lg:  1024px;
  --container-xl:  1280px;
  --container-2xl: 1536px;
}
```

---

## 9. Integração com Tailwind CSS

### tailwind.config.ts
```typescript
import type { Config } from 'tailwindcss';

const config: Config = {
  darkMode: 'class', // Controlado via .dark no <html>
  content: ['./src/**/*.{ts,tsx,js,jsx,mdx}'],
  theme: {
    extend: {
      colors: {
        // Mapeia tokens CSS para utilitários Tailwind
        brand: {
          50: 'var(--color-brand-50)',
          100: 'var(--color-brand-100)',
          200: 'var(--color-brand-200)',
          300: 'var(--color-brand-300)',
          400: 'var(--color-brand-400)',
          500: 'var(--color-brand-500)',
          600: 'var(--color-brand-600)',
          700: 'var(--color-brand-700)',
          800: 'var(--color-brand-800)',
          900: 'var(--color-brand-900)',
          950: 'var(--color-brand-950)',
        },
        status: {
          novo: 'var(--color-status-novo)',
          preparando: 'var(--color-status-preparando)',
          pronto: 'var(--color-status-pronto)',
          entregue: 'var(--color-status-entregue)',
          cancelado: 'var(--color-status-cancelado)',
          aguardando: 'var(--color-status-aguardando)',
        },
        // Semantic aliases
        background: 'var(--bg-primary)',
        foreground: 'var(--text-primary)',
        muted: 'var(--bg-secondary)',
        card: 'var(--card-bg)',
        border: 'var(--border-primary)',
        primary: 'var(--button-primary-bg)',
        'primary-foreground': 'var(--button-primary-text)',
        secondary: 'var(--button-secondary-bg)',
        'secondary-foreground': 'var(--button-secondary-text)',
        destructive: 'var(--button-destructive-bg)',
        'destructive-foreground': 'var(--button-destructive-text)',
      },
      spacing: {
        // Usa valores padrão do Tailwind (base 4px) - tokens CSS para valores customizados
      },
      borderRadius: {
        button: 'var(--radius-button)',
        input: 'var(--radius-input)',
        card: 'var(--radius-card)',
        badge: 'var(--radius-badge)',
        modal: 'var(--radius-modal)',
      },
      boxShadow: {
        card: 'var(--shadow-card)',
        'card-hover': 'var(--shadow-card-hover)',
        dropdown: 'var(--shadow-dropdown)',
        modal: 'var(--shadow-modal)',
        focus: 'var(--shadow-focus)',
      },
      transitionDuration: {
        fast: 'var(--duration-fast)',
        normal: 'var(--duration-normal)',
        slow: 'var(--duration-slow)',
      },
      transitionTimingFunction: {
        spring: 'var(--ease-spring)',
      },
      fontFamily: {
        sans: ['var(--font-sans)'],
        mono: ['var(--font-mono)'],
        display: ['var(--font-display)'],
      },
      fontSize: {
        display: ['var(--text-display)', { lineHeight: 'var(--leading-tight)' }],
        h1: ['var(--text-h1)', { lineHeight: 'var(--leading-tight)' }],
        h2: ['var(--text-h2)', { lineHeight: 'var(--leading-tight)' }],
        h3: ['var(--text-h3)', { lineHeight: 'var(--leading-snug)' }],
        body: ['var(--text-body)', { lineHeight: 'var(--leading-normal)' }],
        'body-sm': ['var(--text-body-sm)', { lineHeight: 'var(--leading-normal)' }],
        caption: ['var(--text-caption)', { lineHeight: 'var(--leading-normal)' }],
      },
    },
  },
  plugins: [],
};

export default config;
```

---

## 10. Uso nos Componentes (Exemplos)

### Button (shadcn/ui style)
```tsx
// components/ui/button.tsx
const buttonVariants = cva(
  'inline-flex items-center justify-center font-medium transition-colors ' +
  'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary ' +
  'disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground hover:bg-primary/90 h-10 px-4 py-2',
        destructive: 'bg-destructive text-destructive-foreground hover:bg-destructive/90 h-10 px-4 py-2',
        outline: 'border border-input bg-background hover:bg-accent hover:text-accent-foreground h-10 px-4 py-2',
        secondary: 'bg-secondary text-secondary-foreground hover:bg-secondary/80 h-10 px-4 py-2',
        ghost: 'hover:bg-accent hover:text-accent-foreground h-10 px-4 py-2',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default: 'h-10 px-4 py-2 text-sm',
        sm: 'h-9 rounded-md px-3 text-xs',
        lg: 'h-11 rounded-lg px-8 text-base',
        xl: 'h-12 rounded-lg px-10 text-lg', // Mobile touch-friendly
        icon: 'h-10 w-10',
      },
    },
    defaultVariants: { variant: 'default', size: 'default' },
  }
);
```

### Badge Status
```tsx
// components/ui/status-badge.tsx
const statusVariants = {
  novo: 'bg-[var(--badge-novo-bg)] text-[var(--badge-novo-text)] border-[var(--badge-novo-border)]',
  preparando: 'bg-[var(--badge-preparando-bg)] text-[var(--badge-preparando-text)] border-[var(--badge-preparando-border)]',
  pronto: 'bg-[var(--badge-pronto-bg)] text-[var(--badge-pronto-text)] border-[var(--badge-pronto-border)]',
  entregue: 'bg-[var(--badge-entregue-bg)] text-[var(--badge-entregue-text)] border-[var(--badge-entregue-border)]',
  cancelado: 'bg-[var(--badge-cancelado-bg)] text-[var(--badge-cancelado-text)] border-[var(--badge-cancelado-border)]',
  aguardando_abertura: 'bg-[var(--badge-aguardando-bg)] text-[var(--badge-aguardando-text)] border-[var(--badge-aguardando-border)]',
};

export function StatusBadge({ status }: { status: PedidoStatus }) {
  return (
    <span className={cn(
      'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium border',
      statusVariants[status]
    )}>
      {statusLabels[status]}
    </span>
  );
}
```

### Card Pedido
```tsx
// components/dashboard/pedido-card.tsx
export function PedidoCard({ pedido }: { pedido: Pedido }) {
  return (
    <article className={cn(
      'relative border border-border bg-card rounded-card p-4 transition-all',
      'hover:bg-accent/50',
      'touch-manipulation', // Remove 300ms tap delay
      pedido.status === 'NOVO' && 'border-l-4 border-l-status-novo'
    )}>
      <header className="flex items-start justify-between gap-2 mb-2">
        <div className="flex items-center gap-2">
          <span className="font-mono text-sm text-muted-foreground">#{pedido.id.slice(-4)}</span>
          <StatusBadge status={pedido.status} />
        </div>
        <span className="font-semibold text-foreground">{formatCurrency(pedido.total)}</span>
      </header>
      <p className="text-sm text-muted-foreground line-clamp-2 mb-2">
        {pedido.clienteNome} • {pedido.itensJson.map(i => `${i.quantidade}x ${i.nome}`).join(', ')}
      </p>
      <footer className="flex items-center justify-between text-xs text-muted-foreground">
        <time dateTime={pedido.createdAt.toISOString()}>
          {formatTime(pedido.createdAt)}
        </time>
        <span className="flex items-center gap-1">
          <MessageCircle className="w-3 h-3" />
          WhatsApp
        </span>
      </footer>
    </article>
  );
}
```

---

## 11. Checklist Design Tokens

- [ ] Cores primitivas (brand, neutral) definidas 50-950
- [ ] Cores semânticas de status (novo, preparando, pronto, entregue, cancelado, aguardando)
- [ ] Cores semânticas de feedback (success, warning, error, info)
- [ ] Aliases semânticos para bg, text, border, component-specific
- [ ] Dark mode completo com contraste WCAG AA
- [ ] Espaçamento base 4px + aliases semânticos
- [ ] Tipografia: font families, fluid scale, weights, line heights
- [ ] Sombras: elevation levels + semantic shadows
- [ ] Border radius: primitivos + semantic
- [ ] Motion: durations, easings, reduced motion support
- [ ] Z-index layers documentados
- [ ] Breakpoints mobile-first
- [ ] Tailwind config mapeia tokens para utilitários
- [ ] Componentes usam tokens (não valores hardcoded)
- [ ] Documentado como adicionar novo token

---

## Próxima Etapa
[[02 - Modelos/UX e UI/Especificação de Componentes]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 07 - Pesquisar Referências e Definir a Direção Visual]]
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 08 - Estruturar a Interface (UI)]]
- [[02 - Modelos/UX e UI/Jornada do Usuário]]
- [[02 - Modelos/UX e UI/Wireframes e Fluxos]]
- [[02 - Modelos/UX e UI/Especificação de Componentes]]
- [[02 - Modelos/UX e UI/Acessibilidade e Checklist UI]]
- [[04 - Referência/Glossário/Design Tokens]]
- [[04 - Referência/Glossário/Atomic Design]]
- [[04 - Referência/Glossário/CSS Custom Properties]]