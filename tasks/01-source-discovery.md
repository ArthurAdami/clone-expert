# Task: Source Discovery

**ID:** 01-source-discovery
**Agent:** clone-expert
**Input:** Nome do expert + fontes disponíveis (opcional)
**Output:** `sources.yaml` — fontes classificadas e qualificadas
**Gate:** Mínimo 3 fontes Tier 1 para prosseguir

---

## Objetivo

Identificar, classificar e qualificar todas as fontes disponíveis sobre o expert antes de iniciar a extração de DNA.

**Regra central:** A qualidade do clone é limitada pela qualidade das fontes. Fontes ruins produzem clone ruim — sempre.

---

## Passo 1 — Elicitação de Fontes

Perguntar ao usuário:

```
Para clonar [EXPERT], preciso mapear as fontes disponíveis.

Me passa o que você tem ou sabe que existe:

1. LIVROS — Escritos pelo próprio expert?
   [ ] Sim → quais? (título + ano)
   [ ] Não / Não sei

2. CURSOS/PROGRAMAS — Gravados pelo expert?
   [ ] Sim → links ou nomes?
   [ ] Não / Não sei

3. ENTREVISTAS LONGAS — Podcasts ou vídeos 1h+?
   [ ] Sim → links ou nomes de programas?
   [ ] Não / Não sei

4. PALESTRAS/KEYNOTES — Transcrições disponíveis?
   [ ] Sim → links?
   [ ] Não / Não sei

5. NEWSLETTER/BLOG — Arquivo de posts do próprio expert?
   [ ] Sim → URL?
   [ ] Não / Não sei

6. OUTROS — Qualquer coisa adicional?
```

---

## Passo 2 — Pesquisa Autônoma (se necessário)

Se o usuário não tem fontes ou quer complementar, buscar:

### 2.1 — Busca por Livros
```
Queries:
  "[Expert Name] book"
  "[Expert Name] livro"
  "[Expert Name] wrote"
  "books by [Expert Name]"

Filtrar: apenas livros escritos PELO expert (não sobre ele)
Classificar: Tier 1
```

### 2.2 — Busca por Entrevistas Longas
```
Queries:
  "[Expert Name] podcast interview"
  "[Expert Name] long form interview"
  "[Expert Name] Tim Ferriss" (ou outros entrevistadores conhecidos)
  site:youtube.com "[Expert Name]" filetype:transcript

Filtrar: duração ≥ 1 hora
Classificar: Tier 1
```

### 2.3 — Busca por Cursos/Programas
```
Queries:
  "[Expert Name] course"
  "[Expert Name] program"
  "[Expert Name] masterclass"
  "[Expert Name] training"

Filtrar: criados pelo expert, não por terceiros
Classificar: Tier 1
```

### 2.4 — Busca por Newsletter/Blog
```
Queries:
  "[Expert Name] newsletter"
  "[Expert Name] blog"
  "[Expert Name] substack"
  "[Expert Name] writes"

Filtrar: escrito pelo expert
Classificar: Tier 2
```

### 2.5 — Busca por YouTube
```
Queries:
  "[Expert Name]" canal oficial
  "[Expert Name] talk"
  "[Expert Name] lecture"

Filtrar: canal oficial ou entrevistas 20min+
Classificar: Tier 2 (20-60min) ou Tier 1 (60min+)
```

---

## Passo 3 — Classificação de Cada Fonte

Para cada fonte identificada, preencher:

```yaml
fonte:
  titulo: ""
  url_ou_ref: ""
  tipo: "livro | curso | entrevista | palestra | newsletter | podcast | youtube | social | outro"
  autor_ou_host: ""
  ano: ""
  duracao_ou_paginas: ""
  tier: "1 | 2 | 3 | discard"
  motivo_tier: ""
  acessivel: true | false
  formato: "texto | audio | video | pdf"
  notas: ""
```

### Regras de Classificação

```
TIER 1 (Ouro):
  ✓ Escrito/gravado pelo próprio expert
  ✓ Volume: livro (150p+) | curso (2h+) | entrevista (60min+) | palestra (45min+)
  ✓ Conteúdo substantivo, não superficial
  ✓ Expert responde perguntas (não apenas lê script)

TIER 2 (Prata):
  ✓ Criado pelo expert OU entrevista substancial
  ✓ Volume: 20-60min ou 2000+ palavras
  ~ Pode ser mais "preparado" que espontâneo

TIER 3 (Bronze):
  ✓ Qualquer outro conteúdo verificável
  ✗ Volume baixo (< 5min ou < 500 palavras)

DISCARD:
  ✗ Criado por terceiros sobre o expert
  ✗ Resumos, paráfrases, especulações
  ✗ Sem fonte verificável
```

---

## Passo 4 — Aplicar Quality Gate

```
GATE 1 — Quantidade Mínima
  Fontes Tier 1 encontradas: ___
  Mínimo necessário: 3

  SE < 3 Tier 1:
    → Reportar o que foi encontrado
    → Listar o que falta
    → Oferecer alternativas:
       a) User fornece fontes adicionais
       b) Reduzir escopo para sub-domínio
       c) Continuar com aviso de baixa fidelidade (aceita score < 60)

GATE 2 — Cobertura Temporal
  Fontes dos últimos 3 anos disponíveis?
  SE NÃO → avisar que DNA pode estar desatualizado

GATE 3 — Diversidade de Formato
  Tem pelo menos 2 formatos diferentes (ex: livro + entrevista)?
  SE NÃO → avisar que pode ter viés de formato
```

---

## Passo 5 — Priorização para Extração

Ordenar fontes por prioridade de extração:

```
PRIORIDADE 1 (extrair primeiro):
  - Livros mais recentes (últimos 3 anos)
  - Entrevistas adversariais longas (expert sob pressão = DNA mais autêntico)
  - Cursos (conteúdo mais deliberado e completo)

PRIORIDADE 2:
  - Livros mais antigos (DNA de origem)
  - Podcasts longos
  - Palestras gravadas

PRIORIDADE 3:
  - Newsletter/blog
  - Entrevistas médias
  - YouTube educacional
```

---

## Output: sources.yaml

```yaml
expert: "[Nome do Expert]"
discovery_date: "[YYYY-MM-DD]"
total_sources: 0
tier1_count: 0
tier2_count: 0
tier3_count: 0
gate_passed: false
notes: ""

sources:
  tier1:
    - titulo: ""
      url_ou_ref: ""
      tipo: ""
      ano: ""
      duracao_paginas: ""
      acessivel: true
      formato: ""
      prioridade_extracao: 1

  tier2:
    - titulo: ""
      url_ou_ref: ""
      tipo: ""
      ano: ""
      duracao_paginas: ""
      acessivel: true
      formato: ""

  tier3:
    - titulo: ""
      url_ou_ref: ""
      tipo: ""
      notas: ""

  discarded:
    - titulo: ""
      motivo: ""

extraction_order:
  - source: ""
    priority: 1
    reason: ""
```

---

## Passo 6 — Criar pipeline-state.yaml

Ao final da Task 01, criar o arquivo de estado do pipeline:

```yaml
# output/{expert-slug}/pipeline-state.yaml
expert:
  name: "{Expert Name}"
  slug: "{expert-slug}"
  started_at: "{YYYY-MM-DDTHH:MM:SS}"
  last_updated: "{YYYY-MM-DDTHH:MM:SS}"

pipeline:
  current_phase: 1
  completed_tasks: ["01"]
  in_progress_task: null
  blocking_task: null
  resume_from: ""

gates:
  G1: { status: "PASSED | FAILED", tier1_count: 0, total_sources: 0 }
  G2: { status: "PENDING", fidelity_score: null }
  G3: { status: "PENDING", confirmed_by_user: false }
  G4: { status: "PENDING", pre_validation_verdict: "" }

fidelity:
  current_score: null
  components:
    A_frameworks: null
    B_phrases: null
    C_behavioral: null
    D_vocabulary: null
    E_examples: null

artifacts:
  sources_yaml:       { exists: true,  completeness: "full", path: "output/{slug}/sources.yaml" }
  thinking_dna:       { exists: false, completeness: null, framework_count: 0, path: "" }
  voice_dna:          { exists: false, completeness: null, vocabulary_count: 0, phrases_count: 0, path: "" }
  behavioral_dna:     { exists: false, completeness: null, states_count: 0, path: "" }
  values_dna:         { exists: false, completeness: null, path: "" }
  validation_report:  { exists: false, path: "" }
  clone_type:         { exists: false, confirmed_by_user: false, path: "" }
  agent_file:         { exists: false, path: "" }
  smoke_test:         { exists: false, pre_validation_verdict: "", real_execution_status: "PENDING_MANUAL_TEST", path: "" }
  pipeline_state:     { exists: true,  path: "output/{slug}/pipeline-state.yaml" }

next_step: "tasks/02-extract-thinking-dna.md"
blocked_reason: ""
```

**Instrução:** Cada task subsequente (02-09) deve atualizar `current_phase`, `completed_tasks`, e o artifact correspondente neste arquivo ao concluir.

---

## Done When

- [ ] Todas as fontes identificadas e classificadas
- [ ] Gate 1 passado (≥ 3 Tier 1) OU alternativa acordada com user
- [ ] Ordem de extração definida
- [ ] `sources.yaml` produzido
- [ ] `pipeline-state.yaml` criado em `output/{expert-slug}/`
