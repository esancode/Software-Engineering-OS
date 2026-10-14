# Modelo de Dados

> **Copie para:** `05 - Projetos/SeuProjeto/03 - Arquitetura/Modelo de Dados.md`
> Baseado na ETAPA 05. **Diagrama Entidade-Relacionamento + Dicionário de Dados + Prisma Schema.**

---

## Diagrama Entidade-Relacionamento (Mermaid)

```mermaid
erDiagram
    LOJA ||--o{ USUARIO : "tem"
    LOJA ||--o{ PEDIDO : "recebe"
    LOJA ||--o{ PRODUTO : "cataloga"
    LOJA ||--|| CONFIGURACAO_LOJA : "configura"
    LOJA ||--o{ HORARIO_FUNCIONAMENTO : "define"
    LOJA ||--o{ CONSENTIMENTO_LGPD : "registra"
    
    USUARIO }|--o{ PEDIDO_STATUS_HISTORICO : "altera (opcional)"
    
    PEDIDO ||--o{ PEDIDO_ITEM : "contém"
    PEDIDO ||--o{ PEDIDO_STATUS_HISTORICO : "histórico"
    PEDIDO ||--o{ MENSAGEM : "troca"
    
    PEDIDO {
        string id PK "cuid"
        string lojaId FK
        string clienteTelefone "E.164"
        string clienteNome?
        enum status "NOVO|PREPARANDO|PRONTO|ENTREGUE|CANCELADO|AGUARDANDO_ABERTURA"
        json itensJson "snapshot [{nome,qtd,preco,obs}]"
        decimal total? "soma(qtd*preco)"
        text observacoes?
        string whatsappMessageId UK "idempotency key"
        string midiaUrl?
        datetime preparandoEm?
        datetime prontoEm?
        datetime entregueEm?
        datetime canceladoEm?
        string motivoCancelamento?
        datetime createdAt
        datetime updatedAt
        datetime deletedAt? "soft delete LGPD"
    }
    
    PEDIDO_ITEM {
        string id PK
        string pedidoId FK
        string nome
        int quantidade
        decimal precoUnitario?
        string observacoes?
    }
    
    PEDIDO_STATUS_HISTORICO {
        string id PK
        string pedidoId FK
        enum statusAnterior?
        enum statusNovo
        string usuarioId? FK "null se automático"
        string motivo? "se cancelado"
        datetime createdAt
    }
    
    MENSAGEM {
        string id PK
        string pedidoId FK
        enum tipo "TEXTO|TEMPLATE|IMAGEM|LOCALIZACAO"
        enum direcao "ENTRADA|SAIDA"
        text conteudo
        string templateNome?
        string whatsappMessageId?
        string statusEnvio "ENVIADO|FALHOU|LIDO"
        string erroEnvio?
        datetime createdAt
    }
    
    USUARIO {
        string id PK
        string email UK
        string nome?
        string senhaHash? "null se só Magic Link"
        string role "OWNER|FUNCIONARIO"
        string lojaId FK
        datetime createdAt
        datetime updatedAt
        datetime lastLoginAt?
    }
    
    LOJA {
        string id PK
        string nome
        string slug UK
        string telefone UK "E.164"
        string timezone "America/Sao_Paulo"
        datetime createdAt
        datetime updatedAt
        datetime deletedAt?
    }
    
    PRODUTO {
        string id PK
        string lojaId FK
        string nome
        decimal preco
        text descricao?
        string categoria?
        boolean ativo
        datetime createdAt
        datetime updatedAt
    }
    
    CONFIGURACAO_LOJA {
        string id PK
        string lojaId FK UK
        text mensagemFechado?
        text mensagemAbertura?
        json templatesJson? "templates customizados"
        datetime updatedAt
    }
    
    HORARIO_FUNCIONAMENTO {
        string id PK
        string lojaId FK
        int diaSemana "0=Dom...6=Sab"
        string abertura? "HH:MM"
        string fechamento? "HH:MM"
        boolean fechado
    }
    
    CONSENTIMENTO_LGPD {
        string id PK
        string lojaId FK
        string telefone "E.164 titular"
        enum tipo "MARKETING|NECESSARIO|FUNCIONAL"
        boolean concedido
        string versaoPolitica
        string ipOrigem?
        string userAgent?
        datetime createdAt
        datetime revogadoEm?
    }
    
    DEAD_LETTER {
        string id PK
        string lojaId?
        json payload "webhook body original"
        text erro
        int tentativas
        boolean resolvido
        datetime resolvidoEm?
        datetime createdAt
    }
```

---

## Dicionário de Dados

### Tabela: `Loja` (Tenant principal)

| Campo | Tipo | Nullable | Default | Descrição | Validação/Regra |
|-------|------|----------|---------|-----------|-----------------|
| `id` | `String` (CUID) | Não | `cuid()` | PK único | - |
| `nome` | `String` | Não | - | Nome fantasia da loja | 1-100 chars |
| `slug` | `String` | Não | - | URL amigável única | lowercase, kebab-case, 3-50 chars, unique |
| `telefone` | `String` | Não | - | WhatsApp Business da loja | E.164, unique |
| `timezone` | `String` | Não | `America/Sao_Paulo` | Fuso horário para relatórios | IANA tz database |
| `createdAt` | `DateTime` | Não | `now()` | Criação | - |
| `updatedAt` | `DateTime` | Não | `updatedAt` | Última atualização | Auto |
| `deletedAt` | `DateTime` | Sim | `null` | Soft delete LGPD | Se setado, loja inativa |

**Índices:** `@@unique([slug])`, `@@unique([telefone])`, `@@index([slug])`

---

### Tabela: `Usuario` (Acesso ao painel)

| Campo | Tipo | Nullable | Default | Descrição | Validação/Regra |
|-------|------|----------|---------|-----------|-----------------|
| `id` | `String` (CUID) | Não | `cuid()` | PK único | - |
| `email` | `String` | Não | - | Email login (unique global) | Email válido, unique |
| `nome` | `String` | Sim | `null` | Nome exibição | 1-100 chars |
| `senhaHash` | `String` | Sim | `null` | Bcrypt/Argon2 (se Credentials) | Null se só Magic Link |
| `role` | `String` | Não | `OWNER` | `OWNER` ou `FUNCIONARIO` | Enum app-level |
| `lojaId` | `String` | Não | - | FK → Loja | Cascade delete |
| `createdAt` | `DateTime` | Não | `now()` | Criação | - |
| `updatedAt` | `DateTime` | Não | `updatedAt` | Última atualização | Auto |
| `lastLoginAt` | `DateTime` | Sim | `null` | Último login | Atualizado no callback auth |

**Índices:** `@@unique([email])`, `@@index([lojaId])`

**Relacionamento:** `loja` (Loja @relation fields:[lojaId] references:[id] onDelete:Cascade)

---

### Tabela: `Pedido` (Core do negócio)

| Campo | Tipo | Nullable | Default | Descrição | Validação/Regra |
|-------|------|----------|---------|-----------|-----------------|
| `id` | `String` (CUID) | Não | `cuid()` | PK único | - |
| `lojaId` | `String` | Não | - | FK → Loja | Cascade delete |
| `clienteTelefone` | `String` | Não | - | Cliente WhatsApp | E.164 normalized |
| `clienteNome` | `String` | Sim | `null` | Nome se conhecido | 1-100 chars |
| `status` | `PedidoStatus` | Não | `NOVO` | Estado atual | Enum: NOVO, PREPARANDO, PRONTO, ENTREGUE, CANCELADO, AGUARDANDO_ABERTURA |
| `itensJson` | `Json` | Não | `[]` | Snapshot itens | Array[{nome,qtd,preco?,obs?}] |
| `total` | `Decimal(10,2)` | Sim | `null` | Total calculado | Soma(qtd*preco), null se algum item sem preço |
| `observacoes` | `String` | Sim | `null` | Obs livre do cliente | Max 500 chars (RN-034) |
| `whatsappMessageId` | `String` | Não | - | ID msg WhatsApp (idempotência) | **Unique**, format `wamid.*` |
| `midiaUrl` | `String` | Sim | `null` | URL imagem se pedido por foto | HTTPS, Vercel Blob/S3 |
| `preparandoEm` | `DateTime` | Sim | `null` | Timestamp status PREPARANDO | Setado na transição |
| `prontoEm` | `DateTime` | Sim | `null` | Timestamp status PRONTO | Setado na transição |
| `entregueEm` | `DateTime` | Sim | `null` | Timestamp status ENTREGUE | Setado na transição |
| `canceladoEm` | `DateTime` | Sim | `null` | Timestamp status CANCELADO | Setado na transição |
| `motivoCancelamento` | `String` | Sim | `null` | Motivo se cancelado | Max 200 chars |
| `createdAt` | `DateTime` | Não | `now()` | Criação pedido | - |
| `updatedAt` | `DateTime` | Não | `updatedAt` | Última alteração | Auto |
| `deletedAt` | `DateTime` | Sim | `null` | Soft delete LGPD | Anonimiza cliente |

**Índices:**
- `@@index([lojaId, status, createdAt])` — Lista dashboard (filtro status + ordenação data)
- `@@index([lojaId, clienteTelefone, status])` — Busca pedidos do cliente
- `@@unique([whatsappMessageId])` — Idempotência webhook
- `@@index([createdAt])` — Relatórios por período

**Relacionamentos:**
- `loja` (Loja)
- `itens` (PedidoItem[])
- `historico` (PedidoStatusHistorico[])
- `mensagens` (Mensagem[])

---

### Tabela: `PedidoItem` (Normalizado para relatórios)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `pedidoId` | `String` | Não | - | FK → Pedido (Cascade) |
| `nome` | `String` | Não | - | Nome do item (snapshot) |
| `quantidade` | `Int` | Não | - | Qtd ≥ 1 |
| `precoUnitario` | `Decimal(10,2)` | Sim | `null` | Preço no momento do pedido |
| `observacoes` | `String` | Sim | `null` | Obs do item (ex: "sem cebola") |

**Índice:** `@@index([pedidoId])`

> **Regra:** `itensJson` no Pedido = cache para UI. `PedidoItem` = fonte verdade para relatórios/agregações. Mantidos em sync via Prisma middleware/transaction.

---

### Tabela: `PedidoStatusHistorico` (Auditoria completa)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `pedidoId` | `String` | Não | - | FK → Pedido (Cascade) |
| `statusAnterior` | `PedidoStatus` | Sim | `null` | Null se primeiro status (criação) |
| `statusNovo` | `PedidoStatus` | Não | - | Novo status |
| `usuarioId` | `String` | Sim | `null` | FK → Usuario (SetNull se user deletado) |
| `motivo` | `String` | Sim | `null` | Obrigatório se statusNovo = CANCELADO |
| `createdAt` | `DateTime` | Não | `now()` | Timestamp da mudança |

**Índice:** `@@index([pedidoId, createdAt])`

---

### Tabela: `Mensagem` (Histórico comunicação WhatsApp)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `pedidoId` | `String` | Não | - | FK → Pedido (Cascade) |
| `tipo` | `MensagemTipo` | Não | - | TEXTO, TEMPLATE, IMAGEM, LOCALIZACAO |
| `direcao` | `MensagemDirecao` | Não | - | ENTRADA (cliente), SAIDA (lojista) |
| `conteudo` | `String` | Não | - | Texto da mensagem |
| `templateNome` | `String` | Sim | `null` | Nome template Meta (se TEMPLATE) |
| `whatsappMessageId` | `String` | Sim | `null` | ID retornado pela API WhatsApp |
| `statusEnvio` | `String` | Não | `ENVIADO` | ENVIADO, FALHOU, LIDO |
| `erroEnvio` | `String` | Sim | `null` | Detalhe erro se FALHOU |
| `createdAt` | `DateTime` | Não | `now()` | Timestamp |

**Índices:** `@@index([pedidoId, createdAt])`, `@@index([whatsappMessageId])`

---

### Tabela: `Produto` (Catálogo opcional)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `lojaId` | `String` | Não | - | FK → Loja (Cascade) |
| `nome` | `String` | Não | - | Nome produto |
| `preco` | `Decimal(10,2)` | Não | - | Preço atual |
| `descricao` | `String` | Sim | `null` | Descrição |
| `categoria` | `String` | Sim | `null` | Categoria (livre, autocomplete) |
| `ativo` | `Boolean` | Não | `true` | Se false, não sugere |
| `createdAt` | `DateTime` | Não | `now()` | - |
| `updatedAt` | `DateTime` | Não | `updatedAt` | - |

**Índices:** `@@index([lojaId, ativo, categoria])`, `@@unique([lojaId, nome])`

---

### Tabela: `ConfiguracaoLoja` (Settings 1:1)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `lojaId` | `String` | Não | - | FK → Loja (Unique, Cascade) |
| `mensagemFechado` | `String` | Sim | `null` | Msg automática fora horário |
| `mensagemAbertura` | `String` | Sim | `null` | Msg quando abre |
| `templatesJson` | `Json` | Sim | `null` | Templates customizados loja |
| `updatedAt` | `DateTime` | Não | `updatedAt` | - |

---

### Tabela: `HorarioFuncionamento` (Recorrente semanal)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `lojaId` | `String` | Não | - | FK → Loja (Cascade) |
| `diaSemana` | `Int` | Não | - | 0=Domingo ... 6=Sábado |
| `abertura` | `String` | Sim | `null` | "HH:MM" (24h) ou null |
| `fechamento` | `String` | Sim | `null` | "HH:MM" ou null |
| `fechado` | `Boolean` | Não | `false` | True = dia todo fechado |

**Constraints:** `@@unique([lojaId, diaSemana])` — Um registro por dia por loja

---

### Tabela: `ConsentimentoLGPD` (Compliance)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `lojaId` | `String` | Não | - | FK → Loja |
| `telefone` | `String` | Não | - | Titular (E.164) |
| `tipo` | `ConsentimentoTipo` | Não | - | MARKETING, NECESSARIO, FUNCIONAL |
| `concedido` | `Boolean` | Não | `true` | True = consentiu |
| `versaoPolitica` | `String` | Não | - | Ex: "2025-01-15-v1" |
| `ipOrigem` | `String` | Sim | `null` | IP no momento consentimento |
| `userAgent` | `String` | Sim | `null` | UA no momento |
| `createdAt` | `DateTime` | Não | `now()` | - |
| `revogadoEm` | `DateTime` | Sim | `null` | Se revogado |

**Índices:** `@@index([lojaId, telefone, tipo])`, `@@unique([lojaId, telefone, tipo, versaoPolitica])`

---

### Tabela: `DeadLetter` (Falhas processamento)

| Campo | Tipo | Nullable | Default | Descrição |
|-------|------|----------|---------|-----------|
| `id` | `String` (CUID) | Não | `cuid()` | PK |
| `lojaId` | `String` | Sim | `null` | Loja se identificável |
| `payload` | `Json` | Não | - | Body webhook original |
| `erro` | `String` | Não | - | Mensagem erro/exception |
| `tentativas` | `Int` | Não | `0` | Tentativas de reprocessamento |
| `resolvido` | `Boolean` | Não | `false` | Se já tratado manualmente |
| `resolvidoEm` | `DateTime` | Sim | `null` | Timestamp resolução |
| `createdAt` | `DateTime` | Não | `now()` | - |

**Índice:** `@@index([lojaId, resolvido, createdAt])`

---

## Prisma Schema Completo (Copiável)

Ver arquivo: `prisma/schema.prisma` no projeto.

Principais pontos:
- `generator client` com `previewFeatures = ["jsonProtocol"]` se necessário
- `datasource db` com `provider = "postgresql"` e `url = env("DATABASE_URL")`
- Enums definidos no schema (não no DB nativo) para portabilidade
- Middleware `softDelete` em `Pedido`, `Loja`, `Usuario` (opcional)
- `@@index` estrategicamente colocados para queries do dashboard/relatórios

---

## Queries Críticas (Performance)

### 1. Dashboard: Lista paginada com contadores por status
```sql
-- Prisma: findMany + count por status
SELECT p.*, 
       COUNT(CASE WHEN p.status = 'NOVO' THEN 1 END) OVER() as count_novo,
       COUNT(CASE WHEN p.status = 'PREPARANDO' THEN 1 END) OVER() as count_preparando,
       ...
FROM Pedido p
WHERE p.lojaId = $1 
  AND p.deletedAt IS NULL
  AND ($2::text IS NULL OR p.status = ANY($2))
  AND ($3::timestamptz IS NULL OR p.createdAt >= $3)
ORDER BY p.createdAt DESC
LIMIT 20 OFFSET $4;
```

### 2. Detalhe pedido com itens, histórico, mensagens
```sql
-- Prisma: include { itens: true, historico: true, mensagens: true }
SELECT * FROM Pedido WHERE id = $1 AND deletedAt IS NULL;
SELECT * FROM PedidoItem WHERE pedidoId = $1 ORDER BY id;
SELECT * FROM PedidoStatusHistorico WHERE pedidoId = $1 ORDER BY createdAt;
SELECT * FROM Mensagem WHERE pedidoId = $1 ORDER BY createdAt;
```

### 3. Relatório diário (agregação)
```sql
SELECT 
  COUNT(*) as total_pedidos,
  COUNT(*) FILTER (WHERE status = 'ENTREGUE') as entregues,
  COUNT(*) FILTER (WHERE status = 'CANCELADO') as cancelados,
  COALESCE(SUM(total) FILTER (WHERE status = 'ENTREGUE'), 0) as faturamento,
  CASE WHEN COUNT(*) FILTER (WHERE status = 'ENTREGUE') > 0
       THEN COALESCE(SUM(total) FILTER (WHERE status = 'ENTREGUE'), 0) / COUNT(*) FILTER (WHERE status = 'ENTREGUE')
       ELSE 0 END as ticket_medio
FROM Pedido
WHERE lojaId = $1 
  AND deletedAt IS NULL
  AND createdAt >= $2  -- inicio dia
  AND createdAt < $3;  -- fim dia (exclusive)
```

---

## Migrações e Versionamento

| Comando | Uso |
|---------|-----|
| `prisma migrate dev` | Desenvolvimento (cria migration + aplica + seed) |
| `prisma migrate deploy` | CI/CD (aplica migrations pendentes) |
| `prisma migrate status` | Ver migrations aplicadas/pendentes |
| `prisma db pull` | Sincronizar schema com DB existente (engenharia reversa) |
| `prisma generate` | Regenerar Prisma Client após schema change |

**Convenção nome migration:** `YYYYMMDD_HHMMSS_descricao_curta`
Ex: `20250115_143000_add_pedido_status_historico`

---

## Seeds de Desenvolvimento

```typescript
// prisma/seed.ts
await prisma.loja.upsert({
  where: { slug: 'demo' },
  update: {},
  create: {
    nome: 'Lanches do João',
    slug: 'demo',
    telefone: '5511999999999',
    usuarios: { create: { email: 'joao@demo.com', nome: 'João', role: 'OWNER' }},
    produtos: { createMany: { data: [
      { nome: 'X-Burger', preco: 25.00, categoria: 'Lanches', ativo: true },
      { nome: 'Batata Frita', preco: 15.00, categoria: 'Acompanhamentos', ativo: true },
      { nome: 'Refrigerante', preco: 8.00, categoria: 'Bebidas', ativo: true },
    ]}},
    horarios: { createMany: { data: [
      { diaSemana: 1, abertura: '18:00', fechamento: '23:30' }, // Seg
      { diaSemana: 2, abertura: '18:00', fechamento: '23:30' }, // Ter
      { diaSemana: 3, abertura: '18:00', fechamento: '23:30' }, // Qua
      { diaSemana: 4, abertura: '18:00', fechamento: '23:30' }, // Qui
      { diaSemana: 5, abertura: '18:00', fechamento: '00:00' }, // Sex
      { diaSemana: 6, abertura: '18:00', fechamento: '00:00' }, // Sab
      { diaSemana: 0, fechado: true }, // Dom
    ]}},
  },
});
```

---

## Checklist de Validação do Modelo

- [ ] ERD desenhado e versionado (Mermaid no repo)
- [ ] Dicionário de dados completo (todas tabelas, campos, regras)
- [ ] Prisma schema válido (`prisma validate`)
- [ ] Migrações geradas e testadas (`prisma migrate dev`)
- [ ] Seeds funcionam (`prisma db seed`)
- [ ] Índices cobrem queries críticas (dashboard, relatórios, busca, idempotência)
- [ ] Soft delete implementado (LGPD)
- [ ] Snapshot itens (itensJson + PedidoItem normalizado)
- [ ] Constraints: unique (whatsappMessageId, email, slug, telefone loja, produto nome/loja)
- [ ] Enums para status, tipos mensagem, direção, consentimento
- [ ] Relacionamentos com delete rules corretos (Cascade, SetNull)
- [ ] Timezone consistente (America/Sao_Paulo em Loja, UTC no banco)

---

## Próxima Etapa
[[02 - Modelos/Requisitos e Arquitetura/Registro de Integrações]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/03 - Arquitetura e Planejamento/ETAPA 05 - Planejar a Arquitetura e a Solução Técnica]]
- [[02 - Modelos/Requisitos e Arquitetura/Visão Técnica]]
- [[02 - Modelos/Requisitos e Arquitetura/ADR - Registro de Decisões Arquiteturais]]
- [[02 - Modelos/Requisitos e Arquitetura/Registro de Integrações]]
- [[02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth]]
- [[05 - Projetos/Exemplo-PedidosSimples/03 - Arquitetura/ADR-002-modelo-de-dados]]
- [[04 - Referência/Glossário/ERD]]
- [[04 - Referência/Glossário/Prisma]]
- [[04 - Referência/Glossário/Soft Delete]]