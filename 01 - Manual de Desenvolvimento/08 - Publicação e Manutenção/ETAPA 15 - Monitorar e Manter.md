# ETAPA 15 — Monitorar e Manter

## Objetivo

Manter o software **funcionando, seguro e evoluindo** após a publicação: monitoramento, investigação de problemas, atualizações, backups, correções e planejamento de melhorias.

## Por que esta etapa existe?

Software não "termina" no deploy. Produção é onde o valor acontece — e onde os problemas reais aparecem. Manutenção estruturada evita incêndios, dívida técnica acumulada e abandono do produto.

## Quando começar

- ETAPA 14 concluída: **Produção Estável** (24h+ sem incidentes críticos)
- Software em uso por usuários reais (mesmo que poucos)
- Você assume papel de "mantenedor" contínuo

## O que você precisa fazer

1. **Configure monitoramento contínuo** — erros, uptime, performance, negócio
2. **Estabeleça rotinas de verificação** — diária, semanal, mensal
3. **Investigue problemas relatados** — triagem, reprodução, correção, validação
4. **Gerencie atualizações de dependências** — `pnpm audit`, Dependabot, breaking changes
5. **Mantenha backups e teste restore** — frequência, retenção, validação
6. **Planeje e execute correções** — bugs, melhorias pequenas, debt técnico
7. **Controle mudanças** — versionamento, changelog, comunicação
8. **Priorize: Incidente > Manutenção > Nova Feature**
9. **Registre versões e histórico** — semver, changelog, decisões
10. **Revise riscos periodicamente** — segurança, negócio, técnico
11. **Planeje melhorias (Roadmap)** — feedback usuários, métricas, visão
12. **Produza os documentos: Plano de Manutenção + Registro de Incidentes + Changelog + Roadmap**

## Quem faz o quê

| Atividade | Responsável | Frequência |
|-----------|-------------|------------|
| Verificar dashboards (Sentry, Uptime, Vercel) | 👤 USUÁRIO | Diária (5 min) |
| Revisar alertas disparados | 👤 USUÁRIO | Conforme alerta (imediato P0) |
| `pnpm audit` + Dependabot PRs | 🤖 IA (automatizado) + 👤 USUÁRIO (revisa) | Semanal |
| Backup restore test | 👤 USUÁRIO | Mensal |
| Investigação de bugs relatados | 👤 USUÁRIO + 🤖 IA | Conforme demanda |
| Correções (hotfix / patch) | 🤖 IA + 👤 USUÁRIO | Conforme prioridade |
| Atualização de dependências | 🤖 IA (PRs) + 👤 USUÁRIO (aprova) | Semanal/Quinzenal |
| Revisão de riscos/segurança | 👤 USUÁRIO | Mensal |
| Planejamento roadmap | 👤 USUÁRIO | Mensal/Trimestral |
| Registro de versão (patch/minor) | 👤 USUÁRIO | Conforme release |

## Ferramentas recomendadas

| Finalidade | Ferramentas |
|------------|-------------|
| Error Tracking | Sentry (alertas, dashboards, release tracking) |
| Uptime | UptimeRobot, Better Uptime, Pingdom |
| Performance | Vercel Analytics, Web Vitals, Lighthouse CI |
| Logs | Vercel Logs, Sentry, `pino` local |
| Dependências | `pnpm audit`, GitHub Dependabot, Snyk, `npm outdated` |
| Banco | Neon Console (PITR, branching, metrics) |
| Incident Management | GitHub Issues (labels: incident, bug, maintenance), Linear |
| Versionamento | Semantic Versioning, `changesets` ou manual, CHANGELOG |
| Comunicação | Status page (se usuários externos), email, in-app banner |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Plano de Manutenção | `02 - Modelos/Publicação e Manutenção/Plano de Manutenção.md` | `05 - Projetos/SeuProjeto/09 - Encerramento/` |
| Registro de Incidentes | `02 - Modelos/Publicação e Manutenção/Registro de Incidentes.md` | `05 - Projetos/SeuProjeto/09 - Encerramento/` |
| CHANGELOG | `CHANGELOG.md` (raiz do projeto) | Raiz do projeto (`05 - Projetos/SeuProjeto/`) |

### Condicionais
- Runbook de Operações (se múltiplos mantenedores)
- Relatório de Pós-Mortem (se incidente grave)

### Opcionais
- Roadmap Detalhado (se planejamento ativo)
- Relatório Mensal de Saúde do Sistema

## Como produzir os documentos

### Plano de Manutenção

```markdown
# Plano de Manutenção — PedidosSimples

## Visão Geral
**Produto:** PedidosSimples v1.x
**Ambiente:** Produção (Vercel + Neon)
**Mantenedor:** [Seu Nome]
**Início Vigência:** 2026-10-18 (pós-estabilização v1.0.0)
**Revisão:** Mensal (dia 1 de cada mês)

---

## Rotinas de Monitoramento

### Diária (5-10 min, manhã)
- [ ] Sentry: Novos erros (última 24h) → triagem
- [ ] UptimeRobot: Uptime 24h = 100%?
- [ ] Vercel Analytics: P95 latency, error rate, traffic
- [ ] WhatsApp Webhook: Logs de falha (Sentry filter `webhook`)
- [ ] Auth: Taxa de sucesso login (NextAuth + Sentry)

### Semanal (30 min, segunda-feira)
- [ ] `pnpm audit --prod` → 0 critical/high
- [ ] Dependabot PRs → revisar + merge (testes passam)
- [ ] `npm outdated` → avaliar updates major
- [ ] Sentry: Trends (erros recorrentes, novos padrões)
- [ ] Neon: Storage usage, connection pool, query performance
- [ ] Vercel: Function duration, bandwidth, edge cache hit rate
- [ ] Backup restore test (se mensal não feito)

### Mensal (1-2h, dia 1)
- [ ] **Teste real de restore** Neon (point-in-time → staging) → validação dados
- [ ] Revisão de riscos (ETAPA 12) → novos? mitigados?
- [ ] Revisão de limitações conhecidas (ETAPA 13) → resolveram?
- [ ] Revisão de roadmap → priorizar próximo ciclo
- [ ] Revisão de custos (Vercel, Neon, Resend, Sentry) → dentro do orçado?
- [ ] Atualização de documentação (README, CLAUDE.md, RUNBOOK)
- [ ] Rotação de segredos (se política exige) → `NEXTAUTH_SECRET`, WhatsApp token
- [ ] Verificação de conformidade LGPD (consentimentos, exclusões solicitadas)

### Trimestral (meio dia)
- [ ] Penetration test leve (OWASP ZAP scan staging)
- [ ] Revisão de arquitetura (ADRs) → ainda válidos?
- [ ] Avaliação de tech stack → migrações necessárias?
- [ ] Feedback usuários reais → sintetizar → roadmap
- [ ] Planejamento de versão minor/major

---

## Classificação e Priorização

| Tipo | Definição | SLA Resposta | SLA Resolução | Exemplo |
|------|-----------|--------------|---------------|---------|
| **Incidente P0** | Sistema indisponível / perda de dados / vazamento | **Imediato** (você para tudo) | **< 1h** | Deploy quebrado, DB down, vazamento PII |
| **Incidente P1** | Funcionalidade crítica quebrada (pedidos, auth) | **< 30 min** | **< 4h** | Webhook falha, login não funciona |
| **Incidente P2** | Funcionalidade não-crítica degradada / bug chato | **< 4h** | **< 24h** | Relatório errado, template não envia |
| **Manutenção** | Atualizações, refatoração, debt técnico, dependências | **Planejado** (semanal) | **Sprint/Release** | Update Prisma, refactor component |
| **Melhoria (Feature)** | Nova funcionalidade, UX improvement | **Roadmap** (mensal) | **Release planejado** | Relatório semanal, multi-loja |

> [!important]
> **Regra de Ouro: Incidente > Manutenção > Feature.**
> Se P0/P1 aparece, TUDO para. Só volta ao planejado após resolvido + pós-mortem.

---

## Processo de Investigação e Correção (Bug/Incidente)

```
1. DETECTAR
   ├── Alerta (Sentry, Uptime, usuário)
   └── Log manual (você vê)

2. TRIAGEM (5-15 min)
   ├── Severidade (P0/P1/P2)
   ├── Impacto (usuários afetados, receita, dados)
   ├── Reprodutibilidade
   └── Workaround temporário?

3. REGISTRAR
   ├── Issue GitHub (template: Incident/Bug)
   ├── Label: `incident` ou `bug` + `P0/P1/P2`
   ├── Assignee: você
   └── Link Sentry/Log/Deploy relacionado

4. INVESTIGAR
   ├── Reproduzir (local/staging)
   ├── Root cause (logs, traces, diff recente)
   ├── Blast radius (o mais afetado?)

5. CORRIGIR
   ├── Hotfix branch (`hotfix/issue-123`)
   ├── Mínima mudança necessária
   ├── Testes (unit + e2e smoke)
   ├── Code review (você mesmo, rigoroso)

6. VALIDAR
   ├── Staging: fluxo completo
   ├── Deploy (vercel --prod ou merge main)
   ├── Smoke tests produção
   ├── Monitorar 30 min

7. FECHAR
   ├── Issue: "Fixed in v1.0.1"
   ├── CHANGELOG atualizado
   ├── Pós-mortem se P0/P1 (template)
   ├── Atualizar runbook se novo padrão
```

---

## Gestão de Dependências

### Semanal (Automatizado + Revisão Manual)
```bash
# 1. Audit
pnpm audit --prod
# → Se critical/high: PR imediato (patch)

# 2. Dependabot PRs (config: weekly, group: minor/patch)
# → Revisar: breaking changes? testes passam? merge.

# 3. Outdated check
npm outdated --depth=0
# → Major updates: avaliar esforço vs benefício → roadmap
```

### Política de Atualização
| Tipo | Política | Exemplo |
|------|----------|---------|
| Patch (x.x.Z) | Auto-merge se testes passam | `zod 3.22.4 → 3.22.5` |
| Minor (x.Y.0) | Revisar changelog + testar → merge | `next 14.2.0 → 14.3.0` |
| Major (X.0.0) | **Nunca auto**. Avaliar: breaking changes, migração, esforço. Planejar sprint. | `next 14 → 15`, `react 18 → 19` |

---

## Versionamento e Releases

### Semantic Versioning (SemVer)
- **PATCH (1.0.X):** Bug fixes, hotfixes, dependências patch, docs
- **MINOR (1.X.0):** Novas features backward-compatible, melhorias UX
- **MAJOR (X.0.0):** Breaking changes, reescrita, migração de dados

### Processo de Release
```bash
# Patch (hotfix)
git checkout -b hotfix/fix-webhook-timeout main
# ... correção + testes ...
git commit -m "fix: webhook timeout handling"
git tag v1.0.1
git push origin v1.0.1
# → Vercel auto-deploy via tag

# Minor (feature)
git checkout -b feature/weekly-report main
# ... implementação + testes ...
git commit -m "feat: weekly report export"
git tag v1.1.0
git push origin v1.1.0
```

### CHANGELOG (Keep a Changelog format)
```markdown
# Changelog

## [1.0.1] - 2026-10-20
### Fixed
- Webhook timeout increased to 30s (was 10s) - resolves occasional WhatsApp delivery failures
- SSE reconnection backoff max reduced from 30s to 10s

## [1.1.0] - 2026-11-15
### Added
- Weekly report generation (CSV + PDF)
- Export pedidos por período
### Changed
- Dashboard: added date range picker
### Fixed
- Memory leak in SSE connection handler
```

---

## Backup e Recuperação (Operação Contínua)

| Item | Frequência | Retenção | Teste |
|------|------------|----------|-------|
| Neon PITR (Point-in-Time Recovery) | Contínuo | 7 dias | Mensal (restore staging) |
| Neon Branch Snapshot (pré-deploy) | Por deploy | 30 dias | Mensal |
| GitHub Repo (código + history) | Contínuo | Permanente | Clone test |
| Vercel Deploy History | Contínuo | 100 deploys | Rollback test |
| Segredos (1Password) | Manual | Permanente | Acesso test |

**RPO:** < 24h (Neon PITR)
**RTO:** < 1h (Vercel rollback + Neon restore)

---

## Revisão de Riscos (Mensal)

| Categoria | Perguntas-Chave | Ação se Sim |
|-----------|-----------------|-------------|
| Segurança | Novas vulnerabilidades em deps? | Atualizar/patch imediatamente |
| Segurança | Vazamento de segredo? | Rotacionar IMEDIATAMENTE |
| Dados | Solicitação exclusão LGPD? | Processar em < 15 dias |
| Negócio | Concorrente lançou feature crítica? | Avaliar roadmap |
| Técnico | Dívida técnica atrapalhando velocity? | Agendar refatoração (manutenção) |
| Custo | Custo > 2x orçado? | Otimizar (query, function, tier) |

---

## Roadmap v1.x (Exemplo)

| Versão | Foco | Features Principais | Target |
|--------|------|---------------------|--------|
| v1.0.x | Estabilidade | Hotfixes, rate limiting, observabilidade | Contínuo |
| v1.1.0 | Relatórios | Relatório semanal, export CSV/PDF, gráficos básicos | 4-6 semanas |
| v1.2.0 | Multi-loja | Múltiplos estabelecimentos por conta, switcher | 8-12 semanas |
| v1.3.0 | Integrações | iFood/Rappi webhook, API pública básica | 12+ semanas |
| v2.0.0 | Arquitetura | Migração para tRPC/GraphQL, mobile app (React Native), multi-tenant | 6+ meses |

> [!tip]
> **Roadmap é vivo.** Atualize mensal baseado em: feedback real, métricas, capacidade, mercado.
```

### Registro de Incidentes

```markdown
# Registro de Incidentes — PedidosSimples

## Template de Incidente

### INC-001: [Título Curto]
**Data/Hora Início:** 2026-10-20 14:32 BRT
**Data/Hora Fim:** 2026-10-20 15:10 BRT
**Duração:** 38 min
**Severidade:** P1
**Status:** RESOLVIDO

#### Descrição
Webhook WhatsApp falhando intermitentemente (502 Bad Gateway) → pedidos não criados → usuários não viam pedidos novos.

#### Impacto
- ~15 pedidos perdidos (não criados no sistema)
- 3 estabelecimentos afetados
- Nenhum dado vazado

#### Detecção
- Alerta Sentry: "Webhook error rate > 5%"
- Usuário reportou "pedido não apareceu"

#### Investigação
1. Logs Vercel: `Function timeout` em `/api/webhooks/whatsapp`
2. WhatsApp API latency: p99 > 8s (normal < 2s)
3. Nossa função: timeout 10s (padrão Vercel hobby)
4. Root cause: WhatsApp API instável + timeout muito baixo

#### Correção
- Aumentar timeout webhook para 30s (`export const maxDuration = 30`)
- Adicionar queue local (array em memória + retry) para picos
- Deploy hotfix v1.0.1

#### Validação
- Staging: simulado latência 15s → pedido criado ✅
- Produção: deploy 15:05 → monitoramento 30 min → 0 erros ✅
- Pedidos perdidos: reenviados manualmente via Meta Console (3) + usuários notificados (12)

#### Pós-Mortem (P0/P1 obrigatório)
**O que funcionou bem:** Alerta Sentry detectou em < 2 min. Rollback testado funcionaria.
**O que falhou:** Timeout não dimensionado para variabilidade WhatsApp. Sem queue local.
**Ação preventiva:** Queue local implementada. Alertas de latência WhatsApp adicionados.
**Atualização Runbook:** Adicionado "Webhook Timeout" no RUNBOOK.md

#### Lições Aprendidas
- APIs externas têm variabilidade alta → sempre queue + timeout generoso
- Monitorar latência de dependências externas, não só erros
- Hotfix deploy < 10 min é viável com pipeline atual
```

---

## Como trabalhar com IA nesta etapa

**Prompt para triagem de alerta Sentry:**
```
Alerta Sentry: [cole JSON do alerta ou descrição].
Contexto: [stack, versão atual, deploy recente].
Me ajude a triage:
1. Severidade provável (P0/P1/P2) baseado em impacto
2. Passos para reproduzir
3. Possíveis causas (diff recente, dependência externa, config)
4. Workaround temporário
5. Plano de investigação (logs, traces, queries)
Não corrija — só estruture a investigação.
```

**Prompt para investigar bug em produção:**
```
Bug reportado: [descrição do usuário + logs Sentry].
Código relevante: [cole arquivos suspeitos].
Gere:
1. Hipóteses de causa raiz (ordenadas por probabilidade)
2. Passos para reproduzir local/staging
3. Testes para validar correção
4. Plano de hotfix (mínima mudança, rollback fácil)
Formato: Checklist acionável.
```

**Prompt para revisão semanal de dependências:**
```
Output do `pnpm audit --prod` e `npm outdated`: [cole].
Dependabot PRs abertos: [liste].
Analise:
1. Vulnerabilidades critical/high → ação imediata
2. Updates minor/patch → safe to merge?
3. Updates major → breaking changes? esforço? prioridade?
4. Deps não usadas / duplicadas → remover
Formato: Tabela de ação com Prioridade (Now/This Week/Next Sprint/Backlog).
```

**Prompt para gerar pós-mortem:**
```
Incidente: [resumo do INC-XXX].
Dados: timeline, logs, correção, validação.
Gere pós-mortem estruturado:
- Resumo executivo
- Timeline
- Root cause (5 Whys)
- Impacto
- O que funcionou / não funcionou
- Ações preventivas (técnicas + processo)
- Atualizações runbook/docs
Formato: Markdown para Registro de Incidentes.
```

**Contexto a fornecer:** Alertas Sentry, logs Vercel, código atual, Plano de Manutenção, Runbook.
**Resultado esperado:** Rotinas executadas, incidentes registrados e resolvidos, dependências atualizadas, roadmap atualizado.
**Como verificar:** Dashboards limpos, `pnpm audit` 0 critical/high, CHANGELOG atualizado, restore testado mensal.

## Checklist de execução (Contínuo)

### Diário
- [ ] Sentry, Uptime, Vercel Analytics verificados
- [ ] Alertas P0/P1 respondidos imediatamente

### Semanal
- [ ] `pnpm audit --prod` → 0 critical/high
- [ ] Dependabot PRs revisados e mergeados
- [ ] `npm outdated` avaliado
- [ ] Sentry trends revisados
- [ ] Métricas Neon/Vercel revisadas

### Mensal
- [ ] **Restore testado** (Neon → staging, dados validados)
- [ ] Revisão de riscos (ETAPA 12)
- [ ] Revisão limitações conhecidas
- [ ] Roadmap atualizado
- [ ] Custos revisados
- [ ] Documentação atualizada
- [ ] Rotação de segredos (se política)

### Por Release (Patch/Minor)
- [ ] CHANGELOG atualizado
- [ ] Tag SemVer criada
- [ ] Smoke tests produção
- [ ] Monitoramento 2h pós-deploy

## Critérios de conclusão (Contínuo — não "termina")

- [ ] Rotinas diárias/semanais/mensais estabelecidas e executadas
- [ ] Incidentes P0/P1: 0 não resolvidos
- [ ] `pnpm audit --prod` consistentemente 0 critical/high
- [ ] Restore testado mensalmente com sucesso
- [ ] CHANGELOG atualizado a cada release
- [ ] Roadmap revisado mensalmente
- [ ] Riscos residuais monitorados
- [ ] Acompanhamento reflete estado operacional atual

## Verificações e testes

- **Teste da rotina:** Você consegue fazer a rotina diária em < 10 min sem pensar?
- **Teste do restore:** Restore mensal → staging funcional com dados recentes?
- **Teste do hotfix:** Hotfix P1 → deploy < 30 min → smoke tests passam?
- **Teste do roadmap:** Roadmap reflete feedback real + capacidade real?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Ignorar alertas "baixos" | P2 vira P1 se ignorado. Triagem obrigatória. |
| Não testar restore | **Teste real mensal** — não confie em "backup existe". |
| Acumular dependências desatualizadas | Semana = patch/minor. Mês = avaliar major. |
| Misturar manutenção com feature | **Separe branches.** Manutenção = patch. Feature = minor. |
| Não documentar incidente | Todo P0/P1 = pós-mortem. Processo melhora com registro. |
| Deploy sem CHANGELOG | Automatize: `changesets` ou script no tag push. |

## Se algo der errado

| Situação | Ação |
|----------|------|
| P0 em produção (sistema down) | 1. `vercel rollback` IMEDIATO 2. Investigar 3. Hotfix 4. Deploy 5. Pós-mortem |
| Vazamento de dado (LGPD) | 1. Contenção 2. ANPD 48h 3. Titulares afetados 4. Medidas corretivas 5. Auditoria |
| Dependência critical sem fix | 1. Fork + patch próprio 2. Workaround (feature flag off) 3. Migrar alternativa |
| Custo explode | 1. Identificar driver (Vercel functions, Neon storage, Resend) 2. Otimizar query/cache 3. Downgrade tier se possível |
| Burnout / sem tempo | 1. Reduzir escopo (só P0/P1 + manutenção crítica) 2. Automatizar mais (Dependabot, alerts) 3. Pedir ajuda / contratar |

## Condições para continuar (Não "avançar" — esta etapa é contínua)

- ✅ Rotinas estabelecidas e sendo executadas
- [ ] Incidentes gerenciados conforme SLA
- [ ] Dependências saudáveis
- [ ] Backups validados
- [ ] Roadmap vivo e realista
- [ ] Documentação operacional atualizada (RUNBOOK)

## Próxima etapa

[[ETAPA 16 - Encerrar ou Transferir o Projeto]]

> [!note]
> **ETAPA 15 não tem "fim" enquanto o software vive.** Ela vira rotina. A ETAPA 16 é para quando você DECIDE encerrar ou transferir.

## Materiais relacionados

- [[02 - Modelos/Publicação e Manutenção/Plano de Manutenção]]
- [[02 - Modelos/Publicação e Manutenção/Registro de Incidentes]]
- [[03 - Guias Práticos/Publicação/Runbook de Operações]]
- [[03 - Guias Práticos/Publicação/Gestão de Dependências]]
- [[04 - Referência/Glossário/SemVer]]
- [[04 - Referência/Glossário/Incidente]]
- [[04 - Referência/Glossário/Pós-Mortem]]
- [[04 - Referência/Glossário/RPO/RTO]]