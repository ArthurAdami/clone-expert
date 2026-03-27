# Task: Extract Voice DNA

**ID:** 03-extract-voice-dna
**Agent:** clone-expert
**Input:** Fontes classificadas (sources.yaml)
**Output:** `voice_dna.yaml`
**Mínimo viável:** 8 vocabulário exclusivo + 5 frases assinatura + 3 proibidos + 3 metáforas

---

## Objetivo

Extrair como o expert FALA. Não o que ele diz — como ele diz. Vocabulário específico, frases que só ele usa, termos que ele evita, metáforas recorrentes, estrutura de comunicação.

**Regra central:** Cada termo ou frase precisa passar no teste de unicidade: "Outro expert do mesmo domínio usaria exatamente essa expressão?" Se sim — é ruído. Se não — é DNA.

---

## O que extrair

### 1. Vocabulário Exclusivo (always_use)

Termos que ele usa consistentemente que outros NÃO usariam:

```yaml
vocabulario_exclusivo:
  - termo: ""
    em_vez_de: ""       # o que outros usariam no lugar
    definicao_dele: ""  # como ele mesmo define (se definir)
    contexto_uso: ""
    confirmacoes: 0
    fonte: "[SOURCE:]"
    tier_pareto: ""
```

**Como encontrar:**
- Termos em maiúsculas ou aspas que se repetem
- Neologismos ou combinações de palavras incomuns
- Termos técnicos do domínio que ele usa de forma específica
- Nomes que ele dá para conceitos (ex: "Starving Crowd" vs "mercado-alvo")

---

### 2. Vocabulário Proibido (never_use)

Termos que ele explicitamente evita ou critica:

```yaml
vocabulario_proibido:
  - termo: ""
    por_que_evita: ""   # se ele explicou o motivo
    usa_em_vez: ""      # o que ele usa no lugar
    evidencia: ""       # como sabemos que ele evita
    fonte: "[SOURCE:]"
```

**Como encontrar:**
- Momentos onde ele corrige alguém que usa um termo
- Passagens com "eu não gosto de chamar de X"
- "O que as pessoas chamam de X, eu prefiro chamar de Y"
- Termos que simplesmente NUNCA aparecem nas fontes

---

### 3. Frases Assinatura

Frases que ele repete em múltiplos contextos e fontes:

```yaml
frases_assinatura:
  - frase: ""           # exatamente como ele diz
    variacoes: []       # versões ligeiramente diferentes
    quando_usa: ""      # em qual contexto
    significado: ""     # o que comunica
    confirmacoes: 0     # em quantas fontes aparece
    fontes:
      - "[SOURCE: livro p.X]"
      - "[SOURCE: podcast min Y:ZZ]"
    tier_pareto: ""
```

**Threshold de confirmação:**
- 1 ocorrência: possível assinatura
- 2 ocorrências: provável assinatura
- 3+ ocorrências em fontes diferentes: CONFIRMADA

---

### 4. Metáforas e Analogias

Imagens visuais recorrentes que ele usa para explicar conceitos:

```yaml
metaforas:
  - nome: ""
    metafora_exata: ""
    conceito_que_explica: ""
    poder_comunicativo: 1-10
    recorrencia: ""
    fonte: "[SOURCE:]"
    confirmacoes: 0
```

---

### 5. Estilo de Comunicação

Padrões estruturais de como ele constrói respostas:

```yaml
estilo_comunicacao:
  tom_profile:
    certeza: 1-10       # 10 = nunca hedges
    autoridade: 1-10    # 10 = extremamente autoridade
    calor: 1-10         # 10 = muito caloroso
    diretividade: 1-10  # 10 = extremamente direto
    pedagogia: 1-10     # 10 = sempre ensinando

  estrutura_tipica:
    abertura: ""        # como ele começa uma resposta
    desenvolvimento: "" # como ele constrói o argumento
    fechamento: ""      # como ele termina

  padroes_de_frase:
    comprimento: "curto | médio | longo | misto"
    uso_de_numeros: "frequente | ocasional | raro"
    uso_de_exemplos: "sempre | frequente | quando útil"
    uso_de_perguntas_retoricas: "frequente | ocasional | raro"
    negrito_ou_enfase: "onde e como usa"

  abertura_hooks:
    - tipo: ""
      exemplo: ""
      fonte: "[SOURCE:]"

  estrutura_de_historia:
    - padrao: ""
      estrutura: ""
      fonte: "[SOURCE:]"

  fechamento_signatures:
    - padrao: ""
      exemplo: ""
      fonte: "[SOURCE:]"
```

---

### 6. Contradições Produtivas

Tensões aparentes no estilo que são features, não bugs:

```yaml
contradicoes:
  - tensao: ""
    polo_a: ""
    polo_b: ""
    resolucao: ""
    nota: "A tensão é feature, não bug. Não resolver."
    fonte: "[SOURCE:]"
```

---

## Processo de Extração por Fonte

### Para Livros

```
1. Primeira leitura: sublinhar TODOS os termos em maiúsculas, aspas, ou repetidos
2. Segunda passagem: contar frequência de cada termo por capítulo
3. Frases que abrem ou fecham capítulos = candidatas a assinatura
4. Buscar definições explícitas: "Por X eu quero dizer..."
5. Buscar negações explícitas: "Não é X, é Y"
```

### Para Podcasts/Entrevistas

```
1. Buscar frases que o entrevistador CITA de volta para ele
   (= frases tão marcantes que o entrevistador as memorizou)
2. Momentos em que ele repete a mesma frase em contextos diferentes
3. Termos que ele usa sem explicar (assume que o ouvinte sabe)
4. Metáforas que aparecem em múltiplas entrevistas
5. Como ele responde quando não concorda com uma premissa
```

### Para Newsletter/Blog

```
1. Frases de abertura de posts (padrão recorrente?)
2. One-liners: frases de 1 linha com impacto alto
3. Termos que aparecem no título + corpo consistentemente
4. Como ele termina cada post
```

---

## Filtro de Unicidade (teste obrigatório)

Para cada item de vocabulário ou frase candidata:

```
TESTE: "Outro expert de alto nível no mesmo domínio usaria exatamente isso?"

SE SIM  → É do domínio, não do expert → DESCARTAR do DNA
SE NÃO  → É DNA do expert → INCLUIR

Exemplos:
  "value proposition"  → qualquer marketer usa → DESCARTAR
  "Dream Outcome"       → só Hormozi usa → INCLUIR
  "marketing"          → qualquer um usa → DESCARTAR
  "Starving Crowd"     → só Hormozi usa → INCLUIR
```

---

## Output: voice_dna.yaml

```yaml
expert: ""
extraction_date: ""
sources_used: []
total_vocabulary_terms: 0
confirmed_phrases: 0

identity_statement: ""  # "X comunica com [estilo], usando [abordagem] para [objetivo]"

tone_profile:
  certeza: 0
  autoridade: 0
  calor: 0
  diretividade: 0
  pedagogia: 0

vocabulary:
  always_use:
    - termo: ""
      em_vez_de: ""
      confirmacoes: 0
      fonte: "[SOURCE:]"

  never_use:
    - termo: ""
      por_que: ""
      usa_em_vez: ""
      fonte: "[SOURCE:]"

signature_phrases:
  - frase: ""
    quando_usa: ""
    confirmacoes: 0
    fontes: []
    tier_pareto: ""

metaphors:
  - nome: ""
    texto_exato: ""
    conceito: ""
    poder: 0
    confirmacoes: 0
    fonte: "[SOURCE:]"

communication_patterns:
  opening_hooks: []
  story_structure: []
  closing_signatures: []
  paragraph_style: ""

writing_style:
  comprimento_frases: ""
  uso_numeros: ""
  uso_exemplos: ""
  uso_perguntas_retoricas: ""

contradictions:
  - tensao: ""
    nota: ""
    fonte: "[SOURCE:]"

gaps:
  - componente: ""
    o_que_falta: ""
    fonte_sugerida: ""
```

---

## Done When

- [ ] Mínimo 8 termos vocabulário exclusivo (always_use) com fonte
- [ ] Mínimo 3 termos proibidos (never_use) com evidência
- [ ] Mínimo 5 frases assinatura confirmadas (3+ ocorrências cada)
- [ ] Mínimo 3 metáforas documentadas com fonte
- [ ] Tom profile preenchido (certeza, autoridade, calor, diretividade)
- [ ] Filtro de unicidade aplicado em todos os itens
- [ ] `voice_dna.yaml` produzido
