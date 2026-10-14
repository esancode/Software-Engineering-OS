# ETAPA 07 — Pesquisar Referências e Definir a Direção Visual

## Objetivo

Evitar interfaces genéricas e estabelecer uma **identidade visual coerente** através de pesquisa de referências reais, análise de padrões e definição consciente de direção estética.

## Por que esta etapa existe?

Interfaces "feitas por IA" tendem a ficar genéricas, inconsistentes e sem personalidade. Pesquisa deliberada de referências reais cria base para decisões visuais intencionais.

## Quando começar

- ETAPA 06 concluída (Fluxos do Usuário aprovados)
- Você sabe as telas, fluxos e necessidades — agora define a "cara" do produto

## O que você precisa fazer

1. **Pesquise referências visuais** — produtos reais, concorrentes, sites de referência (Dribbble, Behance, Mobbin, Land-book)
2. **Analise produtos reais** — não apenas capturas de tela bonitas, mas produtos que você usa
3. **Registre as referências selecionadas** — o que funciona, o que não funciona, por quê
4. **Identifique características visuais recorrentes** — cores, tipografia, espaçamento, formas, ícones
5. **Escolha uma direção estética** — minimalista, bold, friendly, technical, playful, etc.
6. **Defina paleta de cores** — primária, secundária, neutras, semânticas (sucesso, erro, aviso)
7. **Escolha tipografia** — fonte principal, hierarquia (tamanhos, pesos), fallback
8. **Defina sistema de espaçamento** — escala base (4px, 8px), uso consistente
9. **Defina formas e bordas** — border-radius, sombras, elevação
10. **Escolha ícones** — biblioteca (Lucide, Heroicons, Phosphor), estilo (outline, fill), peso
11. **Defina ilustrações e imagens** — estilo, quando usar, fontes, IA vs. real
12. **Defina tratamento de estados interativos** — hover, focus, active, disabled, loading
13. **Garanta consistência visual** — design tokens, componentes base
14. **Previna aparência genérica de IA** — personalidade, decisões intencionais
15. **Produza o documento: Direção Visual + Referências Documentadas**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Pesquisar e coletar referências | 👤 USUÁRIO (IA pode sugerir onde buscar) |
| Analisar e registrar referências | 👤 USUÁRIO + 🤖 IA (organiza) |
| Escolher direção estética | 👤 USUÁRIO (decisão subjetiva) |
| Definir tokens visuais (cores, typo, spacing) | 🤖 IA (sugere baseada na direção) + 👤 USUÁRIO (aprova) |
| Definir estados interativos | 🤖 IA (padrões) + 👤 USUÁRIO (valida) |
| Verificar licenças de imagens/ícones | 👤 USUÁRIO |
| Aprovar direção visual final | ✅ VALIDAÇÃO (você) |

## Ferramentas recomendadas

| Finalidade | Ferramentas |
|------------|-------------|
| Coleta de referências | Mobbin, Land-book, Dribbble, Behance, Pinterest, screenshots próprios |
| Análise de cores | Coolors, Adobe Color, Tailwind CSS color palette |
| Tipografia | Google Fonts, Fontshare, Tailwind Typography |
| Ícones | Lucide, Heroicons, Phosphor, Tabler Icons (todos open source) |
| Design tokens | Figma (se usar), ou JSON/TS no código (Tailwind config) |
| Verificação de contraste | WebAIM Contrast Checker, Stark (plugin Figma) |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Direção Visual | `02 - Modelos/UX e UI/Direção Visual.md` | `05 - Projetos/SeuProjeto/04 - UX-UI/` |
| Referências Visuais | `02 - Modelos/UX e UI/Referências Visuais.md` | `05 - Projetos/SeuProjeto/04 - UX-UI/` |

### Condicionais
- Guia de Estilo de Ilustrações/Imagens (se usar ilustrações customizadas)
- Especificação de Motion/Animação (se motion for importante)

### Opcionais
- Moodboard visual (imagens no Obsidian ou link Figma/Pinterest)

## Como produzir os documentos

### Referências Visuais

**Formato por referência:**
```markdown
## Referência: [Nome do Produto/Site]

**Link:** [URL]
**Tipo:** [App mobile / Web app / Site / Componente específico]
**Captura de tela:** ![descrição](caminho/imagem.png) ou [link para imagem]
**Data da coleta:** 2026-10-11

### O que funciona bem
- [Ex: Hierarquia clara de tipografia — títulos bem distintos do body]
- [Ex: Uso de cor de marca apenas em ações primárias — resto neutro]
- [Ex: Espaçamento generoso — respira, não parece apertado]
- [Ex: Ícones outline consistentes — 2px stroke, 24x24px]

### O que NÃO deve ser reproduzido
- [Ex: Muitas sombras pesadas — pesa visualmente no mobile]
- [Ex: Gradientes em todo lugar — cansa, parece datado]
- [Ex: Botões com borda + sombra + gradiente — overdesigned]

### Ideias que podem ser adaptadas
- [Ex: Card de pedido com swipe actions — adaptar para ações rápidas]
- [Ex: Badge de status com ícone + cor — usar no nosso status de pedidos]
- [Ex: Empty state com ilustração + CTA — copiar padrão para nosso "sem pedidos"]

### Relação com nosso projeto
- Aplicável a: [Dashboard, Detalhes do Pedido, Configurações]
- Prioridade: [Alta/Média/Baixa]
```

### Direção Visual

```markdown
# Direção Visual — PedidosSimples

## Direção Estética Escolhida
**Limpo, Funcional, Confiável — "Mobile-first utilitário com personalidade sutil"**
- Não minimalista extremo (precisa densidade de informação)
- Não "playful" (contexto trabalho, eficiência)
- Personalidade: tipografia distinta + cor de marca pontual + micro-interações

## Paleta de Cores

### Primária (Marca)
| Uso | Hex | Tailwind | Contraste BG Branco |
|-----|-----|----------|---------------------|
| Primary 500 (Principal) | #2563EB | `blue-500` | 4.5:1 ✅ AA |
| Primary 600 (Hover/Active) | #1D4ED8 | `blue-600` | 7:1 ✅ AAA |
| Primary 100 (BG sutil) | #DBEAFE | `blue-100` | - |

### Neutras (Base)
| Token | Hex | Tailwind | Uso |
|-------|-----|----------|-----|
| Neutral 950 | #030712 | `neutral-950` | Texto principal |
| Neutral 700 | #374151 | `neutral-700` | Texto secundário |
| Neutral 500 | #6B7280 | `neutral-500` | Placeholder, label |
| Neutral 300 | #D1D5DB | `neutral-300` | Borda, divisor |
| Neutral 100 | #F3F4F6 | `neutral-100` | BG sutil, hover |
| Neutral 50 | #F9FAFB | `neutral-50` | BG página |
| White | #FFFFFF | `white` | Cards, modais |

### Semânticas (Sistema)
| Token | Hex | Tailwind | Uso |
|-------|-----|----------|-----|
| Success 500 | #10B981 | `emerald-500` | Sucesso, entregue, confirmado |
| Success 100 | #D1FAE5 | `emerald-100` | BG sucesso |
| Warning 500 | #F59E0B | `amber-500` | Atenção, preparando, fora horário |
| Warning 100 | #FEF3C7 | `amber-100` | BG aviso |
| Error 500 | #EF4444 | `red-500` | Erro, cancelado, destrutivo |
| Error 100 | #FEE2E2 | `red-100` | BG erro |
| Info 500 | #06B6D4 | `cyan-500` | Info, novo, neutro |
| Info 100 | #CFFAFE | `cyan-100` | BG info |

> [!important]
> **Teste de contraste:** Todos os pares texto/BG devem passar WCAG AA (4.5:1). Use WebAIM Contrast Checker.

## Tipografia

### Fonte Principal
**Inter** (Google Fonts, variable font, open source)
- Fallback: `system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`

### Escala Tipográfica (Tailwind + clamp para fluidez)
| Token | Tamanho | Peso | Line-height | Uso |
|-------|---------|------|-------------|-----|
| Display XL | clamp(2rem, 5vw, 3rem) | 700 | 1.1 | Hero, landing |
| Display LG | clamp(1.5rem, 4vw, 2rem) | 700 | 1.2 | Título página |
| Heading 1 | 1.5rem (24px) | 600 | 1.3 | Título seção principal |
| Heading 2 | 1.25rem (20px) | 600 | 1.3 | Título subseção |
| Heading 3 | 1.125rem (18px) | 600 | 1.4 | Título card, modal |
| Body LG | 1.125rem (18px) | 400 | 1.6 | Body 강조 |
| Body | 1rem (16px) | 400 | 1.6 | Texto padrão |
| Body SM | 0.875rem (14px) | 400 | 1.5 | Meta, legenda |
| Caption | 0.75rem (12px) | 500 | 1.4 | Badge, timestamp, help text |
| Button | 0.875rem (14px) | 500 | 1 | Botões |

## Espaçamento

**Escala base: 4px (Tailwind padrão)**
| Token | Valor | Uso |
|-------|-------|-----|
| space-1 | 4px | Gap interno pequeno (icon + text) |
| space-2 | 8px | Gap padrão entre elementos relacionados |
| space-3 | 12px | Gap médio |
| space-4 | 16px | Padding padrão card, gap seção |
| space-5 | 20px | Padding modal, gap grande |
| space-6 | 24px | Padding página, gap seções grandes |
| space-8 | 32px | Gap entre seções principais |
| space-10 | 40px | Padding landing, hero |
| space-12 | 48px | Espaçamento extra grande |

## Formas e Bordas

| Elemento | Border Radius | Sombra | Borda |
|----------|---------------|--------|-------|
| Botão | `rounded-lg` (8px) | `shadow-sm` hover `shadow` | Nenhuma (cor sólida) |
| Card | `rounded-xl` (12px) | `shadow-sm` | `border neutral-200` |
| Input | `rounded-lg` (8px) | `shadow-sm` focus `ring-2 ring-primary-500/20` | `border neutral-300` |
| Badge | `rounded-full` (9999px) | Nenhuma | Nenhuma |
| Modal | `rounded-2xl` (16px) | `shadow-xl` | Nenhuma |
| Dropdown | `rounded-xl` (12px) | `shadow-lg` | `border neutral-200` |

## Ícones

**Biblioteca: Lucide React** (open source, consistente, tree-shakable)
- Estilo: Outline, 2px stroke
- Tamanho padrão: 20x20px (inline), 24x24px (botões), 16x16px (compacto)
- Peso: 2px (padrão Lucide)
- Cor: `currentColor` (herda cor do texto) ou cor semântica

## Ilustrações e Imagens

| Tipo | Estilo | Fonte | Quando Usar |
|------|--------|-------|-------------|
| Empty States | Line art minimalista, cor primária sutil | Undraw (MIT) ou custom SVG | Estados vazios principais |
| Onboarding | Line art + cor primária | Undraw ou custom | Primeira vez |
| Imagens de produto | Foto real do estabelecimento | Upload do usuário | Perfil, catálogo |
| Imagens geradas por IA | **Evitar no MVP** — usar placeholders neutros | - | Não usar como "conteúdo real" |

> [!warning]
> **Não use imagens geradas por IA como conteúdo real do produto.** Placeholders OK, mas marque como tal. Imagens reais = upload do usuário ou bibliotecas com licença clara (Unsplash, Pexels, Undraw MIT).

## Estados Interativos

| Estado | Botão Primário | Botão Secundário | Input | Card/Item Lista | Link |
|--------|----------------|------------------|-------|-----------------|------|
| Default | `bg-primary-500 text-white` | `bg-neutral-100 text-neutral-900` | `border-neutral-300` | `border-neutral-200` | `text-primary-500` |
| Hover | `bg-primary-600` | `bg-neutral-200` | `border-neutral-400` | `bg-neutral-50` | `text-primary-600 underline` |
| Active/Pressed | `bg-primary-700 scale-[0.98]` | `bg-neutral-300 scale-[0.98]` | - | `bg-neutral-100` | `text-primary-700` |
| Focus | `ring-2 ring-primary-500/50` | `ring-2 ring-neutral-500/50` | `ring-2 ring-primary-500/20` | `ring-2 ring-primary-500/20` | `outline-none ring-2 ring-primary-500/50` |
| Disabled | `opacity-50 cursor-not-allowed` | `opacity-50 cursor-not-allowed` | `opacity-50 cursor-not-allowed` | `opacity-50` | `opacity-50 cursor-not-allowed` |
| Loading | `opacity-75` + spinner | `opacity-75` + spinner | `opacity-75` + skeleton | `animate-pulse` (skeleton) | - |

## Design Tokens (para implementação)

```typescript
// tailwind.config.ts ou tokens.ts
export const tokens = {
  colors: { /* paleta acima */ },
  spacing: { /* escala 4px */ },
  borderRadius: { /* formas acima */ },
  typography: { /* escala acima */ },
  shadows: {
    sm: '0 1px 2px 0 rgb(0 0 0 / 0.05)',
    DEFAULT: '0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)',
    lg: '0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)',
    xl: '0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1)',
  },
  transitions: {
    fast: '150ms ease-out',
    normal: '200ms ease-out',
    slow: '300ms ease-out',
  },
}
```

## Prevenção de "Visual Genérico de IA"

| Sintoma Genérico | Nossa Decisão Intencional |
|------------------|---------------------------|
| Azul genérico #3B82F6 em tudo | Azul marca apenas em ações primárias; resto neutro |
| Sombras pesadas em cards | Sombra sutil (`shadow-sm`) + borda clara |
| Gradientes em backgrounds | Sólidos; gradiente só em hero landing |
| Border-radius 0.5rem (8px) everywhere | Diferenciado: card 12px, button 8px, badge full, modal 16px |
| Inter font sem personalidade | Inter + escala tipográfica distinta + letter-spacing ajustado |
| Ícones fill (preenchidos) | Outline consistente (Lucide) |
| Espaçamento aleatório | Escala 4px obrigatória |
| Estados hover/focus idênticos | Focus visível (ring) ≠ hover; acessibilidade |

## Como trabalhar com IA nesta etapa

**Prompt para analisar referências:**
```
Tenho estas capturas de referência: [descreva ou cole URLs].
Extraia padrões visuais: paleta, tipografia, espaçamento, border-radius, sombras, ícones, densidade.
Formato: tabela comparativa.
Não gere CSS — só análise.
```

**Prompt para gerar direção visual:**
```
Minha direção escolhida: [ex: "Limpo, funcional, confiável"].
Referências que gostei: [resuma 3-5 pontos-chave].
Gere:
1. Paleta completa (primária, neutras, semânticas) com hex, Tailwind, contraste
2. Escala tipográfica com clamp fluido
3. Escala espaçamento 4px
4. Tabela border-radius/sombra/borda por componente
5. Estados interativos (hover, focus, active, disabled, loading)
6. Tokens TypeScript para Tailwind config
Formato: Markdown pronto para copiar no modelo.
```

**Prompt para revisar consistência:**
```
Revise esta direção visual contra as 10 heurísticas visuais:
1. Hierarquia clara? 2. Contraste suficiente? 3. Espaçamento consistente?
4. Estados interativos diferenciados? 5. Focus visível? 6. Cores semânticas?
7. Tipografia legível mobile? 8. Ícones consistentes? 9. Não overdesigned?
10. Personalidade própria (não genérico)?
Liste problemas e sugestões.
```

**Contexto a fornecer:** Fluxos do Usuário + referências coletadas + direção escolhida.
**Resultado esperado:** Direção Visual completa + Referências documentadas (mínimo 5-8 referências).
**Como verificar:** Paleta passa contraste? Tokens cobrem todos componentes dos fluxos? Direção é diferente de "padrão Tailwind/Shadcn"?

## Checklist de execução

- [ ] Mínimo 5-8 referências reais coletadas e registradas
- [ ] Cada referência tem: link, screenshot, o que funciona, o que não, ideias adaptáveis, relação com projeto
- [ ] Direção estética definida em 1 frase + 3 adjetivos
- [ ] Paleta completa: primária, neutras, semânticas — com contraste testado
- [ ] Tipografia: fonte, escala, pesos, line-height, fallback
- [ ] Espaçamento: escala base 4px, tokens nomeados
- [ ] Formas/bordas/sombras: tabela por tipo de componente
- [ ] Ícones: biblioteca, estilo, tamanhos, cor
- [ ] Ilustrações/imagens: estilo, fontes, licenças verificadas
- [ ] Estados interativos: tabela completa (default, hover, focus, active, disabled, loading)
- [ ] Design tokens prontos para implementação (Tailwind config ou JSON)
- [ ] Verificação "não genérico" feita
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Direção Visual completa e aprovada por você
- [ ] Referências Visuais com mínimo 5 entradas de qualidade
- [ ] Tokens prontos para usar no código (Tailwind config)
- [ ] Contraste WCAG AA validado em todos os pares texto/BG
- [ ] Direção tem personalidade — não parece "template padrão"
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste do contraste:** WebAIM Contrast Checker em todos os pares semânticos
- **Teste da escala:** Tamanhos tipográficos legíveis em 360px mobile?
- **Teste dos estados:** Focus visível em TODOS elementos interativos?
- **Teste da personalidade:** Alguém olha e diz "isso é [seu produto]" não "template X"?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Copiar referência sem adaptar | Sempre registre: "ideia adaptada para nosso contexto: X" |
| Paleta só com cor primária | Neutras e semânticas SÃO o sistema — defina todas |
| Ignorar estados focus/hover/disabled | Checklist obrigatório por componente |
| Usar imagens IA sem marcação | Placeholder = cinza neutro + label "placeholder" |
| Muitas fontes/pesos | Máx 2 fontes (1 principal + 1 mono se código), 3-4 pesos |
| Border-radius/sombra inconsistentes | Tabela única por componente — siga à risca |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Não consegue escolher direção | Faça 3 opções rápidas no Figma/Excalidraw, compare lado a lado, escolha. |
| Contraste falha | Ajuste tom (mais escuro/claro) ou mude cor. Não abra mão de AA. |
| Referências não ajudam | Busque referências de PRODUTOS REAIS que você USA, não Dribbble conceitual. |
| Tokens não cobrem fluxo novo | Adicione token, documente no ADR visual. Não crie valores "solto" no código. |

## Condições para avançar

- ✅ Direção Visual + Referências salvas e completas
- ✅ Tokens prontos para implementação
- ✅ Contraste validado
- ✅ Personalidade clara (não genérico)
- ✅ Você consegue descrever o visual em 3 adjetivos

## Próxima etapa

[[ETAPA 08 - Estruturar a Interface (UI)]]

## Materiais relacionados

- [[02 - Modelos/UX e UI/Direção Visual]]
- [[02 - Modelos/UX e UI/Referências Visuais]]
- [[02 - Modelos/UX e UI/Registro de Componentes e Padrões]]
- [[03 - Guias Práticos/UI e UX/Pesquisa de Referências Visuais]]
- [[03 - Guias Práticos/UI e UX/Design Tokens e Consistência]]
- [[04 - Referência/Glossário/Design System]]
- [[04 - Referência/Glossário/Design Tokens]]
- [[04 - Referência/Glossário/WCAG]]