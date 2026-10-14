# Acessibilidade e Checklist UI

> **Copie para:** `05 - Projetos/SeuProjeto/04 - UX e UI/Acessibilidade e Checklist UI.md`
> Baseado na ETAPA 08. **Requisitos de acessibilidade (WCAG 2.1 AA) + Checklist de qualidade UI** para validação contínua.

---

## Compromisso de Acessibilidade

**Meta:** WCAG 2.1 Level AA conformance para todas as funcionalidades core do MVP.
**Escopo:** Dashboard, Detalhe Pedido, Onboarding, Relatórios, Configurações.
**Não escopo v1:** Áreas admin avançadas, relatórios complexos, configurações de integração.

> [!important] Acessibilidade não é feature - é requisito de qualidade. Testar **desde o primeiro componente**.

---

## Checklist WCAG 2.1 AA por Princípio

### 1. Perceptível (Perceivable)

#### 1.1 Alternativas Textuais
- [ ] **1.1.1** Todo conteúdo não-textual tem alternativa textual (alt, aria-label, aria-labelledby)
- [ ] Ícones decorativos: `aria-hidden="true"` + `role="img"` se SVG inline
- [ ] Ícones funcionais: `aria-label="Ação"` ou `aria-labelledby="id-do-texto-visivel"`
- [ ] Imagens de produto: `alt="Nome do produto"` (não "imagem de produto")
- [ ] Gráficos relatórios: Tabela de dados alternativa + `aria-hidden="true"` no gráfico

#### 1.2 Mídia Baseada em Tempo
- [ ] **1.2.1** Legendas em vídeos (não aplicável v1 - sem vídeo)
- [ ] **1.2.3** Audiodescrição ou alternativa textual (não aplicável v1)

#### 1.3 Adaptável
- [ ] **1.3.1** Estrutura semântica: `header`, `main`, `nav`, `section`, `article`, `aside`, `footer`
- [ ] **1.3.1** Headings hierárquicos: `h1` → `h2` → `h3` (nunca pular níveis)
- [ ] **1.3.1** Lists: `<ul>`/`<ol>` para listas, `<dl>` para definições
- [ ] **1.3.2** Ordem de leitura lógica (DOM order = visual order)
- [ ] **1.3.3** Instruções não dependem apenas de cor, forma, posição, som
- [ ] **1.3.4** Orientação: Funciona portrait e landscape (mobile)
- [ ] **1.3.5** Identificação de propósito: `autocomplete` em inputs (ex: `autocomplete="email"`)

#### 1.4 Distinguível
- [ ] **1.4.1** Cor não é único meio de transmitir informação (badges têm texto + ícone + cor)
- [ ] **1.4.3** Contraste texto: **4.5:1** (normal), **3:1** (large text 18px+ ou bold 14px+)
- [ ] **1.4.3** Contraste UI components: **3:1** (borders, icons, focus indicators)
- [ ] **1.4.4** Redimensionamento texto: 200% zoom sem perda de função (sem `max-width` fixo em containers)
- [ ] **1.4.5** Imagens de texto: Evitar (usar CSS text). Se necessário, alt text.
- [ ] **1.4.10** Reflow: 320px largura sem scroll horizontal (mobile-first garante)
- [ ] **1.4.11** Contraste não-texto: 3:1 em borders, icons, focus states
- [ ] **1.4.12** Espaçamento texto: Line height 1.5, paragraph 2x, letter 0.12em, word 0.16em (não quebrar layout)
- [ ] **1.4.13** Conteúdo em hover/focus: Dismissable, hoverable, persistent (tooltips, dropdowns)

---

### 2. Operável (Operable)

#### 2.1 Teclado
- [ ] **2.1.1** Toda funcionalidade via teclado (Tab, Enter, Space, Arrows, Esc)
- [ ] **2.1.2** Sem armadilha de teclado (focus trap em modais, ESC fecha)
- [ ] **2.1.4** Atalhos de teclado: Se existirem, desligáveis/remapáveis (não aplicável v1)

#### 2.2 Tempo Suficiente
- [ ] **2.2.1** Timeouts: Usuário pode estender/desligar (sessão: refresh silencioso, aviso antes de expirar)
- [ ] **2.2.2** Pausar/parar/esconder: Carrosséis, auto-updates (SSE: não pausa, mas não é "moving content")

#### 2.3 Convulsões
- [ ] **2.3.1** Sem flashes > 3Hz (não aplicável - sem animações intensas)

#### 2.4 Navegável
- [ ] **2.4.1** Skip link: "Pular para conteúdo principal" (primeiro link focável)
- [ ] **2.4.2** Título da página descritivo (`<title>Dashboard - PedidosSimples</title>`)
- [ ] **2.4.3** Ordem de foco lógica (Tab sequence segue visual)
- [ ] **2.4.4** Link purpose: Texto do link explica destino (não "clique aqui")
- [ ] **2.4.5** Múltiplos caminhos: Nav principal + busca + breadcrumbs (onde aplicável)
- [ ] **2.4.6** Headings e labels descritivos (não genéricos)
- [ ] **2.4.7** Focus visible: **Sempre** visível (ring-2 ring-primary ring-offset-2)
- [ ] **2.4.11** Foco não ocultado: Elemento focado não coberto por sticky header/footer
- [ ] **2.4.12** Focus appearance: Área mínima 2px thick, contrast 3:1 (nosso ring atende)
- [ ] **2.4.13** Focus not obscured (enhanced): Mesmo com sticky elements

---

### 3. Compreensível (Understandable)

#### 3.1 Legível
- [ ] **3.1.1** Idioma da página: `<html lang="pt-BR">`
- [ ] **3.1.2** Idioma de partes: `lang="en"` em trechos em inglês (ex: termos técnicos)

#### 3.2 Previsível
- [ ] **3.2.1** Foco não dispara mudança de contexto (não redirect no focus)
- [ ] **3.2.2** Input não dispara mudança de contexto (não submit no change, só no submit)
- [ ] **3.2.3** Navegação consistente: Bottom nav sempre mesma ordem, header sempre mesmo padrão
- [ ] **3.2.4** Identificação consistente: Mesmo componente = mesmo label/ícone/funcionamento

#### 3.3 Assistência de Entrada
- [ ] **3.3.1** Erros identificados: Texto claro + `aria-invalid="true"` + `aria-describedby="error-id"`
- [ ] **3.3.2** Labels/instruções: Todos inputs têm `<label>` visível ou `aria-label`
- [ ] **3.3.3** Sugestão de erro: Se detectável, sugestão de correção (ex: "Email inválido - verifique @")
- [ ] **3.3.4** Prevenção de erros (legais/financeiros): Confirmação antes de ações irreversíveis (excluir, cancelar pedido)
- [ ] **3.3.5** Ajuda: Tooltip/hint em campos complexos (ex: formato telefone)
- [ ] **3.3.7** Redundância de entrada: Não pedir mesma info duas vezes (onboarding → perfil)

---

### 4. Robusto (Robust)

#### 4.1 Compatível
- [ ] **4.1.1** HTML válido: Sem tags duplicadas, atributos válidos, nesting correto
- [ ] **4.1.2** Name, Role, Value: Componentes customizados expõem role/state/value via ARIA
- [ ] **4.1.3** Status messages: `role="status"` ou `aria-live="polite"` para toasts, loading, erros

---

## Checklist Específico por Componente

### Botões
- [ ] `type="button"` padrão (evita submit acidental em forms)
- [ ] Texto descritivo (não "OK", "Clique" - usar "Salvar", "Cancelar", "Enviar")
- [ ] Loading: `aria-busy="true"` + `aria-disabled="true"` + spinner com `aria-hidden="true"`
- [ ] Disabled: `aria-disabled="true"` + `pointer-events-none` + visual muted
- [ ] Focus: ring visível, não removido

### Inputs / Formulários
- [ ] `<label htmlFor="id">` visível **sempre** (não placeholder como label)
- [ ] `id` único + `htmlFor` correspondente
- [ ] `aria-describedby` aponta para error + hint (concatenados)
- [ ] `aria-invalid="true"` quando error
- [ ] `autocomplete` apropriado (`email`, `tel`, `name`, `street-address`, `postal-code`, `cc-number`, `off`)
- [ ] `required` + `aria-required="true"` (redundante para SR antigos)
- [ ] `type` correto: `email`, `tel`, `password`, `number`, `text`, `search`
- [ ] `inputmode` para mobile: `numeric`, `decimal`, `email`, `tel`, `url`
- [ ] Erro: role="alert" ou `aria-live="assertive"` para anúncio imediato

### Modais / Dialogs
- [ ] `role="dialog"` + `aria-modal="true"` + `aria-labelledby="title-id"` + `aria-describedby="desc-id"`
- [ ] Focus trap: Tab cicla dentro do modal
- [ ] ESC fecha (configurável: `closeOnEscape=true`)
- [ ] Click backdrop fecha (configurável: `closeOnOverlayClick=true`)
- [ ] Restore focus: Elemento que abriu modal recebe foco ao fechar
- [ ] Scroll body bloqueado (`overflow: hidden`) enquanto aberto

### Tabelas / Listas de Dados
- [ ] `<table>` apenas para dados tabulares reais (não layout)
- [ ] `<thead>` + `<th scope="col">` para cabeçalhos
- [ ] `<tbody>` + `<th scope="row">` para cabeçalhos de linha
- [ ] `caption` ou `aria-label` descrevendo propósito da tabela
- [ ] Lista de pedidos: `<ul role="list">` + `<li role="listitem">` (não table)

### Navegação
- [ ] `<nav aria-label="Navegação principal">` + `<ul role="list">`
- [ ] Link ativo: `aria-current="page"` (não só visual)
- [ ] Breadcrumbs: `<nav aria-label="Breadcrumb">` + `ol` + `aria-current="location"` no último

### Status Messages / Live Regions
- [ ] Toast success/info: `role="status"` + `aria-live="polite"` + `aria-atomic="true"`
- [ ] Toast error: `role="alert"` + `aria-live="assertive"` + `aria-atomic="true"`
- [ ] Loading: `aria-busy="true"` no container + `aria-live="polite"` "Carregando..."
- [ ] SSE indicator: `aria-live="polite"` anuncia "Tempo real conectado/desconectado"
- [ ] Contadores badges: `aria-label="3 pedidos novos"` (não só número)

### Imagens / Ícones
- [ ] `alt=""` (vazio) para decorativos + `aria-hidden="true"`
- [ ] `alt="descrição funcional"` para informativos
- [ ] SVG inline: `role="img"` + `aria-labelledby="title-id"` + `<title id="title-id">`
- [ ] Ícones em botões: `aria-hidden="true"` se botão tem texto; `aria-label` se icon-only

---

## Testes de Acessibilidade (Automatizados + Manuais)

### Automatizados (CI/CD)
```bash
# axe-core no Playwright
npx playwright test --project=chromium --grep @a11y

# Lighthouse CI
npx lhci autorun

# eslint-plugin-jsx-a11y (já no ESLint)
npm run lint
```

### Manuais (Por Release)
| Teste | Ferramenta | Frequência |
|-------|------------|------------|
| Navegação só teclado | Teclado (Tab, Shift+Tab, Enter, Space, Arrows, Esc) | Cada PR |
| Leitor de tela | NVDA (Windows) / VoiceOver (Mac/iOS) | Semanal / Release |
| Zoom 200% | Ctrl/Cmd + "+" | Cada PR |
| Contraste | axe DevTools / Color Contrast Analyzer | Cada PR |
| Reduz movimento | OS setting "Reduzir movimento" | Cada PR |
| Orientação | Rotacionar dispositivo / DevTools device toolbar | Semanal |

### Checklist Manual Rápido (5 min)
- [ ] Tab por toda página: ordem lógica, focus visível, sem armadilhas
- [ ] Shift+Tab: ordem reversa funcional
- [ ] Enter/Space em botões/links: ação esperada
- [ ] ESC em modais/dropdowns: fecha e restaura foco
- [ ] Setas em rádios/tabs/selects: navegação interna
- [ ] Zoom 200%: layout não quebra, texto legível, sem scroll horizontal
- [ ] Cores: informação não só por cor (badges têm texto + ícone)
- [ ] Leitor de tela (NVDA/VoiceOver): anuncia headings, labels, estados, erros

---

## Ferramentas Recomendadas

| Categoria | Ferramenta | Uso |
|-----------|------------|-----|
| Lint | `eslint-plugin-jsx-a11y` | Catch em dev |
| Unit/Integration | `@testing-library/react` + `jest-axe` | Testes componentes |
| E2E | `playwright` + `@axe-core/playwright` | Testes fluxos completos |
| CI | `lhci` (Lighthouse CI) | Budget performance + a11y |
| Browser | `axe DevTools` extension | Debug manual |
| Screen Reader | NVDA (grátis, Windows) / VoiceOver (Mac/iOS) | Teste real |
| Contraste | Color Contrast Analyzer (TPGi) | Verificação cores |
| Keyboard | Tab, Shift+Tab, Arrows, Esc, Enter, Space | Teste manual |

---

## Padrões de Implementação (Code Snippets)

### Skip Link (Primeiro elemento no body)
```tsx
// components/ui/skip-link.tsx
export function SkipLink() {
  return (
    <a 
      href="#main-content" 
      className="sr-only focus:not-sr-only focus:absolute focus:z-[100] focus:top-4 focus:left-4 
                 bg-primary text-primary-foreground px-4 py-2 rounded-md 
                 transition-colors"
    >
      Pular para conteúdo principal
    </a>
  );
}

// Em layout.tsx, primeiro filho do <body>
<body>
  <SkipLink />
  <header>...</header>
  <main id="main-content">...</main>
</body>
```

### Focus Visível Global (CSS)
```css
/* globals.css */
:focus-visible {
  outline: none;
  ring: 2px solid var(--color-brand-500);
  ring-offset: 2px;
  ring-offset-background: var(--bg-primary);
}

/* Remover outline padrão apenas se focus-visible suportado */
:focus:not(:focus-visible) {
  outline: none;
  ring: none;
}

/* Exceção: elementos que precisam de focus sempre visível (ex: skip link) */
.skip-link:focus {
  outline: none;
  ring: 2px solid var(--color-brand-500);
  ring-offset: 2px;
}
```

### Live Region para Toasts
```tsx
// components/molecules/toast-provider.tsx
export function ToastProvider({ children }) {
  return (
    <>
      {children}
      <div aria-live="polite" aria-atomic="true" className="sr-only" id="toast-live-region" />
      <div aria-live="assertive" aria-atomic="true" className="sr-only" id="toast-alert-region" />
      {/* Sonner/Toaster visual renderiza aqui */}
    </>
  );
}

// Uso no toast:
toast.success("Pedido salvo", { 
  id: "pedido-salvo",
  // Sonner já injeta no live region automaticamente se configurado
});
```

### Form Field Acessível
```tsx
// components/molecules/form-field.tsx
export function FormField({ label, name, error, hint, required, children, render }) {
  const errorId = error ? `${name}-error` : undefined;
  const hintId = hint && !error ? `${name}-hint` : undefined;
  const describedBy = [errorId, hintId].filter(Boolean).join(' ') || undefined;
  
  const renderedInput = render?.({ 
    id: name, 
    errorId, 
    hintId, 
    describedBy 
  }) ?? React.cloneElement(children as React.ReactElement, { 
    id: name, 
    'aria-describedby': describedBy,
    'aria-invalid': !!error,
    'aria-required': required,
  });

  return (
    <div className="space-y-1.5">
      <Label htmlFor={name} className="text-sm font-medium">
        {label} 
        {required && <span className="text-destructive" aria-hidden="true">*</span>}
      </Label>
      {renderedInput}
      {error && (
        <p id={errorId} className="text-sm text-destructive" role="alert">
          {error}
        </p>
      )}
      {hint && !error && (
        <p id={hintId} className="text-sm text-muted-foreground">
          {hint}
        </p>
      )}
    </div>
  );
}
```

---

## Métricas de Acessibilidade (KPIs)

| Métrica | Target | Ferramenta |
|---------|--------|------------|
| **axe violations (critical/serious)** | 0 | Playwright + axe |
| **Lighthouse Accessibility Score** | ≥ 95 | Lighthouse CI |
| **Keyboard navigation coverage** | 100% core flows | Manual checklist |
| **Screen reader announcements** | 100% status changes | Manual NVDA/VO |
| **Contrast ratio failures** | 0 | axe / CCA |
| **Focus visible coverage** | 100% interactive | Manual + axe |

---

## Documentação de Exceções (Se Houver)

| Componente | Critério WCAG | Exceção | Justificativa | Mitigação | Revisão |
|------------|---------------|---------|---------------|-----------|---------|
| *Ex: Gráfico relatório* | 1.1.1 | Sem alt text detalhado | Complexidade alta v1 | Tabela dados alternativa acessível | v1.1 |

> **Regra:** Toda exceção deve ser documentada, justificada, mitigação definida e data de revisão.

---

## Checklist de Validação por Release

### Pré-Commit (Automatizado)
- [ ] `npm run lint` passa (inclui jsx-a11y)
- [ ] `npm run typecheck` passa
- [ ] `npm run test` passa (inclui jest-axe nos componentes)

### PR Review (Humano)
- [ ] Novos componentes têm stories Storybook com estados a11y
- [ ] Mudanças em formulários: labels, errors, hints verificados
- [ ] Novas cores: contraste verificado (4.5:1 texto, 3:1 UI)
- [ ] Novos modais/dropdowns: focus trap, ESC, restore focus

### Release (Manual - 15 min)
- [ ] Navegação só teclado: Login → Onboarding → Dashboard → Detalhe → Config → Logout
- [ ] NVDA/VoiceOver: Fluxo core (pedido chega → prepara → pronto → entrega)
- [ ] Zoom 200%: Dashboard, Detalhe, Relatórios, Config
- [ ] Contraste: Todas cores status + feedback
- [ ] Reduced motion: Animações respeitadas

---

## Próxima Etapa
[[02 - Modelos/Desenvolvimento/Plano de Implementação]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 08 - Estruturar a Interface (UI)]]
- [[02 - Modelos/UX e UI/Jornada do Usuário]]
- [[02 - Modelos/UX e UI/Wireframes e Fluxos]]
- [[02 - Modelos/UX e UI/Design System - Tokens]]
- [[02 - Modelos/UX e UI/Especificação de Componentes]]
- [[04 - Referência/Glossário/WCAG]]
- [[04 - Referência/Glossário/ARIA]]
- [[04 - Referência/Glossário/Screen Reader]]
- [[04 - Referência/Glossário/Focus Management]]