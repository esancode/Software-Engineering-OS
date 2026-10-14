# Como Usar Este Manual

> Guia prático de navegação e uso diário do Software Engineering OS.

---

## Visão geral da navegação

O manual foi feito para ser **consultado sob demanda**. Você não precisa ler tudo antes de começar.

```
┌─────────────────────────────────────────────────────────────┐
│  INÍCIO (você está aqui)                                    │
├─────────────────────────────────────────────────────────────┤
│  1. Tem uma ideia? → Iniciar Novo Projeto                   │
│  2. Quer ver o caminho todo? → Fluxo Completo               │
│  3. Está no meio de uma etapa? → Manual de Desenvolvimento  │
│  4. Precisa de um documento? → Modelos                      │
│  5. Travou em algo específico? → Guias Práticos             │
│  6. Esqueceu um termo? → Glossário                          │
└─────────────────────────────────────────────────────────────┘
```

---

## Fluxo de trabalho típico

### 1. Início de projeto
```
[[Iniciar Novo Projeto]] 
    ↓ (cria pasta em 05 - Projetos/, copia modelo de acompanhamento)
[[ETAPA 00 - Início do Projeto]]
    ↓
[[ETAPA 01 - Descoberta e Pesquisa]]
    ↓
... etapas em ordem ...
    ↓
[[ETAPA 16 - Encerramento]]
```

### 2. Durante o desenvolvimento
- Abra a **etapa atual** no Manual de Desenvolvimento
- Consulte os **modelos** indicados na etapa
- Use os **guias práticos** quando travar
- Atualize o **Acompanhamento do Projeto** (checklist manual)

### 3. Consulta rápida
| Preciso...                | Onde ir                                                 |
| ------------------------- | ------------------------------------------------------- |
| Saber o que faz uma etapa | `01 - Manual de Desenvolvimento/XX - Nome da Etapa/`    |
| Copiar um modelo          | `02 - Modelos/[Categoria]/Nome do Modelo.md`            |
| Entender como testar      | `03 - Guias Práticos/Testes/Como Testar um Software.md` |
| Ver prompt para IA        | `03 - Guias Práticos/Vibe Coding/Prompts/`              |
| Consultar termo técnico   | `04 - Referência/Glossário/[Termo].md`                  |

---

## Recursos nativos do Obsidian que usamos

### Links internos `[[Nome da Nota]]`
- Clicar abre a nota
- `Ctrl+Click` (ou `Cmd+Click`) abre em nova aba
- Digite `[[` e comece a digitar para buscar

### Checklists `- [ ]`
- Clique para marcar/desmarcar
- Use nos acompanhamentos de projeto e checklists de etapa

### Propriedades (YAML frontmatter)
```yaml
---
etapa: 01
status: em_andamento
projeto: MeuApp
tags: [descoberta, pesquisa]
---
```
- Úteis para filtrar e buscar (não obrigatórias)

### Blocos de aviso
> [!important] Crítico
> [!warning] Atenção
> [!tip] Dica
> [!note] Nota

### Tabelas
Para comparações e listas estruturadas.

---

## Como criar um novo projeto (resumo)

1. Abra **[[Iniciar Novo Projeto]]**
2. Siga o passo a passo (copia modelos, cria pasta, registra ideia)
3. Abra a **ETAPA 00** correspondente
4. Comece a executar

> [!tip]
> Mantenha a pasta do projeto em `05 - Projetos/SeuProjeto/`
> O manual em `01-04` fica **independente** — não misture.

---

## Acompanhamento de progresso (manual)

Cada projeto tem seu arquivo `Acompanhamento.md` com:

```markdown
# Acompanhamento - MeuProjeto

## Etapa atual: 03 - Definir o produto e seu escopo

## Etapas concluídas
- [x] 00 - Início do Projeto
- [x] 01 - Entender o problema
- [x] 02 - Pesquisar e validar

## Próxima etapa: 04 - Organizar os requisitos

## Bloqueios atuais
- [ ] Decidir entre PostgreSQL e SQLite (aguardando pesquisa)

## Decisões pendentes
- Nome do produto
- Público-alvo principal

## Documentos necessários nesta etapa
- [ ] Definição do Produto
- [ ] Escopo e Limites
- [ ] Priorização de Funcionalidades

## Última atualização: 2026-10-11
```

> [!important]
> **Atualize manualmente.** Não há automação — isso mantém você no controle.

---

## Trabalhando com IA (Vibe Coding)

O manual assume que você usa **Claude Code** como principal ferramenta de IA.

### Em cada etapa você verá:
| Seção | O que contém |
|-------|--------------|
| **O que a IA faz** | Atividades para delegar |
| **O que você faz** | Decisões e aprovações |
| **Como verificar** | Testes e evidências |
| **Prompts sugeridos** | Modelos de prompt prontos |

### Regra de ouro
> **A IA executa, você aprova.** Nunca aceite "está pronto" sem verificar as evidências.

---

## Personalizando o manual

Este é **seu** manual. Sinta-se livre para:

- ✅ Adicionar, remover, reordenar etapas
- ✅ Criar novos modelos
- ✅ Escrever seus próprios guias
- ✅ Adicionar termos ao glossário
- ✅ Mudar a estrutura de pastas

> [!tip]
> Faça alterações **dentro do Obsidian** para manter links funcionando.
> Use `Ctrl+Shift+N` para nova nota, arraste arquivos no painel lateral.

---

## Dicas de produtividade

### Atalhos úteis
| Ação | Atalho (Windows/Linux) | Atalho (Mac) |
|------|------------------------|--------------|
| Nova nota | `Ctrl+N` | `Cmd+N` |
| Buscar nota | `Ctrl+O` | `Cmd+O` |
| Buscar em arquivos | `Ctrl+Shift+F` | `Cmd+Shift+F` |
| Abrir link em nova aba | `Ctrl+Click` | `Cmd+Click` |
| Voltar | `Alt+Left` | `Ctrl+Cmd+Left` |
| Avançar | `Alt+Right` | `Ctrl+Cmd+Right` |
| Paleta de comandos | `Ctrl+P` | `Cmd+P` |

### Pesquisa rápida
- `[[` + nome → abre nota
- `#tag` → busca por tag
- `tag:#projeto` → busca tags específicas

---

## Estrutura de um projeto típico

```
05 - Projetos/
└── MeuProjeto/
    ├── Acompanhamento.md           # Progresso manual
    ├── 00 - Início/
    │   └── Registro Inicial.md
    ├── 01 - Descoberta/
    │   ├── Definição do Problema.md
    │   └── Pesquisa e Validação.md
    ├── 02 - Produto/
    │   ├── Definição do Produto.md
    │   ├── Escopo e Limites.md
    │   └── Requisitos.md
    ├── 03 - Arquitetura/
    │   ├── Decisões de Arquitetura.md
    │   └── Visão Técnica.md
    ├── 04 - UX-UI/
    │   ├── Fluxos do Usuário.md
    │   ├── Direção Visual.md
    │   └── Inventário de Telas.md
    ├── 05 - Desenvolvimento/
    │   ├── Plano de Implementação.md
    │   └── Registro de Decisões.md
    ├── 06 - Testes/
    │   ├── Plano de Testes.md
    │   └── Registro de Execução.md
    ├── 07 - Segurança/
    │   └── Revisão de Segurança.md
    ├── 08 - Publicação/
    │   ├── Checklist de Publicação.md
    │   └── Registro de Versão.md
    └── 09 - Encerramento/
        └── Documentação Final.md
```

> [!note]
> Esta estrutura é **sugerida**, não obrigatória. Adapte ao seu projeto.

---

## Perguntas frequentes

### "Preciso fazer todas as etapas?"
Sim, na ordem. Mas a **profundidade** varia:
- Projeto pequeno/pessoal: documentos mais leves, algumas etapas rápidas
- Projeto complexo/produção: documentos completos, todas as validações

### "E se eu já começou um projeto sem o manual?"
Comece pela etapa que corresponde ao seu momento atual. Use o manual para **organizar o que falta**.

### "Posso pular etapas?"
> [!warning]
> **Não pule etapas obrigatórias.** Cada uma tem critérios que alimentam a próxima.
> Etapas condicionais só se aplicam se o projeto tiver aquela característica (ex: pagamentos, dados sensíveis).

### "O manual funciona sem IA?"
Funciona, mas foi otimizado para Vibe Coding. Sem IA, você fará mais atividades manualmente (redação, código, testes).

### "Como sei se um documento é obrigatório?"
Na seção **Documentos necessários** de cada etapa:
- **Obrigatório** = todo projeto faz
- **Condicional** = só se o projeto tiver X característica
- **Opcional** = útil mas dispensável

---

## Próximos passos recomendados

1. Leia **[[Fluxo Completo]]** para ver o mapa todo
2. Explore **[[02 - Modelos/Índice de Modelos]]** para conhecer os modelos
3. Quando tiver uma ideia, vá para **[[Iniciar Novo Projeto]]**

---

## Links relacionados

- [[Início]] — Voltar ao começo
- [[Iniciar Novo Projeto]] — Criar projeto
- [[Fluxo Completo]] — Ver todas as etapas
- [[01 - Manual de Desenvolvimento/Visão Geral das Etapas]] — Detalhes das etapas
- [[02 - Modelos/Índice de Modelos]] — Catálogo de modelos
- [[03 - Guias Práticos/Índice de Guias]] — Catálogo de guias