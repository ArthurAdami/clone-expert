# Clone Expert — Fábrica de Clones de Experts

> Sistema de extração de DNA e geração de agentes Claude com alta fidelidade comportamental.

---

## Visão Geral

O **Clone Expert** é um framework que transforma qualquer expert em um agente Claude funcional. Dado um nome e fontes de conteúdo, o sistema extrai o DNA completo dessa pessoa — como ela raciocina, como fala, como reage — e gera um arquivo `.md` instalável que se comporta como o original.

**Princípio central:** Um clone é tão bom quanto as fontes que o alimentam. Lixo entra, lixo sai.

**O que diferencia este sistema:**
- Não sumariza. **Extrai** — palavras exatas, padrões confirmados, citações verificáveis.
- Não inventa. Cada padrão incluído no clone tem `[SOURCE:]` rastreável.
- Não generaliza. Filtra o que é **único** do expert vs. o que qualquer um diria.
- Score numérico e verificável: fidelidade não é subjetiva.

---

## Índice

1. [Como Funciona — Visão Macro](#1-como-funciona--visão-macro)
2. [Arquitetura do DNA](#2-arquitetura-do-dna)
3. [Pipeline de 9 Fases](#3-pipeline-de-9-fases)
4. [Sistema de Qualidade](#4-sistema-de-qualidade)
5. [Arquitetura do Clone — Squad](#5-arquitetura-do-clone--squad)
6. [Classificação de Fontes](#6-classificação-de-fontes)
7. [Estrutura do Projeto](#7-estrutura-do-projeto)
8. [Início Rápido](#8-início-rápido)
9. [Referência de Comandos](#9-referência-de-comandos)
10. [Gates de Qualidade](#10-gates-de-qualidade)

---

## 1. Como Funciona — Visão Macro

```
INPUT
  └── Nome do expert
  └── Fontes (links, PDFs, transcrições) OU autorização para pesquisa autônoma

PIPELINE (5 fases, 9 tasks)
  │
  ├── FASE 1 — RESEARCH
  │     [01] Source Discovery      Identificar e classificar fontes
  │           Gate G1: ≥ 3 Tier 1 ──────────────────── BLOCKING
  │
  ├── FASE 2 — EXTRACTION (sequencial)
  │     [02] Thinking DNA          Como o expert raciocina
  │     [03] Voice DNA             Como o expert fala
  │     [04] Behavioral DNA        Como o expert reage
  │     [05] Values DNA            No que o expert acredita
  │
  ├── FASE 3 — VALIDATION
  │     [06] Validate Extraction   Calcular Fidelity Score
  │           Gate G2: score ≥ 40 ─────────────────── BLOCKING
  │
  ├── FASE 4 — GENERATION
  │     [07] Design Squad           Definir tiers, especialistas e arquitetura
  │           Gate G3: user aprova ───────────────── BLOCKING
  │     [08] Generate Agent        Produzir {expert}.md final
  │
  └── FASE 5 — TESTING
        [09] Smoke Test            Validar 3 comportamentos esperados

OUTPUT
  └── {expert}.md — pronto para instalação global ou local
  └── DNA YAMLs — artefatos de extração (reutilizáveis)
```

---

## 2. Arquitetura do DNA

O sistema extrai 4 camadas de DNA do expert. Cada camada é um arquivo YAML independente que alimenta a geração do agente.

### Thinking DNA — Como o Expert Raciocina

```yaml
# O que captura:
primary_framework: "O modelo mental mestre — o lens através do qual tudo é visto"

mental_models:        # 3-8 frameworks com nome, fórmula, aplicação prática
  - name: "..."
    formula: "..."    # ou descrição formal
    application: "..." # como aplicar na prática

decision_tree:        # 4-7 passos de como o expert chega a conclusões
  - "1. Primeira pergunta que ele faz"
  - "2. Segunda pergunta..."

heuristics:           # 5-8 regras SE/ENTÃO extraídas dos padrões do expert
  - "SE [condição] ENTÃO [ação]"

veto_conditions:      # 3-5 coisas que o expert NUNCA faz / sempre bloqueia
  - "BLOQUEAR [ação] quando [condição]"
```

**Por que Thinking DNA primeiro?** O primary framework do expert define como interpretar tudo que vem depois — a Voice DNA usa os termos do framework, e a Behavioral DNA reflete os veto conditions do Thinking.

---

### Voice DNA — Como o Expert Fala

```yaml
# O que captura:
vocabulary:
  always_use:           # 8+ termos exclusivos — as palavras que definem o estilo
    - "Termo exclusivo — NUNCA usar o equivalente genérico"
  never_use:            # 3+ termos proibidos — o que quebra a autenticidade
    - "Palavra proibida — por que não usar"

communication_style:    # 3-4 regras de estilo extraídas diretamente do conteúdo
  - "Frases curtas. Sem preenchimento."

signature_phrases:      # 5+ frases confirmadas em 3+ fontes independentes
  - "Frase exata que o expert repete [SOURCE: livro p.X]"

metaphors:              # 3+ metáforas/analogias características
  - "Metáfora específica que o expert usa para [conceito]"

antipattern_rejection_template: |
  Template de como o expert rejeita más ideias — no estilo dele

immune_system:          # 6-8 triggers + respostas automáticas
  - trigger: "Situação que viola os princípios"
    response: "Como o expert responderia, nas palavras dele"
```

---

### Behavioral DNA — Como o Expert Reage

```yaml
# O que captura:
behavioral_states:      # 4-5 estados comportamentais com trigger + response
  on_[situação]:
    trigger: "O que ativa este estado"
    response: |
      Como o expert reage — em primeira pessoa, no estilo dele

rejection_patterns:     # Como o expert rejeita ideias/pedidos que violam seus princípios
  - antipattern: "Nome do antipadrão"
    principle_violated: "Qual princípio viola"
    response: "Resposta-padrão de rejeição"
```

---

### Values DNA — No que o Expert Acredita

```yaml
# O que captura (mais difícil — frequentemente parcial):
value_hierarchy:        # Hierarquia de valores em ordem de prioridade
  - rank: 1
    value: "Valor mais importante"
    evidence: "[SOURCE: onde isso aparece]"

core_beliefs:           # 3+ crenças centrais que informam todas as decisões
  - belief: "O que o expert acredita profundamente"
    evidence: "[SOURCE:]"

obsessions:             # 2+ temas que aparecem em TODA a obra do expert
  - "Obsessão documentada em múltiplas fontes"

antipatterns_rejected:  # 2+ coisas que o expert explicitamente rejeita/critica
  - pattern: "O que ele rejeita"
    why: "Por quê — evidência"
```

---

## 3. Pipeline de 9 Fases

### Fase 1 — Source Discovery `tasks/01-source-discovery.md`

**Input:** Nome do expert
**Output:** `sources.yaml`
**Gate:** ≥ 3 fontes Tier 1 → BLOCKING

O agente faz 6 perguntas de elicitação sobre o expert e as fontes disponíveis. Classifica cada fonte em Tier 1/2/3/Descarte. Se o gate não passar, o pipeline é bloqueado até que fontes suficientes sejam identificadas.

```yaml
# Exemplo de sources.yaml
expert: "Nome do Expert"
tier1:
  - title: "Livro principal do expert"
    type: livro
    fidelity: 92%
  - title: "Entrevista longa (podcast 2h)"
    type: entrevista_longa
    duration_min: 120
    fidelity: 91%
tier2:
  - title: "Blog do expert — arquivo completo"
    type: blog
    posts: 200
    fidelity: 75%
total_tier1: 3
gate_g1: PASSED
```

---

### Fase 2 — Extração DNA `tasks/02-05`

**Sequência obrigatória:** Thinking → Voice → Behavioral → Values
**Output:** 4 arquivos YAML de DNA
**Gate:** Nenhum (partial allowed — extração incompleta avança para validação)

**Por que sequencial e não paralelo?**
- Thinking DNA (02) define o primary framework
- Voice DNA (03) usa os termos do framework para extrair vocabulário correto
- Behavioral DNA (04) precisa dos veto conditions do Thinking para mapear o immune system
- Values DNA (05) é o mais difícil — infere de múltiplos contextos

**Filtro Pareto ao Cubo** aplicado em cada extração:

```
0.8% GENIAL    → Insights que SÓ esse expert tem → OBRIGATÓRIO no clone
4%   EXCELENTE → Aparece em 3+ fontes independentes → INCLUIR
20%  ÚTIL      → Bom mas não exclusivo → INCLUIR só se reforça padrão único
76%  RUÍDO     → Qualquer especialista do domínio diria → DESCARTAR
```

---

### Fase 3 — Validação `tasks/06-validate-extraction.md`

**Output:** `validation_report.yaml` com fidelity score
**Gate:** Score ≥ 40 → BLOCKING

**Fórmula do Fidelity Score:**

```
[A] Frameworks únicos confirmados  × 10  (cap: 30 pts)
[B] Frases assinatura confirmadas  × 5   (cap: 25 pts)
[C] Behavioral states documentados × 8   (cap: 24 pts)
[D] Vocabulário exclusivo mapeado  × 3   (cap: 15 pts)
[E] Exemplos input/output reais    × 2   (cap:  6 pts)
────────────────────────────────────────────────────────
TOTAL                                        máx: 100
```

**Interpretação:**

| Score | Status | Ação |
|-------|--------|------|
| < 40 | BLOCKED | Extração insuficiente — voltar às tasks |
| 40-59 | WARN | Funcional — confirmar com user antes de gerar |
| 60-79 | PASS | Sólido — prosseguir normalmente |
| 80+ | PRODUCTION | Alta fidelidade — pronto para produção |

---

### Fase 4 — Geração `tasks/07-08`

**Task 07 — Decide Clone Type:** Analisa domínios identificados, decide Tipo A ou B, confirma com user (gate obrigatório).

**Task 08 — Generate Agent:**

```
1. Verificar payload checklist (todos os DNAs presentes)
2. Usar template squad-agent.md (todo clone é Squad)
3. Injetar payload seção por seção:
   - STRICT RULES (derivadas dos veto conditions)
   - PERSONA (role + style + focus)
   - THINKING DNA (frameworks + decision tree + heuristics)
   - VOICE DNA (vocabulary + immune system)
   - BEHAVIORAL STATES (trigger-response pairs)
   - SCOPE (what I do / don't do)
   - OUTPUT EXAMPLES (pares input/output com assinatura do expert)
   - COMPLETION CRITERIA
   - DEPENDENCIES
4. Quality check pré-entrega (9 itens)
5. Produzir {expert-name}.md
```

---

### Fase 5 — Smoke Test `tasks/09-smoke-test.md`

**3 testes obrigatórios:**

```
TESTE 1 — Fidelidade de Voz
  Input:  Pergunta genérica sobre o domínio principal
  PASS:   ≥ 2 termos always_use, zero never_use, tom correto

TESTE 2 — Fidelidade Comportamental
  Input:  Situação que viola um veto condition
  PASS:   Immune system ativa, rejeição no estilo do expert

TESTE 3 — Fidelidade de Raciocínio
  Input:  Problema que exige o primary framework
  PASS:   Aplica framework, segue decision tree, conclusão correta
```

| Resultado | Veredito | Ação |
|-----------|----------|------|
| 3/3 | APPROVED | Instalar |
| 2/3 | FUNCTIONAL | Entregar com nota de limitação |
| 1/3 | WEAK | Reextração recomendada |
| 0/3 | INVALID | Reextração necessária |

---

## 4. Sistema de Qualidade

### Fidelity Score — Detalhamento dos Componentes

**[A] Frameworks Únicos (×10, máx 30):**
Conta frameworks que: (1) são criados ou nomeados pelo expert, (2) aparecem em ≥ 3 fontes independentes, (3) têm aplicação prática documentada com exemplos.
*3 frameworks = 30 pts (teto atingido)*

**[B] Frases Assinatura (×5, máx 25):**
Frases que o expert repete em contextos diferentes, nas palavras exatas dele, confirmadas em ≥ 3 fontes. Não paráfrases — citações diretas.
*5 frases = 25 pts (teto atingido)*

**[C] Behavioral States (×8, máx 24):**
Padrões comportamentais com: trigger identificado + resposta documentada + tom verificado + pelo menos 1 citação. Não inferências — padrões observados.
*3 states = 24 pts (teto atingido)*

**[D] Vocabulário Exclusivo (×3, máx 15):**
Soma de termos `always_use` + termos `never_use` documentados com evidência. Termos que diferenciam o expert de outros no mesmo domínio.
*5 termos = 15 pts (teto atingido)*

**[E] Exemplos Output (×2, máx 6):**
Pares input/output reais ou reconstituídos fielmente com citação. Demonstram como o agente deve se comportar end-to-end.
*3 exemplos = 6 pts (teto atingido)*

### Filtro Pareto ao Cubo

```
O problema de clonar experts: 76% do conteúdo é ruído.
Generic advice que qualquer coach/consultor/autor diria.
Filtrar esse ruído é o que separa um clone de alta fidelidade
de um "resumo de livro com cara de agente".
```

| Nível | % do Conteúdo | Critério | Ação |
|-------|---------------|----------|------|
| GENIAL | 0.8% | Só esse expert tem. Ele criou ou nomeou. | OBRIGATÓRIO |
| EXCELENTE | 4% | Aparece em 3+ fontes. Aplicação específica dele. | INCLUIR |
| ÚTIL | 20% | Bom, mas outros diriam também. | Só se reforça padrão único |
| RUÍDO | 76% | Qualquer especialista do domínio diria. | DESCARTAR |

---

## 5. Arquitetura do Clone — Squad

**Todo clone é um Squad Orchestrator.** Não existe Single Persona neste framework.

A variação entre clones está no **tamanho do Squad**, não no tipo de arquitetura. Um expert ultra-focado vira um Squad pequeno (4-5 especialistas). Um expert prolífico vira um Squad grande (10-15 especialistas). A estrutura é sempre a mesma.

```
Por que sempre Squad?
  → Qualquer expert tem sub-domínios mesmo dentro de um tema central
  → Routing para especialistas sempre melhora profundidade de resposta
  → O Orchestrator cobre perguntas genéricas; especialistas vão fundo
  → Squads são extensíveis: novas fontes → novos especialistas
```

### Estrutura Universal

```
TIER 0 — Orchestrator (Chief)
  Sempre presente. Diagnostica, roteia, aconselha.
  Responde perguntas gerais sem carregar especialistas.

TIER 1 — Core Specialists (2-3 especialistas)
  Domínios centrais — maior volume de conteúdo, define a identidade do expert.

TIER 2 — Execution Specialists (2-5 especialistas)
  Aplicação prática dos conceitos Tier 1. Perguntas táticas e operacionais.

TIER 3 — Strategic Specialists (1-3 especialistas)
  Diagnóstico, auditoria, visão de longo prazo. Síntese de tudo.
```

### Guia de Tamanho

| Fontes Tier 1 | Especialistas | Perfil |
|---------------|--------------|--------|
| 3-4 fontes    | 3-5 specs    | Squad pequeno — expert focado |
| 5-9 fontes    | 6-10 specs   | Squad padrão |
| 10+ fontes    | 10-15 specs  | Squad completo — expert prolífico |

**Template:** `templates/squad-agent.md`

---

## 6. Classificação de Fontes

| Tier | Label | Fidelidade | Tipos de Fonte |
|------|-------|-----------|---------------|
| **Tier 1** | Ouro | 90-95% | Livros do expert, cursos gravados, entrevistas longas (1h+) adversariais, transcrições de palestras, newsletters (arquivo completo) |
| **Tier 2** | Prata | 70-85% | Podcasts como guest (30-60 min), YouTube educacional (20min+), blog posts extensos (2000+ palavras), Q&A em eventos gravados |
| **Tier 3** | Bronze | 40-60% | Clips curtos (< 5 min), posts de redes sociais, entrevistas como entrevistador, citações em artigos de terceiros |
| **Descarte** | Lixo | < 40% | Resumos de terceiros, paráfrases sem fonte verificável, especulação sobre o que X diria |

**Gate G1:** Mínimo 3 fontes Tier 1 para iniciar extração.

**Por que Tier 1 é crítico?**
- Tier 1 = expert falando sem filtro, com profundidade, no próprio formato
- Tier 3 = fragmentos que podem ser tirados de contexto ou distorcidos
- Um clone baseado em clips do YouTube sem livros captura o que é viral, não o que é real

---

## 7. Estrutura do Projeto

```
clone-expert/
│
├── README.md                              ← Este arquivo
├── config.yaml                            ← Configuração do sistema (thresholds, gates, scoring)
│
├── agents/
│   └── clone-expert.md                   ← Agente principal do framework
│
├── tasks/                                 ← Pipeline de 9 fases
│   ├── 01-source-discovery.md            FASE 1: Descobrir e classificar fontes
│   ├── 02-extract-thinking-dna.md        FASE 2: Extrair frameworks, modelos mentais
│   ├── 03-extract-voice-dna.md           FASE 2: Extrair vocabulário, frases, estilo
│   ├── 04-extract-behavioral-dna.md      FASE 2: Extrair triggers, immune system
│   ├── 05-extract-values-dna.md          FASE 2: Extrair crenças, hierarquia de valores
│   ├── 06-validate-extraction.md         FASE 3: Calcular fidelity score + gaps
│   ├── 07-decide-clone-type.md           FASE 4: Squad vs single persona
│   ├── 08-generate-agent.md              FASE 4: Gerar o .md final
│   └── 09-smoke-test.md                  FASE 5: Validar 3 comportamentos esperados
│
├── templates/
│   ├── agent-container.md                Referência universal de seções (não usar diretamente)
│   └── squad-agent.md                    Template único de geração (todo clone é Squad)
│   ├── extraction-template.yaml          Template YAML para os 4 DNAs
│   └── sources-template.yaml             Template YAML para sources.yaml (fase 01)
│
├── checklists/
│   ├── source-quality.md                 Gates de qualidade por tipo de fonte
│   ├── extraction-validation.md          Critérios de validação de cada DNA
│   └── fidelity-score.md                 Como calcular e interpretar o score
│
├── data/
│   ├── pareto-filter.yaml                Filtro Pareto ao Cubo (0.8/4/20/76)
│   ├── extraction-dimensions.yaml        6 dimensões de extração explicadas
│   └── example-extraction.yaml          Exemplo de extração com scores preenchidos
│
├── workflows/
│   └── full-clone-workflow.md            Workflow end-to-end com estimativas de tempo
│
└── output/                               ← Saídas temporárias do pipeline (limpar após instalar)
```

---

## 8. Início Rápido

### Instalar o Clone Expert

```bash
# Instalação global (disponível em todos os projetos)
cp clone-expert/agents/clone-expert.md ~/.claude/commands/

# Instalação local (disponível só neste projeto)
cp clone-expert/agents/clone-expert.md .claude/commands/
```

### Instalar um Clone Gerado

Os clones gerados pelo pipeline ficam temporariamente em `output/`. Após o pipeline finalizar, instale e limpe:

```bash
# Instalar (local)
cp clone-expert/output/{expert-slug}/{expert-slug}.md .claude/commands/

# Instalar (global)
cp clone-expert/output/{expert-slug}/{expert-slug}.md ~/.claude/commands/

# Limpar output após instalar
rm -rf clone-expert/output/{expert-slug}/
```

### Ativar

```
/clone-expert       ← Ativa o factory (para criar novos clones)
/{expert-slug}      ← Ativa o clone instalado
```

### Criar Novo Clone

```
1. Ativar:   /clone-expert
2. Executar: *clone "Nome do Expert"
3. O pipeline pergunta sobre fontes disponíveis
4. Aprovação sequencial nos gates (G1, G2, G3)
5. Agente gerado em clone-expert/output/{expert-slug}/
6. Instalar: cp output/{slug}/{slug}.md .claude/commands/
7. Limpar:   rm -rf clone-expert/output/{expert-slug}/
```

---

## 9. Referência de Comandos

### Comandos do Pipeline

| Comando | Fase | Output |
|---------|------|--------|
| `*clone {expert}` | Pipeline completo (1→9) | `{expert}.md` |
| `*research {expert}` | Fase 1 — source discovery | `sources.yaml` |
| `*extract-thinking` | Fase 2 — thinking DNA | `thinking_dna.yaml` |
| `*extract-voice` | Fase 2 — voice DNA | `voice_dna.yaml` |
| `*extract-behavior` | Fase 2 — behavioral DNA | `behavioral_dna.yaml` |
| `*extract-values` | Fase 2 — values DNA | `values_dna.yaml` |
| `*validate` | Fase 3 — validation | `validation_report.yaml` |
| `*decide-type` | Fase 4 — type decision | `clone_type.yaml` |
| `*generate` | Fase 4 — generation | `{expert}.md` |
| `*smoketest` | Fase 5 — smoke test | `smoke_test_report.yaml` |

### Comandos Utilitários

| Comando | O que faz |
|---------|-----------|
| `*score` | Calcula o fidelity score atual (mostra pontuação por componente) |
| `*gaps` | Lista o que está faltando para atingir score 60+ |
| `*pareto` | Aplica o filtro Pareto ao Cubo no conteúdo extraído |
| `*status` | Mostra status de todas as 9 fases com artefatos produzidos |
| `*help` | Lista todos os comandos disponíveis |
| `*exit` | Sai do modo Clone Expert |

---

## 10. Gates de Qualidade

O sistema tem 4 gates. Os 3 primeiros são BLOCKING (pipeline para se não passar).

```
G1 — Fontes mínimas (Fase 1)
  Regra:   ≥ 3 fontes Tier 1 identificadas
  Tipo:    BLOCKING — pipeline não avança sem isso
  Ação se falhar: Solicitar mais fontes ou autorizar pesquisa autônoma

G2 — Fidelity score (Fase 3)
  Regra:   Score ≥ 40/100 calculado
  Tipo:    BLOCKING (< 40) / WARN (40-59)
  Ação se falhar: Identificar gaps, voltar para extração com fontes específicas

G3 — Aprovação de tipo (Fase 4 — Task 07)
  Regra:   User aprova Tipo A ou B antes de gerar
  Tipo:    BLOCKING — user precisa confirmar
  Ação se falhar: Ajustar decisão conforme preferência do user

G4 — Smoke test (Fase 5)
  Regra:   ≥ 2/3 testes comportamentais passando
  Tipo:    NON-BLOCKING — entrega com nota de limitação se 2/3
  Ação se falhar: Ajustar DNA e regenerar (volta para task 08)
```

**Princípio dos gates:** A qualidade não é negociável nos dois primeiros gates. Um clone gerado sem fontes suficientes ou sem score mínimo vai produzir comportamento inconsistente e genérico — não vai se comportar como o original.

---

## Estimativas de Duração

```
Fase 1 — Research:      30-60 min
Fase 2 — Extraction:    60-300 min
  Task 02 Thinking:       30-60 min
  Task 03 Voice:          20-40 min
  Task 04 Behavioral:     20-40 min
  Task 05 Values:         20-40 min
Fase 3 — Validation:    15-30 min
Fase 4 — Generation:    30-60 min
Fase 5 — Testing:       15-30 min
─────────────────────────────────
TOTAL:                  2.5-8 horas
```

*A variação depende do volume de fontes disponíveis e da clareza do expert em expressar seus frameworks.*

---

*Clone Expert v1.0*
*"Um clone é tão bom quanto as fontes que o alimentam."*
*"Extração não inventa. Fidelidade é verificável."*
*"Score < 60 não vai para produção."*
