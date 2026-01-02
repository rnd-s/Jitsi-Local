# Jitsi Local (Custom)

Este repositório contém uma **instalação local e customizada do Jitsi Meet**,
baseada no projeto oficial [`jitsi/docker-jitsi-meet`](https://github.com/jitsi/docker-jitsi-meet).

O objetivo é servir como **ambiente de estudo, testes e implantação própria**,
permitindo customizações sem afetar o repositório oficial.

---

## 📌 Objetivo do Projeto

- Implantar o **Jitsi Meet via Docker** em ambiente local
- Permitir **customizações próprias** (configurações, segurança, testes)
- Facilitar estudos de:
  - Docker e Docker Compose
  - Infraestrutura de comunicação em tempo real
  - WebRTC
- Manter possibilidade de **atualização a partir do projeto oficial**

---

## 🧱 Base do Projeto

Este repositório é derivado de:

- **Projeto oficial**: `jitsi/docker-jitsi-meet`
- **Licença**: Apache 2.0

O histórico de commits foi preservado para manter compatibilidade e rastreabilidade.

⚠️ **Este NÃO é o repositório oficial do Jitsi.**

---

## 🗂️ Estrutura do Projeto

Principais componentes:

- `docker-compose.yml` — Orquestração dos serviços
- `env.example` — Exemplo de variáveis de ambiente
- `prosody/` — Servidor XMPP
- `jicofo/` — Gerenciamento de conferências
- `jvb/` — Jitsi Video Bridge
- `web/` — Interface Web
- `.github/workflows/` — CI (GitHub Actions)

---

## ⚙️ Pré-requisitos

- Docker
- Docker Compose
- Sistema Linux (recomendado)
- Porta 80 / 443 / 10000 liberadas (dependendo do setup)

---

## 🚀 Uso Básico

### 1️⃣ Clonar o repositório

```bash
git clone https://github.com/rnd-s/Jitsi-Local.git
cd Jitsi-Local
