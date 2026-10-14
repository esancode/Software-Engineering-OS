# Pesquisa e Validação

> **Copie para:** `05 - Projetos/SeuProjeto/01 - Descoberta/Pesquisa e Validação.md`
> Baseado na ETAPA 02. Documente toda a pesquisa e evidências.

---

## 1. Perguntas de Pesquisa (Derivadas das Hipóteses da ETAPA 01)

| Hipótese (ETAPA 01) | Pergunta de Pesquisa | Método | Status | Evidência/Link |
|---------------------|---------------------|--------|--------|----------------|
| H1: [Ex: Maior problema é organizar pedidos] | [Ex: Como comerciantes organizam pedidos hoje?] | [Entrevistas + reviews] | [✅ Feito / 🔄 Em andamento / ⏳ Pendente] | [Link para registro/entrevista] |
| H2: [Ex: WhatsApp é canal principal] | [Ex: Qual % de pedidos vem por WhatsApp?] | [Entrevistas] | | |
| H3: [Ex: Pagariam R$ 50-100/mês] | [Ex: Disposição a pagar por solução?] | [Landing page teste / pesquisa] | | |
| H4: | | | | |
| H5: | | | | |

---

## 2. Soluções Existentes (Concorrentes Diretos)

| Produto | Público-Alvo | Preço | Pontos Fortes | Pontos Fracos | Gap para Nós |
|---------|--------------|-------|---------------|---------------|--------------|
| [Concorrente A] | [Ex: Médias empresas] | [R$ 200/mês] | [Integração ERP, relatórios] | [Caro, complexo, curva aprendizado] | [Simplicidade + preço acessível] |
| [Concorrente B] | [Ex: Pequenos] | [Freemium] | [Fácil uso, mobile] | [Sem WhatsApp, relatórios básicos] | [Integração WhatsApp nativa] |
| [Concorrente C] | | | | | |
| [Concorrente D] | | | | | |

---

## 3. Alternativas Atuais (Como o Usuário Resolve HOJE)

| Alternativa | Vantagens | Desvantagens | Por Que Não Resolve Tudo |
|-------------|-----------|--------------|--------------------------|
| [Ex: Anotar no papel] | Grátis, simples, sem tech | Perde info, não escala, erros | Erros frequentes, não busca histórico |
| [Ex: Planilha Excel] | Organiza, grátis, flexível | Manual, não mobile, não integra WhatsApp | Não integra WhatsApp, retrabalho |
| [Ex: App genérico de notas] | Grátis, mobile | Não estruturado para pedidos | Não tem status, não calcula total |
| [Ex: Não resolve / Ignora] | - | Perde vendas, estresse | - |

---

## 4. Necessidades dos Usuários (Evidências)

| Necessidade | Evidência (Fonte) | Tipo | Prioridade |
|-------------|-------------------|------|------------|
| [Ex: Ver todos pedidos em um lugar] | [8/10 entrevistados citaram] | [Fato (entrevista)] | [Alta] |
| [Ex: Responder rápido no WhatsApp] | [Reviews apps atuais reclamam lentidão] | [Fato (review)] | [Alta] |
| [Ex: Relatório de vendas semanal] | [3/10 citaram "gostaria de ter"] | [Hipótese] | [Média] |
| [Ex: Funcionar offline] | [2/10 mencionaram internet instável] | [Hipótese] | [Baixa] |

> [!important] **Separe claramente: Fato vs. Hipótese vs. Opinião**
> - **Fato:** Dado observado com fonte (entrevista, review, métrica)
> - **Hipótese:** Inferência sua, precisa validação
> - **Opinião:** Preferência subjetiva sem evidência

---

## 5. Riscos Identificados

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| [Ex: API WhatsApp mudar/fechar] | [Média] | [Alto] | [Arquitetura desacoplada, fallback email] |
| [Ex: Concorrente grande lança similar] | [Baixa] | [Alto] | [Foco em nicho, speed to market, lock-in via dados] |
| [Ex: Usuários não pagam] | [Média] | [Médio] | [Validar disposição a pagar na pesquisa, freemium] |
| [Ex: Complexidade técnica > esperado] | [Média] | [Médio] | [Spike técnico na ETAPA 05, simplificar escopo] |
| [Ex: Regulamentação WhatsApp/API Meta] | [Baixa] | [Alto] | [Monitorar anúncios Meta, plano B comunicação] |

---

## 6. Oportunidades Identificadas

| Oportunidade | Evidência | Ação |
|--------------|-----------|------|
| [Ex: Nenhum app foca em micro-comerciante] | [Gap concorrentes: todos visam médios/grandes] | [Posicionamento claro: "Para micro-lojas"] |
| [Ex: WhatsApp Business API acessível] | [Doc técnica pública, sandbox grátis] | [MVP com integração nativa] |
| [Ex: Mercado crescente e-commerce local] | [SEBRAE: +30% micro-lojas online 2023] | [Timing favorável] |
| [Ex: Falta de ferramentas em português] | [Concorrentes 대부분 em inglês] | [Diferencial: 100% PT-BR, suporte local] |

---

## 7. Fontes Consultadas (Registro para Rastreabilidade)

| Fonte | Tipo | Data | Link/Referência | Confiabilidade | Notas |
|-------|------|------|-----------------|----------------|-------|
| [Entrevista João - dono loja] | [Primária] | [YYYY-MM-DD] | [Gravação local / anotações] | [Alta] | [Persona principal] |
| [Reviews App Store "Gestão Pedidos"] | [Secundária] | [YYYY-MM-DD] | [Link App Store] | [Média] | [50+ reviews analisados] |
| [Artigo "Mercado varejo BR 2024"] | [Secundária] | [YYYY-MM-DD] | [IBGE/SEBRAE link] | [Alta] | [Dados macro] |
| [Concorrente A - site + trial] | [Primária] | [YYYY-MM-DD] | [URL + prints] | [Alta] | [Teste prático 30 min] |
| [Reddit r/smallbusiness BR] | [Secundária] | [YYYY-MM-DD] | [Link tópicos] | [Baixa-Média] | [Qualitativo, não representativo] |

---

## 8. Síntese: Fatos vs. Hipóteses vs. Opiniões

### Fatos Comprovados (Evidência Direta)
- [Fato 1: Ex: 80% entrevistados perdem pedidos no WhatsApp]
- [Fato 2: Ex: Concorrente A custa R$ 200/mês mínimo]
- [Fato 3: ]

### Hipóteses a Validar (Ainda Incertas)
- [Hipótese 1: Ex: Pagariam R$ 50-100/mês]
- [Hipótese 2: Ex: Funcionam offline é necessário]
- [Hipótese 3: ]

### Opiniões/Preferências (Subjetivas)
- [Opinião 1: Ex: "Gostaria que tivesse tema escuro"]
- [Opinião 2: ]
- [Opinião 3: ]

---

## 9. Decisão Preliminar (Será Formalizada na Decisão de Continuidade)

> [!warning] Esta é uma **anotação de trabalho**. A decisão formal vai no documento "Decisão de Continuidade".

**Tendência:** [CONTINUAR / REFORMULAR / ABANDONAR]

**Principais Motivos:**
- [Fato que apoia continuar]
- [Fato que apoia continuar]
- [Risco aceito: X (mitigação Y)]

**Ajustes no MVP se CONTINUAR:**
- [Priorizar feature X (validada como alta necessidade)]
- [Remover feature Y (não validada)]
- [Adicionar feature Z (descoberta na pesquisa)]

---

## Checklist de Conclusão (ETAPA 02)

- [ ] Perguntas de pesquisa definidas (baseadas nas hipóteses da ETAPA 01)
- [ ] Mínimo 3 concorrentes/alternativas analisados
- [ ] Mínimo 5 reviews de usuários lidos (ou 3 entrevistas realizadas)
- [ ] Alternativas atuais do usuário mapeadas
- [ ] Necessidades validadas com evidências (fonte registrada)
- [ ] Riscos e oportunidades identificados
- [ ] Fontes consultadas registradas com data e link
- [ ] Fatos, hipóteses e opiniões claramente separados
- [ ] Decisão preliminar anotada

---

## Próxima Etapa
[[ETAPA 03 - Definir o Produto e seu Escopo]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/01 - Descoberta e Pesquisa/ETAPA 02 - Pesquisar e Validar a Ideia]]
- [[02 - Modelos/Pesquisa e Planejamento/Decisão de Continuidade]]
- [[02 - Modelos/Pesquisa e Planejamento/Análise de Concorrentes]]
- [[03 - Guias Práticos/UI e UX/Pesquisa de Usuários]]
- [[04 - Referência/Glossário/Validação]]
- [[04 - Referência/Glossário/MVP]]