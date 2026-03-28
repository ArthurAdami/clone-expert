# Task: Extract Thinking DNA

**ID:** 02-extract-thinking-dna
**Agent:** clone-expert
**Input:** Fontes classificadas (sources.yaml)
**Output:** `thinking_dna.yaml`
**Mínimo viável:** 3 frameworks + decision tree + 5 heurísticas

---

## Pre-Flight Check

Executar ANTES de qualquer extração:

```
[ ] output/{expert-slug}/sources.yaml existe?
[ ] sources.yaml.gate_passed == true?
[ ] sources.yaml.tier1_count >= 3?

SE qualquer check falhar → BLOQUEAR:
  "Task 02 bloqueada: [motivo específico].
   Execute *research {expert} e confirme gate_passed: true antes de continuar."
```

---

## PARETO FILTER (aplicar antes de extrair qualquer item)

Para cada framework ou modelo mental identificado, classificar antes de registrar:

```
🔥 GENIAL (0.8%) — Framework ou raciocínio que só este expert tem ou nomeou.
   Confirmar em 3+ fontes. EXTRAIR OBRIGATORIAMENTE.

💎 EXCELENTE (4%) — Modelo mental que aparece em 3+ fontes de forma consistente.
   INCLUIR no DNA.

🚀 ÚTIL (20%) — Bom framework, mas qualquer expert sério do domínio poderia ter.
   INCLUIR apenas se especificamente associado a este expert.

💩 RUÍDO (76%) — Frameworks genéricos de "bom pensador do domínio".
   DESCARTAR.

Regra de ouro: SE o framework poderia estar no livro de qualquer outro expert do domínio
→ RUÍDO → descartar.
```

---

## Objetivo

Extrair como o expert PENSA. Não o que ele sabe — como ele processa problemas, quais modelos mentais aplica, qual é a sequência de raciocínio dele.

**Regra central:** Um framework só é válido se o expert o aplica repetidamente em contextos diferentes. Menção única = possível padrão. 3+ menções = confirmado.

---

## O que extrair

### 1. Mental Models / Frameworks Principais

Para cada framework identificado:

```yaml
framework:
  nome: ""
  descricao: ""
  formula_ou_estrutura: ""  # se tiver (ex: "Value = X/Y")
  quando_aplica: ""         # em qual tipo de problema
  como_explica: ""          # como ele mesmo descreve
  aplicacoes_encontradas:
    - contexto: ""
      fonte: "[SOURCE: livro p.X]"
  confirmacoes: 0           # quantas vezes aparece em fontes diferentes
  exclusividade: "alto | medio | baixo"  # é único dele ou comum?
  tier_pareto: "genial | excelente | util | ruido"
```

**Perguntas para extrair:**
- Quando ele se depara com um problema, qual é o PRIMEIRO framework que aciona?
- Ele tem um "master model" que tudo subordina?
- Quais são os frameworks que ele CRIOU ou NOMEOU?
- Quais são os que ele usa sem parar mesmo que sejam de outros?

---

### 2. Decision Tree (Como ele toma decisões)

Documentar a sequência de raciocínio:

```yaml
decision_tree:
  descricao: ""
  passos:
    - passo: 1
      acao: ""
      pergunta_que_faz: ""
      fonte: "[SOURCE:]"
    - passo: 2
      ...
  exemplos_aplicados:
    - situacao: ""
      sequencia_usada: ""
      fonte: "[SOURCE:]"
```

**Perguntas para extrair:**
- Quando alguém apresenta um problema pra ele, qual é a primeira pergunta que ele faz?
- Existe uma sequência recorrente de perguntas?
- Como ele diagnostica antes de prescrever?

---

### 3. Heurísticas (Regras rápidas de decisão)

```yaml
heuristicas:
  - id: "H001"
    regra: "SE [condição] ENTÃO [ação]"
    quando_usar: ""
    fonte: "[SOURCE:]"
    confirmacoes: 0
    origem: "criada por ele | adaptada | citada de terceiro"
```

**Padrões a identificar:**
- "Always do X before Y"
- "Never X unless Y"
- "If X, then Y" patterns
- Números e thresholds que ele usa repetidamente

---

### 4. Veto Conditions (O que ele rejeita imediatamente)

```yaml
veto_conditions:
  - trigger: ""
    acao: "REJEITAR | BLOQUEAR | FORÇAR diagnóstico"
    razao: ""
    fonte: "[SOURCE:]"
    confirmacoes: 0
```

**Perguntas para extrair:**
- O que ele diz "nunca faça" sem exceção?
- Quais ideias ele rejeita sempre, independente do contexto?
- Quais antipadrões ele combate sistematicamente?

---

### 5. Prioritization Rules

```yaml
prioritization:
  - regra: ""
    exemplo: ""
    fonte: "[SOURCE:]"
```

**Padrões:** X > Y (sempre), A antes de B, fix Z before doing W

---

## Processo de Extração por Fonte

### Para Livros

```
1. Ler índice e identificar capítulos com frameworks nomeados
2. Em cada capítulo, buscar:
   - Nomes em caixa alta ou aspas (= framework nomeado)
   - "The [X] principle", "The [X] rule", "The [X] framework"
   - Fórmulas e equações
   - "Always", "Never", "Before you", "First you must"
3. Para cada framework: copiar a definição exata do livro
4. Anotar página e verificar se reaparece em outros capítulos
```

### Para Entrevistas/Podcasts

```
1. Buscar momentos onde o entrevistador pede "como você pensa sobre X"
2. Identificar quando o expert discorda e explica por quê
3. Anotar frameworks mencionados pelo nome
4. Identificar sequências de raciocínio ("primeiro eu olho para X, depois Y")
5. Notar quando ele usa a mesma estrutura para problemas diferentes
```

### Para YouTube/Palestras

```
1. Timestamps de slides com frameworks visuais
2. Repetições de terminologia específica
3. Exemplos onde ele aplica o mesmo modelo a casos diferentes
```

---

## Filtro Pareto para Thinking DNA

```
Para cada framework identificado, classificar:

GENIAL (incluir obrigatoriamente):
  - Criado ou nomeado pelo próprio expert
  - Aparece em 5+ contextos diferentes
  - Não existe equivalente em outros experts

EXCELENTE (incluir):
  - Adaptação significativa de framework existente
  - Aplicação única que distingue esse expert
  - 3+ confirmações em fontes diferentes

ÚTIL (incluir se reforça):
  - Framework amplamente usado mas ele aplica de forma específica
  - Conecta com outros frameworks dele

RUÍDO (descartar):
  - Framework comum que qualquer especialista do domínio usaria
  - Mencionado 1x sem aprofundamento
```

---

## Output: thinking_dna.yaml

```yaml
expert: ""
extraction_date: ""
sources_used: []
total_frameworks: 0
confirmed_frameworks: 0  # com 3+ confirmações

primary_framework:
  nome: ""
  descricao: ""
  formula: ""
  importancia: "É o master model que tudo subordina"
  fonte: "[SOURCE:]"

mental_models:
  - id: "MM_001"
    nome: ""
    descricao: ""
    formula_ou_estrutura: ""
    quando_aplica: ""
    confirmacoes: 0
    tier_pareto: ""
    citacoes:
      - texto: ""
        fonte: "[SOURCE:]"

decision_tree:
  descricao: ""
  passos:
    - passo: 1
      acao: ""
      fonte: "[SOURCE:]"

heuristics:
  - id: "H001"
    regra: ""
    fonte: "[SOURCE:]"
    confirmacoes: 0

veto_conditions:
  - trigger: ""
    acao: ""
    razao: ""
    fonte: "[SOURCE:]"

prioritization:
  - regra: ""
    fonte: "[SOURCE:]"

gaps:
  - componente: ""
    o_que_falta: ""
    fonte_sugerida: ""
```

---

## Done When

- [ ] Mínimo 3 frameworks confirmados (3+ citações independentes)
- [ ] Primary framework identificado
- [ ] Decision tree documentada com pelo menos 3 passos
- [ ] Mínimo 5 heurísticas com fonte
- [ ] Mínimo 2 veto conditions documentadas
- [ ] Filtro Pareto aplicado em todos os frameworks
- [ ] `thinking_dna.yaml` produzido
