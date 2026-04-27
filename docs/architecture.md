# 🏛️ Guia de Arquitetura e Padronização Técnica

Este documento define os padrões de arquitetura, nomenclatura e fluxo de dados para garantir a consistência entre o Backend (Django) e o Frontend (Vite).

---

## 1. Padrão de Arquitetura: Event-Driven Layered Architecture

O projeto seguirá uma estrutura de camadas para separar a lógica de jogo da infraestrutura da API.

* **API Layer (View/Consumer):** Recebe a requisição (REST) ou o evento (WebSocket).
* **Service Layer (Business Logic):** Onde reside a "inteligência" (cálculo de PI, validação de rota, gastos de orçamento).
* **Domain Layer (Models):** Definição das entidades (Jogador, Evidência, Crime, Sala).
* **Integration Layer (IA/Websockets):** Comunicação com serviços externos (OpenAI/LangChain) e broadcast para a tela principal.


---

## 2. Padrão de Nomenclatura (Naming Conventions)

### 🐍 Backend (Python/Django)
* **Arquivos e Pastas:** `snake_case` (ex: `services/investigation_logic.py`).
* **Classes:** `PascalCase` (ex: `CrimeSceneService`).
* **Funções e Variáveis:** `snake_case`.
    * *Funções de ação:* Devem começar com verbos (ex: `calculate_pi_cost`, `validate_alibi`).
    * *Booleans:* Devem começar com prefixos como `is_`, `has_` ou `can_` (ex: `is_guilty`, `has_remaining_budget`).

### ⚛️ Frontend (React/TypeScript)
* **Componentes:** `PascalCase` (ex: `EvidenceMap.tsx`).
* **Hooks Customizados:** prefixo `use` (ex: `useGameState.ts`).
* **Variáveis e Funções:** `camelCase` (ex: `currentPlayer`, `handleActionClick`).
* **Constantes/Enums:** `UPPER_SNAKE_CASE` (ex: `MAX_PLAYERS`, `GAME_STATUS.INVESTIGATING`).

---

## 3. Fluxo de Comunicação (WebSocket Events)

Para manter a TV e os celulares sincronizados, os eventos de WebSocket devem seguir o padrão `DOMAIN:ACTION`:

| Evento (Tipo) | Origem | Descrição |
| :--- | :--- | :--- |
| `GAME:START` | Celular (Mestre) | Inicia a partida para todos. |
| `PLAYER:MOVE` | Celular (Jogador) | Atualiza a posição no mapa SVG da TV. |
| `EVIDENCE:REVEAL` | Backend (IA) | Exibe nova pista na tela principal após validação. |
| `UI:UPDATE_PI` | Backend | Atualiza o saldo de pontos no celular do jogador. |

---

## 4. Estrutura de Pastas Sugerida

```text
root/
├── backend/ (Django)
│   ├── core/           # Configurações do projeto
│   ├── games/          # App principal do jogo
│   │   ├── services/   # Lógica de negócio (Onde a mágica acontece)
│   │   ├── consumers.py # Handlers de WebSocket
│   │   ├── signals.py   # Gatilhos automáticos (ex: IA dispara após ação)
│   │   └── models.py
├── frontend/ (Vite)
│   ├── src/
│   │   ├── components/
│   │   │   ├── TVView/    # Componentes exclusivos da tela principal
│   │   │   ├── PlayerView/# Componentes exclusivos do Mobile
│   │   │   └── Shared/    # UI comum (Botões, Modais)
│   │   ├── hooks/         # TanStack Query e logic reuse
│   │   ├── store/         # Zustand (Estado Global)
│   │   └── assets/        # SVGs dos Mapas
```

## 5. Regras de Ouro para o Desenvolvimento
* **Stateless API**: O backend não guarda estado na memória (RAM). Tudo deve estar no PostgreSQL ou Redis para evitar desonexões fatais.
* **Mobile First for Input**: O design do celular deve ser operável com uma mão.
* **TV Optimization**: A View da TV não deve ter botões clicáveis, apenas elementos visuais e animações automáticas.
* **IA Validation**: Toda resposta da IA deve ser salva em uma tabela de AuditLog para caso os jogadores questionem o resultado final.

---

## 6. Tecnologias
O projeto utilizará as seguintes tecnologias:
* **Backend:** Django REST Framework
* **Frontend:** Vite + React + TypeScript
* **Database:** PostgreSQL
* **Real-Time:** Django Channels + Redis + Daphne
* **AI:** LangChain + Pydantic / Instructor
* **Mapas:** SVG + Framer Motion + React-SVG
* **Sessões:** Zustand + TanStack Query + SimpleJWT
* **DevOps:** Docker + Docker Compose

---

## 7. Recomendações Finais
* **Documentação** Sempre mantenha esta documentação atualizada conforme o projeto evolui.
* **Testes** Escreva testes para garantir que as regras de negócio sejam sempre validadas corretamente.
* **Segurança** Implemente medidas de segurança para proteger os dados dos jogadores e do jogo.
* **Performance** Otimize o código para garantir que o jogo funcione perfeitamente em dispositivos móveis e na TV.
