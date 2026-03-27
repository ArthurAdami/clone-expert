# Checklist: Extraction Validation

**Usado em:** Tasks 02-05 (durante extração) + Task 06 (validação formal)
**Agente:** clone-expert
**Propósito:** Garantir que cada DNA extraído atende os critérios de qualidade antes de prosseguir.

---

## Validação por DNA

### Thinking DNA (Task 02)

```
CRITÉRIOS OBRIGATÓRIOS:
[ ] Primary framework identificado com nome específico
[ ] ≥ 3 mental models com nome próprio (não genérico de IA)
[ ] ≥ 1 mental model com fórmula ou estrutura única
[ ] Decision tree tem ≥ 4 passos sequenciais
[ ] ≥ 5 heurísticas no formato SE → ENTÃO
[ ] ≥ 2 veto conditions (o que o expert NUNCA faz)

CRITÉRIOS DE QUALIDADE:
[ ] Cada framework tem ≥ 2 fontes diferentes como evidência
[ ] Primary framework é genuinamente exclusivo do expert?
    (ou é framework comum que ele apenas usa?)
[ ] Decision tree mapeia o raciocínio real ou é genérico?
[ ] Heurísticas são específicas do domínio do expert?

CRITÉRIOS DE UNICIDADE:
[ ] Os mental models identificados existem com esse nome no mercado?
    SE SIM → confirmar que o expert os nomeou ou popularizou
    SE NÃO → confirmar que é terminologia exclusiva do expert

SCORE COMPONENTE A (máx 30):
  Frameworks confirmados (3+ fontes): ___ × 10 = ___
  Cap: 30 pts

STATUS: COMPLETO | PARCIAL (indicar gaps) | INSUFICIENTE
```

---

### Voice DNA (Task 03)

```
CRITÉRIOS OBRIGATÓRIOS:
[ ] ≥ 8 termos no always_use com alternativas genéricas
[ ] ≥ 3 termos no never_use com motivo
[ ] ≥ 5 frases assinatura com ≥ 3 ocorrências cada
[ ] ≥ 3 metáforas recorrentes identificadas
[ ] Tone profile tem ≥ 3 adjetivos descritivos

TESTE DE UNICIDADE — para cada termo/frase:
[ ] "Outro expert do mesmo domínio usaria esta frase?"
    SE SIM → remover ou rebaixar para tier 2
    SE NÃO → confirmar como vocabulário exclusivo

TESTE DO GRAVADOR — para cada frase assinatura:
[ ] "Se eu ouvisse essa frase sem contexto, saberia quem é?"
    SE SIM → alta fidelidade
    SE NÃO → generalista, remover

CRITÉRIOS DE EVIDÊNCIA:
[ ] Cada frase assinatura tem ≥ 2 fontes diferentes?
[ ] Há frases que aparecem em entrevistas E conteúdo próprio?
    (confirma que não é só performance)

SCORE COMPONENTE B (máx 25):
  Frases confirmadas (3+ fontes, únicas): ___ × 5 = ___
  Cap: 25 pts

SCORE COMPONENTE D (máx 15):
  Termos always_use + never_use mapeados: ___ × 3 = ___
  Cap: 15 pts

STATUS: COMPLETO | PARCIAL (indicar gaps) | INSUFICIENTE
```

---

### Behavioral DNA (Task 04)

```
CRITÉRIOS OBRIGATÓRIOS:
[ ] ≥ 3 behavioral states com trigger + response
[ ] ≥ 5 immune system triggers mapeados
[ ] Antipattern rejection template no estilo do expert
[ ] ≥ 3 antipadrões que o expert sistematicamente rejeita

FILTRO COMPORTAMENTO REAL vs. PERFORMANCE:
[ ] Cada behavioral state tem evidência em múltiplos contextos?
[ ] O comportamento aparece em situações espontâneas (não só palestras)?
[ ] Há consistência entre o que o expert prega e como age?

TESTE DO IMMUNE SYSTEM:
[ ] Para cada trigger: o expert rejeita desta forma em ≥ 2 fontes?
[ ] O tom de rejeição é específico (não genérico)?
[ ] A rejeição usa os frameworks do próprio expert?

SCORE COMPONENTE C (máx 24):
  Behavioral states documentados: ___ × 8 = ___
  Cap: 24 pts

SCORE COMPONENTE E — Exemplos (máx 6):
  Pares input/output com citação verificável: ___ × 2 = ___
  Cap: 6 pts

STATUS: COMPLETO | PARCIAL (indicar gaps) | INSUFICIENTE
```

---

### Values DNA (Task 05)

```
CRITÉRIOS (este DNA é aceito como parcial):
[ ] ≥ 2 valores Tier 1 (existenciais) identificados
[ ] ≥ 3 crenças centrais com evidência
[ ] ≥ 2 obsessões recorrentes documentadas
[ ] ≥ 1 non-negotiable com evidência direta

CRITÉRIOS DE QUALIDADE:
[ ] Valores são inferidos de comportamento real (não só declarações)?
[ ] Há consistência entre valores declarados e ações documentadas?
[ ] Definition of success é específica do expert?

NOTA: Values DNA incompleto NÃO bloqueia geração
      Mas afeta consistência do clone em perguntas filosóficas

STATUS: COMPLETO | PARCIAL (aceitável) | AUSENTE (sinalizar)
```

---

## Validação Cruzada

```
Para cada padrão extraído, verificar consistência entre fontes:

CONSISTÊNCIA ALTA (confirmar):
  [ ] Padrão aparece em ≥ 2 tipos de fonte (livro + entrevista)
  [ ] Padrão está em conteúdo recente (< 3 anos)
  [ ] Padrão não contradiz outros padrões do mesmo expert

CONSISTÊNCIA MÉDIA (marcar como possível):
  [ ] Padrão aparece em apenas 1 tipo de fonte
  [ ] Padrão é de fonte antiga sem confirmação recente

CONTRADIÇÃO (investigar):
  [ ] Padrão contradiz outro padrão extraído
  [ ] SE temporal → usar o mais recente
  [ ] SE mesma época → documentar ambos como possibilidades
  [ ] SE sem explicação → remover ou marcar como incerto
```

---

## Red Flags de Extração

```
🚩 Framework idêntico a outro expert do domínio
   → Verificar se o expert realmente criou ou apenas usa

🚩 Frases assinatura que qualquer coach/consultor usaria
   → Falso positivo — remover do always_use

🚩 Immune system não ativa em situações óbvias do domínio
   → Extração incompleta — buscar mais exemplos

🚩 Behavioral states são genéricos ("fica animado com desafios")
   → Não captura o expert específico — extrair com mais granularidade

🚩 Voice DNA parece o expert mas também parece 10 outros
   → Insuficiente unicidade — aplicar Pareto Filter mais rigoroso

🚩 Values DNA contradiz comportamento documentado
   → Expert pode ter dito uma coisa e feito outra
   → Priorizar comportamento sobre declaração
```

---

## Checklist Final Pré-Validação Formal

```
Antes de executar Task 06, confirmar:

THINKING DNA:
  [ ] ≥ 3 frameworks confirmados
  [ ] Decision tree completo
  [ ] ≥ 5 heurísticas
  [ ] ≥ 2 veto conditions

VOICE DNA:
  [ ] ≥ 8 termos always_use únicos
  [ ] ≥ 5 frases assinatura confirmadas
  [ ] Tone profile definido

BEHAVIORAL DNA:
  [ ] ≥ 3 behavioral states
  [ ] ≥ 5 immune system triggers
  [ ] Rejection template no estilo do expert

VALUES DNA:
  [ ] ≥ 2 valores Tier 1 (ou sinalizado como parcial)

CROSS-VALIDATION:
  [ ] Padrões confirmados vs. possíveis documentados
  [ ] Contradições investigadas e resolvidas

SE TUDO OK → Executar Task 06 (validate-extraction)
SE GAPS → Voltar para task correspondente com lista específica
```
