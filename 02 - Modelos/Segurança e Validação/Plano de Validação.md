# Plano de Validação

> **Copie para:** `05 - Projetos/SeuProjeto/07 - Segurança e Validação/Plano de Validação.md`
> Baseado na ETAPA 13. **Plano detalhado de validação do produto completo** antes de publicar.

---

## Objetivo da Validação

Confirmar que o **MVP atende aos critérios de sucesso definidos na ETAPA 03** e está pronto para uso real por lojistas.

**Critério de Sucesso Principal (ETAPA 03):**
> "Usuário consegue processar 10 pedidos sem ajuda" — Testado com 5 usuários reais, 100% sucesso.

---

## Escopo da Validação

| Incluído | Não Incluído (Pós-MVP) |
|----------|------------------------|
| Fluxo core: Pedido WhatsApp → Dashboard → Status → Entrega | Multi-loja, multi-usuário |
| Comunicação: Templates + Custom | Integração iFood/Rappi |
| Relatórios: Diário/Semanal | Relatórios avançados (ABC, curva) |
| Config: Produtos, Horários | Programa fidelidade, cupons |
| Auth: Magic Link + Credentials | SSO, MFA |
| PWA: Install + Offline básico | Push notifications nativas |
| LGPD: Consentimento, Acesso, Exclusão | DPIA completo (se não necessário) |

---

## Fases de Validação

### Fase 1: Validação Técnica (Sprint 3 - Automatizada)
| Atividade | Ferramenta | Critério | Responsável |
|-----------|------------|----------|-------------|
| Testes Unit + Integration | Vitest | 100% pass, coverage ≥ targets | Você |
| Testes E2E @smoke + @core | Playwright | 100% pass (chromium+firefox) | Você |
| Acessibilidade automatizada | axe + Playwright | 0 critical/serious | Você |
| Lighthouse CI | lhci | Perf ≥ 90, A11y ≥ 95 | Você |
| Load Test | k6 | Targets atingidos (ver abaixo) | Você |
| Security Scan | CodeQL, npm audit, TruffleHog | 0 high/critical | Você |

### Fase 2: Validação Funcional Manual (Pré-Deploy)
| Atividade | Cenários | Critério | Responsável |
|-----------|----------|----------|-------------|
| Smoke Test Produção | 10 fluxos core | 100% pass | Você |
| Cross-browser | Chrome, Safari, Firefox, Edge | Funcional + visual OK | Você |
| Mobile Real | iOS Safari, Android Chrome | Touch, layout, PWA install | Você |
| Acessibilidade Manual | Teclado, NVDA/VoiceOver, Zoom 200% | Checklist TC-A11Y | Você |
| LGPD Flows | Exportar, Excluir, Consentimento | Funcionam | Você |

### Fase 3: Validação com Usuários Reais (Pós-Deploy Controlado)
| Atividade | Participantes | Critério | Responsável |
|-----------|---------------|----------|-------------|
| Teste Usabilidade | 5 lojistas reais (amigos/contatos) | 100% completam 10 pedidos sem ajuda | Você |
| Feedback Qualitativo | Entrevista 15 min cada | NPS > 40, dor resolvida | Você |
| Monitoramento 48h | Logs, métricas, alertas | Zero erros críticos, latência OK | Você |

---

## Cenários de Validação Detalhados

### V1: Onboarding Completo (Usuário Novo)
**Objetivo:** Validar que usuário novo consegue configurar e receber primeiro pedido em < 10 min.

| Passo | Ação | Tempo Esperado | Critério Sucesso |
|-------|------|----------------|------------------|
| 1 | Acessa landing → Clica "Começar" | 30s | Entende proposta |
| 2 | Login Magic Link (email) | 1 min | Email chega < 30s, link funciona |
| 3 | Onboarding 1: Nome + Tel loja | 1 min | Salva, valida E.164 |
| 4 | Onboarding 2: Conecta WhatsApp | 3 min | Instruções claras, token validado |
| 5 | Onboarding 3: Horário (preset) | 1 min | Preset "Lanchonete Noite" funciona |
| 6 | Onboarding 4: Teste pedido | 1 min | Pedido fake aparece no dashboard |
| 7 | Dashboard vazio → pronto | - | "Pronto! Receba pedidos" |

**Tempo Total Target:** < 10 min
**Métrica:** > 80% completam sem ajuda (RNF-036)

---

### V2: Fluxo Core - Dia Típico (10 Pedidos)
**Objetivo:** Validar processamento de 10 pedidos variados sem ajuda.

| Pedido | Tipo | Ações Lojista | Validação |
|--------|------|---------------|-----------|
| 1 | Simples: "1 x-burger" | Iniciar → Pronto → Entregar | Fluxo básico |
| 2 | Múltiplos: "2 x-salada, 1 batata, 2 refri" | Mesmo fluxo | Itens corretos |
| 3 | Com obs: "1 x-burger sem cebola, batata sequinha" | Ver obs no detalhe | Obs preservada |
| 4 | Com preço: "2 burger 30 cada" | Ver total R$ 60 | Preço extraído |
| 5 | Fora horário (simular) | Ver status AGUARDANDO | Auto-resposta enviada |
| 6 | Cliente pergunta "quanto tempo?" | Responder template "Em preparo" | Template enviado |
| 7 | Cliente pede cancelamento | Cancelar + motivo | Status CANCELADO |
| 8 | Pedido por foto (imagem) | Ver imagem no detalhe | Mídia salva + obs |
| 9 | Relatório do dia | Ver faturamento, ticket | Métricas corretas |
| 10 | Configurar produto novo | Adicionar "X-Novo" R$ 28 | Sugere em pedido manual |

**Critério:** 100% dos 5 usuários completam os 10 pedidos **sem pedir ajuda**.

---

### V3: Cenários de Borda e Erro
| Cenário | Ação | Resultado Esperado |
|---------|------|-------------------|
| Rede cai durante uso | Desconectar WiFi 10s | "Reconectando..." → "LIVE" auto |
| WhatsApp API down | Simular erro 500 no webhook | Dead letter + alerta, fallback email |
| Sessão expira mid-action | Manipular token expiry | Refresh silencioso ou login preservando contexto |
| Pedido duplicado (webhook retry) | Mesmo message_id 2x | 1 pedido só, log duplicata |
| Transição inválida via API | Tentar ENTREGUE → PREPARANDO | 400 erro, status mantido |
| Zoom 200% + Mobile | Testar todas telas | Usável, sem scroll horizontal |
| Screen Reader | Navegar fluxo core | Anuncia status, erros, ações |

---

## Métricas de Validação (KPIs)

| Métrica | Target | Como Medir | Frequência |
|---------|--------|------------|------------|
| **Onboarding Completion** | > 80% | Eventos step1→step4→dashboard | Contínuo |
| **Time to First Value** | < 10 min | Timestamp create → primeiro pedido teste | Contínuo |
| **Core Task Success** | 100% (5/5 usuários) | Observação teste usabilidade | Validação |
| **Pedidos Processados s/ Ajuda** | 10/10 por usuário | Contador ações help/erro | Validação |
| **NPS Pós-Validação** | > 40 | Survey 0-10 "Recomendaria?" | Validação |
| **Webhook → UI Latency (p95)** | < 2s | Logs timestamp webhook → SSE | Contínuo |
| **Dashboard Load (p95)** | < 500ms | Lighthouse / RUM | Contínuo |
| **Error Rate (5xx + 4xx não esperados)** | < 0.1% | Logs / Sentry | Contínuo |
| **Uptime** | > 99.9% | Upptime / UptimeRobot | Contínuo |
| **LGPD Requests Atendidas** | 100% em 30 dias | Log solicitações | Contínuo |

---

## Critérios de Go/No-Go para Produção

### GO (Todos Obrigatórios)
- [ ] Fase 1: 100% testes automatizados passam
- [ ] Fase 2: 100% smoke test manual passa
- [ ] Fase 3: 5/5 usuários completam 10 pedidos sem ajuda
- [ ] NPS ≥ 40 na validação
- [ ] Zero vulnerabilidades high/critical
- [ ] Load test targets atingidos
- [ ] LGPD flows funcionam
- [ ] Runbook incidentes pronto
- [ ] Rollback testado (< 2 min)
- [ ] Domínio custom + SSL ativo

### NO-GO (Qualquer Um Bloqueia)
- [ ] Qualquer teste @smoke falha
- [ ] Vulnerabilidade high/critical não mitigada
- [ ] Usuário real não consegue completar fluxo core
- [ ] NPS < 30
- [ ] Load test falha (p95 > targets)
- [ ] LGPD flow quebrado (exclusão/export)
- [ ] Rollback falha
- [ ] Secrets vazados em repo/build

---

## Plano de Rollback (Se No-Go Pós-Deploy)

| Gatilho | Ação | Tempo | Responsável |
|---------|------|-------|-------------|
| Error rate > 5% em 10 min | `vercel rollback` | < 2 min | Você |
| Webhook falha > 50% | Desabilitar webhook no Meta → processar manual | 5 min | Você |
| SSE down > 50% usuários | Restart Vercel functions / check Neon | 5 min | Você |
| DB erro crítico | Neon PITR restore point anterior | 15 min | Você |
| LGPD breach | Isolar sistema, notificar ANPD 48h | Imediato | Você |

---

## Checklist Final de Validação (Assinatura)

> **Preencha e assine antes de declarar MVP pronto.**

| Item | ✅ | Observações |
|------|----|-------------|
| **Validação Técnica** | | |
| Testes automatizados 100% verdes | ☐ | |
| Coverage ≥ targets | ☐ | |
| Lighthouse CI ≥ targets | ☐ | |
| Load test targets atingidos | ☐ | |
| Security scan limpo | ☐ | |
| **Validação Funcional** | | |
| Smoke test produção 100% | ☐ | |
| Cross-browser OK | ☐ | |
| Mobile real OK | ☐ | |
| Acessibilidade manual OK | ☐ | |
| LGPD flows OK | ☐ | |
| **Validação Usuário** | | |
| 5 usuários × 10 pedidos = 50 pedidos sem ajuda | ☐ | |
| NPS ≥ 40 | ☐ | |
| Feedback qualitativo documentado | ☐ | |
| **Operacional** | | |
| Monitoramento 48h limpo | ☐ | |
| Alertas configurados | ☐ | |
| Runbook incidentes pronto | ☐ | |
| Rollback testado | ☐ | |
| Domínio + SSL produção | ☐ | |
| **Documentação** | | |
| README atualizado | ☐ | |
| ADRs finalizados | ☐ | |
| Changelog v1.0.0 | ☐ | |
| Runbook incidentes | ☐ | |

---

## Aprovação Final

**Produto Validado e Aprovado para Produção por:** [Seu Nome]
**Data:** [YYYY-MM-DD]
**Assinatura:** [Digital]

> [!important] **Não delegue esta aprovação.** Mesmo que pareça óbvio, registre explicitamente.

---

## Pós-Validação (Primeiras 2 Semanas)

| Atividade | Frequência | Responsável |
|-----------|------------|-------------|
| Daily standup métricas (erros, latência, usuários ativos) | Diária | Você |
| Weekly user check-in (3 usuários piloto) | Semanal | Você |
| Bug triage + fix deploy | Conforme necessário | Você |
| Feature requests priorização (Icebox/Backlog) | Semanal | Você |
| Retrospectiva validação + lições | Fim 2 semanas | Você |

---

## Próxima Etapa
[[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/07 - Segurança e Validação/ETAPA 13 - Validar o Produto Completo]]
- [[01 - Manual de Desenvolvimento/08 - Publicação e Manutenção/ETAPA 14 - Publicar o Software]]
- [[02 - Modelos/Testes/Casos de Teste]]
- [[02 - Modelos/Segurança e Validação/Checklist de Segurança]]
- [[02 - Modelos/Publicação e Manutenção/Checklist de Deploy]]
- [[02 - Modelos/Publicação e Manutenção/Runbook de Incidentes]]
- [[04 - Referência/Glossário/Validation]]
- [[04 - Referência/Glossário/Go/No-Go]]
- [[04 - Referência/Glossário/Rollback]]