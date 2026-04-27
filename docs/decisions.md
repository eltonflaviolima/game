# 📝 Registro de Decisões Técnicas (ADR)

Este documento registra as decisões arquiteturais e técnicas tomadas durante o desenvolvimento do projeto **Investigação RPG**.

---

## [ADR-001] Padronização de Infraestrutura com Docker
**Data:** 2026-04-27  
**Status:** Aceito

### Contexto
O projeto possui múltiplos serviços (Backend Django, Frontend React, Banco Postgres, Cache Redis). Garantir que todos os desenvolvedores e o ambiente de produção rodem a mesma configuração é crítico.

### Decisão
Utilizar **Docker e Docker Compose** para orquestração.
- **Backend:** Python 3.11-slim para manter a imagem leve.
- **Frontend:** Node 20-slim com Vite configurado para rodar em modo host (0.0.0.0).
- **Banco de Dados:** PostgreSQL 15-alpine.
- **Cache/WebSockets:** Redis 7-alpine.

### Consequências
- Facilidade de setup ("one-click start").
- Isolamento de dependências.
- Necessidade de ter o Docker Desktop/Daemon ativo na máquina local.

---

## [ADR-002] Comunicação em Tempo Real (WebSockets)
**Data:** 2026-04-27  
**Status:** Aceito

### Contexto
O jogo exige sincronismo em tempo real entre a tela principal (TV) e os dispositivos individuais (Mobile). Ações como gasto de PI, revelação de pistas e uso de poderes devem ser refletidas instantaneamente.

### Decisão
Utilizar **Django Channels** com **Redis** como Channel Layer.
- Implementação de `ProtocolTypeRouter` no `asgi.py`.
- Uso de `AuthMiddlewareStack` para autenticação de sockets.
- Servidor ASGI **Daphne** para gerenciar o tráfego.

### Consequências
- Baixa latência nas atualizações da TV.
- Complexidade adicional no gerenciamento de conexões e grupos (rooms).
- Dependência do Redis como infraestrutura crítica para o gameplay.

---
