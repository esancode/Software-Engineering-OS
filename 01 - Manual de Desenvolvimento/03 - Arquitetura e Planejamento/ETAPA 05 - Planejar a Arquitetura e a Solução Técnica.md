# ETAPA 05 — Planejar a Arquitetura e a Solução Técnica

## Objetivo

Decidir **como** o software será estruturado tecnicamente antes de construir partes importantes: tech stack, dados, APIs, decisões arquiteturais, riscos técnicos.

## Por que esta etapa existe?

Decisões técnicas tardias ou implícitas geram retrabalho, dívida técnica, incompatibilidades e surpresas caras. Esta etapa torna as decisões explícitas, justificadas e rastreáveis.

## Quando começar

- ETAPA 04 concluída (Requisitos aprovados, critérios de aceitação definidos)
- Você sabe O QUÊ construir — agora define COMO

## O que você precisa fazer

1. **Escolha a abordagem técnica geral** — monólito, serverless, SPA+API, mobile-first, PWA, etc.
2. **Compare alternativas** — para cada decisão principal, liste 2-3 opções com prós/contras
3. **Escolha linguagens e ferramentas** — frontend, backend, banco, deploy, CI/CD
4. **Defina arquitetura de dados** — modelo conceitual, banco escolhido, migrações
5. **Planeje integrações** — APIs externas, webhooks, autenticação terceiros
6. **Defina autenticação e autorização** — estratégia, tokens, sessões, roles
7. **Planeje armazenamento de arquivos** — se aplicável (local, S3, CDN)
8. **Estruture o código** — pastas, módulos, convenções, separação de responsabilidades
9. **Defina ambientes** — dev, staging, production; variáveis de ambiente; segredos
10. **Escolha estratégia de deploy** — plataforma, processo, rollback
11. **Planeje backups e recuperação** — se dados críticos
12. **Identifique riscos técnicos principais** — desconhecidos, dependências, performance
13. **Registre decisões arquiteturais importantes (ADRs)**
14. **Produza os documentos: Decisões de Arquitetura + Visão Técnica + Modelo de Dados (se aplicável) + Registro de Integrações**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Escolher abordagem geral | 👤 USUÁRIO (com sugestão da IA) |
| Comparar alternativas técnicas | 🤖 IA (pesquisa e sintetiza) + 👤 USUÁRIO (decide) |
| Escolher linguagens/ferramentas | 👤 USUÁRIO (considerando experiência da equipe=você) |
| Modelar dados | 🤖 IA (você valida) |
| Planejar integrações | 🤖 IA + 👤 USUÁRIO |
| Definir auth/autorização | 👤 USUÁRIO (decisão de segurança) |
| Estruturar código | 🤖 IA (sugere baseado em boas práticas) |
| Definir ambientes/deploy | 👤 USUÁRIO + 🤖 IA |
| Registrar ADRs | 👤 USUÁRIO (você decide, IA documenta) |
| Validar viabilidade técnica | ✅ VALIDAÇÃO |

## Ferramentas recomendadas

| Finalidade | Ferramentas |
|------------|-------------|
| Diagramas arquiteturais | Mermaid.js (no Obsidian), Excalidraw, draw.io |
| ADRs (Architecture Decision Records) | Markdown simples (modelo no manual) |
| Comparação de tech stack | Planilha ou tabela Markdown |
| Modelagem de dados | Mermaid ERD, dbdiagram.io, Prisma schema |
| Gestão de segredos | .env.local (dev), plataforma (prod) — nunca no git |

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Decisões de Arquitetura (ADRs) | `02 - Modelos/Requisitos e Arquitetura/Decisões de Arquitetura (ADR).md` | `05 - Projetos/SeuProjeto/03 - Arquitetura/` |
| Visão Técnica Geral | `02 - Modelos/Requisitos e Arquitetura/Visão Técnica.md` | `05 - Projetos/SeuProjeto/03 - Arquitetura/` |

### Condicionais
| Documento | Quando | Modelo |
|-----------|--------|--------|
| Modelo de Dados | Se houver banco de dados | `02 - Modelos/Requisitos e Arquitetura/Modelo de Dados.md` |
| Registro de Integrações | Se houver APIs externas | `02 - Modelos/Requisitos e Arquitetura/Registro de Integrações.md` |
| Estratégia de Auth Detalhada | Se auth complexo (OAuth, MFA, SSO) | `02 - Modelos/Requisitos e Arquitetura/Estratégia de Auth.md` |

### Opcionais
- Diagrama de Arquitetura (Mermaid/Excalidraw)
- Plano de Migração de Dados (se migrando sistema legado)
- Análise de Custos de Infraestrutura

## Como produzir os documentos

### Decisões de Arquitetura (ADRs)

**Formato por ADR:**
```markdown
# ADR-001: Escolha do Backend — Node.js/TypeScript vs Python/FastAPI vs Go

## Status
Aceito

## Contexto
Precisamos de API REST + WebSocket para tempo real. Equipe de 1 pessoa (eu). Conhecimento: JS/TS forte, Python médio, Go básico. Deploy em Vercel/Render.

## Decisão
**Node.js + TypeScript + Fastify** (ou Next.js API routes se full-stack)

## Justificativa
| Critério | Node/TS | Python/FastAPI | Go |
|----------|---------|----------------|-----|
| Minha produtividade | Alta | Média | Baixa |
| Ecossistema WhatsApp API | Excelente | Bom | Médio |
| Deploy serverless (Vercel) | Nativo | Precisa container | Precisa container |
| Type safety | TypeScript nativo | Pydantic (bom) | Nativo |
| WebSocket | Socket.io nativo | Precisa lib extra | Precisa lib extra |
| Curva de aprendizado | Zero | Baixa | Média |

## Consequências
- Positivas: Velocidade máxima, type safety, deploy simples, ecossistema maduro
- Negativas: Single-threaded (mitigado com worker threads se needed), cold start serverless

## Alternativas Consideradas
- Python/FastAPI: Boa opção se foco em ML/dados no futuro
- Go: Melhor performance, mas curva de aprendizado não justifica para MVP

## Referências
- Doc WhatsApp Cloud API (Node SDK oficial)
- Vercel docs: Node.js serverless functions
```

### Visão Técnica Geral

```markdown
# Visão Técnica — PedidosSimples

## Abordagem Geral
**Full-stack TypeScript (Next.js 14 App Router)**
- Frontend: React 18 + Next.js 14 (App Router) + Tailwind CSS
- Backend: Next.js API Routes + Server Actions
- Banco: PostgreSQL (Neon serverless) + Prisma ORM
- Tempo real: Server-Sent Events (SSE) ou Pusher/Ably
- Auth: NextAuth.js (Credentials + Magic Link)
- Deploy: Vercel (frontend + API) + Neon (DB)
- CI/CD: GitHub Actions

## Estrutura de Pastas (Next.js App Router)
```
src/
├── app/
│   ├── (auth)/           # Rotas de auth (login, register)
│   ├── (dashboard)/      # Rotas protegidas (dashboard, pedidos)
│   │   ├── pedidos/
│   │   │   ├── [id]/
│   │   │   └── page.tsx
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── api/
│   │   ├── webhooks/
│   │   │   └── whatsapp/route.ts
│   │   ├── pedidos/route.ts
│   │   └── auth/
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx         # Landing page
├── components/
│   ├── ui/              # Componentes base (Button, Input, Card, etc.)
│   ├── pedidos/         # Componentes específicos de pedidos
│   └── layout/          # Header, Sidebar, etc.
├── lib/
│   ├── prisma.ts        # Cliente Prisma singleton
│   ├── auth.ts          # Config NextAuth
│   ├── whatsapp.ts      # Cliente WhatsApp Business API
│   ├── validations/     # Schemas Zod
│   └── utils.ts
├── hooks/               # Custom React hooks
├── types/               # Types TypeScript globais
└── styles/              # CSS adicional se needed
```

## Modelo de Dados (Prisma Schema)
```prisma
// Ver documento Modelo de Dados separado
```

## Variáveis de Ambiente
| Variável | Dev | Staging | Prod | Descrição |
|----------|-----|---------|------|-----------|
| DATABASE_URL | ✅ | ✅ | ✅ | Conexão PostgreSQL |
| NEXTAUTH_SECRET | ✅ | ✅ | ✅ | Segredo NextAuth (gerar com `openssl rand -base64 32`) |
| NEXTAUTH_URL | ✅ | ✅ | ✅ | URL base da aplicação |
| WHATSAPP_API_TOKEN | ✅ | ✅ | ✅ | Token WhatsApp Cloud API |
| WHATSAPP_PHONE_NUMBER_ID | ✅ | ✅ | ✅ | Phone Number ID do WhatsApp |
| WHATSAPP_WEBHOOK_VERIFY_TOKEN | ✅ | ✅ | ✅ | Token verificação webhook |
| PUSHER_APP_ID / ABLY_KEY | ❌ | ✅ | ✅ | Tempo real (opcional v1) |

## Decisões Principais (Resumo dos ADRs)
| ADR | Decisão |
|-----|---------|
| ADR-001 | Backend: Next.js API Routes (TypeScript) |
| ADR-002 | Banco: PostgreSQL + Prisma ORM |
| ADR-003 | Auth: NextAuth.js (Credentials + Email Magic Link) |
| ADR-004 | Tempo real: SSE nativo (v1), migrar para Pusher se escalar |
| ADR-005 | Deploy: Vercel + Neon (serverless, zero ops) |
| ADR-006 | Estilo: Tailwind CSS + shadcn/ui components |
| ADR-007 | Validação: Zod (schema compartilhado client/server) |

## Riscos Técnicos
| Risco | Prob. | Impacto | Mitigação |
|-------|-------|---------|-----------|
| WhatsApp API rate limits | Média | Alto | Queue local + retry exponential backoff |
| Cold start Vercel (API) | Alta | Médio | Manter funções quentes (cron ping) ou aceitar |
| Prisma cold start serverless | Média | Médio | Connection pooling (PgBouncer via Neon) |
| SSE escalabilidade | Baixa (v1) | Médio | Migrar para Pusher/Ably no v2 |
| Schema changes (migrações) | Média | Alto | Prisma Migrate + revisão manual em prod |
```

### Modelo de Dados (se aplicável)

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  passwordHash  String?   // Null se magic link only
  role          Role      @default(OWNER)
  establishment Establishment? @relation(fields: [establishmentId], references: [id])
  establishmentId String? @unique
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  sessions      Session[]
  magicLinks    MagicLink[]
}

model Establishment {
  id              String   @id @default(cuid())
  name            String
  phoneNumber     String   @unique // WhatsApp Business number
  whatsappToken   String   @unique // Encrypted
  webhookVerifyToken String @unique
  businessHours   Json?    // { "mon": {"open": "09:00", "close": "18:00", "closed": false}, ... }
  timezone        String   @default("America/Sao_Paulo")
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt
  user            User?    @relation(fields: [id], references: [establishmentId])
  products        Product[]
  orders          Order[]
}

model Product {
  id              String   @id @default(cuid())
  name            String
  description     String?
  priceCents      Int      // Armazenar em centavos
  isActive        Boolean  @default(true)
  establishmentId String
  establishment   Establishment @relation(fields: [establishmentId], references: [id], onDelete: Cascade)
  orderItems      OrderItem[]
  createdAt       DateTime @default(now())
  updatedAt       DateTime @updatedAt

  @@index([establishmentId])
}

model Order {
  id              String       @id @default(cuid())
  externalId      String?      @unique // WhatsApp message ID
  customerName    String
  customerPhone   String
  status          OrderStatus  @default(NEW)
  totalCents      Int
  notes           String?
  establishmentId String
  establishment   Establishment @relation(fields: [establishmentId], references: [id], onDelete: Cascade)
  items           OrderItem[]
  createdAt       DateTime     @default(now())
  updatedAt       DateTime     @updatedAt

  @@index([establishmentId])
  @@index([status])
  @@index([createdAt])
}

model OrderItem {
  id         String  @id @default(cuid())
  quantity   Int
  unitPriceCents Int
  productId  String?
  product    Product? @relation(fields: [productId], references: [id], onDelete: SetNull)
  orderId    String
  order      Order   @relation(fields: [orderId], references: [id], onDelete: Cascade)
  createdAt  DateTime @default(now())
}

enum Role {
  OWNER
  STAFF
}

enum OrderStatus {
  NEW
  PREPARING
  READY
  DELIVERED
  OUT_OF_HOURS
  CANCELLED
}
```

### Registro de Integrações

| Integração | Tipo | Auth | Endpoint Base | Webhook | Rate Limit | Fallback |
|------------|------|------|---------------|---------|------------|----------|
| WhatsApp Cloud API | REST + Webhook | Bearer Token | `https://graph.facebook.com/v18.0/` | `/api/webhooks/whatsapp` | 1000 req/min | Queue local + retry |
| NextAuth (Email) | SMTP | API Key | Resend/SendGrid | N/A | 100/hora | Log local |
| Pusher/Ably (v2) | WebSocket | Token | API respective | N/A | Conforme plano | SSE nativo |

## Como trabalhar com IA nesta etapa

**Prompt para comparar alternativas:**
```
Preciso decidir: [decisão técnica, ex: banco de dados].
Meu contexto: [projeto, equipe=1, conhecimento, requisitos de qualidade da ETAPA 04].
Compare 3 opções viáveis em tabela: Critério | Opção A | Opção B | Opção C.
Critérios: minha produtividade, custo, escalabilidade v1, manutenção, deploy, curva aprendizado.
Não recomende — só compare. Eu decido.
```

**Prompt para gerar ADR:**
```
Tomei esta decisão: [descreva].
Contexto: [resumo].
Alternativas consideradas: [liste].
Justificativa principal: [explique].
Gere o ADR no formato padrão do manual (Status, Contexto, Decisão, Justificativa tabela, Consequências, Alternativas, Referências).
```

**Prompt para modelo de dados (Prisma):**
```
Tenho estes requisitos funcionais: [liste RFs principais].
Entidades identificadas: [liste].
Relacionamentos: [descreva].
Gere schema Prisma completo com: models, enums, indexes, relations, cascades apropriados.
Use convenções: id CUID, timestamps, soft delete se needed, nomes em inglês singular.
```

**Prompt para visão técnica:**
```
Com base nos ADRs decididos: [liste ADRs].
Gere o documento Visão Técnica com: abordagem geral, estrutura pastas Next.js, variáveis ambiente (tabela dev/staging/prod), resumo decisões, riscos técnicos.
Formato Markdown pronto para salvar.
```

**Contexto a fornecer:** Requisitos aprovados + pesquisa + seu nível de conhecimento técnico.
**Resultado esperado:** ADRs registrados + Visão Técnica + Modelo de Dados (se BD) + Integrações.
**Como verificar:** Cada decisão principal tem ADR? Tech stack cobre todos os requisitos? Modelo de dados suporta RFs? Variáveis de ambiente listadas?

## Checklist de execução

- [ ] Abordagem técnica geral definida e justificada
- [ ] Pelo menos 2 alternativas comparadas para cada decisão principal (backend, banco, auth, deploy, tempo real)
- [ ] Linguagens e ferramentas escolhidas (frontend, backend, banco, deploy, CI/CD, estilo, validação)
- [ ] Modelo de dados criado (se banco) — Prisma schema ou equivalente
- [ ] Integrações mapeadas com auth, endpoints, webhooks, rate limits, fallbacks
- [ ] Auth e autorização definidas (estratégia, tokens, sessões, roles)
- [ ] Armazenamento de arquivos planejado (se aplicável)
- [ ] Estrutura de código definida (pastas, módulos, convenções)
- [ ] Ambientes definidos (dev, staging, prod) + variáveis + segredos
- [ ] Estratégia de deploy escolhida + processo + rollback
- [ ] Backups/recuperação planejados (se dados críticos)
- [ ] Riscos técnicos identificados com mitigação
- [ ] ADRs registrados para decisões principais (mínimo 5-7)
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Visão Técnica completa e coerente
- [ ] ADRs para todas as decisões arquiteturais importantes
- [ ] Modelo de dados (se aplicável) suporta todos os RFs da ETAPA 04
- [ ] Integrações mapeadas com fallbacks
- [ ] Variáveis de ambiente listadas por ambiente
- [ ] Riscos técnicos têm mitigação plausível
- [ ] Você consegue explicar a arquitetura em 2 minutos
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste da rastreabilidade:** Cada RF da ETAPA 04 tem suporte na arquitetura?
- **Teste da viabilidade:** Consegue implementar um "Hello World" deployado em < 1 hora com este stack?
- **Teste dos segredos:** Nenhum segredo real nos documentos (use placeholders)?
- **Teste do rollback:** Sabe exatamente como voltar versão anterior em produção?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Escolher tech por "hype" não por adequação | Use tabela de comparação com SEUS critérios |
| Subestimar complexidade de auth/tempo real | Comece simples (NextAuth + SSE), evolua depois |
| Não planejar migrações de banco | Prisma Migrate + revisão manual = padrão |
| Hardcoded segredos nos docs | SEMPRE placeholders: `WHATSAPP_TOKEN=***` |
| Ignorar cold start serverless | Teste cedo. Aceite ou mitigue (ping, edge, container) |
| Modelo de dados não cobre RFs | Valide: cada entidade atende a qual RF? |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Descobre requisito técnico novo | Adicione ADR, atualize Visão Técnica, avalie impacto no MVP |
| Tech stack escolhida não suporta requisito | ADR novo documentando mudança + justificativa |
| Modelo de dados precisa mudar | Prisma: `prisma migrate dev` — documente no ADR |
| Integração externa muda/fecha | Fallback já planejado? Se não, ADR de emergência |
| Performance não atende RQ | Profile primeiro. Otimize query/index. Depois reavalie stack. |

## Condições para avançar

- ✅ Visão Técnica + ADRs + Modelo de Dados (se BD) salvos
- [ ] Tech stack definida e viável para MVP
- [ ] Variáveis de ambiente por ambiente listadas
- [ ] Riscos técnicos principais com mitigação
- [ ] Você sabe iniciar o projeto tecnicamente amanhã

## Próxima etapa

[[ETAPA 06 - Planejar a Experiência do Usuário (UX)]]

## Materiais relacionados

- [[02 - Modelos/Requisitos e Arquitetura/Decisões de Arquitetura (ADR)]]
- [[02 - Modelos/Requisitos e Arquitetura/Visão Técnica]]
- [[02 - Modelos/Requisitos e Arquitetura/Modelo de Dados]]
- [[02 - Modelos/Requisitos e Arquitetura/Registro de Integrações]]
- [[03 - Guias Práticos/Vibe Coding/Como Escolher Tech Stack]]
- [[04 - Referência/Glossário/ADR]]
- [[04 - Referência/Glossário/Tech Stack]]
- [[04 - Referência/Glossário/Serverless]]