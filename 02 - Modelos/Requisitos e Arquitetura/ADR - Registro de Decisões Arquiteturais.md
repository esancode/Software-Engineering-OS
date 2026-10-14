# ADR - Registro de Decisões Arquiteturais

> **Copie para:** `05 - Projetos/SeuProjeto/03 - Arquitetura/ADR-XXX-titulo-da-decisao.md`
> Baseado na ETAPA 05. **Uma decisão arquitetural por arquivo.** Use numeração sequencial (ADR-001, ADR-002...).

---

## Template ADR (Preencha e salve como arquivo separado)

```markdown
# ADR-XXX: [Título Curto da Decisão]

**Status:** [Proposto | Aceito | Rejeitado | Obsoleto | Substituído por ADR-YYY]
**Data:** [YYYY-MM-DD]
**Autor(es):** [Nome(s)]
**Decisores:** [Quem aprova]

---

## Contexto

[Descreva a situação que exige a decisão. Qual problema estamos resolvendo? Quais restrições existem?]
- Fator 1: [ex: Time de 1 pessoa, prazo 4 semanas]
- Fator 2: [ex: Orçamento zero para infra (Vercel Hobby + Neon Free)]
- Fator 3: [ex: Necessidade de deploy simples, sem DevOps dedicado]
- Fator 4: [ex: Requisito LGPD - dados no Brasil]

---

## Decisão

**Escolhemos:** [Tecnologia / Padrão / Abordagem escolhida]

**Alternativas consideradas:**
| Alternativa | Prós | Contras | Por que não |
|-------------|------|---------|-------------|
| [Opção A] | [Prós] | [Contras] | [Motivo rejeição] |
| [Opção B] | [Prós] | [Contras] | [Motivo rejeição] |
| [Opção C] | [Prós] | [Contras] | [Motivo rejeição] |

---

## Consequências

### Positivas
- [Consequência boa 1]
- [Consequência boa 2]
- [Consequência boa 3]

### Negativas / Riscos
- [Risco/Desvantagem 1] → Mitigação: [como lidar]
- [Risco/Desvantagem 2] → Mitigação: [como lidar]
- [Risco/Desvantagem 3] → Mitigação: [como lidar]

### Neutras / Observações
- [Observação importante]
- [Trade-off aceito conscientemente]

---

## Implementação

**Arquivos/Componentes afetados:**
- `[caminho/arquivo.ts]` - [o que muda]
- `[caminho/outro.ts]` - [o que muda]

**Migração necessária:** [Sim/Não - se sim, descreva]
**Rollback plan:** [Como voltar se der errado]

---

## Validação

**Como validar que a decisão funciona:**
- [ ] Critério 1: [ex: Load test 100 req/s < 500ms p95]
- [ ] Critério 2: [ex: Deploy em < 3 min no Vercel]
- [ ] Critério 3: [ex: Custo mensal < $0 no plano free]

**Métricas de acompanhamento:**
- [Métrica 1]: [Target]
- [Métrica 2]: [Target]

---

## Referências

- [Link para doc, issue, discussão, benchmark]
- [ADR relacionado: ADR-XXX]

---

## Checklist ADR

- [ ] Título claro e numerado
- [ ] Contexto explica o "porquê" da decisão
- [ ] Alternativas reais consideradas (pelo menos 2)
- [ ] Decisão explícita (não "talvez")
- [ ] Consequências honestas (prós E contras)
- [ ] Plano de implementação/migração
- [ ] Critérios de validação mensuráveis
- [ ] Status = "Aceito" (após aprovação)
- [ ] Arquivo salvo em `docs/adr/ADR-XXX-titulo.md`