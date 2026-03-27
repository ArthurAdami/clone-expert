# Task: Smoke Test

**ID:** 09-smoke-test
**Agent:** clone-expert
**Input:** {expert-name}.md gerado
**Output:** `smoke_test_report.yaml`
**Gate:** 3/3 testes passando = clone aprovado para uso

---

## Objetivo

Validar que o clone gerado se comporta como o expert real, não como um agente genérico com o nome do expert.

**Regra central:** Se o clone responde da mesma forma que qualquer outro agente de IA, ele falhou. O clone deve ser identificável como aquele expert específico por alguém que o conhece bem.

---

> ⚠️ **NOTA IMPORTANTE — Natureza dos Testes**
>
> Os testes nesta task são **simulações executadas pelo clone-expert** com base nos DNAs extraídos.
> O clone-expert simula como o agente gerado responderia, comparando o output esperado com os padrões documentados.
>
> **Isso é uma pré-validação, não um teste real de execução.**
>
> Para um teste real e independente:
> 1. Instale o arquivo conforme o escopo definido na task 07:
>    - Global: `cp {expert-name}.md ~/.claude/commands/`
>    - Local: `mkdir -p .claude/commands/ && cp {expert-name}.md .claude/commands/`
> 2. Abra uma nova sessão e ative: `/{expert-name}`
> 3. Execute manualmente os 3 inputs documentados no `smoke_test_report.yaml`
> 4. Compare o comportamento real com o esperado
>
> **Veredito APPROVED aqui = pré-aprovação. Teste manual = validação final.**

---

---

## Os 3 Testes Obrigatórios

### Teste 1 — Fidelidade de Voz

**Objetivo:** Verificar que o vocabulário, estilo e frases são do expert.

**Input:** Pergunta genérica sobre o domínio principal do expert.

**Como avaliar:**
```
✓ PASS se:
  - Usa pelo menos 2 termos do vocabulário exclusivo (always_use)
  - NÃO usa nenhum termo proibido (never_use)
  - Tom profile bate com o extraído (certeza, diretividade, etc.)
  - Pelo menos 1 frase assinatura aparece ou é referenciada

✗ FAIL se:
  - Resposta poderia ser de qualquer agente de IA
  - Usa linguagem hedging que o expert nunca usaria
  - Vocabulário é genérico do domínio, não específico do expert
```

**Template de teste:**
```
Input: "Me explica o conceito mais importante que você ensina."

Esperado: resposta no estilo e vocabulário específico do expert,
          com framework central + frase assinatura + tom característico
```

---

### Teste 2 — Fidelidade Comportamental

**Objetivo:** Verificar que o immune system funciona.

**Input:** Situação que deveria ativar uma resposta de rejeição/redirecionamento.

**Como avaliar:**
```
✓ PASS se:
  - Aciona o immune system corretamente
  - Redireciona para o framework certo
  - Tom de rejeição bate com o comportamento documentado
  - Usa antipattern rejection template quando aplicável

✗ FAIL se:
  - Aceita a premissa errada sem questionar
  - Responde como se a pergunta estivesse correta
  - Tom de rejeição é genérico ("boa observação, mas...")
```

**Template de teste:**
```
Input: [situação que viola um veto condition do expert]

Esperado: rejeição imediata com explicação usando framework do expert,
          não uma resposta polida e genérica
```

---

### Teste 3 — Fidelidade de Raciocínio

**Objetivo:** Verificar que o decision tree é aplicado corretamente.

**Input:** Problema que requer o processo de raciocínio característico do expert.

**Como avaliar:**
```
✓ PASS se:
  - Aplica o primary framework ao problema
  - Segue a sequência do decision tree documentado
  - Usa heurísticas específicas do expert
  - Chega à mesma conclusão que o expert chegaria

✗ FAIL se:
  - Aplica raciocínio genérico de IA
  - Pula etapas do decision tree
  - Conclusão é diferente do que o expert diria
```

**Template de teste:**
```
Input: [problema típico do domínio do expert]

Esperado: aplicação do primary framework → decision tree →
          prescrição no estilo do expert
```

---

## Testes Complementares (Opcionais)

### Teste 4 — Roteamento (Tipo A apenas)

```
Input: "[domínio específico de um especialista]"
Esperado: routing correto para o especialista certo, não resposta direta
```

### Teste 5 — Contexto Mid-Conversation

```
Input: Carregar em conversa com contexto anterior
Esperado: Step 1.5 detecta e adapta o greeting
```

### Teste 6 — Consistência de Persona

```
Input: 3 perguntas diferentes em sequência
Esperado: Mesmo tom, mesmo vocabulário, mesmos frameworks em todas
```

---

## Passo 1 — Preparar os Testes

Antes de executar, personalizar os 3 testes para o expert específico:

```yaml
teste_1:
  input: "[pergunta sobre conceito central do expert]"
  vocabulario_esperado: []  # termos do always_use que devem aparecer
  vocabulario_proibido: []  # termos do never_use que NÃO devem aparecer
  frase_assinatura_esperada: ""
  tom_esperado: ""

teste_2:
  input: "[situação que viola um veto condition do expert]"
  immune_system_trigger: ""
  resposta_esperada_pattern: ""
  principio_que_deve_proteger: ""

teste_3:
  input: "[problema que requer o primary framework]"
  framework_esperado: ""
  passos_decision_tree: []
  conclusao_esperada: ""
```

---

## Passo 2 — Executar e Avaliar

Para cada teste:

```
1. Submeter o input ao clone gerado
2. Comparar output com critérios de avaliação
3. Classificar: PASS / FAIL / PARTIAL
4. Documentar o que passou e o que falhou
```

---

## Passo 3 — Diagnóstico de Falhas

SE algum teste FAIL:

```
FAIL em Teste 1 (Voz):
  → Revisar voice_dna — vocabulário pode estar insuficiente
  → Verificar se STRICT RULES inclui vocabulário proibido
  → Verificar se frases assinatura estão no OUTPUT EXAMPLES

FAIL em Teste 2 (Comportamento):
  → Revisar behavioral_dna — immune system pode estar fraco
  → Verificar se veto conditions estão no THINKING DNA
  → Adicionar behavioral state específico para o trigger

FAIL em Teste 3 (Raciocínio):
  → Revisar thinking_dna — decision tree pode estar incompleto
  → Verificar se primary framework está bem documentado
  → Adicionar heurísticas específicas para o tipo de problema
```

---

## Decisão Final

```
3/3 PASS  → Clone aprovado para uso. Instalar.
2/3 PASS  → Clone funcional mas imperfeito. Indicar qual área falhou.
1/3 PASS  → Clone fraco. Recomendar reextração antes do uso.
0/3 PASS  → Clone inválido. Reextração necessária.
```

---

## Output: smoke_test_report.yaml

```yaml
expert: ""
agent_file: ""
test_date: ""

tests:
  test_1_voice:
    input: ""
    output_received: ""
    resultado: "PASS | PARTIAL | FAIL"
    vocabulario_encontrado: []
    vocabulario_proibido_encontrado: []
    frases_assinatura_encontradas: []
    notas: ""

  test_2_behavioral:
    input: ""
    output_received: ""
    resultado: "PASS | PARTIAL | FAIL"
    immune_system_ativado: true | false
    redirecionamento_correto: true | false
    notas: ""

  test_3_reasoning:
    input: ""
    output_received: ""
    resultado: "PASS | PARTIAL | FAIL"
    framework_aplicado: ""
    decision_tree_seguido: true | false
    notas: ""

overall:
  passed: 0  # de 3
  verdict: "APPROVED | FUNCTIONAL | WEAK | INVALID"
  recommendation: ""
  gaps_identified: []
  next_steps: ""
```

---

## Done When

- [ ] 3 testes personalizados para o expert
- [ ] Testes executados e avaliados
- [ ] Diagnóstico de falhas documentado (se houver)
- [ ] Decisão final registrada
- [ ] `smoke_test_report.yaml` produzido
- [ ] SE aprovado: instruções de instalação entregues ao user
- [ ] SE aprovado: lembrar user de executar teste manual após instalação
