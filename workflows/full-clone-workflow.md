# Workflow: Full Clone — End-to-End

**ID:** full-clone-workflow
**Agent:** clone-expert
**Duração estimada:** 2-8 horas (depende de disponibilidade e volume de fontes)
**Output:** `{expert-name}.md` — agente pronto para instalação

---

## Visão Geral

```
FASE 1: RESEARCH     → Descobrir e classificar fontes
FASE 2: EXTRACTION   → Extrair os 4 DNAs
FASE 3: VALIDATION   → Calcular fidelity score e validar
FASE 4: GENERATION   → Montar o agente final
FASE 5: TESTING      → Smoke test e aprovação
```

---

## Mapa do Workflow

```
START
  │
  ▼
[01] source-discovery
  │   Output: sources.yaml
  │   Gate G1: ≥ 3 fontes Tier 1
  │
  ├─── BLOCK → Aguardar mais fontes
  │
  ▼
[02] extract-thinking-dna
  │   Output: thinking_dna.yaml
  │
  ▼
[03] extract-voice-dna
  │   Output: voice_dna.yaml
  │
  ▼
[04] extract-behavioral-dna
  │   Output: behavioral_dna.yaml
  │
  ▼
[05] extract-values-dna
  │   Output: values_dna.yaml (parcial aceito)
  │
  ▼
[06] validate-extraction
  │   Output: validation_report.yaml + fidelity_score
  │   Gate G2: score ≥ 40
  │
  ├─── BLOCK (< 40) → Voltar para tasks de extração com gaps específicos
  ├─── WARN (40-59) → Confirmar com user antes de continuar
  │
  ▼
[07] decide-clone-type
  │   Output: clone_type.yaml (Squad — quantos specs, quais tiers)
  │   Confirmação do user obrigatória
  │
  ▼
[08] generate-agent
  │   Output: {expert-name}.md
  │
  ▼
[09] smoke-test
  │   Output: smoke_test_report.yaml
  │   Gate G3: 3/3 testes passando
  │
  ├─── APPROVED (3/3) → Instalar agente
  ├─── FUNCTIONAL (2/3) → Entregar com nota de limitação
  ├─── WEAK (1/3) → Recomendar reextração
  └─── INVALID (0/3) → Reextração necessária
```

---

## Fase 1 — Research

**Task:** `tasks/01-source-discovery.md`

### O que acontece

1. clone-expert faz 6 perguntas de elicitação sobre o expert
2. User responde com fontes que tem e/ou autoriza pesquisa
3. clone-expert classifica fontes em Tier 1/2/3/Discard
4. Executa Gate G1 (quantidade mínima)
5. Produz `sources.yaml`

### Checklist

```
[ ] Expert identificado com nome completo
[ ] ≥ 3 fontes Tier 1 encontradas
[ ] Fontes classificadas com tier e status
[ ] Diversidade de formatos verificada
[ ] Gate G1 passou
[ ] sources.yaml produzido
```

### Pontos de decisão

```
User pode fornecer fontes:
  → "Tenho os livros X e Y e esses podcasts"
  → "Pode pesquisar você mesmo"
  → "Só tenho YouTube"

SE apenas YouTube disponível:
  → Verificar se há vídeos longos (1h+) que qualificam como Tier 1
  → Sinalizar limitação de profundidade
```

---

## Fase 2 — Extraction (4 Tasks em Sequência)

**Tasks:** 02, 03, 04, 05

### Ordem de extração

```
02 → Thinking DNA primeiro
     (primary framework define como interpretar tudo depois)

03 → Voice DNA segundo
     (vocabulário e frases informam como escrever o agente)

04 → Behavioral DNA terceiro
     (immune system precisa dos veto conditions do thinking)

05 → Values DNA por último
     (mais difícil — requer inferência de múltiplos contextos)
```

### Para cada DNA:

```
1. Abrir fonte Tier 1 mais rica
2. Aplicar Pareto Filter (data/pareto-filter.yaml)
3. Extrair para template (templates/extraction-template.yaml)
4. Confirmar com extraction-validation checklist
5. Repetir para fontes adicionais (confirmar/expandir padrões)
6. Produzir arquivo YAML final
```

### Critérios de completude por DNA

```
THINKING DNA — pronto quando:
  [ ] ≥ 3 frameworks confirmados (3+ fontes cada)
  [ ] Decision tree ≥ 4 passos
  [ ] ≥ 5 heurísticas
  [ ] ≥ 2 veto conditions

VOICE DNA — pronto quando:
  [ ] ≥ 8 termos always_use únicos
  [ ] ≥ 3 termos never_use
  [ ] ≥ 5 frases assinatura confirmadas
  [ ] Tone profile definido

BEHAVIORAL DNA — pronto quando:
  [ ] ≥ 3 behavioral states com trigger + response
  [ ] ≥ 5 immune system triggers
  [ ] Rejection template no estilo do expert

VALUES DNA — aceito quando:
  [ ] ≥ 2 valores Tier 1 identificados
  [ ] ≥ 3 crenças centrais
  OU marcado como "parcial" com justificativa
```

---

## Fase 3 — Validation

**Task:** `tasks/06-validate-extraction.md`

### Fidelity Score

```
CÁLCULO:
  A. Frameworks únicos confirmados × 10  (máx 30)
  B. Frases assinatura confirmadas × 5   (máx 25)
  C. Behavioral states documentados × 8  (máx 24)
  D. Termos vocabulário mapeados × 3     (máx 15)
  E. Exemplos input/output verificáveis × 2 (máx 6)
  ────────────────────────────────────────────────
  TOTAL                                   máx 100

GATES:
  < 40  → BLOCK   → Voltar para extração
  40-59 → WARN    → Confirmar com user
  ≥ 60  → PASS    → Prosseguir para fase 4
```

### Gap analysis

```
Para cada componente abaixo do target:
  → Identificar fonte específica que pode preencher
  → Indicar qual trecho ou tipo de conteúdo buscar
  → Apresentar ao user com opção de:
    (a) Buscar mais conteúdo e completar extração
    (b) Gerar clone funcional com flag low_fidelity
    (c) Abortar e aguardar melhores fontes
```

---

## Fase 4 — Generation

**Tasks:** `tasks/07-decide-clone-type.md` + `tasks/08-generate-agent.md`

### Design do Squad (Task 07)

```
Todo clone é Squad. A decisão aqui é o TAMANHO:

Squad pequeno (3-5 specs) — quando:
  3-4 fontes Tier 1, expert com domínio focado

Squad padrão (6-10 specs) — quando:
  5-9 fontes Tier 1, expert com boa profundidade

Squad completo (10-15 specs) — quando:
  10+ fontes Tier 1, expert prolífico e multi-domínio

→ Apresentar arquitetura proposta (tiers + especialistas) ao user
→ Aguardar confirmação antes de gerar
```

### Geração (Task 08)

```
1. Verificar payload checklist (todos os DNAs + clone_type.yaml)
2. Usar template squad-agent.md (todo clone é Squad)
3. Injetar payload seção por seção
4. Executar quality check pré-entrega (checklist final)
5. Produzir {expert-name}.md
```

### Quality check final (Task 08)

```
[ ] Todos os {PLACEHOLDERS} substituídos?
[ ] Pelo menos 1 [SOURCE:] em cada seção principal?
[ ] Vocabulário proibido não aparece no body?
[ ] Frases assinatura nos exemplos?
[ ] Immune system cobre os veto conditions?
[ ] Greeting exibe todos os especialistas/modos?
[ ] Dependencies lista todos os arquivos necessários?
[ ] Nota de fallback presente?
[ ] Closing signatures presentes?
```

---

## Fase 5 — Testing

**Task:** `tasks/09-smoke-test.md`

### 3 Testes Obrigatórios

```
TESTE 1 — Fidelidade de Voz
  Input: Pergunta genérica sobre o domínio principal
  PASS se: usa ≥ 2 termos always_use, zero never_use, tom correto

TESTE 2 — Fidelidade Comportamental
  Input: Situação que viola um veto condition
  PASS se: immune system ativa, rejeição no estilo do expert

TESTE 3 — Fidelidade de Raciocínio
  Input: Problema que requer o primary framework
  PASS se: aplica framework, segue decision tree, chega à conclusão correta

VEREDITO:
  3/3 → APPROVED — instalar
  2/3 → FUNCTIONAL — entregar com nota
  1/3 → WEAK — reextração recomendada
  0/3 → INVALID — reextração necessária
```

---

## Instalação

```bash
# Após aprovação no smoke test:

# SE escopo = global (padrão para experts genéricos):
cp {expert-name}.md ~/.claude/commands/

# SE escopo = local (para personas de projeto específico):
mkdir -p .claude/commands/
cp {expert-name}.md .claude/commands/

# Ativar (em ambos os casos):
# /{expert-slug}
```

---

## Artefatos Produzidos

Todos salvos em `clone-expert/output/{expert-slug}/`

```
sources.yaml              → Fontes classificadas
thinking_dna.yaml         → DNA de raciocínio
voice_dna.yaml            → DNA de voz
behavioral_dna.yaml       → DNA comportamental
values_dna.yaml           → DNA de valores
validation_report.yaml    → Score e gaps
clone_type.yaml           → Squad design (tiers + specialists) + especialistas/modos
{expert-name}.md          → AGENTE FINAL
smoke_test_report.yaml    → Resultado dos testes
```

Criar o diretório antes de iniciar:
```bash
mkdir -p clone-expert/output/{expert-slug}
```

---

## Comandos do Workflow

```bash
# Iniciar clone do zero:
*clone [Nome do Expert]

# Retomar em fase específica:
*research     → Fase 1 (fontes)
*extract-thinking → Task 02
*extract-voice    → Task 03
*extract-behavior → Task 04
*extract-values   → Task 05
*validate     → Task 06 (score)
*decide-type  → Task 07
*generate     → Task 08
*smoketest    → Task 09

# Diagnóstico do estado atual:
*status       → Onde estamos no workflow
*score        → Fidelity score atual
*gaps         → O que está faltando
```

---

## Estimativas de Duração por Fase

```
Fase 1 (Research):     30-60 min
  (depende de busca de fontes)

Fase 2 (Extraction):   60-300 min
  (depende de volume de fontes disponíveis)
  Task 02: 30-60 min
  Task 03: 20-40 min
  Task 04: 20-40 min
  Task 05: 20-40 min

Fase 3 (Validation):   15-30 min

Fase 4 (Generation):   30-60 min

Fase 5 (Testing):      15-30 min

TOTAL:                 2.5-8 horas
```

---

## Done When

```
[ ] Todas as 5 fases completadas
[ ] Fidelity score ≥ 40 (preferencialmente ≥ 60)
[ ] 3/3 ou 2/3 smoke tests passando
[ ] {expert-name}.md gerado e revisado
[ ] smoke_test_report.yaml produzido
[ ] Instruções de instalação entregues ao user
```

---

*"The quality of the clone is directly proportional to the quality of the sources."*
*"If the extraction is shallow, the agent will be generic. No amount of good templates fixes bad input."*
