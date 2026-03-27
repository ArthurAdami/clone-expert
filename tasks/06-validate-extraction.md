# Task: Validate Extraction

**ID:** 06-validate-extraction
**Agent:** clone-expert
**Input:** thinking_dna.yaml + voice_dna.yaml + behavioral_dna.yaml + values_dna.yaml
**Output:** `validation_report.yaml` + fidelity score
**Gate:** Score ≥ 40 para prosseguir, ≥ 60 recomendado

---

## Objetivo

Calcular o fidelity score da extração, identificar gaps específicos e decidir se a qualidade é suficiente para gerar o clone.

---

## Passo 1 — Calcular Fidelity Score

### Componente A — Frameworks Únicos (max 30 pts)

```
Contar frameworks com 3+ citações independentes em fontes diferentes:
  X frameworks confirmados × 10 pts = ___
  Cap: 30 pts

Verificar:
  [ ] Cada framework tem nome específico?
  [ ] Cada framework tem pelo menos 1 citação com [SOURCE:]?
  [ ] Algum framework é genuinamente exclusivo do expert?
```

### Componente B — Frases Assinatura (max 25 pts)

```
Contar frases com 3+ ocorrências em fontes diferentes:
  X frases confirmadas × 5 pts = ___
  Cap: 25 pts

Verificar:
  [ ] Cada frase passou no teste de unicidade?
  [ ] Cada frase tem pelo menos 2 fontes diferentes como evidência?
```

### Componente C — Padrões Comportamentais (max 24 pts)

```
Contar behavioral states com trigger + resposta documentados:
  X states documentados × 8 pts = ___
  Cap: 24 pts

Verificar:
  [ ] Cada state tem trigger claro?
  [ ] Cada state tem exemplo real com [SOURCE:]?
  [ ] Immune system tem pelo menos 5 triggers?
```

### Componente D — Vocabulário Exclusivo (max 15 pts)

```
Contar termos always_use + never_use mapeados:
  X termos × 3 pts = ___
  Cap: 15 pts

Verificar:
  [ ] Cada termo passou no filtro de unicidade?
  [ ] Termos never_use têm evidência de que ele evita?
```

### Componente E — Exemplos Input/Output (max 6 pts)

```
Contar pares input/output com citação verificável:
  X exemplos × 2 pts = ___
  Cap: 6 pts

Verificar:
  [ ] Cada exemplo vem de fonte real (não inventado)?
  [ ] O output é fiel ao estilo de resposta do expert?
```

### Score Total

```
SCORE = A + B + C + D + E
      = ___ + ___ + ___ + ___ + ___
      = ___ / 100

Interpretação:
  < 40  → Raso. Bloquear geração.
  40-59 → Funcional. Gerar com aviso de limitações.
  60-79 → Sólido. Gerar para uso geral.
  80+   → Alta fidelidade. Pronto para produção.
```

---

## Passo 2 — Validação Cruzada

Para cada padrão identificado, verificar consistência entre fontes:

```
TESTE DE CONSISTÊNCIA:
  Padrão X aparece em livro: [ ] Sim [ ] Não
  Padrão X aparece em entrevista: [ ] Sim [ ] Não
  Padrão X aparece em conteúdo recente (< 3 anos): [ ] Sim [ ] Não

SE aparece em 2+ tipos de fonte → CONFIRMAR
SE aparece só em 1 tipo → MARCAR COMO POSSÍVEL
SE contradiz padrão de fonte mais recente → USAR O MAIS RECENTE
```

---

## Passo 3 — Gap Analysis

Para cada componente abaixo do target:

```
GAPS CRÍTICOS (bloqueiam qualidade mínima):
  [ ] < 3 frameworks confirmados → precisa extrair mais thinking DNA
  [ ] < 3 behavioral states → precisa extrair mais behavioral DNA
  [ ] Score < 40 → clone não deve ser gerado

GAPS IMPORTANTES (afetam qualidade):
  [ ] Frases assinatura < 5 → Voice DNA incompleto
  [ ] Vocabulário < 8 termos → Voice DNA fraco
  [ ] Exemplos < 3 → Clone vai responder de forma genérica

GAPS OPCIONAIS (melhoram fidelidade):
  [ ] Values DNA incompleto → Clone pode ter valores inconsistentes
  [ ] Metáforas < 3 → Comunicação menos característica
  [ ] Veto conditions < 2 → Clone não vai rejeitar adequadamente
```

Para cada gap, identificar:
- Qual fonte específica pode preencher
- Qual trecho ou tipo de conteúdo buscar

---

## Passo 4 — Decision

```
SE score < 40:
  → BLOQUEAR geração
  → Reportar gaps específicos com ações concretas
  → Oferecer continuar extração

SE score 40-59:
  → AVISAR sobre limitações
  → Listar áreas onde o clone será inconsistente
  → Perguntar se user quer continuar mesmo assim
  → SE sim: prosseguir com flag "low_fidelity: true"

SE score ≥ 60:
  → PROSSEGUIR para fase 7 (decide type)
  → Reportar score + pontos fortes da extração
  → Mencionar gaps que poderiam melhorar o clone
```

---

## Output: validation_report.yaml

```yaml
expert: ""
validation_date: ""

fidelity_score:
  component_a_frameworks: 0
  component_b_phrases: 0
  component_c_behavioral: 0
  component_d_vocabulary: 0
  component_e_examples: 0
  total: 0
  classification: "raso | funcional | solido | alta_fidelidade"

gate_result: "PASS | WARN | BLOCK"
gate_reason: ""

strengths:
  - ""

gaps:
  critical:
    - componente: ""
      atual: 0
      necessario: 0
      acao: ""
      fonte_sugerida: ""
  important:
    - componente: ""
      atual: 0
      necessario: 0
      acao: ""
  optional:
    - componente: ""
      descricao: ""

cross_validation:
  patterns_confirmed: 0
  patterns_possible: 0
  patterns_contradictory: 0
  contradiction_notes: ""

recommendation: ""
proceed: true | false
low_fidelity_flag: false
```

---

## Done When

- [ ] Score calculado com todos os 5 componentes
- [ ] Gate decision tomada (PASS / WARN / BLOCK)
- [ ] Gap analysis completo com ações específicas
- [ ] Validação cruzada executada
- [ ] `validation_report.yaml` produzido
