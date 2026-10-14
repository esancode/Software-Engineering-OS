# Iniciar Novo Projeto

> Passo a passo para criar um novo projeto usando este manual.

---

## Antes de começar

> [!important]
> Execute estes passos **dentro do Obsidian** para que links e arquivos funcionem corretamente.

---

## Passo 1: Crie a pasta do projeto

1. No painel lateral do Obsidian, navegue até `05 - Projetos`
2. Clique com botão direito → **Nova pasta**
3. Nomeie com o nome do seu projeto (ex: `MeuApp`, `SistemaDeTarefas`, `Portfolio`)
4. **Não use espaços no nome da pasta** — use `MeuApp` não `Meu App`

> [!tip]
> Nomes sugeridos: `meu-app`, `sistema-tarefas`, `portfolio-pessoal`, `api-clientes`, `dashboard-vendas`

---

## Passo 2: Copie o modelo de acompanhamento

1. Abra `02 - Modelos/Projeto/Acompanhamento de Projeto.md`
2. Selecione tudo (`Ctrl+A`) → Copie (`Ctrl+C`)
3. Na pasta do seu projeto (`05 - Projetos/SeuProjeto/`), crie nova nota: `Acompanhamento.md`
4. Cole (`Ctrl+V`) e salve

> [!important]
> Este arquivo será seu **painel de controle** do projeto. Atualize a cada etapa.

---

## Passo 3: Registre a ideia inicial

1. Na pasta do projeto, crie subpasta `00 - Início`
2. Crie nota `Registro Inicial.md`
3. Use o modelo: `02 - Modelos/Projeto/Registro Inicial do Projeto.md`
4. Preencha o que souber agora — complete depois

**Campos mínimos para começar:**
- Nome do projeto (temporário)
- Descrição da ideia em 1-2 frases
- Tipo: pessoal / estudo / freelance / produto / outro
- Tamanho estimado: pequeno / médio / grande
- Prazo desejado (se houver)
- Orçamento (se houver)

---

## Passo 4: Defina o nível de rigor

| Nível | Quando usar | Documentação |
|-------|-------------|--------------|
| **Leve** | Projeto pessoal, estudo, prototype, MVP rápido | Mínima: apenas obrigatórios |
| **Padrão** | Projeto para mostrar, freelance, uso real | Completa: obrigatórios + condicionais aplicáveis |
| **Rigoroso** | Produção com usuários reais, dados sensíveis, pagamentos | Completa + revisões extras + validações formais |

**Registre no Acompanhamento.md:**
```markdown
## Nível de rigor: Padrão
```

---

## Passo 5: Abra a ETAPA 00

1. Vá para `01 - Manual de Desenvolvimento/00 - Início do Projeto/ETAPA 00 - Início do Projeto.md`
2. Leia o objetivo e pré-requisitos
3. Execute as ações listadas
4. Use os modelos indicados
5. Atualize seu `Acompanhamento.md` conforme avança

---

## Passo 6: Siga o fluxo

Cada etapa termina com um link para a próxima:
```
[[ETAPA 01 - Entender o Problema e a Oportunidade]]
```

Siga em ordem. Não pule etapas obrigatórias.

---

## Checklist rápido de criação

- [ ] Pasta criada em `05 - Projetos/NomeDoProjeto/`
- [ ] `Acompanhamento.md` copiado e salvo na pasta do projeto
- [ ] Subpasta `00 - Início/` criada
- [ ] `Registro Inicial.md` criado com modelo preenchido
- [ ] Nível de rigor definido no Acompanhamento
- [ ] ETAPA 00 aberta e pronta para executar

---

## Modelo de estrutura de pastas do projeto

Após criar, seu projeto deve ficar assim:

```
05 - Projetos/
└── SeuProjeto/
    ├── Acompanhamento.md
    ├── 00 - Início/
    │   └── Registro Inicial.md
    ├── 01 - Descoberta/
    │   └── (arquivos da etapa 01)
    ├── 02 - Produto/
    │   └── (arquivos da etapa 02-04)
    ├── 03 - Arquitetura/
    │   └── (arquivos da etapa 05)
    ├── 04 - UX-UI/
    │   └── (arquivos da etapa 06-08)
    ├── 05 - Desenvolvimento/
    │   └── (arquivos da etapa 09-10)
    ├── 06 - Testes/
    │   └── (arquivos da etapa 11)
    ├── 07 - Segurança/
    │   └── (arquivos da etapa 12)
    ├── 08 - Publicação/
    │   └── (arquivos da etapa 13-14)
    ├── 09 - Encerramento/
    │   └── (arquivos da etapa 15-16)
    └── docs/           # Opcional: docs técnicas do produto
```

> [!note]
> As pastas `01-09` são criadas **conforme você avança nas etapas**. Não precisa criar todas agora.

---

## Se já começou um projeto sem o manual

1. Crie a pasta e `Acompanhamento.md` mesmo assim
2. Identifique em qual etapa você está (olhe o Fluxo Completo)
3. Vá direto para essa etapa no Manual
4. Complete o que faltar das etapas anteriores (documentos, decisões)
5. Continue daí em diante

---

## Dicas importantes

> [!tip]
> **Nome da pasta = identificador único.** Use o mesmo nome em todo lugar (GitHub, Vercel, banco de dados, etc.)

> [!tip]
> **Commite cedo.** Inicialize git na pasta do projeto (`git init`) e faça commit inicial com o `Acompanhamento.md` e `Registro Inicial.md`.

> [!warning]
> **Não edite arquivos do manual (pastas 01-04) para seu projeto.** O manual é genérico. Seu projeto fica em `05 - Projetos/`.

---

## Links úteis agora

- [[Fluxo Completo]] — Ver todas as etapas antes de começar
- [[02 - Modelos/Projeto/Acompanhamento de Projeto]] — Modelo de acompanhamento
- [[02 - Modelos/Projeto/Registro Inicial do Projeto]] — Modelo de registro inicial
- [[01 - Manual de Desenvolvimento/00 - Início do Projeto/ETAPA 00 - Início do Projeto]] — Primeira etapa
- [[05 - Projetos/README]] — Ver projetos existentes