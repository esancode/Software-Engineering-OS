# Escopo e Limites

> **Copie para:** `05 - Projetos/SeuProjeto/02 - Produto/Escopo e Limites.md`
> Baseado na ETAPA 03. **Lista explícita do que NÃO será feito no MVP.**

---

## DENTRO do Escopo (MVP v1)

### Funcionalidades Must (Críticas - MVP não funciona sem isso)
- [ ] [F1: Receber pedidos via WhatsApp Webhook]
- [ ] [F2: Lista de pedidos com status]
- [ ] [F3: Detalhes do pedido]
- [ ] [F4: Atualizar status (botões rápidos)]
- [ ] [F9: Autenticação (Magic Link + Credentials)]

### Funcionalidades Should (Importantes - MVP funciona, mas fica incompleto sem)
- [ ] [F5: Responder cliente via app (templates + custom)]
- [ ] [F6: Relatório diário/semanal simples]
- [ ] [F10: Tempo real (SSE) - novos pedidos/atualizações]

### Funcionalidades Could (Nice to have - só se sobrar capacidade)
- [ ] [F7: Cadastro básico de produtos/preços]
- [ ] [F8: Configuração de horário de funcionamento]

### Requisitos Não-Funcionais Must
- [ ] Performance: Lista carrega < 500ms (p95)
- [ ] Performance: Webhook → UI < 2s
- [ ] Segurança: HTTPS, auth obrigatório, segredos em env
- [ ] Confiabilidade: Idempotência webhook, dados não perdidos
- [ ] Usabilidade: Mobile-first, touch-friendly, acessibilidade básica
- [ ] Compatibilidade: Chrome 120+, Safari 17+, Firefox 120+, Mobile iOS 16+/Android 13+

---

## FORA do Escopo (Explícito - v1)

> [!warning] **Esta lista protege contra scope creep. Revise a cada nova ideia.**

### Funcionalidades Explícitamente FORA (Won't v1)

| Funcionalidade | Motivo | Quando Reavaliar |
|----------------|--------|------------------|
| [Múltiplos estabelecimentos/filiais] | [Complexidade multi-tenant, auth, billing] | [v1.2+ se demanda real] |
| [Gestão de estoque avançada] | [Baixa automática, alertas, inventário, fornecedores] | [v1.3+ se pedido recorrente] |
| [Integração iFood / Rappi / Marketplaces] | [APIs diferentes, contratos, compliance] | [v1.3+ se demanda] |
| [Painel administrativo web separado] | [Dashboard mobile-first basta v1] | [v2.0 se equipe crescer] |
| [Relatórios avançados (ABC, curva, ticket médio)] | [Complexidade BI, não core v1] | [v1.1 relatório semanal básico] |
| [Programa de fidelidade / Cupons] | [Regra de negócio complexa, não validad] | [Icebox] |
| [Múltiplos usuários com perfis (admin, cozinha, caixa)] | [RBAC complexo, não necessário single-user v1] | [v1.2 se lojas com equipe] |
| [API pública para terceiros] | [Segurança, versionamento, docs, suporte] | [v2.0 se ecossistema] |
| [Pagamentos integrados (PIX, Cartão)] | [PCI-DSS, compliance, risco financeiro] | [Icebox / parceria] |
| [Notificações push nativas (App Store)] | [Requer app nativo, certificados, review] | [v1.2 PWA + Service Worker] |
| [Modo offline completo (IndexedDB + sync)] | [Complexidade alta, não validad necessidade] | [Icebox] |
| [Multi-idioma / i18n] | [Público inicial 100% BR] | [v2.0 se expansão] |
| [Tema escuro / personalização visual] | [Nice to have, não core] | [v1.1 se pedido] |
| [Webhooks de saída (para outros sistemas)] | [Não solicitado, complexidade] | [Icebox] |
| [Logs de auditoria avançados (SIEM)] | [Não regulado, não necessário v1] | [v2.0 se enterprise] |

### Requisitos Não-Funcionais FORA (v1)
- [ ] Escalabilidade > 10k usuários simultâneos
- [ ] Disponibilidade 99.99% (four nines)
- [ ] Recuperação de desastre < 15 min (RTO)
- [ ] Conformidade SOC2 / ISO27001
- [ ] Penetration testing terceirizado
- [ ] Suporte 24/7 / SLA contratual

---

## Limites Técnicos Explícitos

| Limite | Valor v1 | Ação se Atingir |
|--------|----------|-----------------|
| [Usuários simultâneos] | [~100] | [Otimizar / Migrar para Pusher] |
| [Pedidos/dia por loja] | [~500] | [Queue + worker / Upgrade tier] |
| [Storage Neon] | [512 MB (Free) / 10 GB (Scale)] | [Archivar pedidos antigos / Upgrade] |
| [Vercel Function Duration] | [10s (Hobby) / 60s (Pro)] | [Otimizar queries / Upgrade Pro] |
| [WhatsApp API Rate Limit] | [1000 req/min] | [Queue local + backoff] |
| [Resend Emails/dia] | [3000 (Free) / 50k (Pro)] | [Upgrade / Otimizar templates] |

---

## Critérios de Entrada/Saída do Escopo

### Para ENTRAR no escopo v1 (Deve atender TODOS)
- [ ] Validação direta com usuário real (entrevista/teste)
- [ ] Necessidade para testar hipótese principal (H1/H2)
- [ ] Esforço estimado ≤ 3 dias (1 pessoa)
- [ ] Não introduz dependência externa nova
- [ ] Não requer migração de dados complexa

### Para SAIR do escopo v1 (Basta 1)
- [ ] Não validado com usuário real
- [ ] Não necessário para hipótese principal
- [ ] Esforço > 3 dias
- [ ] Introduz dependência externa nova
- [ ] Requer decisão arquitetural não tomada
- [ ] Você diz "não vou fazer agora"

---

## Checklist de Revisão de Escopo (Semanal durante desenvolvimento)

- [ ] Nova ideia surgiu? → Aplicar critérios de entrada → Se não passar → Won't v1
- [ ] Feature Must ficou mais complexa? → Reavaliar: dividir? cortar? adiar?
- [ ] Dependência externa mudou? → Impacto no escopo?
- [ ] Feedback usuário real contradiz escopo? → Decidir: pivotar ou manter?
- [ ] Prazo apertando? → Cortar Could → Should → Must (nunca corte Must)

---

## Aprovação

**Escopo Definido e Aprovado por:** [Seu Nome]
**Data:** [YYYY-MM-DD]
**Assinatura:** [Digital]

---

## Próxima Etapa
[[ETAPA 04 - Organizar os Requisitos]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/02 - Produto e Requisitos/ETAPA 03 - Definir o Produto e seu Escopo]]
- [[02 - Modelos/Pesquisa e Planejamento/Definição do Produto]]
- [[02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades]]
- [[04 - Referência/Glossário/Scope Creep]]
- [[04 - Referência/Glossário/MVP]]