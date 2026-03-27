# Checklist: Fidelity Score Calculation

**Usado em:** Task 06-validate-extraction.md
**Agente:** clone-expert
**Propósito:** Guia passo a passo para calcular o fidelity score e interpretar os resultados.

---

## Fórmula Geral

```
FIDELITY SCORE = A + B + C + D + E

  A = Frameworks únicos confirmados × 10  (máx 30)
  B = Frases assinatura confirmadas × 5   (máx 25)
  C = Behavioral states documentados × 8  (máx 24)
  D = Termos vocabulário mapeados × 3     (máx 15)
  E = Exemplos input/output verificáveis × 2 (máx 6)
                                    ─────────
                             TOTAL:  máx 100
```

---

## Componente A — Frameworks Únicos (máx 30 pts)

### Como contar

```
CONTA como framework confirmado:
  ✓ Nome específico e único (não genérico)
  ✓ Aparece em ≥ 3 fontes independentes
  ✓ O expert o criou, nomeou ou popularizou
  ✓ Tem estrutura definida (passos, variáveis, fórmula)

NÃO CONTA:
  ✗ Frameworks de outros experts que ele apenas usa
  ✗ Conceitos genéricos do domínio (ex: "funil de vendas")
  ✗ Frameworks com apenas 1-2 citações
  ✗ Versão ligeiramente adaptada de framework existente

CÁLCULO:
  Frameworks confirmados: ___
  × 10 = ___
  Cap aplicado? SE > 30 → usar 30
  COMPONENTE A = ___
```

### Interpretação

| Pontos | Frameworks | Qualidade |
|--------|-----------|-----------|
| 30 | 3+ | DNA de raciocínio sólido |
| 20 | 2 | Funcional, mas limitado |
| 10 | 1 | Fraco — clone vai generalizar |
| 0 | 0 | BLOQUEIO — sem identidade de raciocínio |

---

## Componente B — Frases Assinatura (máx 25 pts)

### Como contar

```
CONTA como frase confirmada:
  ✓ Aparece em ≥ 3 fontes diferentes
  ✓ Passa no teste de unicidade ("só esse expert diria isso?")
  ✓ Aparece em contextos espontâneos, não só palestras
  ✓ É uma frase completa, não apenas um termo

NÃO CONTA:
  ✗ Frases comuns do domínio
  ✗ Frases que qualquer pessoa instruída no domínio usaria
  ✗ Frases com apenas 1-2 ocorrências
  ✗ Variações muito parecidas (contar como 1)

CÁLCULO:
  Frases confirmadas: ___
  × 5 = ___
  Cap aplicado? SE > 25 → usar 25
  COMPONENTE B = ___
```

### Interpretação

| Pontos | Frases | Qualidade |
|--------|--------|-----------|
| 25 | 5+ | Voz muito característica |
| 15 | 3 | Identificável na maioria dos contextos |
| 10 | 2 | Voz fraca — clone soa genérico |
| 0 | 0-1 | FALHA — sem identidade de voz |

---

## Componente C — Padrões Comportamentais (máx 24 pts)

### Como contar

```
CONTA como behavioral state documentado:
  ✓ Trigger específico e identificável
  ✓ Resposta no estilo do expert (não genérica)
  ✓ Evidência em fonte real (não inferida)
  ✓ Aparece em ≥ 2 contextos diferentes

NÃO CONTA:
  ✗ Comportamentos genéricos ("é direto")
  ✗ Estados sem trigger claro
  ✗ Behaviors inferidos sem evidência
  ✗ Behaviors que qualquer expert do domínio teria

CÁLCULO:
  Behavioral states documentados: ___
  × 8 = ___
  Cap aplicado? SE > 24 → usar 24
  COMPONENTE C = ___

BÔNUS (incluído no cálculo):
  Immune system tem ≥ 5 triggers? SE SIM → +0 (já contabilizado)
  SE NÃO → penalizar reduzindo states confirmados
```

### Interpretação

| Pontos | States | Qualidade |
|--------|--------|-----------|
| 24 | 3+ | Clone vai rejeitar corretamente |
| 16 | 2 | Immune system parcial |
| 8 | 1 | Clone muito passivo |
| 0 | 0 | BLOQUEIO — clone não tem caráter |

---

## Componente D — Vocabulário Exclusivo (máx 15 pts)

### Como contar

```
CONTA cada termo:
  ✓ Passa no filtro de unicidade (só o expert usa assim)
  ✓ Tem alternativa genérica mapeada
  ✓ Confirmed por ≥ 2 fontes

SOMA: always_use + never_use

NÃO CONTA:
  ✗ Termos genéricos do domínio
  ✗ Termos sem alternativa genérica (não são exclusivos)
  ✗ Termos never_use sem evidência de que ele evita

CÁLCULO:
  Total de termos únicos (always + never): ___
  × 3 = ___
  Cap aplicado? SE > 15 → usar 15
  COMPONENTE D = ___
```

### Interpretação

| Pontos | Termos | Qualidade |
|--------|--------|-----------|
| 15 | 5+ | Vocabulário bem definido |
| 9 | 3 | Vocabulário básico presente |
| 3-6 | 1-2 | Fraco — clone vai usar linguagem genérica |
| 0 | 0 | FALHA — sem identidade linguística |

---

## Componente E — Exemplos Input/Output (máx 6 pts)

### Como contar

```
CONTA cada par input/output:
  ✓ Input é pergunta/situação real ou típica do domínio
  ✓ Output vem de fonte verificável (não inventado)
  ✓ Output demonstra pelo menos 2 elementos do DNA
  ✓ Output não poderia ser de outro expert

NÃO CONTA:
  ✗ Exemplos compostos inteiramente por inferência
  ✗ Exemplos genéricos que qualquer agente daria
  ✗ Outputs que violam veto conditions do expert

CÁLCULO:
  Pares verificáveis: ___
  × 2 = ___
  Cap aplicado? SE > 6 → usar 6
  COMPONENTE E = ___
```

---

## Cálculo Final

```
FIDELITY SCORE WORKSHEET:

  A (Frameworks):  ___/30
  B (Frases):      ___/25
  C (Behavioral):  ___/24
  D (Vocabulário): ___/15
  E (Exemplos):    ___/6
  ─────────────────────────
  TOTAL:           ___/100

CLASSIFICAÇÃO:
  0-39:  RASO       → BLOQUEAR geração
  40-59: FUNCIONAL  → GERAR com aviso
  60-79: SÓLIDO     → GERAR para uso geral
  80-99: ALTA FIDELIDADE → Pronto para produção
  100:   MÁXIMO (raro, exige fontes excepcionais)
```

---

## Análise de Gaps

### Prioridade de Correção por Componente

```
COMPONENTE A < mínimo (3 frameworks):
  CRÍTICO — bloqueia DNA de raciocínio
  AÇÃO: Extrair mais de fontes Tier 1 (livros especialmente)
  ONDE BUSCAR: Capítulos centrais, explicações de metodologia

COMPONENTE B < mínimo (3 frases):
  IMPORTANTE — clone vai soar genérico
  AÇÃO: Revisar entrevistas para frases repetidas
  ONDE BUSCAR: Memes do expert, citações mais compartilhadas

COMPONENTE C < mínimo (3 states):
  CRÍTICO — clone não tem caráter defensivo
  AÇÃO: Buscar momentos de discordância/rejeição
  ONDE BUSCAR: Debates, Q&As, situações de conflito

COMPONENTE D < mínimo (8 termos — cap em 5):
  IMPORTANTE — vocabulário fraco
  Meta de qualidade: ≥ 8 termos únicos (always_use + never_use)
  Cap de pontuação: 5 termos já atingem o máximo de 15 pts
  AÇÃO: Mapear glossário único do expert
  ONDE BUSCAR: Livros (índice remissivo), cursos, tweets

COMPONENTE E = 0:
  AVISO — clone vai gerar exemplos compostos
  AÇÃO: Extrair transcrições de respostas reais
  ONDE BUSCAR: Entrevistas com Q&A, vídeos de consultoria
```

---

## Benchmarks por Expert

```
Squad completo (10+ Tier 1, expert prolífico):
  Esperado:  70-90 (muitas fontes, voz bem documentada)
  Mínimo:    55 (para gerar squad funcional)

Squad padrão (5-9 Tier 1, expert com boa profundidade):
  Esperado:  50-70 (foco mais estreito, mas profundo)
  Mínimo:    40 (para gerar squad funcional)

Experts emergentes (pouco conteúdo):
  Esperado:  35-55 (limitação de material disponível)
  Mínimo:    40 (avisar sobre limitações)

Score total abaixo de 40:
  → BLOQUEAR geração — extração insuficiente
  → Identificar gaps críticos e extrair mais antes de prosseguir

Score total 40-59:
  → GERAR com flag low_fidelity: true e documentar limitações
  → Avisar user sobre áreas de comportamento inconsistente
```

---

## Output Resumido para validation_report.yaml

```yaml
fidelity_calculation:
  component_a:
    frameworks_confirmed: 0
    calculation: "0 × 10 = 0"
    score: 0
    max: 30
    status: "sufficient | insufficient"

  component_b:
    phrases_confirmed: 0
    calculation: "0 × 5 = 0"
    score: 0
    max: 25
    status: "sufficient | insufficient"

  component_c:
    states_documented: 0
    calculation: "0 × 8 = 0"
    score: 0
    max: 24
    status: "sufficient | insufficient"

  component_d:
    vocabulary_terms: 0
    calculation: "0 × 3 = 0"
    score: 0
    max: 15
    status: "sufficient | insufficient"

  component_e:
    examples_verified: 0
    calculation: "0 × 2 = 0"
    score: 0
    max: 6
    status: "sufficient | insufficient"

  total:
    score: 0
    max: 100
    classification: "raso | funcional | solido | alta_fidelidade"
    gate: "PASS | WARN | BLOCK"
```
