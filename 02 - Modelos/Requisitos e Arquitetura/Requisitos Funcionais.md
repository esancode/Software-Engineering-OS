# Requisitos Funcionais

> **Copie para:** `05 - Projetos/SeuProjeto/02 - Produto/Requisitos Funcionais.md`
> Baseado na ETAPA 04. Um requisito por linha, rastreável a Job/Feature.

---

## Formato do Requisito
| Campo | Descrição |
|-------|-----------|
| **ID** | RF-XXX (sequencial, único) |
| **Descrição** | "O sistema DEVE/PODE [ação] [condição] [resultado esperado]" |
| **Fonte** | Job ID / Feature ID / Hipótese / Stakeholder |
| **Prioridade** | Must / Should / Could / Won't (MoSCoW) |
| **Critério de Aceitação** | Link para [[Critérios de Aceitação#RF-XXX]] |
| **Dependências** | IDs de outros requisitos (RF, RNF, RN) |
| **Esforço** | 1-5 (1=trivial, 5=complexo) |
| **Status** | Proposto / Aprovado / Em Desenvolvimento / Pronto / Testado / Cancelado |

---

## Lista de Requisitos Funcionais

### Autenticação e Acesso (F9)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-001 | O sistema DEVE permitir login via Magic Link (email) | F9, Job: Acesso | Must | [[Critérios de Aceitação#RF-001]] | RNF-001 (HTTPS), RN-001 | 2 | Proposto |
| RF-002 | O sistema DEVE permitir login via Credentials (email/senha) | F9, Job: Acesso | Must | [[Critérios de Aceitação#RF-002]] | RNF-001, RN-001 | 2 | Proposto |
| RF-003 | O sistema DEVE renovar sessão automaticamente (refresh token) | F9 | Must | [[Critérios de Aceitação#RF-003]] | RF-001, RF-002 | 1 | Proposto |
| RF-004 | O sistema DEVE fazer logout e invalidar tokens | F9 | Must | [[Critérios de Aceitação#RF-004]] | RF-001, RF-002 | 1 | Proposto |
| RF-005 | O sistema DEVE proteger rotas autenticadas (middleware) | F9 | Must | [[Critérios de Aceitação#RF-005]] | RF-001..004 | 1 | Proposto |

### Recebimento de Pedidos via WhatsApp (F1)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-010 | O sistema DEVE receber webhook do WhatsApp Business API | F1, Job: Ver pedidos novos | Must | [[Critérios de Aceitação#RF-010]] | RNF-002 (Idempotência), RN-002 | 4 | Proposto |
| RF-011 | O sistema DEVE validar assinatura do webhook (HMAC) | F1 | Must | [[Critérios de Aceitação#RF-011]] | RF-010 | 2 | Proposto |
| RF-012 | O sistema DEVE processar mensagens de texto (pedido) | F1 | Must | [[Critérios de Aceitação#RF-012]] | RF-010 | 3 | Proposto |
| RF-013 | O sistema DEVE processar mensagens de mídia (imagem do pedido) | F1 | Should | [[Critérios de Aceitação#RF-013]] | RF-010 | 3 | Proposto |
| RF-014 | O sistema DEVE processar mensagens de localização (entrega) | F1 | Could | [[Critérios de Aceitação#RF-014]] | RF-010 | 2 | Proposto |
| RF-015 | O sistema DEVE extrair dados estruturados da mensagem livre | F1, HS2 | Must | [[Critérios de Aceitação#RF-015]] | RF-012 | 4 | Proposto |
| RF-016 | O sistema DEVE criar pedido com status "novo" | F1, F2 | Must | [[Critérios de Aceitação#RF-016]] | RF-015, RF-020 | 2 | Proposto |
| RF-017 | O sistema DEVE responder ACK ao WhatsApp < 5s | F1, RNF-003 | Must | [[Critérios de Aceitação#RF-017]] | RF-010 | 1 | Proposto |
| RF-018 | O sistema DEVE lidar com webhook duplicado (idempotência) | F1, RNF-004 | Must | [[Critérios de Aceitação#RF-018]] | RF-010 | 2 | Proposto |
| RF-019 | O sistema DEVE registrar falha de processamento (dead letter) | F1 | Must | [[Critérios de Aceitação#RF-019]] | RF-010 | 2 | Proposto |

### Lista e Visualização de Pedidos (F2, F3)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-020 | O sistema DEVE listar pedidos com paginação (20 por página) | F2 | Must | [[Critérios de Aceitação#RF-020]] | RF-016 | 2 | Proposto |
| RF-021 | O sistema DEVE filtrar pedidos por status | F2 | Must | [[Critérios de Aceitação#RF-021]] | RF-020 | 1 | Proposto |
| RF-022 | O sistema DEVE filtrar pedidos por data (hoje, semana, mês, custom) | F2, F6 | Must | [[Critérios de Aceitação#RF-022]] | RF-020 | 2 | Proposto |
| RF-023 | O sistema DEVE buscar pedidos por nome/cliente/telefone | F2 | Should | [[Critérios de Aceitação#RF-023]] | RF-020 | 2 | Proposto |
| RF-024 | O sistema DEVE mostrar badge de contagem por status | F2 | Must | [[Critérios de Aceitação#RF-024]] | RF-020 | 1 | Proposto |
| RF-025 | O sistema DEVE exibir detalhes do pedido (itens, obs, cliente, total) | F3 | Must | [[Critérios de Aceitação#RF-025]] | RF-016 | 2 | Proposto |
| RF-026 | O sistema DEVE mostrar timeline de status do pedido | F3 | Should | [[Critérios de Aceitação#RF-026]] | RF-025, RF-030 | 2 | Proposto |
| RF-027 | O sistema DEVE carregar lista inicial < 500ms (p95) | F2, RNF-005 | Must | [[Critérios de Aceitação#RF-027]] | RF-020 | 2 | Proposto |

### Atualização de Status (F4)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-030 | O sistema DEVE permitir mudar status: novo → preparando → pronto → entregue | F4 | Must | [[Critérios de Aceitação#RF-030]] | RF-016, RN-003 | 1 | Proposto |
| RF-031 | O sistema DEVE permitir cancelar pedido (novo/preparando → cancelado) | F4 | Must | [[Critérios de Aceitação#RF-031]] | RF-016, RN-003 | 1 | Proposto |
| RF-032 | O sistema DEVE registrar timestamp e usuário em cada mudança | F4, RNF-004 | Must | [[Critérios de Aceitação#RF-032]] | RF-030, RF-031 | 1 | Proposto |
| RF-033 | O sistema DEVE impedir transições inválidas (ex: entregue → preparando) | F4, RN-003 | Must | [[Critérios de Aceitação#RF-033]] | RF-030 | 1 | Proposto |
| RF-034 | O sistema DEVE propagar mudança via SSE tempo real | F10 | Should | [[Critérios de Aceitação#RF-034]] | RF-030, RF-050 | 2 | Proposto |

### Resposta ao Cliente (F5)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-040 | O sistema DEVE permitir enviar mensagem via WhatsApp (template) | F5 | Should | [[Critérios de Aceitação#RF-040]] | RF-010, RN-004 | 3 | Proposto |
| RF-041 | O sistema DEVE permitir enviar mensagem customizada | F5 | Should | [[Critérios de Aceitação#RF-041]] | RF-010, RN-004 | 2 | Proposto |
| RF-042 | O sistema DEVE ter templates pré-definidos (status, confirmação, atraso) | F5, HS3 | Should | [[Critérios de Aceitação#RF-042]] | RF-040 | 1 | Proposto |
| RF-043 | O sistema DEVE mostrar histórico de mensagens enviadas no detalhe | F5 | Should | [[Critérios de Aceitação#RF-043]] | RF-025, RF-040 | 1 | Proposto |

### Tempo Real (F10)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-050 | O sistema DEVE manter conexão SSE por usuário autenticado | F10 | Should | [[Critérios de Aceitação#RF-050]] | RF-001..005 | 3 | Proposto |
| RF-051 | O sistema DEVE enviar evento "novo_pedido" ao receber webhook | F10, F1 | Should | [[Critérios de Aceitação#RF-051]] | RF-010, RF-050 | 2 | Proposto |
| RF-052 | O sistema DEVE enviar evento "status_atualizado" ao mudar status | F10, F4 | Should | [[Critérios de Aceitação#RF-052]] | RF-030, RF-050 | 2 | Proposto |
| RF-053 | O sistema DEVE reconectar automaticamente em queda de conexão | F10 | Should | [[Critérios de Aceitação#RF-053]] | RF-050 | 2 | Proposto |

### Relatórios (F6)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-060 | O sistema DEVE gerar relatório diário (total, por status, ticket médio) | F6 | Should | [[Critérios de Aceitação#RF-060]] | RF-016, RF-030 | 2 | Proposto |
| RF-061 | O sistema DEVE gerar relatório semanal (comparativo, tendências) | F6 | Should | [[Critérios de Aceitação#RF-061]] | RF-060 | 2 | Proposto |
| RF-062 | O sistema DEVE permitir exportar relatório (CSV) | F6 | Could | [[Critérios de Aceitação#RF-062]] | RF-060 | 1 | Proposto |

### Produtos e Configurações (F7, F8)

| ID | Descrição | Fonte | Prioridade | Critério | Dependências | Esforço | Status |
|----|-----------|-------|------------|----------|--------------|---------|--------|
| RF-070 | O sistema DEVE permitir cadastrar produto (nome, preço, descrição, categoria) | F7 | Could | [[Critérios de Aceitação#RF-070]] | - | 2 | Proposto |
| RF-071 | O sistema DEVE permitir editar/excluir produto | F7 | Could | [[Critérios de Aceitação#RF-071]] | RF-070 | 1 | Proposto |
| RF-072 | O sistema DEVE sugerir produtos ao criar pedido manual | F7 | Could | [[Critérios de Aceitação#RF-072]] | RF-070 | 2 | Proposto |
| RF-080 | O sistema DEVE permitir configurar horário de funcionamento (semana) | F8 | Could | [[Critérios de Aceitação#RF-080]] | - | 1 | Proposto |
| RF-081 | O sistema DEVE mostrar "Fechado" fora do horário e bloquear pedidos | F8 | Could | [[Critérios de Aceitação#RF-081]] | RF-080, RF-010 | 2 | Proposto |

---

## Matriz de Rastreabilidade (Requisito ←→ Job ←→ Feature)

| Requisito | Job(s) | Feature(s) | Hipótese Validada |
|-----------|--------|------------|-------------------|
| RF-001..005 | Acesso | F9 | - |
| RF-010..019 | Ver pedidos novos | F1 | HS1, HS2 |
| RF-020..027 | Ver pedidos novos, Ver histórico | F2, F3 | HS2 |
| RF-030..034 | Marcar como preparando, Fechar pedido | F4 | HS2 |
| RF-040..043 | Responder cliente | F5 | HS3 |
| RF-050..053 | Ver pedidos novos | F10 | HS2 |
| RF-060..062 | Fechar pedido, Ver histórico | F6 | - |
| RF-070..072 | Cadastrar produtos | F7 | - |
| RF-080..081 | Configurar horários | F8 | - |

---

## Checklist de Qualidade dos Requisitos

- [ ] Cada requisito tem ID único e sequencial
- [ ] Descrição usa "DEVE" (obrigatório) ou "PODE" (opcional) — nunca "deveria"
- [ ] Cada requisito rastreia a Job/Feature/Hipótese
- [ ] Prioridade MoSCoW definida
- [ ] Critério de aceitação linkado (ou "a definir")
- [ ] Dependências mapeadas
- [ ] Esforço estimado (1-5)
- [ ] Status atualizado
- [ ] Requisitos Must cobrem todas as Features Must
- [ ] Nenhum requisito órfão (sem Job/Feature de origem)

---

## Próxima Etapa
[[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/02 - Produto e Requisitos/ETAPA 04 - Organizar os Requisitos]]
- [[02 - Modelos/Pesquisa e Planejamento/Definição do Produto]]
- [[02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]]
- [[02 - Modelos/Requisitos e Arquitetura/Regras de Negócio]]
- [[04 - Referência/Glossário/MoSCoW]]
- [[04 - Referência/Glossário/Requisito Funcional]]