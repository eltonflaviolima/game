# 🕵️‍♂️ Stack: RPG de Investigação (Hybrid VTT)

Projeto de RPG de mesa virtual competitivo para sessões presenciais. O sistema utiliza uma arquitetura de "Tela Única" para o tabuleiro (TV/Monitor) e dispositivos móveis para ações privadas e confidenciais dos jogadores.

---

## 🏗️ Core Architecture
* **Backend:** [Django REST Framework (DRF)](https://www.django-rest-framework.org/) - Gestão de regras de negócio, persistência e validação de ações.
* **Frontend:** [Vite](https://vitejs.dev/) + [React](https://react.dev/) + [TypeScript](https://www.typescriptlang.org/) - Interfaces de alta performance para a "View do Tabuleiro" (TV) e "View do Jogador" (Mobile).
* **Database:** [PostgreSQL](https://www.postgresql.org/) - Armazenamento de logs do crime, rotas de fuga, orçamentos e fichas de personagens.

## ⚡ Real-Time & WebSockets (O "Coração" do Jogo)
* **[Django Channels](https://channels.readthedocs.io/):** Essencial para o sincronismo instantâneo entre o que o jogador faz no celular e o que aparece na TV.
* **[Redis](https://redis.io/):** Utilizado como *Channel Layer* para gerenciar o tráfego de mensagens em tempo real.
* **[Daphne](https://github.com/django/daphne):** Servidor ASGI para suportar o tráfego de WebSockets em produção.

## 🧠 AI Integration (O Mestre do Jogo)
* **[LangChain](https://www.langchain.com/):** Orquestração dos prompts para transformar a logística do crime em evidências narrativas.
* **[Pydantic / Instructor](https://github.com/jxnl/instructor):** Para garantir que a IA retorne dados estruturados (JSON) que o backend possa processar automaticamente.
* **IA de Validação:** Processamento semântico para validar os palpites finais dos investigadores contra a estratégia do criminoso.

## 🎨 Mapas & SVG Interativo
* **SVG Dinâmico:** Manipulação de arquivos vetoriais para representar o cenário do crime.
* **[Framer Motion](https://www.framer.com/motion/):** Para animações de "névoa de guerra", revelação de pistas e transições de turno no mapa principal.
* **[React-SVG](https://github.com/tanem/react-svg):** Injeção de código SVG para interação direta via DOM (mudança de cores de salas, ícones de evidências).

## 🔐 Sessões e Estado de Jogo
* **[Zustand](https://docs.pmnd.rs/zustand/):** Gerenciamento de estado global leve para controlar o turno atual e o estado do mapa na tela principal.
* **[TanStack Query](https://tanstack.com/query/):** Sincronização assíncrona entre o app mobile do jogador e o backend (inventário, gastos de Pontos de Investigação - PI).
* **[SimpleJWT](https://django-rest-framework-simplejwt.readthedocs.io/):** Autenticação segura para garantir que as ações confidenciais permaneçam privadas ao jogador.

## 📦 DevOps & Infra
* **Docker & Docker Compose:** Padronização dos ambientes de desenvolvimento e produção (Django + Redis + Postgres + Vite).
* **QR Code Session Join:** Sistema para que jogadores entrem na partida presencial escaneando um código gerado na TV.

---
*Documentação gerada para o projeto de RPG de Investigação Híbrido.*