# ETAPA 11 — Executar Testes e Garantir a Qualidade

## Objetivo

Verificar o **comportamento real do software** através de testes proporcionais ao projeto: unitários, integração, ponta a ponta, manuais, acessibilidade, responsividade, segurança, desempenho e regressão.

## Por que esta etapa existe?

Testes não são "algo a mais" — são a **evidência** de que o software faz o que deve. Sem testes executados e documentados, você não sabe se funciona, só acha que funciona.

## Quando começar

- ETAPA 10 concluída (Todas as tasks implementadas, build passando, Acompanhamento atualizado)
- Código "feature complete" para o MVP

## O que você precisa fazer

1. **Revise o Plano de Testes** — defina quais tipos de teste se aplicam (proporcionalidade)
2. **Planeje casos de teste** — para cada requisito funcional (CA da ETAPA 04)
3. **Defina resultados esperados** — pass/fail claros, mensuráveis
4. **Execute testes automatizados** — unit (Vitest), integration (Vitest + test DB), e2e (Playwright)
5. **Execute testes manuais** — fluxos completos, edge cases, validação visual
6. **Execute testes de acessibilidade** — axe-core, navegação teclado, contraste, screen reader
7. **Execute testes de responsividade** — breakpoints definidos na ETAPA 08
8. **Execute testes de segurança básicos** — validação entrada, auth, headers, dependências
9. **Execute testes de desempenho** — Lighthouse, Core Web Vitals, tempo de resposta API
10. **Registre falhas (bugs)** — descrição, reprodução, severidade, prioridade
11. **Corrija e repita testes** — até critérios de qualidade atendidos
12. **Verifique regressões** — suite completa após cada correção
13. **Registre limitações conhecidas** — o que NÃO foi testado e por quê
14. **Produza os documentos: Plano de Testes + Registro de Casos de Teste + Registro de Execução + Registro de Bugs**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Definir estratégia de testes (quais tipos) | 👤 USUÁRIO (com sugestão da IA) |
| Planejar casos de teste (derivar de CA) | 🤖 IA (você valida cobertura) |
| Escrever/atualizar testes automatizados | 🤖 IA (você revisa) |
| Executar suite automatizada | ✅ VALIDAÇÃO (você roda comandos) |
| Testes manuais (fluxos, visual, edge cases) | 👤 USUÁRIO |
| Testes de acessibilidade | 🤖 IA (axe-core) + 👤 USUÁRIO (teclado, screen reader) |
| Testes de responsividade | 👤 USUÁRIO (DevTools device toolbar) |
| Testes de segurança básicos | 🤖 IA (audit, headers) + 👤 USUÁRIO (validação) |
| Testes de desempenho | 🤖 IA (Lighthouse CI) + 👤 USUÁRIO (análise) |
| Registrar bugs | 👤 USUÁRIO |
| Corrigir bugs (iterar com IA) | 🤖 IA + 👤 USUÁRIO |
| Validar correções + regressão | ✅ VALIDAÇÃO |
| Decidir aceitação de limitações | 👤 USUÁRIO |

## Ferramentas recomendadas

| Tipo de Teste | Ferramentas |
|---------------|-------------|
| Unit / Integration | Vitest, Testing Library (React), MSW (mock API), Prisma test utils |
| E2E | Playwright (Chromium, Firefox, WebKit), Playwright UI para debug |
| Acessibilidade | axe-core (automatizado), WAVE, NVDA/VoiceOver (manual), Stark |
| Responsividade | Chrome DevTools Device Toolbar, Playwright viewports |
| Segurança | `pnpm audit`, Snyk, OWASP ZAP (básico), headers check |
| Desempenho | Lighthouse CI, WebPageTest, Vercel Analytics, `console.time` API |
| Gestão de Bugs | GitHub Issues, Linear, ou Markdown no projeto (`06 - Testes/`) |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Plano de Testes | `02 - Modelos/Testes/Plano de Testes.md` | `05 - Projetos/SeuProjeto/06 - Testes/` |
| Registro de Casos de Teste | `02 - Modelos/Testes/Registro de Casos de Teste.md` | `05 - Projetos/SeuProjeto/06 - Testes/` |
| Registro de Execução de Testes | `02 - Modelos/Testes/Registro de Execução de Testes.md` | `05 - Projetos/SeuProjeto/06 - Testes/` |
| Registro de Bugs | `02 - Modelos/Testes/Registro de Bugs.md` | `05 - Projetos/SeuProjeto/06 - Testes/` |

### Condicionais
- Relatório de Acessibilidade (se público externo/produção)
- Relatório de Desempenho (se requisitos de performance rígidos)
- Relatório de Segurança (se dados sensíveis/pagamentos)

### Opcionais
- Matriz de Rastreabilidade Requisito-Teste
- Relatório de Cobertura (HTML do Vitest/Playwright)

## Como produzir os documentos

### Plano de Testes

```markdown
# Plano de Testes — PedidosSimples v1

## Estratégia Geral
**Pirâmide de Testes Proporcional ao MVP:**
- Unitários: 60% (lógica pura, validações, utils, hooks)
- Integração: 30% (API routes, DB queries, Server Actions)
- E2E: 10% (fluxos críticos: login → pedido → status → resposta)
- Manuais: Fluxos completos, visual, acessibilidade, responsividade

## Critérios de Seleção de Testes

| Tipo | Quando Obrigatório | Quando Condicional | Quando Opcional |
|------|-------------------|-------------------|-----------------|
| Unitário | Sempre (lógica de negócio, validações, utils) | - | - |
| Integração | APIs, DB mutations, auth, webhooks | Lógicas complexas multi-step | Queries simples read-only |
| E2E | Fluxos core (Must da ETAPA 03) | Fluxos Should/Could | Fluxos Won't, admin-only |
| Manual | Todos os fluxos Must/Should | Validação visual, estados | Exploratório |
| Acessibilidade | Produção pública | WCAG AA obrigatório | AAA (nice to have) |
| Responsividade | Sempre (mobile-first) | Breakpoints complexos | - |
| Segurança | Auth, dados sensíveis, pagamentos | APIs públicas | - |
| Desempenho | Requisitos RQ de performance | Core Web Vitals | Otimização prematura |
| Regressão | Sempre após correção | - | - |

## Escopo de Testes por Feature (MVP)

| Feature (Must/Should) | Unit | Integration | E2E | Manual | A11y | Resp | Seg | Perf |
|----------------------|------|-------------|-----|--------|------|------|-----|------|
| Auth (Login/Magic Link) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | - |
| Webhook WhatsApp | ✅ | ✅ | - | ✅ | - | - | ✅ | ✅ |
| Lista Pedidos (Dashboard) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | - | ✅ |
| Atualizar Status | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | - | - |
| Responder Cliente | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | - |
| Configurações (CRUD) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | - | - |
| Onboarding | - | ✅ | ✅ | ✅ | ✅ | ✅ | - | - |
| Tempo Real (SSE) | - | ✅ | ✅ | ✅ | - | - | - | ✅ |

## Critérios de Qualidade (Quality Gates)

| Métrica | Mínimo para MVP | Ideal |
|---------|-----------------|-------|
| Cobertura Unit/Integration (linhas) | > 70% | > 85% |
| Cobertura Branches | > 60% | > 80% |
| Testes E2E Fluxos Críticos | 100% (Must) | 100% (Must+Should) |
| Bugs Críticos (P0) | 0 | 0 |
| Bugs Altos (P1) | ≤ 2 | 0 |
| Bugs Médios (P2) | ≤ 5 | ≤ 2 |
| Acessibilidade (axe-core) | 0 violations | 0 violations |
| Lighthouse Performance | > 80 | > 90 |
| Lighthouse Acessibilidade | > 90 | 100 |
| `pnpm audit` vulnerabilities | 0 critical/high | 0 |

## Cronograma de Execução

| Fase | Atividade | Responsável | Critério de Parada |
|------|-----------|-------------|-------------------|
| 1 | Suite automatizada completa (unit + int + e2e) | VALIDAÇÃO | 100% passing |
| 2 | Testes manuais fluxos Must | USUÁRIO | Todos fluxos Must OK |
| 3 | Testes manuais fluxos Should | USUÁRIO | Todos fluxos Should OK |
| 4 | Acessibilidade (automatizado + teclado) | VALIDAÇÃO + USUÁRIO | 0 violations + navegação OK |
| 5 | Responsividade (breakpoints ETAPA 08) | USUÁRIO | Todas telas OK nos 3 breakpoints |
| 6 | Segurança básica (audit + headers + auth) | VALIDAÇÃO | 0 critical/high, auth OK |
| 7 | Desempenho (Lighthouse páginas principais) | VALIDAÇÃO | Métricas acima do mínimo |
| 8 | Correção de bugs + regressão completa | IA + USUÁRIO + VALIDAÇÃO | Quality Gates atendidos |
| 9 | Registro de limitações conhecidas | USUÁRIO | Documentado |

## Definição de Estados de Teste

| Estado | Significado | Quando Usar |
|--------|-------------|-------------|
| **Aprovado** | Teste executado, resultado = esperado | Padrão para testes que passam |
| **Reprovado** | Teste executado, resultado ≠ esperado | Bug encontrado |
| **Não Executado** | Teste planejado mas não rodou | Falta de tempo, bloqueio, dependência |
| **Bloqueado** | Não pode executar por impedimento | Ambiente, dado, dependência externa |
| **Não Aplicável** | Tipo de teste não se aplica à feature | Ex: E2E para validação pura de schema |

> [!important]
> **Não aceite "Aprovado" sem evidência de execução.** Print do terminal, screenshot do Playwright, log do Lighthouse.

## Como trabalhar com IA nesta etapa

**Prompt para gerar casos de teste a partir de CA:**
```
Tenho estes Critérios de Aceitação (Gherkin) da ETAPA 04: [cole CAs].
Gere casos de teste estruturados para:
1. Unitários (lógica pura, validações, utils)
2. Integração (API routes, DB, Server Actions)
3. E2E (Playwright - fluxos críticos)
Para cada caso: ID, Tipo, Descrição, Pré-condição, Passos, Resultado Esperado, Dados de Teste, Prioridade (P0/P1/P2).
Formato: Tabela Markdown pronta para Registro de Casos de Teste.
```

**Prompt para gerar testes Vitest (unit/integration):**
```
Implemente testes Vitest para: [descreva função/API/hook].
Contexto: [cole código da implementação ou descreva].
Padrões do projeto: [cole de .claude/context/testing.md].
Casos a cobrir: [liste do Registro de Casos de Teste].
Requisitos: TypeScript strict, Testing Library, MSW para mocks externos, Prisma test utils.
Gere arquivo de teste completo.
```

**Prompt para gerar testes Playwright (E2E):**
```
Impleva teste Playwright para fluxo: [descreva fluxo crítico].
Seletores: use `data-testid` (padrão do projeto).
Padrões: [cole de .claude/context/testing.md].
Fluxo: login → [passos] → assertion final.
Gere spec completo com setup/teardown se needed.
```

**Prompt para revisar cobertura e gaps:**
```
Analise a cobertura atual (gere relatório: `pnpm test --coverage`).
Compare com Plano de Testes e Critérios de Aceitação.
Identifique:
1. Gaps de cobertura (arquivos/linhas/branches não testados)
2. CAs sem teste correspondente
3. Testes duplicados ou inúteis
4. Sugestões de testes prioritários para adicionar
Formato: Relatório acionável.
```

**Contexto a fornecer:** CAs (ETAPA 04) + Implementação real (código) + Plano de Testes + Padrões de teste do projeto.
**Resultado esperado:** 4 documentos de teste completos + Quality Gates atendidos (ou limitações registradas).
**Como verificar:** Rode `pnpm test`, `pnpm test:e2e`, `pnpm test --coverage`, Lighthouse, axe-core — todos passam nos gates?

## Checklist de execução

- [ ] Plano de Testes revisado e aprovado (tipos proporcionais ao MVP)
- [ ] Casos de teste derivados de TODOS os CAs (Must/Should)
- [ ] Testes unitários/integration escritos/atualizados e passando
- [ ] Testes E2E para fluxos críticos escritos/atualizados e passando
- [ ] Suite automatizada completa roda limpa (`pnpm test` + `pnpm test:e2e`)
- [ ] Cobertura atende mínimos (>70% linhas, >60% branches)
- [ ] Testes manuais fluxos Must executados e aprovados
- [ ] Testes manuais fluxos Should executados e aprovados
- [ ] Acessibilidade: axe-core 0 violations + navegação teclado OK + contraste OK
- [ ] Responsividade: todas telas testadas nos 3 breakpoints (mobile/tablet/desktop)
- [ ] Segurança: `pnpm audit` 0 critical/high, headers de segurança, auth testado
- [ ] Desempenho: Lighthouse páginas principais > 80 performance, > 90 a11y
- [ ] Bugs registrados com: descrição, reprodução, severidade, prioridade
- [ ] Bugs P0/P1 corrigidos e regressão verificada
- [ ] Limitações conhecidas documentadas (o que não testou + por quê)
- [ ] 4 documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Plano de Testes completo
- [ ] Registro de Casos de Teste completo (rastreável aos CAs)
- [ ] Registro de Execução com evidências (logs, screenshots, relatórios)
- [ ] Registro de Bugs com status final (corrigido/aceito/adiado)
- [ ] Quality Gates atendidos (ou exceções justificadas e registradas)
- [ ] Suite completa roda em < 10 min (CI/CD viável)
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste da rastreabilidade:** Cada CA (Must/Should) tem pelo menos 1 caso de teste?
- **Teste da evidência:** Para cada "Aprovado" no Registro de Execução, tem log/screenshot?
- **Teste da regressão:** Após corrigir bug, suite completa roda sem novas falhas?
- **Teste do CI:** Pipeline roda testes e falha se Quality Gates não atendidos?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Testar só "caminho feliz" | Exija: casos de erro, borda, vazio, permissão para cada feature |
| Cobertura alta mas testes fracos | Teste comportamento, não implementação. Assertions significativas. |
| E2E flaky não corrigido | Flaky = bug no teste OU na implementação. Corrija, não ignore. |
| "Testei manualmente" sem registro | Registre: o que testou, como, resultado, data. Print/screenshot. |
| Acessibilidade só automatizada | axe-core pega ~30-50%. Teste teclado + screen reader OBRIGATÓRIO. |
| Performance só no final | Meça cedo (Lighthouse CI no PR). Não otimize prematuramente. |
| Bugs P2 ignorados sem registro | Registre TODOS. Decida: corrigir agora / v2 / não corrigir (com justificativa). |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Muitos bugs P1/P2 | Priorize: P0 → P1 → P2. Pare features novas. Foque estabilização. |
| Cobertura não atinge mínimo | Adicione testes nos gaps identificados. Não abaixe a meta. |
| E2E lentos demais (>10 min) | Paralelize (Playwright shards). Otimize setup. Mock APIs externas. |
| Acessibilidade falha em componente base | Corrija no componente base (atoms) → propaga para todos. |
| Desempenho abaixo do mínimo | Profile (Lighthouse, React DevTools). Otimize: images, bundles, queries, caching. |
| Limitação conhecida vira bug em prod | Se risco alto, teste antes do deploy. Se risco baixo, documente e monitore. |

## Condições para avançar

- ✅ Quality Gates atendidos (ou exceções documentadas e aceitas por você)
- ✅ 4 documentos de teste salvos e completos
- ✅ Suite automatizada passa consistentemente
- ✅ Bugs críticos corrigidos
- ✅ Limitações conhecidas registradas

## Próxima etapa

[[ETAPA 12 - Revisar Segurança, Privacidade e Confiabilidade]]

## Materiais relacionados

- [[02 - Modelos/Testes/Plano de Testes]]
- [[02 - Modelos/Testes/Registro de Casos de Teste]]
- [[02 - Modelos/Testes/Registro de Execução de Testes]]
- [[02 - Modelos/Testes/Registro de Bugs]]
- [[03 - Guias Práticos/Testes/Como Testar um Software]]
- [[03 - Guias Práticos/Testes/Procedimento de Investigação de Bugs]]
- [[03 - Guias Práticos/Segurança/Checklist de Segurança Básica]]
- [[04 - Referência/Glossário/Teste Unitário]]
- [[04 - Referência/Glossário/Teste de Integração]]
- [[04 - Referência/Glossário/Teste de Ponta a Ponta]]
- [[04 - Referência/Glossário/Cobertura de Código]]
- [[04 - Referência/Glossário/Quality Gates]]