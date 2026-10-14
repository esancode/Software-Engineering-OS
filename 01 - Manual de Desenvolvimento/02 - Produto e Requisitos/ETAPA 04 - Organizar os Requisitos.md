# ETAPA 04 — Organizar os Requisitos

## Objetivo

Definir o **comportamento esperado do software** de forma clara, verificável e compreensível — antes de implementar. Transformar funcionalidades em requisitos testáveis.

## Por que esta etapa existe?

Requisitos vagos geram retrabalho, bugs, discussões infinitas e software que "quase funciona". Requisitos claros permitem que a IA implemente certo na primeira vez e que você valide o resultado.

## Quando começar

- ETAPA 03 concluída (Produto definido, MVP e priorização aprovados)
- Você sabe exatamente quais funcionalidades "Must" estão no MVP
- Pronto para especificar o comportamento de cada uma

## O que você precisa fazer

1. **Liste requisitos funcionais** — o que o sistema DEVE fazer (um por funcionalidade Must/Should)
2. **Escreva requisitos de qualidade (não-funcionais)** — performance, usabilidade, segurança, confiabilidade
3. **Documente regras de negócio** — lógicas, cálculos, validações, exceções
4. **Defina tipos de usuário e permissões** — quem faz o quê
5. **Desenhe fluxos principais** — caminho feliz de cada feature
6. **Defina estados vazios, carregamento, erro** — para cada tela/fluxo
7. **Escreva mensagens de erro e validações** — textos exatos quando possível
8. **Defina tratamento de dados** — entrada, armazenamento, saída, retenção
9. **Escreva critérios de aceitação** — para cada requisito: "Dado X, Quando Y, Então Z"
10. **Liste dependências externas** — APIs, serviços, bibliotecas críticas
11. **Registre requisitos de acessibilidade e compatibilidade** — navegadores, dispositivos, WCAG
12. **Produza o documento: Requisitos Aprovados**

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Escrever requisitos funcionais (baseado em features) | 🤖 IA (você revisa e aprova) |
| Definir requisitos de qualidade | 👤 USUÁRIO (com sugestão da IA) |
| Documentar regras de negócio | 👤 USUÁRIO (você conhece o domínio) |
| Definir permissões/tipos de usuário | 👤 USUÁRIO |
| Desenhar fluxos principais | 🤖 IA + 👤 USUÁRIO |
| Definir estados vazios/carregamento/erro | 🤖 IA (você valida UX) |
| Escrever critérios de aceitação (Gherkin) | 🤖 IA (você aprova) |
| Validar completude e clareza | ✅ VALIDAÇÃO |

## Ferramentas recomendadas

- **Obsidian** — documentos
- **Claude Code** — redigir requisitos, sugerir critérios Gherkin, gerar fluxos em Mermaid
- **Notação Gherkin (Given/When/Then)** — para critérios de aceitação
- **Mermaid.js** — para fluxogramas nos documentos

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Requisitos Funcionais | `02 - Modelos/Requisitos e Arquitetura/Requisitos Funcionais.md` | `05 - Projetos/SeuProjeto/02 - Produto/` |
| Requisitos de Qualidade | `02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade.md` | `05 - Projetos/SeuProjeto/02 - Produto/` |
| Critérios de Aceitação | `02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação.md` | `05 - Projetos/SeuProjeto/02 - Produto/` |

### Condicionais
- Regras de Negócio (se houver lógica complexa)
- Registro de Permissões (se múltiplos perfis)
- Modelo de Dados (se banco de dados — ver ETAPA 05)
- Requisitos de Acessibilidade (se público externo/produção)
- Requisitos de Compatibilidade (navegadores, dispositivos alvo)

### Opcionais
- Glossário de Domínio (termos específicos do negócio)
- Matriz de Rastreabilidade Feature-Requisito-Teste

## Como produzir os documentos

### Requisitos Funcionais

**Formato por requisito:**
```markdown
## RF-001: Receber pedido via WhatsApp

**Descrição:** O sistema deve receber notificações de novos pedidos via WhatsApp Business API e criar registro de pedido.

**Origem:** F1 (Must) - Definição do Produto

**Prioridade:** Must

**Fluxo Principal:**
1. Cliente envia mensagem para número WhatsApp Business
2. Webhook recebe payload
3. Sistema parseia mensagem (itens, quantidades, observações)
4. Cria pedido com status "novo"
5. Notifica interface em tempo real

**Dados de Entrada:**
- phone_number (string, E.164)
- message_text (string)
- timestamp (ISO 8601)
- message_id (string, único)

**Dados de Saída:**
- Pedido criado com ID único
- Status: "novo"
- Itens parseados (nome, qtd, preço unitário se informado)
- Observações

**Regras de Negócio:**
- RN-001: Ignorar mensagens não reconhecidas como pedido (logar para revisão)
- RN-002: Agrupar mensagens do mesmo cliente em 5 min como um pedido se não fechado
- RN-003: Rejeitar pedidos fora do horário de funcionamento (configurável)

**Estados de Interface:**
- Vazio: "Nenhum pedido novo" + botão "Ver histórico"
- Carregando: Skeleton da lista + "Atualizando..."
- Erro: "Falha ao conectar WhatsApp. Verifique conexão." + botão "Tentar novamente"

**Critérios de Aceitação:** Ver documento Critérios de Aceitação (CA-RF-001)
```

### Requisitos de Qualidade (Não-Funcionais)

| ID | Categoria | Requisito | Métrica/Verificação | Prioridade |
|----|-----------|-----------|---------------------|------------|
| RQ-001 | Performance | Tempo de resposta da lista de pedidos | < 500ms (p95) | Must |
| RQ-002 | Performance | Sincronização WhatsApp → interface | < 2 segundos | Must |
| RQ-003 | Disponibilidade | Uptime do sistema | > 99% (exceto manutenção) | Should |
| RQ-004 | Usabilidade | Interface mobile-first, touch-friendly | Testado em 360px+ | Must |
| RQ-005 | Usabilidade | Acessibilidade WCAG 2.1 AA | Contraste, navegação teclado, labels | Should |
| RQ-006 | Segurança | Dados em trânsito criptografados | TLS 1.2+ | Must |
| RQ-007 | Segurança | Autenticação obrigatória | Sessão expira em 24h | Must |
| RQ-008 | Confiabilidade | Dados não perdidos em falha | Persistência local + sync | Must |
| RQ-009 | Compatibilidade | Navegadores suportados | Chrome 120+, Safari 17+, Firefox 120+ | Must |
| RQ-010 | Compatibilidade | Dispositivos | Mobile (iOS 16+, Android 13+), Desktop | Must |

> [!tip]
> **Diferencie funcionalidade de qualidade:**
> - Funcionalidade: "O sistema envia email" (RF)
> - Qualidade: "O email é enviado em < 3s" (RQ - performance)
> - Qualidade: "O email não expõe dados sensíveis no log" (RQ - segurança)

### Critérios de Aceitação (Formato Gherkin)

```gherkin
# CA-RF-001: Receber pedido via WhatsApp

Funcionalidade: Recebimento de pedidos via WhatsApp

  Cenário: Pedido válido recebido e processado
    Dado que o WhatsApp Business está configurado e conectado
    E o estabelecimento está aberto
    Quando uma mensagem de pedido válido é recebida via webhook
    Então um novo pedido é criado com status "novo"
    E o pedido aparece na lista em menos de 2 segundos
    E os itens são parseados corretamente (nome, quantidade, preço)

  Cenário: Mensagem não reconhecida como pedido
    Dado que o WhatsApp Business está configurado
    Quando uma mensagem não estruturada é recebida
    Então a mensagem é logada para revisão manual
    E nenhum pedido é criado
    E o sistema continua funcionando normalmente

  Cenário: Pedido fora do horário de funcionamento
    Dado que o estabelecimento está fechado (fora do horário configurado)
    Quando um pedido é recebido
    Então o pedido é criado com status "fora_horario"
    E uma resposta automática é enviada ao cliente informando horário
    E o pedido aparece na lista com badge "Fora de horário"
```

### Regras de Negócio (exemplo)

| ID | Regra | Descrição | Exceções |
|----|-------|-----------|----------|
| RN-001 | Agrupamento de mensagens | Mensagens do mesmo cliente em 5 min = 1 pedido se anterior não fechado | Cliente envia "cancelar" → fecha anterior |
| RN-002 | Horário de funcionamento | Pedidos fora do horário ficam em "fora_horario" | Configurável por dia da semana |
| RN-003 | Parsing de itens | Formato: "2x Hamburger - R$ 25" ou "2 Hamburger 25" | Se preço não informado, usa cadastro |
| RN-004 | Status transitions | novo → preparando → pronto → entregue | Não pode voltar (exceto admin) |

## Como trabalhar com IA nesta etapa

**Prompt para gerar requisitos funcionais:**
```
Tenho estas funcionalidades Must/Should do MVP: [liste da ETAPA 03].
Para cada uma, gere um requisito funcional no formato:
- ID (RF-XXX)
- Descrição clara
- Fluxo principal (passos numerados)
- Dados de entrada/saída (tipos)
- Regras de negócio referenciadas
- Estados de interface (vazio, carregando, erro)
- Link para critério de aceitação

Use linguagem simples. Não invente regras — só estruture.
```

**Prompt para critérios Gherkin:**
```
Para o requisito RF-001: [cole descrição].
Escreva critérios de aceitação em Gherkin (Given/When/Then) cobrindo:
- Caminho feliz
- Pelo menos 2 caminhos alternativos/erro
- Casos de borda óbvios
Formato: feature file completo.
```

**Prompt para requisitos de qualidade:**
```
Meu produto: [descrição]. Público: [persona]. Tech stack provável: [da ETAPA 05].
Sugira requisitos de qualidade (não-funcionais) relevantes nas categorias:
Performance, Usabilidade, Segurança, Confiabilidade, Compatibilidade, Acessibilidade.
Para cada um: ID, descrição, métrica verificável, prioridade (Must/Should/Could).
Não liste genéricos — foque no que importa para ESTE produto.
```

**Contexto a fornecer:** Definição do Produto + Escopo + Priorização + pesquisa.
**Resultado esperado:** 3+ documentos de requisitos completos e aprovados.
**Como verificar:** Cada requisito funcional tem critério de aceitação? Requisitos de qualidade têm métrica? Regras de negócio cobrem exceções?

## Checklist de execução

- [ ] Requisitos funcionais para TODAS as features Must/Should do MVP
- [ ] Cada RF tem: descrição, fluxo, dados entrada/saída, regras, estados UI, link CA
- [ ] Requisitos de qualidade cobrem: performance, usabilidade, segurança, confiabilidade, compatibilidade
- [ ] Regras de negócio documentadas com ID, descrição, exceções
- [ ] Tipos de usuário e permissões definidos
- [ ] Fluxos principais desenhados (Mermaid ou texto)
- [ ] Estados vazios, carregamento, erro definidos por tela/fluxo
- [ ] Mensagens de erro e validações escritas (textos reais quando possível)
- [ ] Tratamento de dados especificado (entrada, armazenamento, saída, retenção)
- [ ] Critérios de aceitação em Gherkin para cada RF
- [ ] Dependências externas listadas
- [ ] Acessibilidade e compatibilidade registradas
- [ ] Documentos salvos na pasta do projeto

## Critérios de conclusão

- [ ] Documento Requisitos Funcionais completo
- [ ] Documento Requisitos de Qualidade completo
- [ ] Documento Critérios de Aceitação completo (Gherkin)
- [ ] Cada requisito funcional tem pelo menos 1 critério de aceitação
- [ ] Requisitos de qualidade têm métricas verificáveis
- [ ] Você consegue ler um requisito e saber exatamente como testar
- [ ] Acompanhamento atualizado

## Verificações e testes

- **Teste da rastreabilidade:** Cada feature Must da ETAPA 03 tem RF(s) correspondente(s)?
- **Teste da verificabilidade:** Cada critério de aceitação pode ser testado com "passa/falha"?
- **Teste da completude:** Fluxos cobrem estados vazios, erro, carregamento?
- **Teste da clareza:** Alguém que não conhece o projeto entende o requisito?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Requisitos vagos ("o sistema deve ser rápido") | Use métricas: "lista carrega em < 500ms p95" |
| Misturar RF e RQ | Separe documentos. RF = o QUÊ. RQ = COMO BEM. |
| Critérios de aceitação não testáveis ("fácil de usar") | Gherkin força testabilidade. Use sempre. |
| Esquecer estados de erro/vazio | Checklist: vazio, carregando, erro, sucesso — para cada tela |
| Não documentar regras de negócio implícitas | Pergunte "e se...?" para cada fluxo. Documente a resposta. |
| Requisitos contraditórios | Revise cruzado: RF vs RQ, regras vs fluxos |

## Se algo der errado

| Situação | Ação |
|----------|------|
| Muitos requisitos, difícil gerenciar | Agrupe por feature. Use IDs hierárquicos (RF-01, RF-01.1, RF-01.2). |
| Regras de negócio complexas demais | Isole em documento separado. Use tabela de decisão se necessário. |
| Critérios Gherkin ficam gigantes | Divida em cenários menores. Um cenário = uma regra testável. |
| Conflito entre RF e RQ | Priorize: segurança > confiabilidade > performance > usabilidade (ajuste ao contexto). |
| Descobre requisito faltando depois | Adicione no documento, marque como "Adicionado na ETAPA XX", rastreie. |

## Condições para avançar

- ✅ 3 documentos obrigatórios salvos e completos
- ✅ Cada feature Must/Should tem RF + CA
- ✅ Requisitos de qualidade têm métricas
- ✅ Regras de negócio cobrem exceções conhecidas
- ✅ Você consegue explicar qualquer requisito em 30 segundos

## Próxima etapa

[[ETAPA 05 - Planejar a Arquitetura e a Solução Técnica]]

## Materiais relacionados

- [[02 - Modelos/Requisitos e Arquitetura/Requisitos Funcionais]]
- [[02 - Modelos/Requisitos e Arquitetura/Requisitos de Qualidade]]
- [[02 - Modelos/Requisitos e Arquitetura/Critérios de Aceitação]]
- [[02 - Modelos/Requisitos e Arquitetura/Regras de Negócio]]
- [[03 - Guias Práticos/Vibe Coding/Como Escrever Bons Requisitos]]
- [[04 - Referência/Glossário/Requisito Funcional]]
- [[04 - Referência/Glossário/Requisito de Qualidade]]
- [[04 - Referência/Glossário/Critério de Aceitação]]
- [[04 - Referência/Glossário/Gherkin]]