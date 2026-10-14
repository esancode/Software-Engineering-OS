# Definição do Produto

> **Copie para:** `05 - Projetos/SeuProjeto/02 - Produto/Definição do Produto.md`
> Baseado na ETAPA 03. Aprovação obrigatória antes de avançar.

---

## 1. Objetivos do Produto (3-5, Mensuráveis)

| Objetivo | Métrica | Meta | Prazo |
|----------|---------|------|-------|
| [Ex: Reduzir pedidos perdidos] | [% pedidos perdidos] | [< 3%] | [3 meses] |
| [Ex: Agilizar resposta ao cliente] | [Tempo médio resposta] | [< 2 min] | [1 mês] |
| [Ex: Validar disposição a pagar] | [% usuários pagando] | [> 20%] | [3 meses] |
| [Ex: Onboarding autônomo] | [% completam onboarding sem ajuda] | [> 80%] | [1 mês] |
| [Ex: Estabilidade produção] | [Uptime] | [> 99.9%] | [Contínuo] |

---

## 2. Público-Alvo (Personas)

| Persona | Descrição | Necessidade Principal | Prioridade | Jobs to be Done |
|---------|-----------|----------------------|------------|-----------------|
| [Ex: Dono de loja roupas] | [30-50 anos, 1-2 func, WhatsApp Business, não tem sistema] | [Organizar pedidos sem perder] | Principal | [Ver pedidos, Atualizar status, Responder cliente, Ver relatório] |
| [Ex: Funcionário de loja] | [20-35 anos, usa celular da loja, menos autonomia] | [Ver pedidos pendentes rápido] | Secundária | [Ver lista, Marcar pronto, Avisar dono] |
| | | | | |

---

## 3. Proposta de Valor
> Formato: **"Para [público], [produto] é a [categoria] que [benefício único] ao contrário de [alternativa]."**

[Ex: "Para **donos de micro-lojas que vendem por WhatsApp**, o **PedidosSimples** é o **gerenciador de pedidos** que **centraliza tudo em uma tela no celular com integração WhatsApp oficial**, ao contrário de **planilhas ou apps complexos caros**."]

---

## 4. Tarefas do Usuário (Jobs to be Done)

| Job | Contexto (Quando) | Outcome Esperado | Prioridade |
|-----|-------------------|------------------|------------|
| [Ver pedidos novos] | [Recebe notificação / Abre app] | [Lista clara em < 2 seg] | Must |
| [Marcar como preparando] | [Cozinhando / Separando] | [Status atualizado instantâneo] | Must |
| [Responder cliente] | [Cliente pergunta status / confirmação] | [Resposta com info real em < 30 seg] | Must |
| [Fechar pedido] | [Entregue / Retirado] | [Registro automático + relatório] | Must |
| [Ver histórico] | [Fim do dia / Semana] | [Filtros data/status + total] | Should |
| [Cadastrar produtos] | [Início / Novo item no cardápio] | [Nome, preço, descrição salvos] | Could |
| [Configurar horários] | [Mudança de turno / Feriado] | [Fora de horário automático] | Could |

---

## 5. Funcionalidades Necessárias (Derivadas dos Jobs)

| ID | Funcionalidade | Job Relacionado | Prioridade (MoSCoW) | Complexidade (1-5) |
|----|----------------|-----------------|---------------------|-------------------|
| F1 | Receber pedidos via WhatsApp (Webhook) | Ver pedidos novos | Must | 4 |
| F2 | Lista de pedidos com status (novo, preparando, pronto, entregue) | Ver pedidos novos | Must | 2 |
| F3 | Detalhes do pedido (itens, observações, cliente) | Ver pedidos novos | Must | 2 |
| F4 | Atualizar status (botões rápidos: preparando/pronto/entregue) | Marcar como preparando | Must | 1 |
| F5 | Responder cliente via app (templates + custom) | Responder cliente | Should | 3 |
| F6 | Relatório diário/semanal simples (total, por status) | Fechar pedido / Ver histórico | Should | 2 |
| F7 | Cadastro básico de produtos/preços | Cadastrar produtos | Could | 2 |
| F8 | Configuração de horário de funcionamento | Configurar horários | Could | 1 |
| F9 | Autenticação (Magic Link + Credentials) | Todos (acesso) | Must | 2 |
| F10 | Tempo real (SSE) - novos pedidos/atualizações | Ver pedidos novos | Should | 3 |

---

## 6. Critérios de Sucesso do Produto

| Critério | Como Medir | Meta v1 | Responsável |
|----------|------------|---------|-------------|
| [Ex: Usuário consegue processar 10 pedidos sem ajuda] | [Teste usabilidade: 5 usuários, 0 ajuda] | [100%] | [Você] |
| [Ex: Pedido aparece na lista < 2s após WhatsApp] | [Log timestamp webhook → UI] | [100% < 2s] | [Você] |
| [Ex: Zero perda de pedidos (idempotência)] | [Simular duplicatas] | [0 perdidos] | [Você] |
| [Ex: Deploy funciona em < 5 min] | [Cronometrar vercel --prod] | [< 5 min] | [Você] |

---

## 7. Restrições e Limitações

| Restrição | Detalhe | Impacto no Escopo |
|-----------|---------|-------------------|
| [Orçamento zero infra] | [Vercel Hobby + Neon Free + Resend Free] | [Limites de execução, storage, emails] |
| [Uma pessoa desenvolve] | [Full-stack solo] | [Escopo enxuto, sem features complexas] |
| [Prazo 4 semanas MVP] | [Timebox rígido] | [Cortar "Could" se necessário] |
| [LGPD] | [Dados pessoais: nome, telefone cliente] | [Consentimento, exclusão, retenção] |
| [WhatsApp API limites] | [1000 req/min, 256MB payload] | [Queue local, retry, monitoramento] |

---

## 8. Riscos Conhecidos (Atualizado da ETAPA 02)

| Risco | Prob. | Impacto | Mitigação |
|-------|-------|---------|-----------|
| [API WhatsApp mudar/fechar] | Média | Alto | Arquitetura desacoplada, fallback email |
| [Usuários não pagam] | Média | Médio | Freemium + validar no onboarding |
| [Complexidade > esperado] | Média | Médio | Spike ETAPA 05, simplificar escopo |
| [Concorrente grande entra no nicho] | Baixa | Alto | Foco micro-lojas, speed, lock-in dados |

---

## 9. Critérios para Mudança de Escopo (Change Control)

> [!important] **Toda mudança de escopo após esta aprovação passa por este processo.**

| Critério | Descrição |
|----------|-----------|
| **Quem pode solicitar** | [Apenas você (dono do produto)] |
| **O que exige aprovação** | [Adicionar feature Must/Should, remover feature Must, mudar prioridade] |
| **Processo** | 1. Registrar no `Registro de Decisões Técnicas` 2. Avaliar impacto (tempo, risco, deps) 3. Você aprova/rejeita 4. Atualizar Plano de Implementação |
| **Documentação** | [Atualizar: Definição do Produto, Escopo e Limites, Priorização, Plano de Implementação, Acompanhamento] |
| **Comunicação** | [Registrar no CHANGELOG se já em desenvolvimento] |

---

## Checklist de Conclusão (ETAPA 03)

- [ ] 3-5 objetivos do produto definidos com métricas
- [ ] Persona principal + secundárias descritas
- [ ] Proposta de valor em uma frase clara (formato padrão)
- [ ] Tarefas do usuário (Jobs) listadas (mínimo 4-6)
- [ ] Funcionalidades necessárias derivadas dos Jobs
- [ ] Priorização MoSCoW feita com justificativa
- [ ] Critérios de sucesso do produto definidos
- [ ] Restrições e limitações registradas
- [ ] Riscos conhecidos atualizados
- [ ] Critérios para mudança de escopo definidos
- [ ] Aprovado por você (assinatura abaixo)

---

## Aprovação

**Produto Definido e Aprovado por:** [Seu Nome]
**Data:** [YYYY-MM-DD]
**Assinatura:** [Digital]

---

## Próxima Etapa
[[ETAPA 04 - Organizar os Requisitos]]

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/02 - Produto e Requisitos/ETAPA 03 - Definir o Produto e seu Escopo]]
- [[02 - Modelos/Pesquisa e Planejamento/Escopo e Limites]]
- [[02 - Modelos/Pesquisa e Planejamento/Priorização de Funcionalidades]]
- [[04 - Referência/Glossário/MVP]]
- [[04 - Referência/Glossário/MoSCoW]]
- [[04 - Referência/Glossário/Jobs to be Done]]