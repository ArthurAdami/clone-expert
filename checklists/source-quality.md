# Checklist: Source Quality Gates

**Usado em:** Task 01-source-discovery.md
**Agente:** clone-expert
**Propósito:** Validar se as fontes coletadas são suficientes para gerar um clone de alta fidelidade.

---

## Gate G1 — Quantidade Mínima de Fontes

```
[ ] ≥ 3 fontes Tier 1 identificadas
    (bloquear se < 3 — extração será rasa)

[ ] ≥ 2 fontes Tier 2 identificadas
    (aceitável se compensado por volume Tier 1)

[ ] Total ≥ 5 fontes de qualquer tier

RESULTADO:
  PASS: ≥ 3 Tier 1 + ≥ 5 total
  WARN: 2 Tier 1 + ≥ 4 total (prosseguir com aviso)
  BLOCK: < 2 Tier 1 (requerer mais fontes)
```

---

## Gate G2 — Cobertura Temporal

```
[ ] Fontes cobrem pelo menos 3 anos de conteúdo?

[ ] Tem conteúdo dos últimos 2 anos?
    (pensamento pode ter evoluído)

[ ] SE mudança de perspectiva detectada:
    fontes mais recentes sobrescrevem antigas?

RISCO: Expert que mudou de posição nos últimos 5 anos
  → Verificar qual visão representa o estado atual
  → Documentar em sources.yaml se há contradições temporais
```

---

## Gate G3 — Diversidade de Formato

```
[ ] Tem pelo menos 2 formatos diferentes?
    (livro + entrevista, podcast + artigo, etc.)

[ ] Tem conteúdo em contextos diferentes?
    (conteúdo próprio vs. entrevistas = perspectivas diferentes)

FORMATO          | VALOR DE EXTRAÇÃO
-----------------|-----------------
Livro            | MÁXIMO (curado, extenso, revisado)
Curso            | MÁXIMO (intencional, estruturado)
Entrevista longa | ALTO (espontâneo, defensivo, real)
Podcast próprio  | ALTO (conteúdo curado para público)
Podcast externo  | MÉDIO (responde, não só apresenta)
Blog/artigo      | MÉDIO (escrito = mais cuidadoso)
Clip/short       | BAIXO (fora de contexto)
Post social      | BAIXO (brevíssimo, possivelmente performático)
```

---

## Filtro de Qualidade Individual

Para cada fonte, verificar:

```
[ ] Fonte é primária? (o expert falando, não terceiros)
[ ] Tem data identificável?
[ ] Conteúdo substancial? (> 10 min ou > 1.000 palavras)
[ ] Não é paródia, crítica ou especulação?
[ ] Atribuição clara? (claramente é o expert que disse)

DESCARTAR automaticamente se:
  - "Top X frases do [expert]" (resumo de terceiros)
  - "O que [expert] quis dizer com..." (interpretação)
  - Conteúdo publicado por outros sobre o expert
  - Data anterior a 10 anos sem fonte Tier 1 mais recente confirmando
```

---

## Checklist de Classificação de Tier

```
TIER 1 (Ouro) — confirmar TODOS:
  [ ] Duração ≥ 45 minutos (ou livro/curso completo)
  [ ] Conteúdo primário do expert (não entrevistado respondendo)
  [ ] Suficientemente recente ou valor histórico confirmado
  [ ] Permite extração profunda de frameworks

TIER 2 (Prata) — confirmar MAIORIA:
  [ ] Duração 20-44 minutos
  [ ] Expert como participante central
  [ ] Conteúdo específico (não só promoção)
  [ ] Acrescenta ou confirma padrões do Tier 1

TIER 3 (Bronze) — confirmar TODOS:
  [ ] Conteúdo genuíno (não marketing puro)
  [ ] Atribuição clara
  [ ] Usa apenas para CONFIRMAR padrões encontrados no Tier 1/2
  [ ] NUNCA usar como fonte primária de extração

DESCARTAR — qualquer um:
  [ ] Conteúdo de terceiros sobre o expert
  [ ] Resumos, listas, "best of"
  [ ] Sem data identificável
  [ ] Contradição não explicada com fontes Tier 1
```

---

## Score de Diversidade de Fontes

```
Tipos diferentes presentes:
  Livro:           +3
  Curso completo:  +3
  Entrevista longa (+1h): +2
  Podcast próprio: +2
  Podcast externo: +1
  Blog/artigo:     +1
  Clip < 20min:    +0.5 (somente se confirmar Tier 1)

Score ≥ 7: Diversidade alta — extração robusta
Score 4-6: Diversidade média — extração funcional
Score < 4: Diversidade baixa — risco de distorção
```

---

## Red Flags — Sinais de Alerta

```
🚩 Todas as fontes são do mesmo formato
   → DNA pode ser afetado pelo contexto (ex: so entrevistas = defensivo)

🚩 Todas as fontes têm mais de 5 anos
   → Expert pode ter evoluído — sinalizar como risco

🚩 Menos de 3 fontes Tier 1
   → Fidelity score vai ser baixo — bloquear ou avisar

🚩 Fontes contraditórias sem explicação temporal
   → Verificar se houve mudança de visão ou fonte é questionável

🚩 Nenhuma fonte espontânea (apenas conteúdo curado)
   → DNA pode ser "versão de palco" — sinalizar como risco
```

---

## Output para sources.yaml

```yaml
source_quality_check:
  gate_g1_quantity:
    tier1_count: 0
    total_count: 0
    result: "PASS | WARN | BLOCK"

  gate_g2_temporal:
    oldest_source_year: 0
    newest_source_year: 0
    temporal_contradiction: false
    contradiction_notes: ""
    result: "PASS | WARN | BLOCK"

  gate_g3_format_diversity:
    formats_present: []
    diversity_score: 0
    result: "PASS | WARN | BLOCK"

  overall:
    result: "PASS | WARN | BLOCK"
    red_flags: []
    recommendation: ""
```
