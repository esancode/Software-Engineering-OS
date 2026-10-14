# ETAPA 00 — Início do Projeto

## Objetivo

Preparar a estrutura inicial do projeto, registrar a ideia e definir o nível de rigor necessário para a documentação e validações.

## Por que esta etapa existe?

Evita começar a codificar sem clareza sobre o que está sendo construído, para quem, com quais restrições e qual nível de qualidade é esperado. Garante que todo projeto tenha um ponto de partida rastreável.

## Quando começar

- Você tem uma ideia de software (mesmo que vaga)
- Quer usar este manual para guiar o desenvolvimento
- Ainda não criou a pasta do projeto em `05 - Projetos/`

## O que você precisa fazer

1. **Criar a pasta do projeto** em `05 - Projetos/NomeDoProjeto/` (sem espaços, use kebab-case)
2. **Copiar o modelo de acompanhamento** (`02 - Modelos/Projeto/Acompanhamento de Projeto.md`) para `Acompanhamento.md` na raiz do projeto
3. **Criar subpasta `00 - Início/`** dentro do projeto
4. **Criar `Registro Inicial.md`** usando o modelo `02 - Modelos/Projeto/Registro Inicial do Projeto.md`
5. **Preencher o registro inicial** com o que você sabe agora
6. **Definir o nível de rigor** (Leve / Padrão / Rigoroso) e registrar no Acompanhamento
7. **Inicializar git** na pasta do projeto (`git init` e commit inicial)
8. **Atualizar o Acompanhamento.md** marcando esta etapa como concluída

## Quem faz o quê

| Atividade | Responsável |
|-----------|-------------|
| Criar pastas e copiar modelos | 👤 USUÁRIO |
| Preencher registro inicial | 👤 USUÁRIO (com ajuda da 🤖 IA para organizar) |
| Definir nível de rigor | 👤 USUÁRIO |
| Inicializar git | 👤 USUÁRIO (ou 🤖 IA com comando) |
| Verificar se estrutura está correta | ✅ VALIDAÇÃO |

## Ferramentas recomendadas

- **Obsidian** — para criar notas e pastas (nativo)
- **Terminal / Git Bash** — para `git init`
- **Claude Code** — para ajudar a preencher o registro inicial se a ideia estiver confusa

## Documentos necessários

### Obrigatórios
| Documento | Modelo | Onde guardar |
|-----------|--------|--------------|
| Acompanhamento do Projeto | `02 - Modelos/Projeto/Acompanhamento de Projeto.md` | Raiz do projeto (`05 - Projetos/SeuProjeto/`) |
| Registro Inicial do Projeto | `02 - Modelos/Projeto/Registro Inicial do Projeto.md` | `05 - Projetos/SeuProjeto/00 - Início/` |

### Condicionais
- Nenhum nesta etapa

### Opcionais
- Briefing expandido (se a ideia já for complexa)

## Como produzir os documentos

### Acompanhamento do Projeto
Copie o modelo e preencha:
- Nome do projeto
- Data de início
- Nível de rigor
- Etapa atual: `00 - Início do Projeto`
- Etapas concluídas: (vazio no início)
- Próxima etapa: `01 - Entender o Problema e a Oportunidade`

### Registro Inicial do Projeto
Preencha os campos do modelo:
- **Nome do projeto** (temporário, pode mudar)
- **Descrição da ideia** — 1-3 frases explicando o que o software faz
- **Tipo de projeto** — pessoal / estudo / freelance / produto / interno / outro
- **Tamanho estimado** — pequeno (dias) / médio (semanas) / grande (meses)
- **Público-alvo inicial** — quem vai usar
- **Problema principal** — que dor resolve
- **Restrições conhecidas** — prazo, orçamento, tecnologia obrigatória, equipe de 1 pessoa, etc.
- **Hipóteses iniciais** — o que você acredita ser verdade mas precisa validar
- **Definição de "pronto" para v1** — o mínimo para considerar a primeira versão utilizável

> [!tip]
> Não precisa ter todas as respostas agora. Preencha o que sabe e marque campos incertos com `[A definir]`. Complete nas próximas etapas.

## Como trabalhar com IA nesta etapa

**Prompt sugerido para organizar a ideia:**

```
Tenho uma ideia para um software: [descreva em 2-3 frases].
Me ajude a estruturar o Registro Inicial do Projeto preenchendo:
- Tipo de projeto provável
- Tamanho estimado
- Possíveis restrições
- Hipóteses iniciais a validar
- Sugestão de MVP (mínimo viável)

Não invente detalhes técnicos. Só organize o que eu der.
```

**Contexto a fornecer à IA:** Sua descrição bruta da ideia.
**Resultado esperado:** Registro Inicial preenchido com campos organizados.
**Como verificar:** Leia o documento — faz sentido? Reflete sua intenção?

## Checklist de execução

- [ ] Pasta do projeto criada em `05 - Projetos/NomeDoProjeto/`
- [ ] `Acompanhamento.md` copiado e salvo na raiz do projeto
- [ ] Subpasta `00 - Início/` criada
- [ ] `Registro Inicial.md` criado com modelo
- [ ] Registro Inicial preenchido (campos conhecidos)
- [ ] Nível de rigor definido e registrado no Acompanhamento
- [ ] `git init` executado na pasta do projeto
- [ ] Primeiro commit feito (Acompanhamento + Registro Inicial)
- [ ] Acompanhamento atualizado: etapa 00 concluída, próxima = 01

## Critérios de conclusão

- [ ] Pasta do projeto existe com estrutura correta
- [ ] `Acompanhamento.md` existe e tem nível de rigor definido
- [ ] `Registro Inicial.md` existe com descrição da ideia e problema
- [ ] Repositório git inicializado com commit inicial
- [ ] Você sabe qual é a próxima etapa

## Verificações e testes

- Abra `Acompanhamento.md` — a etapa atual está como `00`?
- Abra `Registro Inicial.md` — a descrição do problema está clara?
- Rode `git status` — mostra apenas arquivos do projeto?

## Erros comuns

| Erro | Como evitar |
|------|-------------|
| Criar projeto na pasta do manual (01-04) | Use apenas `05 - Projetos/` |
| Usar espaços no nome da pasta | Use `meu-projeto` não `meu projeto` |
| Não fazer commit inicial | `git init && git add . && git commit -m "chore: init project"` |
| Deixar registro inicial vazio | Preencha pelo menos: nome, descrição, problema, tipo |
| Pular definição de rigor | Decida agora — afeta todas as etapas seguintes |

## Se algo der errado

| Problema | Ação |
|----------|------|
| Esqueci de copiar o modelo de acompanhamento | Copie agora de `02 - Modelos/Projeto/` |
| Nome da pasta errado | Renomeie no explorador de arquivos (Obsidian atualiza links) |
| Git não instalado | Instale Git ou pule por enquanto (não bloqueia) |
| Não sei o nível de rigor | Comece com **Padrão** — ajuste depois se needed |

## Condições para avançar

- ✅ Pasta do projeto criada e estruturada
- ✅ Acompanhamento.md preenchido com nível de rigor
- ✅ Registro Inicial.md com ideia e problema registrados
- ✅ Git inicializado
- ✅ Você consegue explicar em 1 frase: "Meu projeto é [X] para [Y] resolver [Z]"

## Próxima etapa

[[ETAPA 01 - Entender o Problema e a Oportunidade]]

## Materiais relacionados

- [[02 - Modelos/Projeto/Acompanhamento de Projeto]]
- [[02 - Modelos/Projeto/Registro Inicial do Projeto]]
- [[03 - Guias Práticos/Vibe Coding/Como Fornecer Contexto à IA]]
- [[Fluxo Completo]]