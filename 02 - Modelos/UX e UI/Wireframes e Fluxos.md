# Wireframes e Fluxos

> **Copie para:** `05 - Projetos/SeuProjeto/04 - UX e UI/Wireframes e Fluxos.md`
> Baseado na ETAPA 06-08. **Wireframes de baixa fidelidade (ASCII/Mermaid) + Fluxos de navegação.**

---

## Convenção de Wireframes

```
┌─────────────────────────────────────┐
│ Header / App Bar                    │  ← Fixo no topo (56px mobile)
├─────────────────────────────────────┤
│                                     │
│   Conteúdo Principal (Scroll)       │  ← Área rolável
│                                     │
├─────────────────────────────────────┤
│ Bottom Actions / Nav (se houver)    │  ← Fixo embaixo (safe area)
└─────────────────────────────────────┘
```

- **Grid:** 4px baseline, 16px gutter mobile, 24px desktop
- **Touch targets:** Mínimo 44×44px (Material / HIG)
- **Cores:** Tokens semânticos (ver Design System - Tokens)
- **Estados:** Default, Hover, Focus, Active, Disabled, Loading, Error, Empty

---

## 1. Login / Autenticação

### Login - Magic Link (Primário)
```
┌─────────────────────────────────────┐
│ ◀ PedidosSimples                    │
├─────────────────────────────────────┤
│                                     │
│      📦 PedidosSimples              │
│   Gerencie pedidos no WhatsApp      │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Email                    ✎  │   │  ← Input email, tipo=email
│   └─────────────────────────────┘   │
│                                     │
│   [ Enviar link mágico ]            │  ← Primary button, full width
│                                     │
│   ────── ou ──────                  │
│                                     │
│   [ Entrar com senha ]              │  ← Secondary button, navega para /login/senha
│                                     │
│   Ao continuar, você aceita os      │
│   Termos e Política de Privacidade  │
│                                     │
└─────────────────────────────────────┘
```

### Login - Credentials (Secundário)
```
┌─────────────────────────────────────┐
│ ◀ Voltar                            │
├─────────────────────────────────────┤
│                                     │
│      Entrar com senha               │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Email                    ✎  │   │
│   └─────────────────────────────┘   │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Senha                    👁  │   │  ← Input password, toggle visibility
│   └─────────────────────────────┘   │
│                                     │
│   [ Entrar ]                        │  ← Primary
│                                     │
│   [ Esqueci a senha ]               │  ← Link → Magic Link "reset"
│                                     │
│   [ Voltar para link mágico ]       │  ← Link → /login
│                                     │
└─────────────────────────────────────┘
```

### Callback Magic Link (`/auth/callback`)
```
┌─────────────────────────────────────┐
│                                     │
│         ⏳ Entrando...              │  ← Spinner + texto
│                                     │
│    Validando seu link de acesso     │
│                                     │
│    (Redirecionando automaticamente) │
│                                     │
└─────────────────────────────────────┘
```

---

## 2. Onboarding (4 Passos)

### Passo 1: Dados da Loja
```
┌─────────────────────────────────────┐
│ ◀ Fechar          1 / 4             │
├─────────────────────────────────────┤
│                                     │
│   Dados da sua loja                 │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Nome da loja             ✎  │   │  ← Ex: "Lanches do João"
│   └─────────────────────────────┘   │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ WhatsApp da loja         ✎  │   │  ← Input tel, máscara (XX) XXXXX-XXXX
│   └─────────────────────────────┘   │
│   💡 Use o número do WhatsApp Business │
│                                     │
│   [ Continuar ]          [Pular]    │  ← Primary + Ghost (pular = preset depois)
│                                     │
└─────────────────────────────────────┘
```

### Passo 2: Conectar WhatsApp
```
┌─────────────────────────────────────┐
│ ◀ Voltar            2 / 4           │
├─────────────────────────────────────┤
│                                     │
│   Conecte seu WhatsApp Business     │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ 📱 Passo a passo:            │   │
│   │ 1. Acesse business.facebook.com│  │
│   │ 2. Vá em WhatsApp > Configuração│  │
│   │ 3. Copie: Phone Number ID     │   │
│   │ 4. Gere: Access Token         │   │
│   └─────────────────────────────┘   │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Phone Number ID          ✎  │   │
│   └─────────────────────────────┘   │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Access Token             ✎  │   │  ← Password input
│   └─────────────────────────────┘   │
│                                     │
│   [ Testar conexão ]                │  ← Secondary (valida token)
│   [ Continuar ]                     │  ← Primary
│                                     │
└─────────────────────────────────────┘
```

### Passo 3: Horário de Funcionamento
```
┌─────────────────────────────────────┐
│ ◀ Voltar            3 / 4           │
├─────────────────────────────────────┤
│                                     │
│   Quando você abre?                 │
│                                     │
│   [🍔 Lanchonete Noite]  [🍽 Almoço]  │  ← Preset chips (seleção única)
│   [☕ Café da Manhã]   [⚙ Personal.] │
│                                     │
│   ┌── Preset: Lanchonete Noite ────┐ │
│   │ Seg-Sex  18:00 ── 23:30  ☐     │ │  ← Linha por dia, toggle "Fechado"
│   │ Sáb      18:00 ── 00:00  ☐     │ │
│   │ Dom      FECHADO        ☑      │ │
│   └────────────────────────────────┘ │
│                                     │
│   [ Continuar ]                     │
│                                     │
└─────────────────────────────────────┘
```

### Passo 4: Testar Webhook
```
┌─────────────────────────────────────┐
│ ◀ Voltar            4 / 4           │
├─────────────────────────────────────┤
│                                     │
│   Teste seu primeiro pedido         │
│                                     │
│   ┌─────────────────────────────┐   │
│   │ 📱 Envie um WhatsApp para:   │   │
│   │    +55 11 99999-9999        │   │
│   │    "Quero 1 x-burger teste"  │   │
│   └─────────────────────────────┘   │
│                                     │
│   [ Enviar pedido de teste ]        │  ← Primary (dispara webhook fake)
│                                     │
│   ⏳ Aguardando...                  │  ← Estado loading
│                                     │
│   ✅ Pedido de teste recebido!      │  ← Estado sucesso
│      #TEST-123 • R$ 25,00 • Novo    │
│                                     │
│   [ Ir para o Dashboard ]           │  ← Primary → /dashboard
│                                     │
└─────────────────────────────────────┘
```

---

## 3. Dashboard - Lista de Pedidos

### Estado Vazio
```
┌─────────────────────────────────────┐
│ 🏪 Lanches do João    👤 ⚙          │
├─────────────────────────────────────┤
│                                     │
│   [Todos] [Novo] [Preparando]       │  ← Filter chips (scroll horizontal)
│   [Pronto] [Hoje]  [Semana]         │
│                                     │
│           📦                        │
│       Nenhum pedido ainda           │
│                                     │
│   Pedidos chegarão aqui             │
│   automaticamente via WhatsApp      │
│                                     │
│        [ Enviar pedido teste ]      │  ← Ghost button
│                                     │
├─────────────────────────────────────┤
│ 🏠 Pedidos  📊 Relatórios  ⚙ Config │  ← Bottom nav (PWA)
└─────────────────────────────────────┘
```

### Estado Carregando (Skeleton)
```
┌─────────────────────────────────────┐
│ 🏪 Lanches do João    👤 ⚙  ● LIVE  │  ← ● Verde pulsando = SSE ativo
├─────────────────────────────────────┤
│ [Todos] [Novo●3] [Prep●2] [Pronto]  │  ← Badges com contadores
│                                     │
│   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │  ← Skeleton card
│   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
│   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
│   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
│   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │
├─────────────────────────────────────┤
│ 🏠 Pedidos  📊 Relatórios  ⚙ Config │
└─────────────────────────────────────┘
```

### Estado Com Dados (Lista)
```
┌─────────────────────────────────────┐
│ 🏪 Lanches do João    👤 ⚙  ● LIVE  │
├─────────────────────────────────────┤
│ [Todos] [Novo●3] [Prep●2] [Pronto]  │
│                                     │
│ ┌─────────────────────────────────┐ │
│ │ #1234  ● NOVO        R$ 65,00   │ │  ← Card pedido
│ │ João S.  •  2x X-Burger, 1x Bat │ │
│ │ 10:32  •  WhatsApp    [Ver ▶]   │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ #1233  ● PREPARANDO   R$ 42,00  │ │  ● Badge colorido por status
│ │ Maria L. •  1x X-Salada, 1x Ref │ │
│ │ 10:15  •  WhatsApp    [Ver ▶]   │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ #1232  ● PRONTO       R$ 38,00  │ │
│ │ Pedro K. •  2x Batata, 2x Refri │ │
│ │ 09:45  •  WhatsApp    [Ver ▶]   │ │
│ └─────────────────────────────────┘ │
│                                     │
│        (scroll para mais...)        │
├─────────────────────────────────────┤
│ 🏠 Pedidos  📊 Relatórios  ⚙ Config │
└─────────────────────────────────────┘
```

**Cores dos Badges de Status:**
| Status | Cor | Ícone |
|--------|-----|-------|
| NOVO | 🔵 Azul `#3b82f6` | 🔔 |
| PREPARANDO | 🟠 Laranja `#f97316` | 🍳 |
| PRONTO | 🟢 Verde `#22c55e` | ✅ |
| ENTREGUE | ⚫ Cinza `#6b7280` | 📦 |
| CANCELADO | 🔴 Vermelho `#ef4444` | ❌ |
| AGUARDANDO_ABERTURA | 🟡 Amarelo `#eab308` | ⏰ |

---

## 4. Detalhe do Pedido

```
┌─────────────────────────────────────┐
│ ◀ #1234        ● NOVO        👤 ⚙  │
├─────────────────────────────────────┤
│                                     │
│ 👤 João Silva                       │
│ 📱 55 11 9999-9999                  │
│                                     │
│ ┌─ Itens ────────────────────────┐  │
│ │ 2x  X-Burger Especial    R$ 50 │  │
│ │ 1x  Batata Grande        R$ 15 │  │
│ │ ────────────────────────────  │  │
│ │ Total:                   R$ 65 │  │
│ └────────────────────────────────┘  │
│                                     │
│ ┌─ Observações ──────────────────┐  │
│ │ "Sem cebola no burger, por     │  │
│ │  favor. Batata bem sequinha."  │  │
│ └────────────────────────────────┘  │
│                                     │
│ ┌─ Timeline ─────────────────────┐  │
│ │ 🔔 NOVO        10:32  (você)   │  │
│ │ 🍳 PREPARANDO  10:35  (você)   │  │
│ │ ✅ PRONTO      10:48  (você)   │  │
│ │ 📦 ENTREGUE    10:55  (você)   │  │
│ └────────────────────────────────┘  │
│                                     │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │  ← Sticky bottom actions
│ │ [ Iniciar preparo ]             │ │  ← Primary (contexto: status=NOVO)
│ │ [ Cancelar pedido ]             │ │  ← Destructive (sempre)
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ [ Responder cliente ]           │ │  ← Secondary
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### Estados dos Botões por Status

| Status | Botão Principal | Botão Secundário |
|--------|-----------------|------------------|
| NOVO | `[ Iniciar preparo ]` | `[ Cancelar ]` |
| PREPARANDO | `[ Marcar pronto ]` | `[ Cancelar ]` |
| PRONTO | `[ Entregar ]` | `[ Avisar cliente ]` |
| ENTREGUE | (nenhum - readonly) | `[ Ver histórico ]` |
| CANCELADO | (nenhum) | `[ Reabrir ]` (Owner only) |
| AGUARDANDO_ABERTURA | `[ Aguardando abertura ]` (disabled) | `[ Cancelar ]` |

---

## 5. Modal: Responder Cliente

```
┌─────────────────────────────────────┐
│ Responder cliente          ✕        │
├─────────────────────────────────────┤
│                                     │
│   Templates rápidos:                │
│                                     │
│ ┌─────────────────────────────────┐ │
│ │ 📦 Pedido confirmado            │ │
│ │ "Seu pedido #1234 confirmado!  │ │
│ │  Tempo estimado: 20 min"       │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ 🍳 Em preparo                   │ │
│ │ "Seu pedido #1234 está sendo   │ │
│ │  preparado 🍳"                 │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ ✅ Pronto para retirada         │ │
│ │ "Seu pedido #1234 está pronto! │ │
│ │  Pode vir buscar 📦"           │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ ⏰ Atraso                       │ │
│ │ "Seu pedido #1234 terá atraso  │ │
│ │  de 10 min. Peço desculpas!"   │ │
│ └─────────────────────────────────┘ │
│                                     │
│   ────── ou ──────                  │
│                                     │
│ ┌─────────────────────────────────┐ │
│ │ Escrever mensagem...         42 │ │  ← Textarea + contador
│ │                                 │ │
│ │                                 │ │
│ └─────────────────────────────────┘ │
│                                     │
│ [ Cancelar ]        [ Enviar ]      │
│                                     │
└─────────────────────────────────────┘
```

---

## 6. Relatórios

### Aba: Hoje
```
┌─────────────────────────────────────┐
│ ◀ Relatórios           Hoje ▼       │
├─────────────────────────────────────┤
│                                     │
│ ┌─────────┬─────────┬────────────┐  │
│ │ Pedidos │ Entregues│ Cancelados│  │  ← 3 cards métricas
│ │   23    │    18    │     2      │  │
│ └─────────┴─────────┴────────────┘  │
│                                     │
│ ┌─────────────┬───────────────────┐ │
│ │ Faturamento │ Ticket Médio      │ │
│ │  R$1.240,00 │    R$ 68,89       │ │
│ └─────────────┴───────────────────┘ │
│                                     │
│ ┌─ Por Hora ─────────────────────┐  │
│ │ ████████████ 18h (5)           │  │  ← Barra horizontal simples
│ │ ████████████████ 19h (8)       │  │
│ │ ████████████████████ 20h (10)  │  │
│ │ ████████████████ 21h (7)       │  │
│ │ ████████ 22h (4)               │  │
│ └────────────────────────────────┘  │
│                                     │
│ ┌─ Pedidos Entregues ────────────┐  │
│ │ #1234  João S.    R$ 65  20:15 │  │
│ │ #1231  Maria L.   R$ 42  19:45 │  │
│ │ #1229  Pedro K.   R$ 38  19:20 │  │
│ │ ...                              │  │
│ └────────────────────────────────┘  │
│                                     │
│ [ Exportar CSV ]                    │
│                                     │
└─────────────────────────────────────┘
```

---

## 7. Configurações

### Produtos
```
┌─────────────────────────────────────┐
│ ◀ Configurações    Produtos ▼  [+]  │
├─────────────────────────────────────┤
│                                     │
│   🔍 Buscar produtos...             │
│                                     │
│   [Todos] [Lanches] [Acomp.] [Beb.] │  ← Filter chips categoria
│                                     │
│ ┌─────────────────────────────────┐ │
│ │ 🍔 X-Burger Especial      ●     │ │  ← Card produto
│ │ R$ 25,00 • Lanches            │ │
│ │ [Editar] [Desativar]          │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ 🍟 Batata Frita           ●     │ │
│ │ R$ 15,00 • Acompanhamentos    │ │
│ │ [Editar] [Desativar]          │ │
│ └─────────────────────────────────┘ │
│ ┌─────────────────────────────────┐ │
│ │ 🥤 Refrigerante           ●     │ │
│ │ R$ 8,00 • Bebidas             │ │
│ │ [Editar] [Desativar]          │ │
│ └─────────────────────────────────┘ │
│                                     │
└─────────────────────────────────────┘
```

### Modal: Novo/Editar Produto
```
┌─────────────────────────────────────┐
│ Novo Produto                ✕       │
├─────────────────────────────────────┤
│                                     │
│ ┌─────────────────────────────┐    │
│ │ Nome *                  ✎   │    │
│ └─────────────────────────────┘    │
│                                     │
│ ┌─────────────────────────────┐    │
│ │ Preço *                 ✎   │    │  ← Input number, step=0.01, prefix=R$
│ └─────────────────────────────┘    │
│                                     │
│ ┌─────────────────────────────┐    │
│ │ Descrição               ✎   │    │  ← Textarea
│ │                             │    │
│ └─────────────────────────────┘    │
│                                     │
│ ┌─────────────────────────────┐    │
│ │ Categoria            ▼      │    │  ← Select com autocomplete
│ │ Lanches                     │    │
│ │ Acompanhamentos             │    │
│ │ Bebidas                     │    │
│ │ + Nova categoria...         │    │
│ └─────────────────────────────┘    │
│                                     │
│ ┌─┐ Ativo para sugestões        ☑  │  ← Toggle switch
│                                     │
│ [ Cancelar ]        [ Salvar ]      │
│                                     │
└─────────────────────────────────────┘
```

### Horários
```
┌─────────────────────────────────────┐
│ ◀ Configurações    Horários ▼       │
├─────────────────────────────────────┤
│                                     │
│   [Lanchonete Noite] [Almoço]       │  ← Presets
│   [Café] [24h] [Personalizado ✓]   │
│                                     │
│ ┌─────────────────────────────────┐ │
│ │ Segunda    18:00 ── 23:30  ☐   │ │
│ │ Terça      18:00 ── 23:30  ☐   │ │
│ │ Quarta     18:00 ── 23:30  ☐   │ │
│ │ Quinta     18:00 ── 23:30  ☐   │ │
│ │ Sexta      18:00 ── 00:00  ☐   │ │
│ │ Sábado     18:00 ── 00:00  ☐   │ │
│ │ Domingo    FECHADO         ☑   │ │
│ └─────────────────────────────────┘ │
│                                     │
│   💡 Fora do horário: pedidos ficam │
│   "Aguardando abertura" e cliente   │
│   recebe mensagem automática.       │
│                                     │
│   ┌─ Mensagem fechado ───────────┐  │
│   │ "Estamos fechados! Abrimos   │  │
│   │  às 18:00. Seu pedido será   │  │
│   │  atendido quando abrirmos."  │  │
│   └──────────────────────────────┘  │
│                                     │
│   [ Salvar horários ]               │
│                                     │
└─────────────────────────────────────┘
```

---

## 8. Fluxos de Navegação (Mermaid)

### Navegação Principal (Bottom Nav PWA)
```mermaid
graph LR
    A[/dashboard] --> B[/pedidos/:id]
    A --> C[/relatorios]
    A --> D[/configuracoes]
    D --> D1[/configuracoes/produtos]
    D --> D2[/configuracoes/horarios]
    B --> E[Modal: Responder]
    B --> F[Modal: Cancelar]
    C --> C1[/relatorios?period=hoje]
    C --> C2[/relatorios?period=semana]
    C --> C3[/relatorios?period=mes]
    C --> C4[/relatorios?period=custom]
```

### Fluxo Onboarding
```mermaid
flowchart TD
    A[Landing /login] --> B{Magic Link ou Credentials?}
    B -->|Magic Link| C[/auth/callback]
    B -->|Credentials| C
    C --> D{Novo usuário?}
    D -->|Sim| E[/onboarding/1-loja]
    D -->|Não| F[/dashboard]
    E --> G[/onboarding/2-whatsapp]
    G --> H[/onboarding/3-horario]
    H --> I[/onboarding/4-teste]
    I --> F
    F --> J[Dashboard vazio]
    J --> K[Recebe pedido real]
```

### Fluxo Core: Pedido → Entrega
```mermaid
flowchart TD
    A[WhatsApp Webhook] --> B[Cria Pedido NOVO]
    B --> C[SSE: novo_pedido]
    C --> D[Dashboard: Badge Novo++]
    D --> E[Lojista tap card]
    E --> F[/pedidos/:id]
    F --> G{Qual ação?}
    G -->|Iniciar preparo| H[Status PREPARANDO]
    G -->|Cancelar| I[Status CANCELADO]
    H --> J[SSE: status_atualizado]
    J --> K[Lojista tap Marcar pronto]
    K --> L[Status PRONTO]
    L --> M[Lojista tap Avisar cliente]
    M --> N[Modal Templates]
    N --> O[Envia WhatsApp Template]
    O --> P[Lojista tap Entregar]
    P --> Q[Status ENTREGUE]
    Q --> R[SSE: status_atualizado]
    R --> S[Sai da lista ativa]
    S --> T[Relatório atualizado]
```

---

## 9. Responsividade (Breakpoints)

| Breakpoint | Largura | Layout Dashboard | Layout Detalhe |
|------------|---------|------------------|----------------|
| Mobile | 375-639px | Lista cards empilhados, bottom nav fixo | Scroll vertical, actions sticky bottom |
| Tablet | 640-1023px | 2 colunas (lista + detalhe side-by-side) | Painel lateral fixo |
| Desktop | ≥1024px | 3 colunas (filtros + lista + detalhe) | Detalhe em painel direito fixo |

---

## 10. Estados de UI por Componente

### Botão (Button)
| Estado | Visual | Acessibilidade |
|--------|--------|----------------|
| Default | `bg-primary text-primary-foreground` | `aria-disabled="false"` |
| Hover | `opacity-90` | - |
| Focus | `outline-none ring-2 ring-primary ring-offset-2` | `tabIndex=0` |
| Active | `scale-[0.98]` | - |
| Loading | `opacity-70 cursor-wait` + Spinner | `aria-busy="true"` |
| Disabled | `opacity-50 cursor-not-allowed` | `aria-disabled="true"` |

### Input (Text, Email, Tel, Number)
| Estado | Visual |
|--------|--------|
| Default | `border-input bg-background` |
| Focus | `border-primary ring-2 ring-primary/20` |
| Error | `border-destructive` + `aria-invalid="true"` + mensagem erro |
| Disabled | `opacity-50 cursor-not-allowed` |
| Com valor | Mostra `✕` limpar (exceto password) |

### Card Pedido
| Estado | Visual |
|--------|--------|
| Default | `border-border bg-card` |
| Hover/Touch | `bg-accent/50` |
| Selecionado (tablet/desktop) | `ring-2 ring-primary` |
| Novo (não lido) | Borda esquerda `border-l-4 border-blue-500` |
| Loading (skeleton) | `animate-pulse bg-muted` |

---

## Checklist Wireframes

- [ ] Todas telas principais cobertas (Login, Onboarding, Dashboard, Detalhe, Modais, Relatórios, Config)
- [ ] Estados: Default, Loading, Empty, Error, Success
- [ ] Responsividade: Mobile, Tablet, Desktop
- [ ] Acessibilidade: Touch targets, contraste, foco, ARIA labels
- [ ] Navegação: Fluxos mapeados, deep links, back navigation
- [ ] Componentes reutilizáveis identificados (Button, Input, Card, Badge, Modal, Chip, Toggle)
- [ ] Design tokens referenciados (não valores hardcoded)

---

## Próxima Etapa
[[02 - Modelos/UX e UI/Design System - Tokens]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 06 - Planejar a Experiência do Usuário (UX)]]
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 07 - Pesquisar Referências e Definir a Direção Visual]]
- [[01 - Manual de Desenvolvimento/04 - UX e UI/ETAPA 08 - Estruturar a Interface (UI)]]
- [[02 - Modelos/UX e UI/Jornada do Usuário]]
- [[02 - Modelos/UX e UI/Design System - Tokens]]
- [[02 - Modelos/UX e UI/Especificação de Componentes]]
- [[02 - Modelos/UX e UI/Acessibilidade e Checklist UI]]
- [[04 - Referência/Glossário/Wireframe]]
- [[04 - Referência/Glossário/Design System]]
- [[04 - Referência/Glossário/Atomic Design]]