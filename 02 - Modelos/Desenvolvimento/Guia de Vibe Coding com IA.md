# Guia de Vibe Coding com IA

> **Copie para:** `05 - Projetos/SeuProjeto/05 - Preparação e Desenvolvimento/Guia de Vibe Coding com IA.md`
> Baseado na ETAPA 09-10. **Como desenvolver com IA (Claude Code) de forma eficiente, segura e com qualidade.**

---

## O que é Vibe Coding?

> **Vibe Coding** = Desenvolvimento assistido por IA onde você **define a intenção** (o "quê" e "porquê") e a IA **executa a implementação** (o "como"), com você validando e iterando.

**Princípios:**
1. **Você é o arquiteto**, IA é o construtor
2. **Contexto é rei** - dê à IA o contexto completo (PRD, arquitetura, padrões)
3. **Valide sempre** - não confie cegamente; teste, revise, approve
4. **Itere pequeno** - tarefas granulares, feedback rápido
5. **Mantenha o controle** - decisões arquiteturais são suas

---

## Setup do Ambiente (Claude Code)

### 1. Configuração Inicial do Projeto
```bash
# No diretório do projeto
claude-code init

# Cria CLAUDE.md local com instruções do projeto
# Inclui: stack, padrões, comandos, convenções
```

### 2. CLAUDE.md Essencial (Template)
```markdown
# CLAUDE.md - PedidosSimples

## Stack
- Next.js 14 (App Router) + TypeScript strict
- Prisma + Neon (PostgreSQL)
- NextAuth v5 (Magic Link + Credentials)
- Tailwind + shadcn/ui (Radix)
- Vercel deploy

## Padrões Obrigatórios
- Server Components por padrão, Client Components só onde necessário
- Server Actions para mutações (não API Routes para CRUD)
- Zod validation em todas inputs
- Prisma middleware para soft delete, timestamps
- Design tokens CSS variables (não hardcoded values)
- Testes: Vitest (unit/integration) + Playwright (E2E)

## Comandos
- `npm run dev` - dev server
- `npm run test` - vitest
- `npm run test:e2e` - playwright
- `npm run lint` - eslint + prettier
- `npm run typecheck` - tsc --noEmit
- `npm run db:push` - prisma db push
- `npm run db:migrate` - prisma migrate dev
- `npm run db:studio` - prisma studio

## Convenções
- Arquivos: kebab-case (pedido-card.tsx)
- Componentes: PascalCase (PedidoCard)
- Hooks: usePascalCase (useSSE)
- Server Actions: verbo + substantivo (createPedido, updateStatus)
- Branches: feat/, fix/, chore/ + short-description
- Commits: convencional (feat: add pedido list)

## O que NÃO fazer
- Não use `any` em TypeScript
- Não crie API Routes para CRUD (use Server Actions)
- Não hardcode cores/spacing (use tokens)
- Não pule validação Zod
- Não commit sem lint + typecheck + test pass
```

---

## Fluxo de Trabalho Recomendado

### 1. Planejamento (Você + IA)
```
Você: "Quero implementar a lista de pedidos com filtros por status e data"
IA: [Analisa PRD, Requisitos, Wireframes, Componentes existentes]
IA: [Propõe plano: Server Component + FilterChips + Pagination + SSE integration]
Você: [Aprova ou ajusta]
```

### 2. Implementação (IA executa, você supervisiona)
```
IA: [Cria/atualiza arquivos]
IA: [Roda testes, lint, typecheck]
IA: [Reporta resultado]
Você: [Revisa diff, testa manual se necessário, aprova commit]
```

### 3. Validação (Você + IA)
```
Você: "Testa o fluxo completo: login → dashboard → filtro → detalhe → status"
IA: [Roda Playwright E2E, reporta screenshots/vídeos]
Você: [Valida em preview deployment]
```

---

## Padrões de Prompt Eficazes

### ❌ Ruim (Vago, sem contexto)
> "Cria a página de pedidos"

### ✅ Bom (Específico, com contexto)
> "Implementa `/dashboard` (Server Component) que:
> - Lista pedidos paginada (20/page) ordenada por createdAt desc
> - Filtros: FilterChips status (NOVO, PREPARANDO, PRONTO) + DatePicker (Hoje, Semana, Mês)
> - Usa PedidoCard component (já existe em components/organisms)
> - Integra SSE: ao receber 'novo_pedido', incrementa badge e adiciona no topo (otimistic)
> - Acessibilidade: aria-live no badge, focus management
> - Testes: unit (filtros), e2e (lista carrega, filtra, SSE)
> - Segue Design System tokens e componentes base"

### 🎯 Prompt Template para Nova Feature
```
## Contexto
- Feature: [nome]
- Requisitos: [link RFs/RNFs]
- Wireframe: [link/descrição]
- Componentes existentes: [lista]

## Implementação
1. [Arquivo 1]: [o que faz]
2. [Arquivo 2]: [o que faz]
...

## Validação
- [ ] Lint + Typecheck
- [ ] Testes unitários: [quais]
- [ ] Testes E2E: [cenários]
- [ ] Acessibilidade: [checklist]
- [ ] Performance: [métricas]

## Entregável
- Diff dos arquivos
- Test results
- Preview URL (se deployado)
```

---

## Tarefas Ideais para Delegar à IA

### ✅ Delegue (IA faz bem)
- **Boilerplate:** Components base, types, schemas, configs
- **CRUD repetitivo:** Server Actions, Prisma queries, Zod schemas
- **Testes:** Unit (vitest), E2E (Playwright) - dado cenários claros
- **Refatoração:** Renomear, extrair componente, mover arquivos
- **Documentação:** JSDoc, README, ADRs, comentários
- **Bug fixes localizados:** Com erro claro e reprodução
- **Acessibilidade:** Adicionar ARIA, focus management, contrast fixes
- **Tailwind styling:** Converter design tokens para classes

### ⚠️ Supervisione de Perto (IA pode errar)
- **Lógica de negócio complexa:** State machines, cálculos financeiros, parsing
- **Segurança:** Auth, rate limit, HMAC, CSP, LGPD
- **Arquitetura:** Novos padrões, integrações externas, decisões irreversíveis
- **Performance:** Queries otimizadas, bundle analysis, caching
- **Migrações DB:** Schema changes, data migration

### ❌ Não Delegue (Sua responsabilidade)
- **Decisões de produto:** Escopo, priorização, UX trade-offs
- **Aprovação final:** Code review, merge, deploy production
- **Debugging misterioso:** Quando IA não consegue reproduzir/entender
- **Revisão de segurança:** Auditoria final antes de produção

---

## Dicas de Ouro para Vibe Coding

### 1. Dê Contexto Completo
```markdown
# Antes de pedir, compartilhe:
- @01-Manual/ETAPA-04.md (requisitos)
- @02-Modelos/Requisitos-Funcionais.md (RFs)
- @02-Modelos/Wireframes.md (UI)
- @components/organisms/pedido-card.tsx (componente existente)
- @prisma/schema.prisma (modelo de dados)
```

### 2. Use Referências de Arquivo (@)
```
"Baseia o novo componente em @components/ui/button.tsx e @components/molecules/form-field.tsx"
```

### 3. Peça Para Explicar Antes de Fazer
```
"Antes de implementar, me explica sua abordagem para o parser de pedidos WhatsApp"
```

### 4. Valide Incrementalmente
```
"Implementa só o Server Component da lista. Para aí e me mostra o diff."
```

### 5. Use Subagents para Paralelismo
```
# Terminal 1 (main): Planejamento + Revisão
# Terminal 2 (subagent): Testes E2E
# Terminal 3 (subagent): Documentação
```

### 6. Commit Frequente, Mensagens Claras
```bash
git add -A
git commit -m "feat(dashboard): add pedido list with status filters

- Server Component with pagination (20/page)
- FilterChips for status + date range
- SSE integration for real-time updates
- Tests: unit filters, e2e list load

Closes #42"
```

---

## Checklist de Qualidade por PR (IA + Você)

### Automático (CI)
- [ ] `npm run lint` - 0 errors, 0 warnings
- [ ] `npm run typecheck` - 0 errors
- [ ] `npm run test` - 100% pass, coverage ≥ 80%
- [ ] `npm run test:e2e` - core flows pass
- [ ] `npm run build` - success
- [ ] Preview deployment - success

### Manual (Você revisa)
- [ ] **Arquitetura:** Segue padrões do projeto? (Server Components, Actions, tokens)
- [ ] **Requisitos:** Atende RFs/RNFs vinculados?
- [ ] **UX:** Match wireframes? Estados loading/empty/error?
- [ ] **Acessibilidade:** axe 0 critical/serious, teclado, screen reader
- [ ] **Segurança:** Validação Zod, auth check, rate limit, sem segredos
- [ ] **Performance:** Queries otimizadas, sem N+1, bundle size OK
- [ ] **Testes:** Cenários cobrem happy path + edge cases + erros
- [ ] **Documentação:** JSDoc em funções públicas, README atualizado se necessário

---

## Armadilhas Comuns e Como Evitar

| Armadilha | Sintoma | Prevenção |
|-----------|---------|-----------|
| **Alucinação de API** | IA inventa métodos que não existem | "Verifica se `prisma.pedido.findMany` aceita esse parâmetro" |
| **Contexto desatualizado** | IA usa padrão antigo (ex: Pages Router) | Sempre referencie arquivos atuais com `@` |
| **Over-engineering** | IA cria abstração desnecessária | "Mantém simples, YAGNI, segue padrão existente" |
| **Ignorar tokens** | Hardcoded `bg-blue-500` em vez de `bg-primary` | "Usa apenas design tokens do Design System" |
| **Pular validação** | Input sem Zod schema | "Todo input externo tem schema Zod" |
| **Testes frágeis** | Testa implementação, não comportamento | "Testa o que o usuário vê/faz, não detalhes internos" |
| **Scope creep** | IA adiciona "features legais" não pedidas | "Implementa SÓ o que está no prompt" |

---

## Prompt de Emergência (Quando Trava)

```
"Estou travado em [problema]. 
Contexto: [arquivos relevantes @]
Erro: [mensagem exata / comportamento]
Já tentei: [o que fez]
Preciso: [solução / direção / debug help]
Por favor: explica o raciocínio, não só o código."
```

---

## Métricas de Produtividade (Acompanhe)

| Métrica | Target | Como Medir |
|---------|--------|------------|
| **Tempo setup → primeiro feature** | < 2h | Cronômetro |
| **PRs por semana** | 5-10 | GitHub |
- **Cycle time (PR aberto → merge)** | < 4h | GitHub |
| **Rework rate (commits fix no mesmo PR)** | < 20% | GitHub |
| **Bugs em produção / sprint** | 0 | Sentry/Logs |
| **Cobertura testes** | ≥ 80% | CI |
| **Lighthouse CI score** | Perf ≥ 90, A11y ≥ 95 | CI |

---

## Exemplo Real: Implementando `updateStatus`

### Prompt
```
Implementa Server Action `updateStatus(pedidoId: string, status: PedidoStatus)`:

Contexto:
- @02-Modelos/Regras-de-Negocio.md (RN-002, RN-003 transições)
- @prisma/schema.prisma (Pedido, PedidoStatusHistorico)
- @src/actions/pedidos.ts (outras actions)
- @src/lib/validators.ts (Zod schemas)

Requisitos:
1. Valida transição permitida (state machine)
2. Transaction: update Pedido.status + insert PedidoStatusHistorico
3. Timestamps: preparandoEm/prontoEm/entregueEm/canceladoEm
4. Broadcast SSE `status_atualizado` para loja
5. Retorna { success: true, pedido: Pedido } ou { error: string }
6. Zod validation input
7. Auth: requireRole(OWNER|FUNCIONARIO)

Testes:
- Unit: transições válidas/inválidas, timestamps
- Integration: action + DB + SSE broadcast
- E2E: fluxo novo → preparando → pronto → entregue
```

### Resultado Esperado (IA entrega)
- Diff: `src/actions/pedidos.ts` + `src/lib/validators.ts` + testes
- Test results passando
- Preview URL para validação manual

---

## Próxima Etapa
[[02 - Modelos/Desenvolvimento/Checklist de Código]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/05 - Preparação e Desenvolvimento/ETAPA 09 - Preparar o Ambiente e o Plano de Implementação]]
- [[01 - Manual de Desenvolvimento/05 - Preparação e Desenvolvimento/ETAPA 10 - Desenvolver o Software por Partes]]
- [[02 - Modelos/Desenvolvimento/Plano de Implementação]]
- [[02 - Modelos/Desenvolvimento/Checklist de Código]]
- [[04 - Referência/Glossário/Vibe Coding]]
- [[04 - Referência/Glossário/Server Actions]]
- [[04 - Referência/Glossário/Server Components]]