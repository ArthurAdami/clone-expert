# Task: Extract Behavioral DNA

**ID:** 04-extract-behavioral-dna
**Agent:** clone-expert
**Input:** Fontes classificadas (sources.yaml)
**Output:** `behavioral_dna.yaml`
**Mínimo viável:** 3 behavioral states + 5 immune system triggers + 2 antipadrões rejeitados

---

## Objetivo

Extrair como o expert REAGE. Como ele responde a situações específicas, o que dispara respostas características, o que ele bloqueia automaticamente, como ele se defende de ataques ao seu modelo de pensamento.

**Regra central:** Behavioral DNA é o que separa um clone que "soa como" do clone que "age como". É a camada mais difícil de extrair — e a mais valiosa.

---

## O que extrair

### 1. Behavioral States (Estados de Resposta)

Situações específicas que ativam um modo de resposta característico:

```yaml
behavioral_state:
  nome: ""
  trigger: ""           # o que ativa esse estado
  output: ""            # o que ele faz/diz nesse estado
  tom: ""               # como é o tom nesse estado
  duracao_tipica: ""    # quanto dura a interação
  sinais_de_ativacao:   # frases/padrões que indicam que está nesse estado
    - ""
  exemplo_real:
    input: ""
    output: ""
    fonte: "[SOURCE:]"
```

**Estados comuns a extrair:**
- Estado de diagnóstico (quando alguém traz um problema vago)
- Estado de ensinamento (quando está explicando um conceito)
- Estado de rejeição (quando discorda fortemente)
- Estado de validação (quando concorda com algo bem executado)
- Estado de questionamento (quando a premissa está errada)
- Estado de conselho direto (quando perguntam "o que você faria?")

---

### 2. Immune System (Defesas Automáticas)

O que ele rejeita ou redireciona automaticamente:

```yaml
immune_system:
  - trigger: ""
    response: ""        # o que ele diz/faz
    principle: ""       # qual princípio está protegendo
    tom: ""             # como é o tom da resposta
    fonte: "[SOURCE:]"
    confirmacoes: 0
```

**Como encontrar:**
- Momentos onde ele corrige a premissa da pergunta
- Quando ele diz "essa não é a pergunta certa"
- Quando ele para e redireciona completamente
- Quando ele recusa explicitamente uma sugestão
- Padrões de "antes de responder X, preciso questionar Y"

---

### 3. Antipadrões Rejeitados

O que ele explicitamente combate e por quê:

```yaml
antipadrao:
  nome: ""
  descricao: ""         # o que é o antipadrão
  principio_violado: "" # qual princípio ele viola
  consequencia: ""      # o que acontece se você faz isso
  alternativa: ""       # o que fazer em vez disso
  resposta_template: "" # como ele tipicamente responde quando vê isso
  fonte: "[SOURCE:]"
  confirmacoes: 0
```

---

### 4. Como Ele Lida com Discordância

Como reage quando alguém discorda ou questiona:

```yaml
discordancia:
  padrao_geral: ""
  quando_cede: ""       # em que condições ele muda de posição
  quando_mantem: ""     # o que o faz manter a posição
  como_argumenta: ""    # estrutura do argumento de defesa
  exemplo_real:
    situacao: ""
    resposta: ""
    fonte: "[SOURCE:]"
```

---

### 5. Como Ele Lida com Pedidos Impossíveis/Ambíguos

Como reage quando a pergunta é mal formulada ou impossível:

```yaml
ambiguidade:
  padrao: ""            # o que ele faz quando a pergunta é vaga
  perguntas_padrao:     # perguntas que ele sempre faz para clarificar
    - ""
  exemplos:
    - input_vago: ""
      como_respondeu: ""
      fonte: "[SOURCE:]"
```

---

## Processo de Extração por Fonte

### Para Entrevistas/Podcasts (melhor fonte para behavioral DNA)

```
1. Identificar momentos onde o entrevistador faz uma pergunta que ele NÃO responde diretamente
   → Registrar como immune system

2. Identificar quando ele claramente discorda e como reage
   → Registrar em discordância

3. Identificar quando ele está no modo "ensinamento intenso"
   → Registrar como behavioral state

4. Identificar perguntas que ele repete para o entrevistador antes de responder
   → Registrar em ambiguidade

5. Identificar momentos onde ele diz "isso é um erro que vejo muito"
   → Registrar como antipadrão

6. Identificar quando ele rejeita a premissa da pergunta
   → Registrar como immune system
```

### Para Livros

```
1. Capítulos com "Common Mistakes", "What Not To Do", "Avoid This"
   → Antipadrões

2. Passagens com "Most people think X, but actually Y"
   → Immune system against conventional wisdom

3. Passagens com "The first thing I always ask is..."
   → Decision tree + behavioral state

4. Q&A sections ou "FAQ" capítulos
   → Behavioral states em ação
```

---

## Filtro: Comportamento Genuíno vs Performance

```
PERGUNTA: Esse comportamento aparece em contextos diferentes (livro + entrevista + ao vivo)?
SE SIM  → comportamento genuíno → INCLUIR
SE NÃO  → pode ser performance → MARCAR COMO INCERTO

PERGUNTA: Ele aplica esse comportamento mesmo quando é inconveniente?
SE SIM  → parte do caráter → INCLUIR
SE NÃO  → provavelmente situacional → INCLUIR COM CONTEXTO
```

---

## Output: behavioral_dna.yaml

```yaml
expert: ""
extraction_date: ""
sources_used: []

behavioral_states:
  - nome: ""
    trigger: ""
    output: ""
    tom: ""
    sinais_de_ativacao: []
    exemplo_real:
      input: ""
      output: ""
      fonte: "[SOURCE:]"

immune_system:
  - trigger: ""
    response: ""
    principle: ""
    fonte: "[SOURCE:]"
    confirmacoes: 0

antipatterns:
  - nome: ""
    descricao: ""
    principio_violado: ""
    consequencia: ""
    alternativa: ""
    resposta_template: ""
    fonte: "[SOURCE:]"

discordancia:
  padrao_geral: ""
  quando_cede: ""
  quando_mantem: ""
  como_argumenta: ""

ambiguidade:
  padrao: ""
  perguntas_padrao: []

gaps:
  - componente: ""
    o_que_falta: ""
    fonte_sugerida: ""
```

---

## Done When

- [ ] Mínimo 3 behavioral states com trigger + output + exemplo real
- [ ] Mínimo 5 immune system triggers documentados com fonte
- [ ] Mínimo 2 antipadrões com princípio violado + alternativa + template de resposta
- [ ] Padrão de lidar com discordância documentado
- [ ] Padrão de lidar com ambiguidade documentado
- [ ] `behavioral_dna.yaml` produzido
