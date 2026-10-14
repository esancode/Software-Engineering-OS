# Regras de Negócio

> **Copie para:** `05 - Projetos/SeuProjeto/02 - Produto/Regras de Negócio.md`
> Baseado na ETAPA 04. Regras que o sistema **sempre** deve obedecer, independentemente de interface.

---

## Formato da Regra de Negócio
| Campo | Descrição |
|-------|-----------|
| **ID** | RN-XXX (sequencial, único) |
| **Descrição** | Regra em linguagem natural (sempre/ nunca/ deve/ não pode) |
| **Origem** | Domínio / Lei / Stakeholder / Decisão técnica |
| **RFs Afetados** | IDs dos requisitos funcionais impactados |
| **Validação** | Onde/Como testar (unitário, integração, E2E, manual) |
| **Exceções** | Casos onde a regra NÃO se aplica (se houver) |
| **Prioridade** | Must / Should / Could |

---

## Lista de Regras de Negócio

### Domínio: Pedidos

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-001 | Um pedido **sempre** tem exatamente um status válido: `novo`, `preparando`, `pronto`, `entregue`, `cancelado`, `aguardando_abertura` | Domínio | RF-016, RF-030, RF-031, RF-033 | Unitário (enum), Integração (criação), E2E (fluxo) | Nenhuma | Must |
| RN-002 | Transições de status **devem** seguir a máquina de estados definida (ver RN-003). Transições inválidas **são rejeitadas** com erro. | Domínio | RF-030, RF-031, RF-033 | Unitário (state machine), Integração (API), E2E (UI) | Admin pode forçar via script (auditado) | Must |
| RN-003 | Matriz de transições permitidas:<br/>• `novo` → `preparando`, `cancelado`, `aguardando_abertura`<br/>• `preparando` → `pronto`, `cancelado`<br/>• `pronto` → `entregue`, `cancelado`<br/>• `entregue` → (nenhuma)<br/>• `cancelado` → (nenhuma)<br/>• `aguardando_abertura` → `novo` (automático na abertura) | Domínio | RF-030, RF-033 | Unitário (tabela transições), Integração | Admin override (auditado) | Must |
| RN-004 | Um cliente (telefone WhatsApp) **pode** ter múltiplos pedidos `novo`/`preparando`/`pronto` simultâneos. | Domínio | RF-016, RF-020 | Integração (criar 2 pedidos mesmo telefone) | Nenhuma | Must |
| RN-005 | Pedido com status `entregue` ou `cancelado` **não pode** ser alterado (imutável). | Domínio | RF-030, RF-031, RF-033 | Unitário (tentativa update), Integração | Admin override (auditado) | Must |
| RN-006 | Todo pedido **deve** ter `whatsapp_message_id` único (chave de idempotência). | Técnica (idempotência) | RF-010, RF-016, RF-018 | Unitário (unique constraint), Integração (webhook dup) | Nenhuma | Must |
| RN-007 | Pedido **deve** registrar `created_at` imutável e `updated_at` a cada mudança. | Técnica (auditoria) | RF-016, RF-030, RF-032 | Unitário (timestamp behavior), Integração | Nenhuma | Must |
| RN-008 | Histórico de status **deve** registrar: pedido_id, status_anterior, status_novo, usuario_id, timestamp, motivo (se cancelado). | Domínio + Auditoria | RF-032 | Integração (verificar tabela historico) | Nenhuma | Must |
| RN-009 | Total do pedido **deve** ser calculado como soma de (quantidade × preço_unitário) dos itens. Se item sem preço, total = null (precisa revisão). | Domínio | RF-016, RF-025 | Unitário (cálculo), Integração (pedido com/sem preço) | Pedido manual pode ter total editado | Must |
| RN-010 | Itens do pedido **são** snapshot no momento da criação (não referenciam tabela produtos). Mudança de preço posterior **não afeta** pedidos antigos. | Domínio | RF-016, RF-070, RF-072 | Integração (alterar preço produto → pedido antigo inalterado) | Nenhuma | Must |

### Domínio: Cliente e Comunicação

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-011 | Telefone do cliente **deve** estar no formato E.164 (ex: `5511999999999`). Entrada normalizada no recebimento. | Domínio + Integração WhatsApp | RF-010, RF-016, RF-040 | Unitário (normalização), Integração (webhook) | Nenhuma | Must |
| RN-012 | Mensagem WhatsApp **só pode** ser enviada para telefone que tenha opt-in (conversa iniciada pelo cliente nas últimas 24h) OU template aprovado fora da janela. | WhatsApp Policy | RF-040, RF-041 | Integração (tentar enviar fora janela → erro) | Template aprovado (HSM) | Must |
| RN-013 | Template WhatsApp **deve** usar variáveis aprovadas no Meta. Variável não aprovada → erro no envio. | WhatsApp Policy | RF-040, RF-042 | Integração (template com var inválida) | Nenhuma | Must |
| RN-014 | Resposta customizada (sessão 24h) **não precisa** de template, mas **deve** ser texto simples (sem mídia no MVP). | WhatsApp Policy | RF-041 | Integração (enviar texto livre) | Mídia = Could v1.1 | Should |
| RN-015 | Histórico de mensagens **deve** registrar: pedido_id, direção (entrada/saída), tipo (template/texto), conteúdo, timestamp, status_envio (enviado/falhou), message_id_whatsapp. | Auditoria + Suporte | RF-040, RF-041, RF-043 | Integração (verificar tabela mensagens) | Nenhuma | Must |

### Domínio: Autenticação e Sessão

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-016 | Magic Link **deve** expirar em 15 minutos. | Segurança | RF-001 | Unitário (token expiry), Integração (link antigo) | Nenhuma | Must |
| RN-017 | Magic Link **deve** ser de uso único (invalidado após primeiro acesso). | Segurança | RF-001 | Unitário (token used flag), Integração (reuso) | Nenhuma | Must |
| RN-018 | Sessão (access token) **deve** expirar em 60 minutos. Refresh token **deve** expirar em 30 dias. | Segurança | RF-003 | Unitário (JWT exp), Integração (refresh flow) | Nenhuma | Must |
| RN-019 | Refresh token **deve** ser rotacionado a cada uso (novo refresh token emitido, antigo invalidado). | Segurança | RF-003 | Unitário (rotation), Integração (múltiplos refresh) | Nenhuma | Must |
| RN-020 | Logout **deve** invalidar refresh token no banco (não apenas remover cookie). | Segurança | RF-004 | Integração (logout → tentar refresh → 401) | Nenhuma | Must |
| RN-021 | Máximo de 5 sessões simultâneas por usuário. Nova sessão → remove a mais antiga (LRU). | Segurança + UX | RF-001, RF-002, RF-003 | Integração (6º login remove 1º) | Admin = ilimitado | Should |

### Domínio: Produtos e Configuração

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-022 | Produto **pode** estar `ativo` ou `inativo`. Inativo **não aparece** em sugestões, mas **mantido** em pedidos antigos (snapshot). | Domínio | RF-070, RF-071, RF-072 | Integração (desativar → não sugere, pedido antigo OK) | Nenhuma | Should |
| RN-023 | Categoria de produto **é** livre (texto), mas **sugere** existentes (autocomplete). | UX | RF-070, RF-072 | Manual | Nenhuma | Could |
| RN-024 | Horário de funcionamento **define** quando pedidos são aceitos como `novo` vs `aguardando_abertura`. | Domínio | RF-080, RF-081, RF-010 | Integração (webhook fora horário → status aguardando) | Feriados = Could v1.1 | Should |
| RN-025 | Fora do horário, resposta automática **deve** informar próximo horário de abertura. | UX + Domínio | RF-081 | Integração (verificar mensagem auto-resposta) | Nenhuma | Should |

### Domínio: Relatórios e Financeiro

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-026 | Faturamento **considera apenas** pedidos `entregue`. `cancelado` e `aguardando_abertura` **não entram**. | Domínio | RF-060, RF-061 | Unitário (query faturamento), Integração (relatório) | Nenhuma | Must |
| RN-027 | Ticket médio = Faturamento / Qtd pedidos `entregue` (se > 0, senão 0). | Domínio | RF-060, RF-061 | Unitário (cálculo), Integração | Nenhuma | Must |
| RN-028 | Relatório diário **corta** à meia-noite (timezone da loja: America/Sao_Paulo). | Domínio | RF-060 | Integração (pedido 23:59 vs 00:01) | Nenhuma | Must |
| RN-029 | Relatório semanal: Segunda a Domingo (ISO week). | Domínio | RF-061 | Integração (virada de semana) | Nenhuma | Should |

### Domínio: LGPD e Dados

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-030 | Dados pessoais (nome, telefone) **devem** ter consentimento registrado (LGPD Art. 7º). | Lei (LGPD) | RF-001, RF-016, RF-070 | Integração (verificar consent_log) | Legítimo interesse (pedido) = base legal | Must |
| RN-031 | Cliente **pode** solicitar exclusão de dados. Sistema **deve** anonimizar (manter pedido sem PII) ou excluir se sem pedidos entregues. | Lei (LGPD) | RF-063 | Integração (fluxo exclusão) | Pedidos entregues = anonimizar (manter fiscal) | Must |
| RN-032 | Logs de auditoria (status, mensagens) **mantidos** por 2 anos. Logs de acesso (login) **mantidos** por 1 ano. | Lei + Boa prática | RNF-019, RF-032, RF-043 | Auditoria manual / job limpeza | Nenhuma | Should |

### Domínio: Limites Técnicos (Regras de Guarda)

| ID | Descrição | Origem | RFs Afetados | Validação | Exceções | Prioridade |
|----|-----------|--------|--------------|-----------|----------|------------|
| RN-033 | Máximo 50 itens por pedido. | Performance + UX | RF-016, RF-025 | Unitário (validação array length), Integração | Nenhuma | Should |
| RN-034 | Máximo 500 caracteres em observações do pedido. | Banco + UX | RF-016, RF-025 | Unitário (validação string length) | Nenhuma | Should |
| RN-035 | Máximo 1000 caracteres em mensagem customizada WhatsApp. | WhatsApp Limit (4096) + margem | RF-041 | Unitário (validação), Integração (API WhatsApp) | Template = 1024 chars (Meta) | Must |
| RN-036 | Rate limit: máx 30 requisições / minuto / IP em rotas de auth. | Segurança | RNF-016 | Integração (burst test) | Nenhuma | Must |
| RN-037 | Rate limit: máx 100 requisições / minuto / usuário autenticado em APIs gerais. | Performance + Fair use | RNF-016 | Integração (load test) | Admin = 500/min | Should |

---

## Matriz RN ←→ RF ←→ Testes

| RN | RFs Principais | Tipo de Teste Sugerido |
|----|----------------|------------------------|
| RN-001..003 | RF-016, RF-030, RF-033 | Unit (state machine), Integration (API), E2E (UI) |
| RN-004..010 | RF-016, RF-020, RF-025 | Integration (banco), E2E (fluxo completo) |
| RN-011..015 | RF-010, RF-016, RF-040..043 | Integration (WhatsApp API), E2E (comunicação) |
| RN-016..021 | RF-001..005 | Unit (auth logic), Integration (session flow), E2E (login/logout) |
| RN-022..025 | RF-070..081 | Integration (CRUD produtos/horários), E2E (config) |
| RN-026..029 | RF-060, RF-061 | Unit (queries), Integration (relatórios) |
| RN-030..032 | RF-001, RF-016, RF-063 | Integration (LGPD flows), Auditoria manual |
| RN-033..037 | Transversais | Unit (validações), Integration (rate limit), Load test |

---

## Checklist de Qualidade das Regras

- [ ] Cada RN tem ID único, descrição clara ("sempre/nunca/deve/não pode")
- [ ] Origem identificada (Domínio, Lei, Técnica, Stakeholder)
- [ ] RFs afetados listados
- [ ] Validação definida (unit/integration/e2e/manual)
- [ ] Exceções documentadas (ou "Nenhuma")
- [ ] Prioridade MoSCoW
- [ ] Regras cobrem: estados, transições, cálculos, limites, compliance, segurança
- [ ] Nenhuma regra contraditória
- [ ] Regras testáveis (não subjetivas)

---

## Próxima Etapa
[[02 - Modelos/Requisitos e Arquitetura/ADR - Registro de Decisões Arquiteturais]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/02 - Produto e Requisitos/ETAPA 04 - Organizar os Requisitos]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos Funcionais]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]]
- [[02 - Modelos/Requisitos e Arquitetura/ADR - Registro de Decisões Arquiteturais]]
- [[04 - Referência/Glossário/Regra de Negócio]]
- [[04 - Referência/Glossário/Máquina de Estados]]
- [[04 - Referência/Glossário/LGPD]]