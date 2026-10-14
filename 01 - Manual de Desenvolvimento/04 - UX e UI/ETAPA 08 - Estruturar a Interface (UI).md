# ETAPA 08 — Estruturar a Interface (UI)

## Objetivo

Transformar os fluxos (UX) e a direção visual em uma **estrutura clara de telas e componentes** pronta para implementação: inventário de telas, hierarquia, componentes reutilizáveis, design system proporcional, estados, responsividade e acessibilidade.

## Por que esta etapa existe?

Sem estrutura de UI definida, a implementação vira "vibe coding" caótico: componentes duplicados, inconsistências, retrabalho CSS, telas que não cobrem todos os estados. Esta etapa cria o "blueprint" visual.

## Quando começar

- ETAPA 07 concluída (Direção Visual aprovada, tokens definidos)
- ETAPA 06 concluída (Fluxos do Usuário aprovados)
- Você sabe O QUÊ (fluxos) e COMO VISUALMENTE (direção) — agora estrutura ONDE (telas/componentes)

## O que você precisa fazer

1. **Crie o inventário de telas** — uma entrada por tela/estado único dos fluxos
2. **Defina a estrutura de cada tela** — layout, seções, componentes, hierarquia visual
3. **Identifique componentes reutilizáveis** — atoms, molecules, organisms (design system proporcional)
4. **Crie wireframes se úteis** — baixa fidelidade, foco em layout e hierarquia
5. **Crie protótipos se úteis** — alta fidelidade, interativos, para validação
6. **Defina design system proporcional** — apenas o que o MVP precisa
7. **Documente estados de interação** — para cada componente: default, hover, focus, active, disabled, loading, error
8. **Planeje responsividade** — breakpoints, adaptações mobile/tablet/desktop
9. **Planeje acessibilidade** — ARIA, foco, contraste, semântica, testes
10. **Faça revisão visual** — consistência com direção visual, cobertura de fluxos
11. **Defina critérios de aprovação** — o que significa "UI aprovada"
12. **Produza os documentos: Inventário de Telas + Registro de Componentes + Design System (tokens + componentes base)**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Inventário de telas (baseado em fluxos) | 🤖 IA (você valida completude) |
| Estrutura de cada tela (layout, hierarquia) | 👤 USUÁRIO + 🤖 IA |
| Identificar componentes reutilizáveis | 🤖 IA (sugere) + 👤 USUÁRIO (aprova) |
| Wireframes/protótipos | 👤 USUÁRIO (decide se precisa) + 🤖 IA (pode gerar código) |
| Design system proporcional | 🤖 IA (gera tokens + componentes base) |
| Estados de interação por componente | 🤖 IA (padrão) + 👤 USUÁRIO (valida) |
| Responsividade/acessibilidade | 🤖 IA (checklist) + 👤 USUÁRIO (aprova) |
| Revisão visual final | ✅ VALIDAÇÃO (você) |
| Aprovar UI para implementação | 👤 USUÁRIO |

## Ferramentas recomendadas

| Finalidade | Ferramentas |
|------------|-------------|
| Inventário/estrutura | Obsidian (Markdown + tabelas), Notion |
| Wireframes | Excalidraw (no Obsidian), Figma, papel |
| Protótipos | Figma, Framer, ou código direto (Storybook) |
| Design system tokens | Tailwind config, Figma tokens, Style Dictionary |
| Componentes base | shadcn/ui (copiar e adaptar), Radix UI, Headless UI |
| Checklist acessibilidade | axe-core, WAVE, Stark, lista WCAG 2.1 AA |

> [!tip]
> **Não precisa de Figma para projetos pequenos/médios.** Wireframes no Excalidraw (dentro do Obsidian) + código direto com Storybook funcionam bem. Figma vale a pena se: equipe >1 designer, handoff complexo, protótipo testado com usuários.

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Inventário de Telas | `02 - Modelos/UX e UI/Inventário de Telas.md` | `05 - Projetos/SeuProjeto/04 - UX-UI/` |
| Registro de Componentes e Padrões | `02 - Modelos/UX e UI/Registro de Componentes e Padrões.md` | `05 - Projetos/SeuProjeto/04 - UX-UI/` |

### Condicionais
- Wireframes (se fluxos complexos ou validação com usuários)
- Protótipo Interativo (se validação com usuários antes de codificar)
- Especificação de Responsividade (se breakpoints complexos)
- Checklist de Acessibilidade (se público externo/produção)

### Opcionais
- Guia de Motion/Animações (se motion importante)
- Especificação de Tema Escuro (se implementar no MVP)

## Como produzir os documentos

### Inventário de Telas

```markdown
# Inventário de Telas — PedidosSimples v1

## Visão Geral
| Total de Telas Únicas | 12 |
| Telas Principais (fluxo core) | 5 |
| Telas Secundárias (config, auth) | 4 |
| Modais/Bottom Sheets | 3 |
| Estados de Erro/Vazio (dedicados) | 2 |

## Lista Detalhada

| ID | Tela | Fluxo Relacionado | Tipo | Prioridade | Estados Necessários |
|----|------|-------------------|------|------------|---------------------|
| SC-01 | Login / Entrada | Auth | Página | Must | Default, Carregando, Erro, Vazio (sem conta) |
| SC-02 | Dashboard (Lista Pedidos) | Ver/Gerenciar Pedidos | Página | Must | Default, Vazio, Carregando, Erro, Com Filtro |
| SC-03 | Detalhes do Pedido | Ver/Gerenciar Pedidos | Modal/Página | Must | Default, Carregando, Erro |
| SC-04 | Responder Cliente (Modal) | Responder Cliente | Modal | Should | Default, Carregando, Erro, Sucesso |
| SC-05 | Histórico de Pedidos | Ver Histórico | Página | Should | Default, Vazio, Carregando, Erro, Filtrado |
| SC-06 | Configurações - Perfil | Configurações | Página | Could | Default, Carregando, Erro, Sucesso |
| SC-07 | Configurações - Estabelecimento | Configurações | Página | Could | Default, Carregando, Erro, Sucesso |
| SC-08 | Configurações - Horários | Configurações | Página | Could | Default, Carregando, Erro, Sucesso |
| SC-09 | Configurações - Produtos | Configurações | Página | Could | Default, Vazio, Carregando, Erro, Edição |
| SC-10 | Configurações - Integrações | Configurações | Página | Could | Default, Carregando, Erro, Conectado |
| SC-11 | Onboarding (Primeira vez) | Auth | Página/Modal | Must | Passo 1, 2, 3, Concluído, Erro |
| SC-12 | Erro Global / Offline | Transversal | Página/Toast | Must | Offline, Erro Crítico, Manutenção |

## Mapeamento Fluxo → Tela

| Fluxo (ETAPA 06) | Telas Envolvidas |
|------------------|------------------|
| Ver e Gerenciar Pedidos | SC-02, SC-03, SC-04 |
| Responder Cliente | SC-04 (modal a partir de SC-02/SC-03) |
| Ver Histórico | SC-05 |
| Configurar Estabelecimento | SC-07, SC-08, SC-09 |
| Configurar Integração WhatsApp | SC-10 |
| Primeiro Acesso | SC-01, SC-11 |
```

### Estrutura de Tela (exemplo para SC-02 Dashboard)

```markdown
## SC-02: Dashboard (Lista de Pedidos)

### Layout Geral (Mobile-First)
```
┌─────────────────────────────┐
│ Header                      │  ← Fixed top: Logo + Status conexão + Avatar
├─────────────────────────────┤
│                             │
│  Filtro de Status (Tabs)    │  ← Scroll horizontal: Todos | Novos | Preparando | Prontos
│  [Todos] [Novos] [Prep]     │
├─────────────────────────────┤
│                             │
│  Lista de Pedidos (Cards)   │  ← Scroll vertical, infinite scroll
│  ┌─────────────────────┐    │
│  │ 👤 Maria Silva      │    │  ← Card: Avatar/Nome | Status Badge | Resumo Itens | Hora | Ações
│  │ 🟢 NOVO             │    │
│  │ 2x Burger, 1x Batata│    │
│  │ 14:32  [Preparando] │    │
│  └─────────────────────┘    │
│  ┌─────────────────────┐    │
│  │ 👤 João Santos      │    │
│  │ 🟡 PREPARANDO       │    │
│  │ 1x Pizza            │    │
│  │ 14:28  [Pronto]     │    │
│  └─────────────────────┘    │
│                             │
├─────────────────────────────┤
│  FAB: [+ Novo Pedido]       │  ← Fixed bottom right
└─────────────────────────────┘
```

### Hierarquia Visual
1. **Primária:** Lista de pedidos (conteúdo principal)
2. **Secundária:** Filtro de status (navegação de contexto)
3. **Terciária:** Header (identidade + status sistema), FAB (ação secundária)

### Componentes Utilizados
- `Header` (organism)
- `StatusTabs` (molecule)
- `OrderCard` (organism) → usa `Avatar`, `StatusBadge`, `ActionButtons` (molecules/atoms)
- `FAB` (atom)
- `EmptyState` (organism) — para estado vazio
- `SkeletonOrderCard` (molecule) — para carregando
- `ErrorToast` / `ErrorBanner` (molecule) — para erro

### Responsividade
| Breakpoint | Adaptação |
|------------|-----------|
| < 640px (mobile) | Layout acima (single column, tabs scroll horizontal, FAB) |
| 640px - 1024px (tablet) | Tabs visíveis sem scroll, FAB → botão no header, 2 colunas se landscape |
| > 1024px (desktop) | Sidebar esquerda (filtros + navegação), área principal lista, header fixo |

### Acessibilidade
- Ordem de foco lógica: Header → Tabs → Lista → FAB
- `aria-label` nos tabs: "Filtrar pedidos por status"
- `aria-live="polite"` na lista para anúncio de novos pedidos
- Contraste verificado em todos os estados
- Navegação por teclado: Tab entre tabs, Enter/Space para selecionar, Setas na lista

### Estados da Tela
| Estado | Descrição | Componentes Específicos |
|--------|-----------|------------------------|
| Default | Lista com pedidos | OrderCard(s) |
| Vazio | "Nenhum pedido hoje" + ilustração + CTA "Ver histórico" | EmptyState |
| Carregando | 3-5 SkeletonOrderCard animados | SkeletonOrderCard |
| Erro | Banner topo "Falha ao carregar" + botão "Tentar novamente" | ErrorBanner |
| Filtrado | Badge "Filtrado: Novos" + botão "Limpar filtro" | StatusTabs (estado ativo) |
```

### Registro de Componentes e Padrões

```markdown
# Registro de Componentes — PedidosSimples v1

## Princípios do Design System Proporcional
- **Apenas o que o MVP usa** — nada de componentes "para o futuro"
- **Base: shadcn/ui + Tailwind** — copiar, adaptar tokens, estender
- **Tokens = Direção Visual (ETAPA 07)** — fonte única da verdade
- **Documentar: Props, Estados, Slots, Acessibilidade, Exemplos de uso**

---

## Atoms (Básicos)

### Button
**Arquivo:** `components/ui/button.tsx`
**Props:** `variant: 'primary' | 'secondary' | 'ghost' | 'destructive' | 'outline'`, `size: 'sm' | 'md' | 'lg'`, `loading?: boolean`, `disabled?: boolean`, `asChild?: boolean`
**Estados:** Default, Hover, Focus, Active, Disabled, Loading
**Acessibilidade:** `type="button"` padrão, `disabled` nativo, `aria-busy` se loading
**Uso:** Ações primárias (primary), secundárias (secondary/ghost), destrutivas (destructive)
**Exemplo:**
```tsx
<Button variant="primary" size="md" onClick={handleSave}>
  Salvar
</Button>
```

### Input
**Arquivo:** `components/ui/input.tsx`
**Props:** `type`, `placeholder`, `value`, `onChange`, `error?: string`, `label?`, `helperText?`, `disabled?`
**Estados:** Default, Focus, Error, Disabled, Filled
**Acessibilidade:** `id` + `label htmlFor`, `aria-describedby` para error/helper, `aria-invalid` se error
**Uso:** Formulários, busca, configurações

### Badge
**Arquivo:** `components/ui/badge.tsx`
**Props:** `variant: 'default' | 'success' | 'warning' | 'error' | 'info' | 'outline'`, `size: 'sm' | 'md'`
**Estados:** Apenas visual (sem interação)
**Acessibilidade:** Texto legível, contraste AA
**Uso:** Status de pedido, tags, contadores

### Avatar
**Arquivo:** `components/ui/avatar.tsx`
**Props:** `src?: string`, `alt: string`, `fallback: string` (iniciais), `size: 'sm' | 'md' | 'lg' | 'xl'`
**Estados:** Default, Loading (skeleton), Error (fallback)
**Acessibilidade:** `alt` descritivo, `role="img"`

### Icon
**Arquivo:** `components/ui/icon.tsx`
**Props:** `name: LucideIconName`, `size?: number`, `className?`
**Estados:** Default (herda `currentColor`)
**Uso:** Em botões, badges, cards, navegação

---

## Molecules (Compostos)

### OrderCard
**Arquivo:** `components/pedidos/order-card.tsx`
**Composição:** `Card` + `Avatar` + `Badge` + `Text` + `ActionButtons`
**Props:** `order: Order`, `onStatusChange: (id, status) => void`, `onReply: (id) => void`, `onViewDetails: (id) => void`
**Estados:** Default, Loading (ações), Pressed (feedback visual)
**Acessibilidade:** Card como `article`, botões com `aria-label` descritivo ("Marcar como preparando pedido #123")

### StatusTabs
**Arquivo:** `components/pedidos/status-tabs.tsx`
**Composição:** `Tabs` (Radix) + `Badge` (contador)
**Props:** `activeStatus: OrderStatus`, `onChange: (status) => void`, `counts: Record<Status, number>`
**Estados:** Active, Hover, Focus, Disabled (se contador 0)
**Acessibilidade:** `Tabs` nativo Radix (ARIA completo), contador em `aria-label` ("Novos, 5 pedidos")

### ActionButtons
**Arquivo:** `components/pedidos/action-buttons.tsx`
**Composição:** `Button` (variant ghost, size sm) × 3
**Props:** `orderId`, `currentStatus`, `onStatusChange`, `onReply`, `onViewDetails`
**Estados:** Loading individual por botão

### EmptyState
**Arquivo:** `components/ui/empty-state.tsx`
**Composição:** `Illustration` + `Heading` + `Text` + `Button` (CTA)
**Props:** `illustration`, `title`, `description`, `ctaLabel?`, `ctaAction?`
**Estados:** Default
**Acessibilidade:** Ilustração `aria-hidden`, texto semântico

### SkeletonOrderCard
**Arquivo:** `components/pedidos/skeleton-order-card.tsx`
**Composição:** `Skeleton` (shadcn) × estrutura do OrderCard
**Props:** Nenhuma
**Estados:** Animado (`animate-pulse`)

---

## Organisms (Complexos)

### Header
**Arquivo:** `components/layout/header.tsx`
**Composição:** `Logo` + `ConnectionStatus` + `UserMenu` (Avatar + Dropdown)
**Props:** `user`, `whatsappConnected: boolean`, `onLogout`, `onSettings`
**Estados:** Default, Menu Aberto, Loading (verificação conexão)

### Modal (Base)
**Arquivo:** `components/ui/modal.tsx` (wrapper Dialog Radix)
**Composição:** `Dialog` + `DialogContent` + `DialogHeader` + `DialogBody` + `DialogFooter`
**Props:** `open`, `onOpenChange`, `title`, `description`, `children`, `size: 'sm' | 'md' | 'lg' | 'xl'`
**Estados:** Aberto, Fechando, Fechado
**Acessibilidade:** Radix Dialog (focus trap, ESC, ARIA completo)

### ReplyModal
**Arquivo:** `components/pedidos/reply-modal.tsx`
**Composição:** `Modal` + `Select` (templates) + `Textarea` + `Button` (enviar)
**Props:** `orderId`, `customerPhone`, `onSend: (message) => void`, `onClose`
**Estados:** Default, Enviando, Sucesso, Erro

---

## Padrões de Composição

### Padrão: Card com Ações (usado em OrderCard, ProductCard)
```
Card
├── Header: Avatar + Título + Badge
├── Body: Conteúdo principal (itens, descrição)
├── Divider
└── Footer: ActionButtons (alinhados direita)
```

### Padrão: Formulário em Modal (usado em ReplyModal, Configurações)
```
Modal
├── Header: Título + Fechar
├── Body: Form (Label + Input + Helper/Error) × N
└── Footer: Cancel (ghost) + Submit (primary)
```

### Padrão: Página Padrão (Dashboard, Histórico, Config)
```
Page
├── Header (fixo ou sticky)
├── Main (scroll)
│   ├── Section Header: Título + Ação primária
│   ├── Toolbar: Filtros, Busca, Tabs
│   └── Content: Lista/Grid/Form (Empty / Loading / Error / Data)
└── FAB (se ação flutuante)
```

---

## Checklist de Cobertura (MVP)

| Componente | Telas que Usam | Status |
|------------|----------------|--------|
| Button | Todas | ✅ Definido |
| Input | SC-01, SC-06, SC-07, SC-08, SC-09, SC-10, SC-11 | ✅ Definido |
| Badge | SC-02, SC-03, SC-05, SC-10 | ✅ Definido |
| Avatar | SC-02, SC-03, SC-04, SC-11 | ✅ Definido |
| Icon | Todas | ✅ Definido |
| Card | SC-02, SC-03, SC-05, SC-09 | ✅ Definido |
| OrderCard | SC-02, SC-05 | ✅ Definido |
| StatusTabs | SC-02 | ✅ Definido |
| ActionButtons | SC-02, SC-03 | ✅ Definido |
| EmptyState | SC-02, SC-05, SC-09 | ✅ Definido |
| SkeletonOrderCard | SC-02 | ✅ Definido |
| Header | SC-02, SC-03, SC-05, SC-06, SC-07, SC-08, SC-09, SC-10 | ✅ Definido |
| Modal | SC-03, SC-04, SC-09, SC-11 | ✅ Definido |
| ReplyModal | SC-04 | ✅ Definido |
| FAB | SC-02 | ✅ Definido |
| ErrorBanner/Toast | Todas (transversal) | ✅ Definido |
```

## Como trabalhar com IA nesta etapa

**Prompt para gerar inventário de telas:**
```
Tenho estes fluxos de usuário: [cole da ETAPA 06].
Gere o inventário completo de telas únicas necessárias.
Para cada tela: ID, nome, fluxo relacionado, tipo (página/modal/bottom sheet), prioridade (Must/Should/Could), estados necessários (default, vazio, carregando, erro, etc.).
Formato: tabela Markdown + mapeamento fluxo→tela.
```

**Prompt para estruturar tela:**
```
Para a tela SC-02 (Dashboard Lista Pedidos):
- Fluxo: [cole fluxo da ETAPA 06]
- Direção visual: [resuma tokens da ETAPA 07]
Gere:
1. Layout mobile-first (ASCII art ou descrição estruturada)
2. Hierarquia visual (primária/secundária/terciária)
3. Lista de componentes usados (referenciando registro de componentes)
4. Responsividade (breakpoints + adaptações)
5. Acessibilidade (foco, ARIA, contraste)
6. Estados da tela (tabela)
Formato: Markdown pronto para copiar no modelo.
```

**Prompt para registrar componentes:**
```
Com base no inventário de telas e direção visual, identifique todos os componentes reutilizáveis.
Classifique em Atoms / Molecules / Organisms (Brad Frost).
Para cada um: nome, arquivo sugerido, props TypeScript, composição, estados, acessibilidade, exemplos de uso.
Gere o Registro de Componentes completo no formato do modelo.
Inclua checklist de cobertura no final.
```

**Prompt para revisar consistência visual:**
```
Revise este inventário + registro de componentes contra a Direção Visual (ETAPA 07).
Verifique:
- Tokens de cor/tipografia/espaçamento/borda/sombra aplicados consistentemente?
- Estados interativos (hover, focus, active, disabled, loading) definidos para TODOS componentes interativos?
- Responsividade prevista para cada tela?
- Acessibilidade: foco visível, ARIA, contraste, semântica?
- Componentes shadcn/ui usados como base onde apropriado?
Liste inconsistências e gaps.
```

**Contexto a fornecer:** Fluxos do Usuário + Direção Visual + Requisitos Funcionais.
**Resultado esperado:** Inventário de Telas + Registro de Componentes completos e aprovados.
**Como verificar:** Cada tela do inventário tem estrutura? Cada componente tem props/estados/acessibilidade? Cobertura 100% das telas Must/Should?

## Checklist de execução

- [ ] Inventário de telas completo (todas as telas dos fluxos Must/Should)
- [ ] Estrutura detalhada para cada tela principal (layout, hierarquia, componentes, responsividade, acessibilidade, estados)
- [ ] Componentes reutilizáveis identificados e classificados (Atoms/Molecules/Organisms)
- [ ] Props TypeScript, estados, acessibilidade, exemplos documentados por componente
- [ ] Wireframes criados (se decididos) — baixa fidelidade, foco layout/hierarquia
- [ ] Protótipo criado (se decidido) — alta fidelidade, interativo
- [ ] Design system proporcional definido (tokens + componentes base apenas MVP)
- [ ] Estados de interação documentados por componente
- [ ] Responsividade planejada (breakpoints + adaptações por tela)
- [ ] Acessibilidade planejada (checklist WCAG 2.1 AA por componente/tela)
- [ ] Revisão visual feita (consistência com direção visual)
- [ ] Critérios de aprovação definidos
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Inventário de Telas completo
- [ ] Registro de Componentes completo com checklist de cobertura 100%
- [ ] Cada tela Must/Should tem estrutura definida
- [ ] Componentes base prontos para implementação (shadcn/ui adaptado)
- [ ] Tokens de design conectados à Direção Visual (ETAPA 07)
- [ ] Responsividade e acessibilidade endereçadas
- [ ] Você consegue visualizar o app tela por tela
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste da cobertura:** Cada fluxo da ETAPA 06 tem telas correspondentes no inventário?
- **Teste dos componentes:** Cada componente do registro é usado em pelo menos 1 tela?
- **Teste dos estados:** Cada componente interativo tem 6 estados (default, hover, focus, active, disabled, loading)?
- **Teste da responsividade:** Layout mobile-first definido para todas as telas?
- **Teste da acessibilidade:** Focus visível, ARIA labels, contraste, ordem de foco — todos previstos?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Criar componentes "para o futuro" | Só MVP. Checklist de cobertura = fonte da verdade. |
| Inventário incompleto (esquece modais, estados) | Derive DO fluxo: cada passo + cada estado = tela/estado único |
| Componentes sem props TypeScript | Props = contrato. IA gera, você valida. |
| Esquecer acessibilidade | Checklist WCAG 2.1 AA por componente (não opcional) |
| Wireframe/protótipo vira "design final" | Wireframe = layout/hierarquia. Protótipo = validação. Não polish visual. |
| Inconsistência com Direção Visual | Revisão obrigatória: tokens vs. componentes |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Muitos componentes, difícil gerenciar | Agrupe. Use shadcn/ui como base — já resolve atoms/molecules básicos. |
| Tela não cabe no mobile | Reavalie hierarquia. Divida em modal/bottom sheet. Priorize conteúdo. |
| Componente precisa de variação não prevista | Adicione variant no registro, documente, implemente. Não crie componente duplicado. |
| Acessibilidade complexa demais | Comece com o básico: foco visível, contraste, semântica HTML, labels. Evolua. |

## Condições para avançar

- ✅ Inventário de Telas + Registro de Componentes salvos e completos
- ✅ Checklist de cobertura 100% para features Must/Should
- ✅ Tokens conectados à Direção Visual
- ✅ Responsividade e acessibilidade endereçadas
- ✅ Você sabe implementar tela por tela

## Próxima etapa

[[ETAPA 09 - Preparar o Ambiente e o Plano de Implementação]]

## Materiais relacionados

- [[02 - Modelos/UX e UI/Inventário de Telas]]
- [[02 - Modelos/UX e UI/Registro de Componentes e Padrões]]
- [[03 - Guias Práticos/UI e UI/Estruturar Interface e Componentes]]
- [[03 - Guias Práticos/UI e UI/Design System Proporcional]]
- [[04 - Referência/Glossário/Design System]]
- [[04 - Referência/Glossário/Atomic Design]]
- [[04 - Referência/Glossário/shadcn/ui]]