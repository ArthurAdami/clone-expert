# Task: Generate Agent

**ID:** 08-generate-agent
**Agent:** clone-expert
**Input:** Todos os DNA YAMLs + clone_type.yaml + validation_report.yaml
**Output:** `{expert-name}.md` — agente pronto para instalação
**Template:** squad-agent.md (todo clone é Squad)

---

## Objetivo

Injetar o payload extraído no container universal e gerar o arquivo `.md` final do agente.

**Regra central:** O container não muda. O payload muda. A qualidade do output é diretamente proporcional à qualidade da extração.

---

## Passo 0.5 — Verificar Existência de Clone Anterior

ANTES de qualquer geração, verificar:

```
[ ] output/{expert-slug}/{expert-name}.md já existe?

SE existe:
  → Reportar: "Clone {expert} já existe (gerado em: {data do comentário HTML no final do arquivo})"
  → Perguntar:
    (A) Sobrescrever — renomeia anterior para {expert-name}-backup-{YYYY-MM-DD}.md e gera novo
    (B) Criar versão paralela — gera como {expert-name}-v2.md sem alterar o original
    (C) Cancelar — interromper geração

SE não existe:
  → Criar diretório output/{expert-slug}/ se necessário
  → Prosseguir para Passo 1
```

---

## Passo 1 — Preparar o Payload Completo

Antes de gerar, verificar que todos os componentes estão prontos:

```
PAYLOAD CHECKLIST:
  [ ] thinking_dna.yaml — completo e validado
  [ ] voice_dna.yaml — completo e validado
  [ ] behavioral_dna.yaml — completo e validado
  [ ] values_dna.yaml — completo (ou parcial aceito com flag)
  [ ] clone_type.yaml — confirmado pelo user
  [ ] validation_report.yaml — score ≥ 40
  [ ] sources.yaml — fontes documentadas

SE qualquer item crítico faltando → BLOQUEAR e reportar
```

---

## Passo 2 — Selecionar Template e Carregar Field Mapping

```
Todo clone usa templates/squad-agent.md
(não existe Single Persona — todo clone é Squad)

OBRIGATÓRIO: Carregar templates/field-mapping.yaml antes de injetar qualquer campo.
O field-mapping.yaml define:
  - Qual campo do YAML fonte mapeia para qual placeholder do template
  - O formato correto de cada seção (yaml_block, numbered_list, SE/ENTÃO, etc.)
  - Onde cada campo aparece (pode aparecer em múltiplas seções)

Seguir o mapeamento campo-a-campo. Dois executores com o mesmo DNA devem
produzir o mesmo output.
```

---

## Passo 3 — Injetar Payload no Container

### Seção por Seção:

**HEADER**
```
# {expert-name-slug}

> **{role_do_expert}** | {filosofia_central} | {descricao_breve}

Você é {Nome do Expert}, agente autônomo de {dominio_principal}.
Siga estes passos EXATAMENTE na ordem.
```

**STRICT RULES**
```
Basear em:
  - Veto conditions do thinking_dna
  - Immune system do behavioral_dna
  - Non-negotiables do values_dna
  - Vocabulário proibido do voice_dna

Formato: NEVER [comportamento proibido] — [razão se necessário]
Obrigatórios em todos os clones:
  - NEVER skip the greeting
  - NEVER use hedging language (listar proibidos do voice_dna)
  - NEVER [veto condition 1 do expert]
  - NEVER [veto condition 2 do expert]
  - Your FIRST action MUST be adopting the persona in Step 1
  - Your SECOND action MUST be checking conversation context (Step 1.5)
  - Your THIRD action MUST be displaying the greeting in Step 2
```

**STEP 2 — GREETING**
```
Construir com:
  - Ícone que represente o expert (emoji)
  - Nome do expert
  - Título/role
  - Quote de abertura = frase assinatura mais forte do voice_dna
  - Lista de especialistas com ícones e descrições (por tier)
  - Comandos inline disponíveis
  - Call to action no estilo de voz do expert
```

**COMMAND ROUTING (Step 3)**
```
Tabela: comando → arquivo do especialista (por tier)
Keywords para cada especialista (EN + PT)

Inline commands:
  - Equivalente ao *diagnose do Hormozi = diagnóstico central do expert
  - *help, *exit sempre presentes
```

**SCOPE**
```
what_i_do: derivado do domínio principal + especialidades identificadas
what_i_dont_do: o que está fora do escopo (rotar para outros agentes)
output_target: o que o agente entrega
```

**PERSONA**
```
role: do thinking_dna
identity: síntese do values_dna + thinking_dna (3-5 frases)
style: do voice_dna.tone_profile
focus: primary_framework + foco principal
communication:
  greeting: frase mais curta de ativação
  signature_closing: frase assinatura mais forte
  flattery: PROHIBITED (sempre)
  hedging: PROHIBITED (sempre)
```

**THINKING DNA**
```
primary_framework: do thinking_dna
mental_models: top 8 (ou todos se < 8) do thinking_dna
decision_tree: do thinking_dna
heuristics: top 7 do thinking_dna
veto_conditions: do thinking_dna + behavioral_dna
```

**VOICE DNA**
```
vocabulary.always_use: do voice_dna
vocabulary.never_use: do voice_dna
communication_style: do voice_dna.communication_patterns
antipattern_rejection_template: baseado no estilo de rejeição do behavioral_dna
immune_system: do behavioral_dna
```

**BEHAVIORAL STATES**
```
Para cada state do behavioral_dna, formatar como:
  on_{trigger_slug}:
    trigger: ""
    response: |
      [resposta exata no estilo do expert]
```

**OUTPUT EXAMPLES**
```
Mínimo 3, idealmente 4-5
Formato:
  - input: "[pergunta ou situação típica]"
    output: |
      [como o expert responderia, no estilo dele]

Fontes para exemplos:
  - Examples reais extraídos do behavioral_dna
  - Compostos a partir de frases assinatura + frameworks + immune system
  NUNCA inventar — apenas combinar elementos confirmados
```

**COMPLETION CRITERIA**
```
Tabela: tipo de missão → quando está done
Derivar dos domínios e modos do clone_type
```

**DEPENDENCIES**
```
Lista todos os arquivos de especialistas (por tier)
+ DNA files
+ config
+ nota de fallback
```

**CLOSING SIGNATURES**
```
3-4 frases assinatura mais fortes do voice_dna
Formato: *"frase"*
```

---

## Passo 4 — Naming Convention

```
Nome do arquivo: {primeiro-nome-sobrenome}.md
  Ex: alex-hormozi.md, seth-godin.md, paul-graham.md

Slug para comandos internos: {primeiro}-{sobrenome}
  Ex: alex-hormozi, seth-godin

ID dos especialistas (Tipo A): {expert}-{dominio}
  Ex: hormozi-offers, godin-brand
```

---

## Passo 5 — Quality Check Pré-Entrega

Antes de entregar o arquivo:

```
CHECKLIST FINAL:
  [ ] Nenhum placeholder {XXX} restante no arquivo? (buscar por { no output)
  [ ] Pelo menos 1 [SOURCE:] em cada seção principal?
  [ ] Vocabulário proibido NÃO aparece no body do agente?
  [ ] Frases assinatura aparecem nos exemplos e no closing?
  [ ] Immune system cobre os veto conditions?
  [ ] Greeting exibe todos os especialistas/modos?
  [ ] Dependencies lista todos os arquivos com paths em output/, não squads/?
  [ ] Todos os paths de specialists em DEPENDENCIES existem fisicamente?
      → SE path não existe: REPORTAR (não silenciar com fallback)
  [ ] Nota de fallback EXPLÍCITA (com aviso visível, não silenciosa)?
  [ ] Closing signatures presentes?

WATERMARK DE FIDELIDADE (verificar se necessário):
  SE validation_report.low_fidelity_flag == true (score < 60):
    [ ] Linha de watermark adicionada no GREETING após a quote de abertura:
        "⚠️ Fidelidade: {SCORE}/100 — Clone limitado | Gaps: {GAPS_SUMMARY} | *gaps para detalhes"
    [ ] Aviso adicionado nas STRICT RULES:
        "- ⚠️ FIDELIDADE LIMITADA: Score {SCORE}/100. Comportamento inconsistente em: {GAPS_LIST}."
```

---

## Output

```
{expert-name}.md — gerado e instalado automaticamente.

PASSO A — Salvar artefato:
  Destino: clone-expert/output/{expert-slug}/{expert-name}.md

PASSO B — Instalar localmente (EXECUTAR, não instruir):
  mkdir -p .claude/commands/
  cp clone-expert/output/{expert-slug}/{expert-name}.md .claude/commands/{expert-name}.md

PASSO C — Confirmar instalação:
  ✓ Arquivo em .claude/commands/{expert-name}.md
  ✓ Ativar com: /{expert-name} (reiniciar sessão se necessário)
```

---

## Done When

- [ ] Payload checklist completo
- [ ] Template selecionado
- [ ] Todos os placeholders substituídos
- [ ] Quality check final passando
- [ ] Arquivo `.md` gerado e revisado
- [ ] Arquivo salvo em `clone-expert/output/{expert-slug}/`
- [ ] Instalação local executada: `.claude/commands/{expert-name}.md`
- [ ] Confirmação de instalação reportada ao user
