# Decisão de Continuidade

> **Copie para:** `05 - Projetos/SeuProjeto/01 - Descoberta/Decisão de Continuidade.md`
> Baseado na ETAPA 02. **Decisão formal e assinada.**

---

## Decisão: [CONTINUAR / REFORMULAR / ABANDONAR]

---

## Justificativa (Baseada em Evidências)

### Fatos Principais que Apoiam a Decisão
- [Fato 1: Ex: 80% dos entrevistados perdem pedidos no WhatsApp — dor real e forte]
- [Fato 2: Ex: Nenhum concorrente foca em micro-lojas com integração WhatsApp nativa — gap de mercado]
- [Fato 3: Ex: WhatsApp Business API disponível e documentada — viabilidade técnica confirmada]
- [Fato 4: ]
- [Fato 5: ]

### Riscos Aceitos (com Mitigação)
| Risco | Probabilidade | Impacto | Mitigação Planejada |
|-------|---------------|---------|---------------------|
| [Ex: API WhatsApp mudar/fechar] | Média | Alto | Arquitetura desacoplada + fallback email |
| [Ex: Usuários não pagam] | Média | Médio | Freemium + validar disposição a pagar no onboarding |
| [Ex: Complexidade > esperado] | Média | Médio | Spike técnico ETAPA 05, simplificar escopo se necessário |

### Hipóteses Ajustadas Após Pesquisa
| Hipótese Original (ETAPA 01) | Ajuste Baseado em Evidência | Nova Prioridade |
|------------------------------|----------------------------|-----------------|
| H1: Maior problema é organizar pedidos | Confirmado: 80% citaram organização como dor #1 | Mantida (Alta) |
| H2: WhatsApp é canal principal | Confirmado: 90%+ pedidos via WhatsApp | Mantida (Alta) |
| H3: Pagariam R$ 50-100/mês | Parcial: 60% disseram "talvez", 20% "sim" → testar no MVP | Reduzida (Média) |

---

## Próximos Passos se CONTINUAR

### Ajustes no MVP (ETAPA 03)
- [ ] Priorizar: [Feature X validada como alta necessidade]
- [ ] Remover/Adiar: [Feature Y não validada / baixa prioridade]
- [ ] Adicionar: [Feature Z descoberta na pesquisa]
- [ ] Definir MVP mínimo para testar hipótese principal (H1/H2)

### Validações Pendentes (Para ETAPA 03/04)
- [ ] Validar disposição a pagar (landing page / pré-venda)
- [ ] Confirmar requisitos técnicos (spike WhatsApp API)
- [ ] Definir modelo de dados preciso

---

## Próximos Passos se REFORMULAR

### Novo Problema a Atacar
[Descreva o novo problema identificado na pesquisa]

### Novas Hipóteses
- H1': 
- H2': 
- H3': 

### Retorno para ETAPA
[ ] Voltar para ETAPA 01 com novo problema
[ ] Voltar para ETAPA 02 com novas hipóteses

---

## Próximos Passos se ABANDONAR

### Motivo Principal
[Ex: Dor não forte o suficiente / Mercado saturado / Viabilidade técnica inviável / Sem disposição a pagar]

### Lição Aprendida
[O que você leva para o próximo projeto? Ex: "Validar disposição a pagar ANTES de pesquisar concorrentes."]

### Registro no Acompanhamento
- [ ] Atualizar `Acompanhamento.md`: Status = ABANDONADO
- [ ] Arquivar pasta do projeto (manter para referência)
- [ ] Registrar lição no glossário pessoal / notas

---

## Assinatura

**Decisor:** [Seu Nome]
**Data:** [YYYY-MM-DD]
**Assinatura:** [Digital/Manual]

> [!important] **Esta decisão deve ser assinada por você.** Não delegue. Mesmo que pareça óbvio, registre explicitamente para evitar viés de confirmação.

---

## Checklist de Conclusão (ETAPA 02)

- [ ] Decisão formal registrada (CONTINUAR / REFORMULAR / ABANDONAR)
- [ ] Justificativa baseada em fatos (não opiniões)
- [ ] Riscos aceitos explicitamente com mitigação
- [ ] Hipóteses ajustadas baseadas em evidências
- [ ] Próximos passos claros para o caminho escolhido
- [ ] Assinatura do decisor

---

## Materiais Relacionados
- [[01 - Manual de Desenvolvimento/01 - Descoberta e Pesquisa/ETAPA 02 - Pesquisar e Validar a Ideia]]
- [[02 - Modelos/Pesquisa e Planejamento/Pesquisa e Validação]]
- [[04 - Referência/Glossário/Validação]]
- [[04 - Referência/Glossário/MVP]]