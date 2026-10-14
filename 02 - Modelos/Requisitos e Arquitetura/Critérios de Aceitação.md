# Critérios de Aceitação

> **Copie para:** `05 - Projetos/SeuProjeto/02 - Produto/Critérios de Aceitação.md`
> Baseado na ETAPA 04. Um critério por requisito funcional, no formato **Gherkin (Given/When/Then)**.

---

## Formato do Critério de Aceitação
- **Um cenário por regra de negócio** (não agrupe múltiplas regras)
- **Dado/Quando/Então** em português
- **Dados concretos** (não "um usuário", mas "usuário 'joao@email.com'")
- **Resultado verificável** (o que aparece na tela, no banco, no log)
- **Independente de implementação** (testa comportamento, não código)

---

## Autenticação e Acesso (RF-001 a RF-005)

### RF-001: Login via Magic Link
```gherkin
Funcionalidade: Login via Magic Link
  Como um usuário novo ou retornando
  Eu quero receber um link mágico por email
  Para acessar o sistema sem senha

Cenário: Solicitar magic link com email válido
  Dado que estou na página de login
  E o email "joao@loja.com" não está cadastrado
  Quando preencho o email "joao@loja.com"
  E clico em "Enviar link mágico"
  Então vejo a mensagem "Verifique seu email para acessar"
  E um email é enviado para "joao@loja.com" com subject "Seu link de acesso ao PedidosSimples"
  E o link contém um token válido de 32 caracteres
  E o token expira em 15 minutos

Cenário: Acessar via magic link válido
  Dado que recebi um magic link por email
  E o token não expirou
  E o token não foi usado antes
  Quando clico no link
  Então sou redirecionado para a página inicial (/dashboard)
  E estou autenticado como "joao@loja.com"
  E um cookie de sessão HttpOnly; Secure; SameSite=Lax é definido
  E o token é invalidado (não pode ser reutilizado)

Cenário: Tentar acessar com magic link expirado
  Dado que tenho um magic link gerado há 20 minutos
  Quando clico no link
  Então vejo a mensagem "Link expirado. Solicite um novo."
  E sou redirecionado para a página de login
  E não estou autenticado

Cenário: Tentar reutilizar magic link já usado
  Dado que já usei um magic link para entrar
  Quando tento usar o mesmo link novamente
  Então vejo a mensagem "Link já utilizado. Solicite um novo."
  E sou redirecionado para a página de login
```

### RF-002: Login via Credentials (Email/Senha)
```gherkin
Funcionalidade: Login via Email e Senha
  Como um usuário que prefere senha
  Eu quero entrar com email e senha
  Para acessar o sistema

Cenário: Login bem-sucedido com credenciais válidas
  Dado que tenho conta com email "joao@loja.com" e senha "SenhaForte123"
  Quando preencho email "joao@loja.com"
  E preencho senha "SenhaForte123"
  E clico em "Entrar"
  Então sou redirecionado para /dashboard
  E estou autenticado
  E cookie de sessão é definido

Cenário: Falha com senha incorreta
  Dado que tenho conta com email "joao@loja.com"
  Quando preencho email "joao@loja.com"
  E preencho senha "SenhaErrada"
  E clico em "Entrar"
  Então vejo a mensagem "Email ou senha incorretos"
  E permaneço na página de login
  E não estou autenticado

Cenário: Falha com email inexistente
  Dado que não existe conta com "naoexiste@email.com"
  Quando preencho email "naoexiste@email.com"
  E preencho senha "qualquer"
  E clico em "Entrar"
  Então vejo a mensagem "Email ou senha incorretos"
  E permaneço na página de login
  (Mensagem genérica para não vazar existência de conta)
```

### RF-003: Renovação de Sessão (Refresh Token)
```gherkin
Funcionalidade: Renovação Automática de Sessão
  Como um usuário logado
  Eu quero minha sessão renovada automaticamente
  Para não ser desconectado durante o uso

Cenário: Sessão renovada transparentemente
  Dado que estou autenticado há 55 minutos
  E o access token expira em 60 minutos
  Quando faço uma requisição para API (/api/pedidos)
  Então a requisição succeeds (200)
  E um novo access token é emitido (via cookie ou header)
  E não sou redirecionado para login
```

### RF-004: Logout
```gherkin
Funcionalidade: Logout
  Como um usuário autenticado
  Eu quero sair do sistema
  Para encerrar minha sessão

Cenário: Logout bem-sucedido
  Dado que estou autenticado
  Quando clico em "Sair" no menu do usuário
  Então sou redirecionado para a página de login
  E o cookie de sessão é removido (expirado)
  E o refresh token no banco é invalidado
  E se tentar acessar /dashboard, sou redirecionado para login
```

### RF-005: Proteção de Rotas Privadas
```gherkin
Funcionalidade: Proteção de Rotas Autenticadas
  Como o sistema
  Eu quero bloquear acesso não autenticado a rotas privadas
  Para proteger dados dos usuários

Cenário: Acessar rota privada sem autenticação
  Dado que não estou autenticado
  Quando tento acessar "/dashboard" diretamente na URL
  Então sou redirecionado para "/login?redirect=/dashboard"
  E vejo a mensagem "Faça login para acessar esta página"

Cenário: Acessar API privada sem autenticação
  Dado que não estou autenticado
  Quando faço GET /api/pedidos
  Então recebo status 401 Unauthorized
  E body: { "error": "Não autenticado" }
```

---

## Recebimento de Pedidos via WhatsApp (RF-010 a RF-019)

### RF-010: Receber Webhook WhatsApp
```gherkin
Funcionalidade: Receber Webhook do WhatsApp Business API
  Como o sistema
  Eu quero receber notificações de novas mensagens
  Para processar pedidos automaticamente

Cenário: Webhook recebido com mensagem de texto válida
  Dado que o WhatsApp envia POST /api/webhook/whatsapp
  Com header "X-Hub-Signature-256" válido (HMAC SHA256)
  E body JSON contendo:
    """
    {
      "entry": [{
        "changes": [{
          "value": {
            "messages": [{
              "id": "wamid.12345",
              "from": "5511999999999",
              "type": "text",
              "text": { "body": "Quero 2 x-burger e 1 batata grande" },
              "timestamp": "1700000000"
            }]
          }
        }]
      }]
    }
    """
  Quando o webhook é processado
  Então retorna HTTP 200 OK em < 5s
  E a mensagem é enfileirada para processamento assíncrono
  E log registra: "Webhook recebido: message_id=wamid.12345 from=5511999999999"
```

### RF-011: Validar Assinatura HMAC
```gherkin
Funcionalidade: Validação de Assinatura Webhook
  Como o sistema
  Eu quero rejeitar webhooks não assinados pelo WhatsApp
  Para evitar injeção de pedidos falsos

Cenário: Webhook com assinatura válida é aceito
  Dado um payload JSON válido
  E header "X-Hub-Signature-256: sha256=abc123..." calculado corretamente com APP_SECRET
  Quando o webhook é recebido
  Então o processamento continua normalmente

Cenário: Webhook sem assinatura é rejeitado
  Dado um payload JSON válido
  E header "X-Hub-Signature-256" ausente
  Quando o webhook é recebido
  Então retorna HTTP 401 Unauthorized
  E log: "Webhook rejeitado: assinatura ausente"

Cenário: Webhook com assinatura inválida é rejeitado
  Dado um payload JSON válido
  E header "X-Hub-Signature-256: sha256=assinatura_errada"
  Quando o webhook é recebido
  Então retorna HTTP 401 Unauthorized
  E log: "Webhook rejeitado: assinatura inválida"
```

### RF-012: Processar Mensagem de Texto
```gherkin
Funcionalidade: Processamento de Mensagem de Texto
  Como o sistema
  Eu quero extrair pedido de mensagem de texto livre
  Para criar pedido estruturado

Cenário: Mensagem de texto com pedido claro cria pedido
  Dado mensagem: "Quero 2 x-burger (R$ 25 cada) e 1 batata grande (R$ 15)"
  E cliente "5511999999999" não tem pedido "novo" aberto
  Quando a mensagem é processada
  Então um pedido é criado com:
    | Campo | Valor |
    | status | "novo" |
    | cliente_telefone | "5511999999999" |
    | itens | [{"nome":"x-burger","qtd":2,"preco":25},{"nome":"batata grande","qtd":1,"preco":15}] |
    | total | 65.00 |
    | observacoes | "" |
  E status do pedido é "novo"

Cenário: Mensagem sem padrão claro cria pedido com observação
  Dado mensagem: "Oi, gostaria de fazer um pedido por favor"
  Quando a mensagem é processada
  Então um pedido é criado com:
    | status | "novo" |
    | itens | [] |
    | observacoes | "Oi, gostaria de fazer um pedido por favor" |
  E notificação interna: "Pedido sem itens identificados - revisar manualmente"
```

### RF-013: Processar Mensagem de Mídia (Should)
```gherkin
Funcionalidade: Processamento de Imagem (Pedido por Foto)
  Como o sistema
  Eu quero receber fotos de pedidos (cardápio, anotação)
  Para permitir pedidos visuais

Cenário: Imagem recebida é salva e associada ao pedido
  Dado mensagem tipo "image" com media_id "mid.123"
  E cliente "5511999999999"
  Quando a mensagem é processada
  Então a imagem é baixada da API WhatsApp
  E salva no storage (ex: Vercel Blob / S3)
  E um pedido é criado com:
    | status | "novo" |
    | itens | [] |
    | observacoes | "Pedido por imagem: [URL da imagem]" |
    | midia_url | "[URL pública da imagem]" |
```

### RF-015: Extração de Dados Estruturados
```gherkin
Funcionalidade: Extração de Dados de Texto Livre
  Como o sistema
  Eu quero parsear linguagem natural para itens estruturados
  Para não exigir formato rígido do cliente

Cenário: Extração com múltiplos itens e quantidades
  Dado texto: "3 hamburguer artesanal, 2 refri coca zero, 1 batata"
  Quando o extrator processa
  Então retorna lista:
    [
      { "nome": "hamburguer artesanal", "quantidade": 3, "preco_estimado": null },
      { "nome": "refri coca zero", "quantidade": 2, "preco_estimado": null },
      { "nome": "batata", "quantidade": 1, "preco_estimado": null }
    ]

Cenário: Extração com preços mencionados
  Dado texto: "2 x-salada 30 reais cada e 1 suco 8 reais"
  Quando o extrator processa
  Então retorna:
    [
      { "nome": "x-salada", "quantidade": 2, "preco_estimado": 30 },
      { "nome": "suco", "quantidade": 1, "preco_estimado": 8 }
    ]

Cenário: Texto sem padrão reconhecível
  Dado texto: "Oi tudo bem"
  Quando o extrator processa
  Então retorna lista vazia []
  E flag "precisa_revisao_humana": true
```

### RF-016: Criar Pedido com Status "Novo"
```gherkin
Funcionalidade: Criação de Pedido Inicial
  Como o sistema
  Eu quero criar registro de pedido com status "novo"
  Para aparecer na lista do lojista

Cenário: Pedido criado e persistido no banco
  Dado dados extraídos válidos (cliente, itens, total)
  Quando o pedido é criado
  Então registro na tabela "pedidos" com:
    | Campo | Valor |
    | id | UUID gerado |
    | cliente_telefone | "5511999999999" |
    | status | "novo" |
    | itens_json | [itens extraídos] |
    | total | 65.00 |
    | created_at | timestamp atual |
    | updated_at | timestamp atual |
    | whatsapp_message_id | "wamid.12345" |
  E evento "pedido_criado" é emitido para SSE
```

### RF-017: Responder ACK < 5s
```gherkin
Funcionalidade: Resposta Rápida ao WhatsApp
  Como o sistema
  Eu quero responder ACK ao WhatsApp em < 5s
  Para evitar reenvio (WhatsApp reenvia após 20s sem ACK)

Cenário: ACK retornado dentro do prazo
  Dado webhook recebido
  Quando o handler processa
  Então HTTP 200 OK é retornado em < 5000ms
  (Processamento pesado deve ser assíncrono - queue)
```

### RF-018: Idempotência de Webhook
```gherkin
Funcionalidade: Idempotência - Webhook Duplicado
  Como o sistema
  Eu quero ignorar webhooks duplicados (mesmo message_id)
  Para não criar pedidos duplicados

Cenário: Mesmo message_id recebido duas vezes cria 1 pedido
  Dado webhook com message_id "wamid.12345" já processado
  Quando o mesmo webhook é recebido novamente
  Então retorna HTTP 200 OK
  E nenhum novo pedido é criado
  E log: "Webhook duplicado ignorado: message_id=wamid.12345"
  E contagem de pedidos do cliente não muda

Cenário: Message_id diferente do mesmo cliente cria novo pedido
  Dado pedido existente de "5511999999999" com message_id "wamid.111"
  Quando webhook chega com message_id "wamid.222" (nova msg)
  Então novo pedido é criado (status "novo")
  E cliente tem 2 pedidos "novo"
```

### RF-019: Dead Letter Queue
```gherkin
Funcionalidade: Falha de Processamento - Dead Letter
  Como o sistema
  Eu quero registrar falhas para investigação
  Para não perder pedidos com erro

Cenário: Erro no processamento vai para dead letter
  Dado webhook que causa exceção no parser (ex: JSON malformado interno)
  Quando o processamento falha após 3 tentativas (retry)
  Então registro em tabela "dead_letter" com:
    | Campo | Valor |
    | payload | JSON original |
    | erro | mensagem da exceção |
    | tentativas | 3 |
    | created_at | timestamp |
  E alerta interno: "Dead letter: webhook falhou após 3 tentativas"
```

---

## Lista e Visualização de Pedidos (RF-020 a RF-027)

### RF-020: Listar Pedidos com Paginação
```gherkin
Funcionalidade: Lista de Pedidos Paginação
  Como um lojista
  Eu quero ver pedidos paginados
  Para navegar no histórico sem lentidão

Cenário: Primeira página carrega 20 pedidos
  Dado 50 pedidos no banco (status variados)
  E estou autenticado
  Quando acesso /dashboard (página 1)
  Então vejo 20 pedidos (mais recentes primeiro)
  E controles de paginação: "Página 1 de 3"
  E botão "Próxima" habilitado

Cenário: Navegar para página 2
  Dado estou na página 1
  Quando clico "Próxima"
  Então vejo pedidos 21-40
  E URL atualiza para "?page=2"
  E botão "Anterior" habilitado
```

### RF-021: Filtrar por Status
```gherkin
Funcionalidade: Filtro por Status
  Como um lojista
  Eu quero filtrar pedidos por status
  Para focar no que preciso fazer agora

Cenário: Filtrar apenas "novo" e "preparando"
  Dado pedidos com status: novo (5), preparando (3), pronto (2), entregue (10)
  Quando seleciono filtro "Novo" e "Preparando"
  Então vejo apenas 8 pedidos
  E badge "Novo: 5" e "Preparando: 3" atualizados
  E URL: "?status=novo,preparando"
```

### RF-022: Filtrar por Data
```gherkin
Funcionalidade: Filtro por Período
  Como um lojista
  Eu quero ver pedidos de hoje/semana/mês
  Para fechar caixa e relatórios

Cenário: Filtro "Hoje" mostra apenas pedidos de hoje
  Dado pedidos de ontem, hoje, amanhã (futuro agendado)
  Quando seleciono "Hoje"
  Então vejo apenas pedidos com created_at entre 00:00 e 23:59 de hoje
  E contagem bate com badge

Cenário: Filtro personalizado (data início/fim)
  Dado pedidos de vários dias
  Quando preencho "De: 2025-01-01" "Até: 2025-01-07"
  E clico "Aplicar"
  Então vejo pedidos nesse intervalo (inclusivo)
```

### RF-027: Performance Lista < 500ms
```gherkin
Funcionalidade: Performance de Carregamento da Lista
  Como um lojista
  Eu quero a lista carregar rápido
  Para não esperar atendendo clientes

Cenário: Lista carrega em < 500ms (p95)
  Dado 100 pedidos no banco
  E rede 4G simulada (throttling Chrome DevTools)
  Quando acesso /dashboard
  Então métrica "Time to First Byte" < 200ms
  E "First Contentful Paint" < 500ms
  E lista renderizada e interativa < 800ms
  (Medido via Lighthouse CI no pipeline)
```

---

## Atualização de Status (RF-030 a RF-034)

### RF-030: Mudança de Status (Fluxo Normal)
```gherkin
Funcionalidade: Atualização de Status do Pedido
  Como um lojista
  Eu quero mudar status do pedido com botões rápidos
  Para acompanhar o fluxo da cozinha

Cenário: Fluxo completo novo → preparando → pronto → entregue
  Dado pedido ID "ped-123" com status "novo"
  Quando clico botão "Iniciar preparo"
  Então status muda para "preparando"
  E timestamp "preparando_em" = agora
  E usuário "joao@loja.com" registrado
  E evento SSE "status_atualizado" emitido

  Quando clico botão "Marcar pronto"
  Então status muda para "pronto"
  E timestamp "pronto_em" = agora

  Quando clico botão "Entregar"
  Então status muda para "entregue"
  E timestamp "entregue_em" = agora
  E pedido sai da lista ativa (filtro padrão exclui "entregue")
```

### RF-031: Cancelar Pedido
```gherkin
Funcionalidade: Cancelamento de Pedido
  Como um lojista
  Eu quero cancelar pedido que não será feito
  Para não contar no relatório

Cenário: Cancelar pedido "novo"
  Dado pedido "ped-123" status "novo"
  Quando clico "Cancelar" e confirmo
  Então status muda para "cancelado"
  E motivo_cancelamento = "Cliente desistiu" (opcional, prompt)
  E timestamp "cancelado_em" = agora

Cenário: Cancelar pedido "preparando" (com confirmação extra)
  Dado pedido "ped-123" status "preparando"
  Quando clico "Cancelar"
  Então vejo modal: "Pedido já em preparo. Tem certeza?"
  Se confirmo → status "cancelado", motivo registrado
  Se cancelo → status permanece "preparando"
```

### RF-032: Timestamp e Usuário em Mudanças
```gherkin
Funcionalidade: Auditoria de Mudança de Status
  Como o sistema
  Eu quero registrar quem e quando mudou o status
  Para rastreabilidade

Cenário: Cada mudança registra auditoria
  Dado pedido "ped-123" status "novo"
  Quando usuário "maria@loja.com" muda para "preparando"
  Então registro em tabela "pedido_status_historico":
    | pedido_id | status_anterior | status_novo | usuario_id | timestamp |
    | ped-123   | novo            | preparando  | maria@...  | 2025-01-15 10:30:00 |
```

### RF-033: Impedir Transições Inválidas
```gherkin
Funcionalidade: Validação de Transições de Status
  Como o sistema
  Eu quero impedir mudanças de status impossíveis
  Para manter integridade do fluxo

Cenário: Não permitir "entregue" → "preparando"
  Dado pedido status "entregue"
  Quando tento mudar para "preparando" (via API direta ou bug UI)
  Então erro 400: "Transição inválida: entregue → preparando"
  E status permanece "entregue"

Cenário: Matriz de transições permitidas
  Dado as transições válidas:
    | De | Para (Permitidos) |
    | novo | preparando, cancelado |
    | preparando | pronto, cancelado |
    | pronto | entregue, cancelado |
    | entregue | (nenhuma) |
    | cancelado | (nenhuma) |
  Qualquer transição fora desta matriz é rejeitada
```

### RF-034: SSE Tempo Real (Should)
```gherkin
Funcionalidade: Atualização Tempo Real via SSE
  Como um lojista
  Eu quero ver novos pedidos e mudanças na hora
  Para não precisar recarregar a página

Cenário: Novo pedido aparece sem reload
  Dado aba /dashboard aberta
  E conexão SSE estabelecida (EventSource)
  Quando webhook cria novo pedido
  Então evento "novo_pedido" recebido via SSE
  E novo item aparece no topo da lista (animação slide-in)
  E badge "Novo: X" incrementa

Cenário: Mudança de status reflete em tempo real
  Dado pedido "ped-123" visível na lista status "novo"
  Quando outro usuário (ou mesma sessão em outra aba) muda para "preparando"
  Então evento "status_atualizado" recebido
  E badge do pedido muda cor/texto para "Preparando"
  E contadores de status atualizam
```

---

## Resposta ao Cliente (RF-040 a RF-043)

### RF-040: Enviar Mensagem via WhatsApp (Template)
```gherkin
Funcionalidade: Envio de Template WhatsApp
  Como um lojista
  Eu quero responder cliente com template aprovado
  Para agilizar e garantir conformidade WhatsApp

Cenário: Enviar template "status_atualizado"
  Dado pedido "ped-123" status mudou para "pronto"
  E template "status_atualizado" aprovado no Meta:
    "Seu pedido *{{pedido_id}}* está *{{status}}*! {{mensagem_personalizada}}"
  Quando clico "Avisar cliente" → escolho template "status_atualizado"
  E preencho mensagem_personalizada: "Pode vir buscar!"
  E confirmo envio
  Então API WhatsApp chamada com template
  E mensagem enviada para telefone do cliente
  E log: "Template enviado: status_atualizado para 5511999999999"
  E no detalhe do pedido aparece: "Enviado: Seu pedido #ped-123 está Pronto! Pode vir buscar!"
```

### RF-041: Enviar Mensagem Customizada
```gherkin
Funcionalidade: Mensagem Livre WhatsApp
  Como um lojista
  Eu quero escrever mensagem personalizada
  Para casos não cobertos por templates

Cenário: Enviar mensagem customizada
  Dado pedido "ped-123" aberto no detalhe
  Quando clico "Responder" → digito "Seu hambúrguer vai bem passado, certo?"
  E confirmo
  Então mensagem de texto enviada via API WhatsApp
  E aparece no histórico do pedido: "Você: Seu hambúrguer vai bem passado, certo?"
```

### RF-042: Templates Pré-definidos
```gherkin
Funcionalidade: Templates de Resposta Rápida
  Como um lojista
  Eu quero templates prontos para situações comuns
  Para responder em segundos

Cenário: Lista de templates disponíveis
  Dado que abro "Responder cliente" no pedido
  Então vejo botões de template:
    - "Pedido confirmado" → "Seu pedido #{{id}} foi confirmado! Tempo estimado: {{tempo}} min"
    - "Em preparo" → "Seu pedido #{{id}} está sendo preparado 🍳"
    - "Pronto para retirada" → "Seu pedido #{{id}} está pronto! Pode vir buscar 📦"
    - "Entregue" → "Seu pedido #{{id}} foi entregue. Bom apetite! 🍔"
    - "Atraso" → "Seu pedido #{{id}} terá atraso de {{min}} min. Peço desculpas!"
    - "Faltando item" → "Item {{item}} indisponível no seu pedido #{{id}}. Substituir por {{opcao}}?"
```

---

## Tempo Real (RF-050 a RF-053)

### RF-050: Conexão SSE
```gherkin
Funcionalidade: Conexão Server-Sent Events
  Como o sistema
  Eu quero manter conexão persistente por usuário
  Para push de atualizações em tempo real

Cenário: Conexão SSE estabelecida ao carregar dashboard
  Dado usuário autenticado acessa /dashboard
  Quando a página carrega
  Então conexão EventSource aberta para /api/events?user_id=...
  E header "Content-Type: text/event-stream"
  E heartbeat (comentário ":") a cada 30s para manter viva

Cenário: Reconexão automática em queda
  Dado conexão SSE ativa
  Quando rede cai por 10s
  Então EventSource reconecta automaticamente (nativo do browser)
  E eventos perdidos durante queda são recuperados via "last-event-id"
```

### RF-051: Evento Novo Pedido via SSE
```gherkin
Funcionalidade: Broadcast de Novo Pedido
  Como o sistema
  Eu quero notificar todos usuários da loja sobre novo pedido
  Para equipe ver na hora

Cenário: Novo pedido dispara evento para usuários da loja
  Dado webhook cria pedido "ped-456" para loja "loja-123"
  E 2 usuários logados na loja-123 (aba aberta com SSE)
  Quando pedido é persistido
  Então evento SSE enviado para ambas conexões:
    "event: novo_pedido\ndata: {\"pedido_id\":\"ped-456\",\"status\":\"novo\",\"cliente\":\"5511...\",\"total\":65}\n\n"
  E ambas UIs atualizam lista
```

---

## Relatórios (RF-060 a RF-062)

### RF-060: Relatório Diário
```gherkin
Funcionalidade: Relatório Diário
  Como um lojista
  Eu quero ver resumo do dia
  Para fechar caixa

Cenário: Relatório diário mostra métricas corretas
  Dado pedidos de hoje:
    - 10 entregues (total R$ 500)
    - 2 preparando
    - 1 cancelado
  Quando abro "Relatório do Dia"
  Então vejo:
    | Métrica | Valor |
    | Total pedidos | 13 |
    | Entregues | 10 |
    | Preparando | 2 |
    | Cancelados | 1 |
    | Faturamento (entregues) | R$ 500,00 |
    | Ticket médio | R$ 50,00 |
```

---

## Produtos e Configurações (RF-070 a RF-081)

### RF-070: Cadastrar Produto
```gherkin
Funcionalidade: Cadastro de Produto
  Como um lojista
  Eu quero cadastrar meus produtos
  Para agilizar criação de pedidos manuais e sugestões

Cenário: Cadastro completo de produto
  Dado estou em "Configurações > Produtos"
  Quando clico "Novo produto"
  E preencho:
    | Nome | X-Burger Especial |
    | Preço | 28.00 |
    | Descrição | Hambúrguer 180g, queijo, bacon, salada especial |
    | Categoria | Lanches |
    | Ativo | Sim |
  E salvo
  Então produto criado no banco
  E aparece na lista de produtos
  E fica disponível para sugestão em pedido manual
```

### RF-080: Configurar Horário de Funcionamento
```gherkin
Funcionalidade: Horário de Funcionamento
  Como um lojista
  Eu quero definir quando estou aberto
  Para clientes saberem e sistema bloquear pedidos fora do horário

Cenário: Configurar horário semanal
  Dado estou em "Configurações > Horários"
  Quando defino:
    | Dia | Abertura | Fechamento | Fechado? |
    | Seg-Sex | 18:00 | 23:30 | Não |
    | Sáb | 18:00 | 00:00 | Não |
    | Dom | - | - | Sim |
  E salvo
  Então horários salvos no banco

Cenário: Fora do horário mostra "Fechado" e bloqueia
  Dado horário configurado: Seg-Sex 18:00-23:30
  E agora é Terça 15:00
  Quando cliente envia mensagem WhatsApp
  Então webhook processa
  Mas resposta automática: "Estamos fechados! Abrimos às 18:00. Seu pedido será atendido quando abrirmos."
  E pedido fica com status "aguardando_abertura" (não "novo")
```

---

## Checklist de Qualidade dos Critérios

- [ ] Cada RF tem pelo menos 1 cenário Gherkin
- [ ] Cenários cobrem: caminho feliz, borda, erro, permissão
- [ ] Dados concretos (IDs, valores, timestamps)
- [ ] Resultado verificável (o que vê, o que persiste, o que emite)
- [ ] Independente de implementação (não menciona React, Prisma, etc.)
- [ ] Linguagem ubíqua do domínio (pedido, status, cliente, lojista)
- [ ] Rastreabilidade: ID do RF no título do cenário

---

## Próxima Etapa
[[02 - Modelos/Requisitos e Arquitetura/Regras de Negócio]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/02 - Produto e Requisitos/ETAPA 04 - Organizar os Requisitos]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos Funcionais]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Regras de Negócio]]
- [[04 - Referência/Glossário/Gherkin]]
- [[04 - Referência/Glossário/Critério de Aceitação]]
- [[04 - Referência/Glossário/BDD]]