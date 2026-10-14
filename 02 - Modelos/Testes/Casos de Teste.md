# Casos de Teste Detalhados

> **Copie para:** `05 - Projetos/SeuProjeto/06 - Testes e Qualidade/Casos de Teste.md`
> Baseado na ETAPA 11. **Casos de teste executáveis** com dados, passos e resultados esperados para validação manual e automação.

---

## Formato do Caso de Teste

| Campo | Descrição |
|-------|-----------|
| **ID** | TC-XXX (único, sequencial por área) |
| **Título** | O que está sendo testado |
| **Pré-condições** | Estado necessário antes de executar |
| **Passos** | Passos numerados, acionáveis |
| **Dados de Teste** | Valores específicos usados |
| **Resultado Esperado** | O que deve acontecer (observável) |
| **Pós-condições** | Estado após execução |
| **Prioridade** | Crítico / Alto / Médio / Baixo |
| **Tipo** | Manual / Automatizado (Unit/Integration/E2E) |
| **RF/RNF** | Requisitos rastreados |

---

## AUTENTICAÇÃO (TC-AUTH-XXX)

### TC-AUTH-001: Login Magic Link - Sucesso
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-001 |
| **Título** | Usuário novo faz login via Magic Link e acessa dashboard |
| **Pré-condições** | Email não cadastrado; Resend configurado; NextAuth ativo |
| **Passos** | 1. Acessar `/login`<br>2. Preencher email `novo@teste.com`<br>3. Clicar "Enviar link mágico"<br>4. Abrir email (Resend dashboard / inbox)<br>5. Clicar link no email<br>6. Verificar redirecionamento para `/dashboard` |
| **Dados** | Email: `novo@teste.com`<br>Link expira: 15 min |
| **Resultado** | Usuário autenticado; Cookie sessão definido; Dashboard carrega vazio; Badge "Novo: 0" |
| **Pós-condições** | Usuário criado no DB; Loja criada; Sessão ativa |
| **Prioridade** | Crítico |
| **Tipo** | E2E (Automatizado) |
| **RF** | RF-001 |

### TC-AUTH-002: Login Magic Link - Link Expirado
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-002 |
| **Título** | Tentar usar Magic Link após 15 minutos exibe erro |
| **Pré-condições** | Magic Link gerado há > 15 min |
| **Passos** | 1. Gerar Magic Link<br>2. Aguardar 16 min (ou manipular timestamp no DB)<br>3. Clicar link expirado |
| **Dados** | Token com `expires` < `now()` |
| **Resultado** | Mensagem "Link expirado. Solicite um novo."; Redirect `/login`; Não autenticado |
| **Prioridade** | Alto |
| **Tipo** | Integration + E2E |
| **RF** | RF-001, RN-016 |

### TC-AUTH-003: Login Magic Link - Reutilização
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-003 |
| **Título** | Tentar reutilizar Magic Link já usado falha |
| **Pré-condições** | Magic Link já usado para login |
| **Passos** | 1. Usar link válido → login sucesso<br>2. Tentar clicar mesmo link novamente |
| **Dados** | Token com `used: true` |
| **Resultado** | Mensagem "Link já utilizado. Solicite um novo."; Redirect `/login` |
| **Prioridade** | Alto |
| **Tipo** | Integration + E2E |
| **RF** | RF-001, RN-017 |

### TC-AUTH-004: Login Credentials - Sucesso
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-004 |
| **Título** | Usuário existente faz login com email/senha |
| **Pré-condições** | Usuário com senha cadastrada (via Magic Link + definir senha) |
| **Passos** | 1. Acessar `/login/senha`<br>2. Preencher email `existente@teste.com`<br>3. Preencher senha `SenhaForte123`<br>4. Clicar "Entrar" |
| **Dados** | Email: `existente@teste.com`<br>Senha: `SenhaForte123` |
| **Resultado** | Redirect `/dashboard`; Sessão ativa; Cookies definidos |
| **Prioridade** | Crítico |
| **Tipo** | E2E |
| **RF** | RF-002 |

### TC-AUTH-005: Login Credentials - Falha Genérica
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-005 |
| **Título** | Senha incorreta ou email inexistente mostra mensagem genérica |
| **Pré-condições** | Usuário `existente@teste.com` existe |
| **Passos** | 1. Tentar login com senha errada<br>2. Tentar login com email inexistente |
| **Dados** | Senha: `Errada123` / Email: `naoexiste@teste.com` |
| **Resultado** | Mensagem "Email ou senha incorretos" em AMBOS casos; Não vaza existência de conta |
| **Prioridade** | Alto |
| **Tipo** | E2E |
| **RF** | RF-002, Segurança |

### TC-AUTH-006: Refresh Token Rotation
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-006 |
| **Título** | Refresh token rotaciona a cada uso; antigo invalidado |
| **Pré-condições** | Usuário logado; Refresh token válido no DB |
| **Passos** | 1. Fazer request autenticado (usa access token)<br>2. Access token expira → refresh automático<br>3. Verificar novo refresh token no DB<br>4. Tentar usar refresh token antigo |
| **Dados** | Access token expiry: 1h; Refresh expiry: 30d |
| **Resultado** | Novo access token emitido; Novo refresh token no DB; Antigo `revokedAt` preenchido; Request antigo falha |
| **Prioridade** | Alto |
| **Tipo** | Integration |
| **RF** | RF-003, RN-019 |

### TC-AUTH-007: Logout Invalida Sessão
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-007 |
| **Título** | Logout remove cookies e invalida refresh token no DB |
| **Pré-condições** | Usuário logado |
| **Passos** | 1. Clicar "Sair" no menu<br>2. Tentar acessar `/dashboard`<br>3. Tentar usar refresh token antigo |
| **Dados** | - |
| **Resultado** | Redirect `/login`; Cookies removidos; Refresh token `revokedAt` preenchido; Acesso `/dashboard` → redirect login |
| **Prioridade** | Crítico |
| **Tipo** | E2E + Integration |
| **RF** | RF-004, RN-020 |

### TC-AUTH-008: Proteção Rotas - Middleware
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-008 |
| **Título** | Rotas `/dashboard/*` e `/api/*` bloqueadas sem autenticação |
| **Pré-condições** | Usuário NÃO autenticado |
| **Passos** | 1. Acessar `/dashboard` diretamente<br>2. Acessar `/api/pedidos`<br>3. Acessar `/pedidos/123` |
| **Dados** | - |
| **Resultado** | Redirect `/login?callbackUrl=...`; API retorna 401 `{error: "Não autenticado"}` |
| **Prioridade** | Crítico |
| **Tipo** | E2E + Integration |
| **RF** | RF-005 |

### TC-AUTH-009: Rate Limit Auth
| Campo | Valor |
|-------|-------|
| **ID** | TC-AUTH-009 |
| **Título** | Mais de 30 tentativas/min/IP em auth retorna 429 |
| **Pré-condições** | IP limpo |
| **Passos** | 1. Fazer 31 requests POST `/api/auth/signin/email` em < 60s |
| **Dados** | 31 requests rápidos |
| **Resultado** | 30 primeiros: 200/400; 31º: 429 "Too Many Requests" |
| **Prioridade** | Alto |
| **Tipo** | Integration + Load (k6) |
| **RNF** | RNF-016, RN-036 |

---

## WHATSAPP WEBHOOK (TC-WHATS-XXX)

### TC-WHATS-001: Webhook HMAC Válido
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-001 |
| **Título** | Webhook com assinatura HMAC válida é processado |
| **Pré-condições** | `WHATSAPP_APP_SECRET` configurado; Endpoint ativo |
| **Passos** | 1. POST `/api/webhook/whatsapp` com payload válido + header `X-Hub-Signature-256` correto |
| **Dados** | Payload: mensagem texto "quero 1 x-burger"<br>HMAC: SHA256(payload, secret) |
| **Resultado** | HTTP 200 < 5s; Mensagem enfileirada; Log "Webhook recebido" |
| **Prioridade** | Crítico |
| **Tipo** | Integration |
| **RF** | RF-010, RF-011, RN-011 |

### TC-WHATS-002: Webhook HMAC Inválido
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-002 |
| **Título** | Webhook com assinatura inválida é rejeitado (401) |
| **Pré-condições** | Mesmo payload, HMAC errado |
| **Passos** | 1. POST com header `X-Hub-Signature-256: sha256=errado` |
| **Dados** | HMAC calculado com secret errado |
| **Resultado** | HTTP 401; Log "Webhook rejeitado: assinatura inválida"; Não processa |
| **Prioridade** | Crítico |
| **Tipo** | Integration |
| **RF** | RF-011, RN-011 |

### TC-WHATS-003: Webhook Sem Assinatura
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-003 |
| **Título** | Webhook sem header HMAC é rejeitado |
| **Pré-condições** | Payload válido, header ausente |
| **Passos** | 1. POST sem header `X-Hub-Signature-256` |
| **Resultado** | HTTP 401; Log "Webhook rejeitado: assinatura ausente" |
| **Prioridade** | Crítico |
| **Tipo** | Integration |
| **RF** | RF-011 |

### TC-WHATS-004: Idempotência - Message ID Duplicado
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-004 |
| **Título** | Mesmo `whatsapp_message_id` recebido duas vezes cria 1 pedido |
| **Pré-condições** | Primeiro webhook processado com sucesso |
| **Passos** | 1. Enviar webhook com `message_id: wamid.123`<br>2. Aguardar processamento<br>3. Enviar MESMO webhook novamente |
| **Dados** | `whatsappMessageId: "wamid.123"` duplicado |
| **Resultado** | Ambos retornam 200; Apenas 1 pedido no DB; Log "Webhook duplicado ignorado" |
| **Prioridade** | Crítico |
| **Tipo** | Integration + E2E |
| **RF** | RF-018, RN-006 |

### TC-WHATS-005: Parser - Pedido Simples
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-005 |
| **Título** | Parser extrai itens de mensagem simples |
| **Pré-condições** | Parser function isolada |
| **Passos** | 1. Chamar `parseMessage("quero 2 x-burger e 1 batata grande")` |
| **Dados** | Input: texto livre |
| **Resultado** | `[{nome:'x-burger',qtd:2},{nome:'batata grande',qtd:1}]` |
| **Prioridade** | Crítico |
| **Tipo** | Unit (25+ casos) |
| **RF** | RF-015 |

### TC-WHATS-006: Parser - Com Preços
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-006 |
| **Título** | Parser extrai preços quando mencionados |
| **Passos** | 1. `parseMessage("2 x-salada 30 reais cada e 1 suco 8 reais")` |
| **Resultado** | `[{nome:'x-salada',qtd:2,preco:30},{nome:'suco',qtd:1,preco:8}]` |
| **Prioridade** | Alto |
| **Tipo** | Unit |
| **RF** | RF-015 |

### TC-WHATS-007: Parser - Sem Padrão (Needs Review)
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-007 |
| **Título** | Mensagem sem padrão reconhecível retorna vazio + flag needsReview |
| **Passos** | 1. `parseMessage("oi tudo bem")` |
| **Resultado** | `itens: []`, `needsReview: true`, `observacoes: "oi tudo bem"` |
| **Prioridade** | Alto |
| **Tipo** | Unit |
| **RF** | RF-015 |

### TC-WHATS-008: Dead Letter - Falha Parser
| Campo | Valor |
|-------|-------|
| **ID** | TC-WHATS-008 |
| **Título** | Falha no parser após 3 retries vai para Dead Letter |
| **Pré-condições** | Mock parser que sempre falha |
| **Passos** | 1. Enviar webhook que causa erro no parser<br>2. Aguardar 3 tentativas (retry exponencial)<br>3. Verificar tabela `dead_letter` |
| **Dados** | Payload que causa exceção |
| **Resultado** | Registro em `dead_letter` com payload, erro, tentativas=3; Alerta logado |
| **Prioridade** | Alto |
| **Tipo** | Integration |
| **RF** | RF-019 |

---

## PEDIDOS - LISTA E DASHBOARD (TC-PED-XXX)

### TC-PED-001: Lista Carrega < 500ms
| Campo | Valor |
|-------|-------|
| **ID** | TC-PED-001 |
| **Título** | Dashboard lista 20 pedidos em < 500ms (p95) |
| **Pré-condições** | 100 pedidos no DB; Usuário logado; Rede 4G simulada |
| **Passos** | 1. Acessar `/dashboard`<br>2. Medir First Contentful Paint + Time to Interactive |
| **Dados** | 100 pedidos variados; Throttling Chrome DevTools "Fast 3G" |
| **Resultado** | FCP < 500ms; TTI < 800ms; Lista renderizada; Skeletons não piscam |
| **Prioridade** | Crítico |
| **Tipo** | E2E + Lighthouse CI |
| **RNF** | RNF-001, RNF-005, RNF-006 |

### TC-PED-002: Filtro Status
| Campo | Valor |
|-------|-------|
| **ID** | TC-PED-002 |
| **Título** | Filtro por status mostra apenas pedidos do status selecionado |
| **Pré-condições** | Pedidos: 5 NOVO, 3 PREPARANDO, 2 PRONTO, 10 ENTREGUE |
| **Passos** | 1. Clicar chip "NOVO"<br>2. Verificar contagem<br>3. Clicar "PREPARANDO"<br>4. Combinar "NOVO" + "PREPARANDO" |
| **Dados** | Chips multi-select |
| **Resultado** | Lista filtrada corretamente; Badges atualizados; URL `?status=novo,preparando` |
| **Prioridade** | Crítico |
| **Tipo** | E2E |
| **RF** | RF-021 |

### TC-PED-003: Filtro Data
| Campo | Valor |
|-------|-------|
| **ID** | TC-PED-003 |
| **Título** | Filtros "Hoje", "Semana", "Mês", "Personalizado" funcionam |
| **Pré-condições** | Pedidos de ontem, hoje, amanhã, semana passada |
| **Passos** | 1. Selecionar "Hoje" → só hoje<br>2. Selecionar "Semana" → Seg-Dom atual<br>3. Custom: De 2025-01-01 Até 2025-01-07 |
| **Dados** | Datas variadas no DB |
| **Resultado** | Lista corresponde ao período; Contadores batem |
| **Prioridade** | Crítico |
| **Tipo** | E2E |
| **RF** | RF-022 |

### TC-PED-004: Busca Cliente
| Campo | Valor |
|-------|-------|
| **ID** | TC-PED-004 |
| **Título** | Busca por nome/telefone encontra pedidos do cliente |
| **Pré-condições** | 3 pedidos de "João Silva" (5511999999999) |
| **Passos** | 1. Digitar "João" no campo busca<br>2. Digitar "99999999" (parte telefone) |
| **Dados** | Query parcial |
| **Resultado** | 3 pedidos do João; Outros ocultos |
| **Prioridade** | Alto |
| **Tipo** | E2E |
| **RF** | RF-023 |

### TC-PED-005: SSE - Novo Pedido Tempo Real
| Campo | Valor |
|-------|-------|
| **ID** | TC-PED-005 |
| **Título** | Novo pedido via webhook aparece no dashboard sem reload |
| **Pré-condições** | Dashboard aberto; SSE conectado (indicador verde) |
| **Passos** | 1. Simular webhook WhatsApp "quero 1 x-burger"<br>2. Observar dashboard |
| **Dados** | Webhook válido → cria pedido #1234 |
| **Resultado** | Badge "Novo" incrementa (animação); Card #1234 aparece no topo (slide-in); Toast "Novo pedido" |
| **Prioridade** | Crítico |
| **Tipo** | E2E |
| **RF** | RF-051, RNF-002 |

### TC-PED-006: SSE - Reconexão Automática
| Campo | Valor |
|-------|-------|
| **ID** | TC-PED-006 |
| **Título** | SSE reconecta automaticamente após queda de rede |
| **Pré-condições** | Dashboard aberto; SSE ativo |
| **Passos** | 1. Desconectar rede (DevTools Offline)<br>2. Aguardar 10s<br>3. Reconectar rede |
| **Dados** | - |
| **Resultado** | Indicador "Reconectando..." → "LIVE" verde; Eventos perdidos recuperados via `last-event-id` |
| **Prioridade** | Alto |
| **Tipo** | E2E |
| **RF** | RF-053 |

---

## PEDIDOS - DETALHE E AÇÕES (TC-DET-XXX)

### TC-DET-001: Detalhe Mostra Info Completa
| Campo | Valor |
|-------|-------|
| **ID** | TC-DET-001 |
| **Título** | Página detalhe mostra itens, total, observações, timeline |
| **Pré-condições** | Pedido #1234 com 2 itens, obs, histórico status |
| **Passos** | 1. Clicar "Ver" no card #1234<br>2. Verificar seções |
| **Dados** | Pedido completo |
| **Resultado** | Header: #1234 + badge status + cliente<br>Itens: 2 linhas com qtd, preço<br>Total: R$ XX,XX<br>Obs: texto completo<br>Timeline: 4 eventos com timestamp + usuário |
| **Prioridade** | Crítico |
| **Tipo** | E2E |
| **RF** | RF-025, RF-026 |

### TC-DET-002: Fluxo Status Completo (Novo → Entregue)
| Campo | Valor |
|-------|-------|
| **ID** | TC-DET-002 |
| **Título** | Lojista consegue avançar pedido: Novo → Preparando → Pronto → Entregue |
| **Pré-condições** | Pedido #1234 status NOVO; Lojista logado |
| **Passos** | 1. Clicar "Iniciar preparo"<br>2. Verificar status PREPARANDO + timestamp<br>3. Clicar "Marcar pronto"<br>4. Verificar status PRONTO<br>5. Clicar "Entregar"<br>6. Verificar status ENTREGUE + sai da lista ativa |
| **Dados** | Botões contextuais |
| **Resultado** | Cada clique: otimistic UI imediato → SSE confirma → timeline atualizado |
| **Prioridade** | Crítico |
| **Tipo** | E2E |
| **RF** | RF-030, RF-032, RF-034 |

### TC-DET-003: Transição Inválida Bloqueada
| Campo | Valor |
|-------|-------|
| **ID** | TC-DET-003 |
| **Título** | Tentar voltar status (Entregue → Preparando) é rejeitado |
| **Pré-condições** | Pedido status ENTREGUE |
| **Passos** | 1. Tentar chamar API `updateStatus` com `PREPARANDO` (via DevTools/Postman) |
| **Dados** | `pedidoId: "entregue-123", status: "PREPARANDO"` |
| **Resultado** | HTTP 400 "Transição inválida: entregue → preparando"; Status permanece ENTREGUE |
| **Prioridade** | Alto |
| **Tipo** | Integration + Unit (state machine) |
| **RF** | RF-033, RN-002, RN-003 |

### TC-DET-004: Cancelar Pedido Novo
| Campo | Valor |
|-------|-------|
| **ID** | TC-DET-004 |
| **Título** | Cancelar pedido NOVO registra motivo e timestamp |
| **Pré-condições** | Pedido #1234 status NOVO |
| **Passos** | 1. Clicar "Cancelar pedido"<br>2. Modal: preencher motivo "Cliente desistiu"<br>3. Confirmar |
| **Dados** | Motivo: "Cliente desistiu" |
| **Resultado** | Status CANCELADO; `canceladoEm` preenchido; `motivoCancelamento` salvo; Histórico registra |
| **Prioridade** | Alto |
| **Tipo** | E2E + Integration |
| **RF** | RF-031 |

### TC-DET-005: Cancelar Preparando - Confirmação Extra
| Campo | Valor |
|-------|-------|
| **ID** | TC-DET-005 |
| **Título** | Cancelar pedido PREPARANDO exige confirmação extra |
| **Pré-condições** | Pedido status PREPARANDO |
| **Passos** | 1. Clicar "Cancelar"<br>2. Modal avisa "Pedido já em preparo. Tem certeza?"<br>3. Cancelar modal → status mantido<br>4. Confirmar → status CANCELADO |
| **Dados** | - |
| **Resultado** | Fluxo com dupla confirmação funciona; Motivo opcional salvo |
| **Prioridade** | Alto |
| **Tipo** | E2E |
| **RF** | RF-031, RN-003 |

---

## COMUNICAÇÃO WHATSAPP (TC-MSG-XXX)

### TC-MSG-001: Enviar Template "Pronto"
| Campo | Valor |
|-------|-------|
| **ID** | TC-MSG-001 |
| **Título** | Lojista envia template "Pronto para retirada" e cliente recebe |
| **Pré-condições** | Pedido #1234 status PRONTO; Template aprovado no Meta; Janela 24h aberta |
| **Passos** | 1. No detalhe, clicar "Responder"<br>2. Clicar template "Pronto para retirada"<br>3. Clicar "Enviar" |
| **Dados** | Template: `status_atualizado` vars: pedido_id=1234, status=Pronto, msg=Pode vir buscar! |
| **Resultado** | API WhatsApp retorna message_id; Toast "Enviado via WhatsApp"; Histórico mostra mensagem enviada |
| **Prioridade** | Crítico |
| **Tipo** | E2E + Integration (mock WhatsApp) |
| **RF** | RF-040, RF-042 |

### TC-MSG-002: Mensagem Customizada
| Campo | Valor |
|-------|-------|
| **ID** | TC-MSG-002 |
| **Título** | Lojista escreve e envia mensagem livre |
| **Pré-condições** | Pedido #1234; Janela 24h aberta |
| **Passos** | 1. "Responder" → "Escrever mensagem"<br>2. Digitar "Seu hambúrguer vai bem passado, certo?"<br>3. Enviar |
| **Dados** | Texto custom 42 chars |
| **Resultado** | Enviado via API WhatsApp (tipo texto); Histórico: "Você: Seu hambúrguer..." |
| **Prioridade** | Alto |
| **Tipo** | E2E |
| **RF** | RF-041 |

### TC-MSG-003: Fora da Janela 24h - Só Template
| Campo | Valor |
|-------|-------|
| **ID** | TC-MSG-003 |
| **Título** | Fora da janela 24h, só templates funcionam (texto livre falha) |
| **Pré-condições** | Última mensagem cliente > 24h |
| **Passos** | 1. Tentar enviar texto livre<br>2. Enviar template aprovado |
| **Dados** | - |
| **Resultado** | Texto livre: erro 131009 (fora janela); Template: sucesso |
| **Prioridade** | Alto |
| **Tipo** | Integration (mock WhatsApp) |
| **RF** | RF-040, RF-041, RN-012 |

### TC-MSG-004: Histórico Mensagens no Detalhe
| Campo | Valor |
|-------|-------|
| **ID** | TC-MSG-004 |
| **Título** | Histórico mostra mensagens entrada/saída com timestamp |
| **Pré-condições** | Pedido com 2 msg entrada (cliente) + 2 saída (lojista) |
| **Passos** | 1. Abrir detalhe → seção "Conversa" |
| **Dados** | Mensagens variadas |
| **Resultado** | Lista cronológica: ícone direção (←/→), tipo badge, conteúdo, hora, status envio |
| **Prioridade** | Alto |
| **Tipo** | E2E |
| **RF** | RF-043 |

---

## RELATÓRIOS (TC-REL-XXX)

### TC-REL-001: Relatório Diário Métricas Corretas
| Campo | Valor |
|-------|-------|
| **ID** | TC-REL-001 |
| **Título** | Relatório "Hoje" calcula faturamento, ticket médio, contadores corretos |
| **Pré-condições** | Hoje: 10 ENTREGUE (R$ 500), 2 PREPARANDO, 1 CANCELADO |
| **Passos** | 1. Acessar `/relatorios` aba "Hoje" |
| **Dados** | Pedidos do dia atual |
| **Resultado** | Cards: Total=13, Entregues=10, Preparando=2, Cancelados=1<br>Faturamento=R$ 500,00<br>Ticket Médio=R$ 50,00 |
| **Prioridade** | Alto |
| **Tipo** | Integration + E2E |
| **RF** | RF-060, RN-026, RN-027 |

### TC-REL-002: Corte Meia-Noite (Timezone)
| Campo | Valor |
|-------|-------|
| **ID** | TC-REL-002 |
| **Título** | Relatório diário corta à meia-noite America/Sao_Paulo |
| **Pré-condições** | Pedido 23:59 hoje + Pedido 00:01 amanhã |
| **Passos** | 1. Ver relatório "Hoje" às 00:05<br>2. Ver relatório "Ontem" |
| **Dados** | Timezone loja: America/Sao_Paulo |
| **Resultado** | Pedido 23:59 → relatório hoje; Pedido 00:01 → relatório amanhã |
| **Prioridade** | Alto |
| **Tipo** | Integration |
| **RF** | RN-028 |

---

## CONFIGURAÇÕES (TC-CFG-XXX)

### TC-CFG-001: CRUD Produtos
| Campo | Valor |
|-------|-------|
| **ID** | TC-CFG-001 |
| **Título** | Criar, editar, desativar, excluir produto |
| **Pré-condições** | Acesso `/configuracoes/produtos` |
| **Passos** | 1. [+] Novo: "X-Teste" R$ 20, Cat "Lanches"<br>2. Ver na lista + sugestão pedido manual<br>3. Editar: preço R$ 22<br>4. Desativar → não sugere mais<br>5. Excluir → removido |
| **Dados** | Produto completo |
| **Resultado** | CRUD completo funcional; Snapshot em pedidos antigos mantido |
| **Prioridade** | Médio |
| **Tipo** | E2E |
| **RF** | RF-070, RF-071, RF-072, RN-010 |

### TC-CFG-002: Horários + Preset
| Campo | Valor |
|-------|-------|
| **ID** | TC-CFG-002 |
| **Título** | Configurar horários via preset + edição manual |
| **Pré-condições** | Acesso `/configuracoes/horarios` |
| **Passos** | 1. Selecionar preset "Lanchonete Noite"<br>2. Ver grid preenchido Seg-Sex 18-23:30, Sáb 18-00, Dom fechado<br>3. Editar Quinta: 18-22<br>4. Salvar |
| **Dados** | Preset + override |
| **Resultado** | Horários salvos; Fora horário → status AGUARDANDO_ABERTURA + auto-resposta |
| **Prioridade** | Médio |
| **Tipo** | E2E + Integration |
| **RF** | RF-080, RF-081, RN-024, RN-025 |

---

## ACESSIBILIDADE (TC-A11Y-XXX)

### TC-A11Y-001: Navegação Só Teclado - Dashboard Completo
| Campo | Valor |
|-------|-------|
| **ID** | TC-A11Y-001 |
| **Título** | Navegar Dashboard → Detalhe → Ações só com teclado |
| **Passos** | 1. Tab: foco no primeiro card<br>2. Enter: abre detalhe<br>3. Tab: foco em "Iniciar preparo"<br>4. Enter: executa ação<br>5. Shift+Tab: volta<br>6. Esc: fecha modal/volta lista |
| **Resultado** | Ordem lógica; Focus visible sempre; Sem armadilhas; Todas ações executáveis |
| **Prioridade** | Crítico |
| **Tipo** | Manual (Checklist) |
| **RNF** | RNF-031, RNF-032 |

### TC-A11Y-002: Screen Reader - Status Anunciado
| Campo | Valor |
|-------|-------|
| **ID** | TC-A11Y-002 |
| **Título** | Mudança de status anunciada por screen reader |
| **Passos** | 1. NVDA/VoiceOver ligado<br>2. Dashboard aberto<br>3. Simular webhook → novo pedido<br>4. Clicar "Iniciar preparo" |
| **Resultado** | "Novo pedido recebido" anunciado (aria-live polite)<br>"Status alterado para Preparando" anunciado |
| **Prioridade** | Alto |
| **Tipo** | Manual |
| **RNF** | RNF-035 |

### TC-A11Y-003: Contraste Cores Status
| Campo | Valor |
|-------|-------|
| **ID** | TC-A11Y-003 |
| **Título** | Badges de status têm contraste 4.5:1 (texto) e 3:1 (borda) |
| **Passos** | 1. Verificar cada badge no Design System Tokens<br>2. Testar no Color Contrast Analyzer |
| **Dados** | 6 status × 2 modos (light/dark) = 12 combinações |
| **Resultado** | Todas ≥ 4.5:1 texto/fundo; ≥ 3:1 borda/fundo |
| **Prioridade** | Crítico |
| **Tipo** | Automated (axe) + Manual |
| **RNF** | RNF-030, RNF-031 |

### TC-A11Y-004: Zoom 200% - Layout Não Quebra
| Campo | Valor |
|-------|-------|
| **ID** | TC-A11Y-004 |
| **Título** | Zoom 200% no navegador mantém usabilidade |
| **Passos** | 1. Ctrl/Cmd + "+" até 200%<br>2. Navegar: Dashboard, Detalhe, Relatórios, Config |
| **Resultado** | Sem scroll horizontal; Textos legíveis; Botões 44px+; Touch targets OK |
| **Prioridade** | Crítico |
| **Tipo** | Manual |
| **RNF** | RNF-030, 1.4.4, 1.4.10 |

---

## PWA (TC-PWA-XXX)

### TC-PWA-001: Install Prompt
| Campo | Valor |
|-------|-------|
| **ID** | TC-PWA-001 |
| **Título** | PWA instalável - prompt "Adicionar à tela inicial" aparece |
| **Pré-condições** | HTTPS; Manifest válido; Service Worker registrado; 2+ visitas |
| **Passos** | 1. Acessar app 2x em dias diferentes<br>2. Verificar prompt nativo do browser |
| **Resultado** | Prompt aparece; Instala como app standalone; Ícone na home screen |
| **Prioridade** | Baixo |
| **Tipo** | Manual (Chrome/Edge/Safari mobile) |
| **RNF** | RNF-045 |

### TC-PWA-002: Offline Fallback
| Campo | Valor |
|-------|-------|
| **ID** | TC-PWA-002 |
| **Título** | Offline mostra última lista cached + aviso |
| **Pré-condições** | PWA instalado; Pelo menos 1 visita online |
| **Passos** | 1. Modo avião / Offline<br>2. Abrir app |
| **Resultado** | Última lista de pedidos exibida (cached); Banner "Você está offline - dados podem estar desatualizados"; Ações enfileiradas localmente |
| **Prioridade** | Baixo |
| **Tipo** | Manual |
| **RNF** | RNF-045 |

---

## Matriz de Rastreabilidade Completa

| RF/RNF | Casos de Teste Cobrindo |
|--------|------------------------|
| RF-001 | TC-AUTH-001, 002, 003 |
| RF-002 | TC-AUTH-004, 005 |
| RF-003 | TC-AUTH-006 |
| RF-004 | TC-AUTH-007 |
| RF-005 | TC-AUTH-008 |
| RF-010 | TC-WHATS-001, 004, 005, 006, 007, 008 |
| RF-011 | TC-WHATS-001, 002, 003 |
| RF-015 | TC-WHATS-005, 006, 007 |
| RF-016 | TC-WHATS-004, TC-PED-005 |
| RF-018 | TC-WHATS-004 |
| RF-019 | TC-WHATS-008 |
| RF-020 | TC-PED-001 |
| RF-021 | TC-PED-002 |
| RF-022 | TC-PED-003 |
| RF-023 | TC-PED-004 |
| RF-025 | TC-DET-001 |
| RF-026 | TC-DET-001 |
| RF-030 | TC-DET-002 |
| RF-031 | TC-DET-004, 005 |
| RF-032 | TC-DET-002 |
| RF-033 | TC-DET-003 |
| RF-034 | TC-DET-002, TC-PED-005 |
| RF-040 | TC-MSG-001, 003 |
| RF-041 | TC-MSG-002, 003 |
| RF-042 | TC-MSG-001 |
| RF-043 | TC-MSG-004 |
| RF-050 | TC-PED-006 |
| RF-051 | TC-PED-005 |
| RF-052 | TC-DET-002 |
| RF-053 | TC-PED-006 |
| RF-060 | TC-REL-001, 002 |
| RF-070 | TC-CFG-001 |
| RF-080 | TC-CFG-002 |
| RNF-001 | TC-PED-001 |
| RNF-002 | TC-PED-005 |
| RNF-011 | TC-WHATS-001, 002, 003 |
| RNF-016 | TC-AUTH-009 |
| RNF-020 | TC-WHATS-004 |
| RNF-030 | TC-A11Y-003, 004 |
| RNF-031 | TC-A11Y-001 |
| RNF-032 | TC-A11Y-001 |
| RNF-035 | TC-A11Y-002 |
| RNF-045 | TC-PWA-001, 002 |

---

## Checklist Execução (Por Release)

### Automatizado (CI - Todo PR)
- [ ] Unit tests: `npm run test` → 100% pass, coverage ≥ targets
- [ ] Integration tests: `npm run test:integration` → 100% pass
- [ ] E2E @smoke: `npx playwright test --grep @smoke` → 100% pass
- [ ] A11y automated: `npx playwright test --grep @a11y` → 0 critical/serious
- [ ] Lighthouse CI: Perf ≥ 90, A11y ≥ 95

### Manual (Semanal / Pré-Release)
- [ ] E2E @core (14 cenários) → 100% pass (chromium + firefox)
- [ ] Teclado only: Dashboard → Detalhe → Ações → Config
- [ ] Screen Reader (NVDA/VoiceOver): Fluxo core
- [ ] Zoom 200%: Todas páginas principais
- [ ] Contraste: Verificar tokens no Design System
- [ ] PWA: Install + Offline fallback

### Load/Security (Sprint 3 / Pré-Produção)
- [ ] k6 load test: 5 cenários → targets atingidos
- [ ] Security: CodeQL, npm audit, secrets scan → 0 high/critical
- [ ] LGPD checklist: Consent, export, delete, retention

---

## Próxima Etapa
[[02 - Modelos/Segurança e Validação/Checklist de Segurança]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/06 - Testes e Qualidade/ETAPA 11 - Executar Testes e Garantir a Qualidade]]
- [[02 - Modelos/Testes/Estratégia de Testes]]
- [[02 - Modelos/Testes/Plano de Testes]]
- [[02 - Modelos/Segurança e Validação/Checklist de Segurança]]
- [[02 - Modelos/Segurança e Validação/Plano de Validação]]
- [[04 - Referência/Glossário/Test Case]]
- [[04 - Referência/Glossário/Test Data]]
- [[04 - Referência/Glossário/Traceability Matrix]]