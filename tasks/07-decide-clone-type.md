# Task: Design Squad Architecture

**ID:** 07-design-squad-architecture
**Agent:** clone-expert
**Input:** thinking_dna.yaml + validation_report.yaml
**Output:** `clone_type.yaml` — arquitetura do Squad com especialistas por tier

---

## Objetivo

Todo clone é um **Squad Orchestrator**. Esta task define a arquitetura do Squad: quantos especialistas, quais domínios, como organizar em tiers. A variação está no tamanho, não no tipo.

---

## Princípio

```
NÃO existe "Single Persona" neste framework.
Um expert com 1 domínio não vira um agente único —
vira um Squad pequeno (3-5 especialistas).

Por quê?
  → Qualquer expert tem sub-domínios mesmo dentro de um tema central
  → Routing para especialistas melhora SEMPRE a profundidade
  → Orchestrator pode responder genericamente, especialistas vão fundo
  → A estrutura Squad é mais extensível quando novas fontes são adicionadas
```

---

## Guia de Tamanho do Squad

| Fontes Tier 1 | Especialistas | Perfil |
|---------------|--------------|--------|
| 3-4 fontes    | 3-5 specs    | Squad pequeno — expert focado |
| 5-9 fontes    | 6-10 specs   | Squad padrão — expert com profundidade |
| 10+ fontes    | 10-15 specs  | Squad completo — expert prolífico |

---

## Passo 1 — Mapear Domínios

Listar todos os domínios identificados na extração:

```yaml
dominios_identificados:
  - nome: ""
    volume_de_conteudo: "alto | medio | baixo"
    profundidade: "especialista | bom | superficial"
    sobreposicao_com_outros: "alta | media | baixa"
    justifica_especialista: true | false
    tier_sugerido: 1 | 2 | 3
```

---

## Passo 2 — Definir Tiers

Organizar os domínios em tiers baseado em centralidade e volume:

```
TIER 0 — Orchestrator (Chief)
  Sempre presente. Diagnostica, roteia, aconselha.
  Cobre perguntas genéricas sem precisar de especialista.

TIER 1 — Core Specialists (2-3 especialistas)
  Critério: domínios com maior volume de conteúdo (livros principais)
  Critério: domínios que aparecem em TODA a obra do expert
  Critério: o que define a identidade profissional do expert

TIER 2 — Execution Specialists (2-5 especialistas)
  Critério: aplicação prática dos conceitos Tier 1
  Critério: domínios táticos com profundidade suficiente
  Critério: perguntas frequentes do público-alvo do expert

TIER 3 — Strategic Specialists (1-3 especialistas)
  Critério: diagnóstico, auditoria, perspectiva de alto nível
  Critério: domínios que requerem síntese de todos os outros
  Critério: mentoria, filosofia, visão de longo prazo
```

---

## Passo 3 — Definir Especialistas

Para cada domínio mapeado, definir o especialista:

```yaml
especialistas:
  - id: "{expert}-{dominio}"
    nome: ""
    papel: ""
    tier: 1 | 2 | 3
    dominio_principal: ""
    keywords_de_ativacao: []
    quando_usar: ""
    arquivo: "clone-expert/output/{expert}/tasks/{expert}-{dominio}.md"
```

**Naming convention para keywords:** Listar 3-5 palavras/frases que roteiam para este especialista.

---

## Passo 4 — Propor ao Usuário

Apresentar a arquitetura do Squad:

```
SQUAD DESIGN — {EXPERT_NOME}
=============================

TIER 0 — Orchestrator
  {expert}: Diagnostica, roteia, aconselha. Responde perguntas gerais.

TIER 1 — Core (X especialistas)
  *{specialist_1}: {desc}
  *{specialist_2}: {desc}
  *{specialist_3}: {desc}

TIER 2 — Execução (X especialistas)
  *{specialist_4}: {desc}
  *{specialist_5}: {desc}

TIER 3 — Estratégia (X especialistas)
  *{specialist_6}: {desc}
  *{specialist_7}: {desc}

Total: X especialistas
Baseado em: X fontes (Y Tier 1)

Confirma esta arquitetura? Posso:
  (A) Ajustar tiers ou renomear especialistas
  (B) Adicionar/remover especialistas
  (C) Confirmar e avançar para geração

SE usuário escolher (C):
  → Atualizar confirmed_by_user: true no clone_type.yaml
  → SOMENTE então prosseguir para Task 08
SE usuário escolher (A) ou (B):
  → Aplicar ajustes e re-apresentar para nova confirmação
```

---

## Passo 5 — Definir Escopo de Instalação

Perguntar ao usuário onde o clone será instalado:

```
ESCOPO DE INSTALAÇÃO

Este clone vai ser usado:

(A) Em TODOS os projetos — instalação global
    Caminho: ~/.claude/commands/{expert-name}.md
    Ativar com: /{expert-name} (em qualquer sessão)
    Ideal para: experts genéricos que servem qualquer contexto

(B) SÓ NESTE PROJETO — instalação local
    Caminho: .claude/commands/{expert-name}.md
    Ativar com: /{expert-name} (só neste projeto)
    Ideal para: persona de negócio, stakeholder, especialista de contexto

Qual o escopo? (A/B)
```

Registrar como `scope: global | local` no clone_type.yaml.

---

## Output: clone_type.yaml

```yaml
expert: ""
decision_date: ""
type: "squad"                    # sempre squad — não existe outro tipo

squad_architecture:
  orchestrator:
    role: "Chief — diagnostica, roteia, aconselha"
    handles_inline: []           # comandos que o orchestrator executa diretamente

  specialists:
    tier1:
      - id: ""
        nome: ""
        papel: ""
        keywords: []
        arquivo: ""
    tier2:
      - id: ""
        nome: ""
        papel: ""
        keywords: []
        arquivo: ""
    tier3:
      - id: ""
        nome: ""
        papel: ""
        keywords: []
        arquivo: ""

  total_specialists: 0

justification: ""
confirmed_by_user: false  # padrão — só muda para true após confirmação explícita no Passo 4
template_to_use: "templates/squad-agent.md"

# Escopo de instalação
scope: "global | local"
install_path: "~/.claude/commands/ | .claude/commands/"
install_command_global: "cp {expert-name}.md ~/.claude/commands/{expert-name}.md"
install_command_local: "mkdir -p .claude/commands/ && cp {expert-name}.md .claude/commands/{expert-name}.md"
install_note: "Usar o comando correspondente ao scope definido acima (global ou local)"
```

---

## Done When

- [ ] Domínios mapeados com tier sugerido
- [ ] Arquitetura do Squad definida (tiers + especialistas)
- [ ] Keywords de ativação definidas para cada especialista
- [ ] Arquitetura confirmada pelo usuário
- [ ] Escopo de instalação definido (global ou local)
- [ ] `clone_type.yaml` produzido com `type: squad`
