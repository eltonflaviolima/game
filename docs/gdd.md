# GDD – Operação Banco Central

Jogo Híbrido de Dedução Social e Investigação Assistida por IA

## 1. Visão Geral do Projeto

**Operação Banco Central** é um jogo assimétrico de dedução social que combina tecnologia, narrativa procedural e investigação lógica.
Um grupo de até 8 jogadores participa de um assalto ocorrido em um banco. Antes da partida, todos criam seus próprios planos de roubo de forma anônima. O sistema escolhe um plano e torna seu autor o Culpado, enquanto os demais são Investigadores.

O jogo acontece em duas interfaces:

- TV – Mapa e mural de evidências.
- PWA – Ferramentas de planejamento, comunicação e palpites.

A IA atua como motor lógico e narrativo, gerando provas, avaliando hipóteses e conduzindo a partida.

## 2. Pilares de Design

### 2.1 Assimetria Estratégica

Um jogador tenta esconder a verdade; os outros tentam revelá-la.

### 2.2 Investigação Híbrida

TV como tabuleiro compartilhado + PWA como painel individual.

### 2.3 Narrativa e Lógica Geradas por IA

A IA analisa planos, cria falhas, gera evidências e valida palpites.

### 2.4 Economia de Ação

Os investigadores possuem Pontos de Investigação (PI) limitados.

### 2.5 Informações Privadas

O Culpado possui habilidades especiais com uso limitado.

## 3. Estrutura Geral (Game Loop)

### 3.1 Fase de Planejamento (PWA)

Todos criam um plano completo de roubo:

Elementos do Plano

- Comparsa: Solo (barato/discreto) ou Equipe (caro/arriscado).
- Ferramentas: Maçarico, explosivo, pendrive, inibidor de sinal.
- Trajeto completo: Entrada → interior → cofre → fuga.
- Orçamento: Pontos entre stealth, velocidade e cobertura de rastros.
  Impacto da IA

A IA avalia:

- coerência do plano,
- probabilidade de falhas,
- pistas relacionadas,
- inconsistências lógicas.

### 3.2 Fase do Incidente (Início da Partida)

O sistema sorteia um plano criado pelos jogadores.

A TV exibe:

- montante roubado,
- mensagem do culpado,
- resumo do incidente.

O autor do plano torna-se o Culpado (informação secreta).

### 3.3 Fase de Investigação (Rodadas)

Cada rodada inclui:

- Líder da Rodada
- Pode gastar até 12 PI por rodada.
- Discussão entre jogadores
- Ações de Investigação
- Revelam evidências conectadas ao plano sorteado.
- Algumas podem falhar de acordo com o nível de risco.

### 3.4 Fase de Solução

Um investigador pode fazer um palpite final, descrevendo:

- trajeto,
- método,
- comparsa,
- ferramentas,
- entrada e fuga,
- detalhe da execução.

A IA avalia o palpite e calcula precisão objetiva.

≥ 80%: Investigadores vencem.
< 80%: Investigador é eliminado.

Se todos falharem → Culpado vence.

## 4. Mecânicas Principais

### 4.1 Pontos de Investigação (PI)

O grupo inicia com 60 PI.

| Ação                | Custo | Resultado                  | Observações                    |
| :------------------ | :---- | :------------------------- | :----------------------------- |
| Perícia de Digitais | 1     | Detecção de marcas         | Pista fraca                    |
| Logs de Acesso      | 3     | Horários e sensores        | Pode gerar coincidências       |
| Câmeras             | 4     | Descrição de vultos e rota | Pode ser ambígua               |
| Interrogar NPC      | 5     | Depoimentos narrativos     | Pode contradizer outras pistas |
| Rastreio de Veículo | 6     | Sugestão de fuga           | Inclui ruído                   |
| DNA                 | 10    | Pista forte                | Limitado pela lógica           |

Novas Regras
Limite: 12 PI por rodada.
Ações podem falhar com chance proporcional ao plano.
Algumas ações geram pistas em cadeia.

### 4.2 Poderes do Culpado

Cada habilidade pode ser usada 1 vez:

| Habilidade       | Efeito                  | Risco                   |
| :--------------- | :---------------------- | :---------------------- |
| Sabotagem Fraca  | Cria pista ambígua      | Pode aparentar fraude   |
| Falsa Informação | Depoimento falso via IA | Pode ser contradito     |
| Pressão no Grupo | Reduz 3 PI do grupo     | Pode levantar suspeitas |

O Culpado não pode interferir em pistas fortes.

### 4.3 Ajuste por Número de Jogadores

| Jogadores | PI Inicial | Nº de Pistas Automáticas |
| :-------- | :--------- | :----------------------- |
| 4–5       | 45         | 4                        |
| 6–7       | 55         | 5                        |
| 8         | 60         | 5                        |

## 5. Sistema de IA

A IA tem três papéis:

### 5.1 Geradora de Provas

A partir do plano real, identifica:

- falhas,
- pistas físicas,
- inconsistências,
- possíveis testemunhas.

### 5.2 Narradora

Produz:

- textos de evidências,
- depoimentos,
- descrições de câmera,
- opcionalmente áudio TTS.

### 5.3 Avaliadora (Palpite Final)

Critérios e pesos:

| Critério               | Peso |
| :--------------------- | :--- |
| Entrada                | 25%  |
| Ferramenta principal   | 25%  |
| Ponto crítico do roubo | 20%  |
| Rota interna           | 15%  |
| Fuga                   | 10%  |
| Comparsa               | 5%   |

Score ≥ 80% = Investigadores vencem

## 6. Estrutura JSON

### 6.1 JSON do Plano

```json
{
  "playerId": "123",
  "entryPoint": "Porta Lateral",
  "tools": ["maçarico", "pendrive"],
  "accomplice": "Equipe",
  "route": [
    { "x": 10, "y": 20 },
    { "x": 15, "y": 30 },
    { "x": 40, "y": 60 }
  ],
  "escapeRoute": "Estacionamento Oeste",
  "budget": {
    "stealth": 4,
    "speed": 3,
    "coveringTracks": 3
  }
}
```

### 6.2 JSON de Evidências

```json
{
  "evidenceId": "ev_01",
  "type": "digital",
  "location": "Porta de Acesso Lateral",
  "strength": "weak",
  "description": "Marcas de toque recentes"
}
```

## 7. Interfaces

### 7.1 TV (Desktop React)

- Mapa SVG interativo
- Mural de Provas
- Lista de Suspeitos
- Ações do Líder

### 7.2 PWA

- Modo Planejador (canvas + inventário)
- Modo Investigador (sugestões + ações + chat)
- Modo Culpado (poderes + plano original)

## 8. Condições de Vitória

### Investigadores vencem se:

- score ≥ 80% no palpite final antes de os PI acabarem.

### Culpado vence se:

- os PI chegam a 0, ou
- todos os investigadores erram seus palpites.

## 9. Regras de Contorno

- Empates de acusação → prioridade do Líder.
- Desconexões → retorno automático, sem punição.
- Reinício → reusar planos ou recriar todos.
