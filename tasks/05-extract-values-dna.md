# Task: Extract Values DNA

**ID:** 05-extract-values-dna
**Agent:** clone-expert
**Input:** Fontes classificadas (sources.yaml)
**Output:** `values_dna.yaml`
**Mínimo viável:** Hierarquia de valores + 2 antipadrões + obsessões centrais

---

## Objetivo

Extrair o que o expert considera não negociável. Os valores que filtram todas as decisões, as crenças centrais que aparecem mesmo quando ele não está ensinando, o que ele nunca abre mão.

**Regra central:** Values DNA explica o PORQUÊ de tudo. Thinking DNA explica o COMO. Voice DNA explica o QUÊ. Values DNA é a fundação.

---

## O que extrair

### 1. Hierarquia de Valores

Valores ordenados por importância e consistência:

```yaml
valor:
  rank: 0
  nome: ""
  categoria: "existencial | operacional | ético | estético"
  descricao: ""
  essencia: ""          # a crença em 1 frase
  filtro_decisao: ""    # "Este caminho [X] ou [Y]?"
  como_se_manifesta: [] # comportamentos que evidenciam esse valor
  tensoes: []           # onde conflita com outros valores
  fonte: "[SOURCE:]"
  confirmacoes: 0
```

**Tier 1 — Valores Existenciais (identidade):**
- O que ele nunca comprometeria por dinheiro?
- O que ele defende mesmo quando é impopular?
- O que define quem ele É, não só o que ele FAZ?

**Tier 2 — Valores Operacionais (método):**
- Como ele prefere trabalhar?
- O que ele valoriza no processo (não no resultado)?
- Velocidade ou qualidade? Escala ou profundidade?

---

### 2. Crenças Centrais

Afirmações que ele considera verdades absolutas:

```yaml
crenca:
  afirmacao: ""
  dominio: ""           # em qual área essa crença se aplica
  evidencia: ""         # como se manifesta no comportamento
  anticrenca: ""        # o que ele acredita ser ERRADO
  fonte: "[SOURCE:]"
  confirmacoes: 0
```

**Como encontrar:**
- Frases iniciadas com "I believe...", "Eu acredito...", "A verdade é..."
- Afirmações que ele repete sem questionar
- O que ele diz quando perguntado sobre "filosofia de vida"
- Declarações absolutas sem qualificação

---

### 3. Obsessões Centrais

O que ele persegue com intensidade acima do normal:

```yaml
obsessao:
  nome: ""
  intensidade: 1-10
  descricao: ""
  por_que_obsessao: ""  # o que o move
  sem_isso: ""          # o que acontece se não tiver isso
  manifestacoes: []     # como aparece no trabalho/vida
  fonte: "[SOURCE:]"
```

---

### 4. Non-Negotiables

O que ele nunca faz, independente do contexto:

```yaml
non_negotiable:
  afirmacao: ""
  quando_testado: ""    # situações onde foi testado e manteve
  exceções: "nenhuma | raramente"
  fonte: "[SOURCE:]"
  confirmacoes: 0
```

---

### 5. Definição de Sucesso

Como ele define sucesso — diferente do convencional:

```yaml
definicao_sucesso:
  o_que_e: ""
  o_que_nao_e: ""       # o que ele rejeita como definição de sucesso
  metrica_propria: ""   # como ele mede
  fonte: "[SOURCE:]"
```

---

## Processo de Extração

### Fontes mais ricas para Values DNA

```
MELHOR: Entrevistas onde ele fala sobre carreira, vida pessoal, fracassos
  → É quando valores emergem naturalmente

BOM: Prefácios e introduções de livros
  → Onde ele explica o POR QUÊ do livro

BOM: Momentos de conflito ou crítica
  → Valores aparecem quando estão sendo testados

RAZOÁVEL: Posts pessoais em newsletter/blog
  → Reflexões sobre vida e trabalho
```

### Padrões a buscar

```
CRENÇAS:
  "I've always believed that..."
  "The truth is..."
  "Most people get this wrong..."
  "What I know for certain is..."
  "Fundamentally, X is about Y"

VALORES:
  "I would rather X than Y"
  "I don't care about X if it means sacrificing Y"
  "The only thing that matters is..."
  "At the end of the day..."

NON-NEGOTIABLES:
  "I never..."
  "No matter what..."
  "Regardless of the outcome..."
  "Even when it's hard..."
  "I refuse to..."

OBSESSÕES:
  Tópicos que aparecem em TODAS as entrevistas
  O que ele menciona sem ser perguntado
  O que ele conecta a outros assuntos desnecessariamente
```

---

## Output: values_dna.yaml

```yaml
expert: ""
extraction_date: ""
sources_used: []

value_hierarchy:
  tier1_existential:
    - rank: 1
      nome: ""
      categoria: ""
      essencia: ""
      filtro_decisao: ""
      fonte: "[SOURCE:]"
      confirmacoes: 0

  tier2_operational:
    - rank: 0
      nome: ""
      categoria: ""
      essencia: ""
      fonte: "[SOURCE:]"

core_beliefs:
  - afirmacao: ""
    dominio: ""
    anticrenca: ""
    fonte: "[SOURCE:]"
    confirmacoes: 0

core_obsessions:
  - nome: ""
    intensidade: 0
    descricao: ""
    manifestacoes: []
    fonte: "[SOURCE:]"

non_negotiables:
  - afirmacao: ""
    confirmacoes: 0
    fonte: "[SOURCE:]"

definition_of_success:
  o_que_e: ""
  o_que_nao_e: ""
  metrica_propria: ""
  fonte: "[SOURCE:]"

gaps:
  - componente: ""
    o_que_falta: ""
    fonte_sugerida: ""
```

---

## Done When

- [ ] Hierarquia de valores com pelo menos 3 valores Tier 1 documentados
- [ ] Mínimo 3 crenças centrais com fonte
- [ ] Mínimo 2 obsessões com intensidade e manifestações
- [ ] Mínimo 2 non-negotiables confirmados
- [ ] Definição de sucesso documentada
- [ ] `values_dna.yaml` produzido
