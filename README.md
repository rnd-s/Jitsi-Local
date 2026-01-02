# Jitsi Local (Custom)

Este repositório contém uma **instalação local e customizada do Jitsi Meet**,
baseada no projeto oficial **docker-jitsi-meet**, mantido pela equipe do Jitsi.

O objetivo deste projeto é servir como **ambiente de estudo, laboratório e implantação própria**,
permitindo modificações e testes sem impactar o repositório oficial.

**Este NÃO é o repositório oficial do Jitsi.**

---

##  Objetivo do Projeto

- Implantar o **Jitsi Meet utilizando Docker e Docker Compose**
- Permitir **customizações locais** (configurações, segurança, testes)
- Servir como base para estudos de:
  - Docker
  - Infraestrutura de redes
  - Comunicação em tempo real (WebRTC)
- Manter compatibilidade com o projeto oficial do Jitsi

---

##  Projeto Base

Este repositório é derivado de:

- Repositório oficial: https://github.com/jitsi/docker-jitsi-meet
- Licença: **Apache License 2.0**

O histórico de commits foi preservado para manter rastreabilidade e compatibilidade.

---

## Estrutura do Projeto

Principais diretórios e arquivos:

- `docker-compose.yml` — Orquestração dos serviços
- `env.example` — Exemplo de variáveis de ambiente
- `prosody/` — Servidor XMPP
- `jicofo/` — Gerenciador de conferências
- `jvb/` — Jitsi Video Bridge
- `web/` — Interface Web
- `.github/workflows/` — Integração contínua (CI)
- `jibri/`, `jigasi/` — Serviços opcionais (gravação, SIP)

---

## Pré-requisitos

- Docker
- Docker Compose
- Sistema operacional Linux (recomendado)
- Portas liberadas (dependendo da configuração):
  - 80 / 443 (HTTP/HTTPS)
  - 10000/UDP (WebRTC)

---

##  Como Utilizar

### Clonar o repositório

```bash
git clone https://github.com/rnd-s/Jitsi-Local.git
cd Jitsi-Local

---


### Criar arquivo de ambiente

```bash
cp env.example .env
```

Edite o `.env` conforme sua necessidade.

---

### Subir os serviços

```bash
docker compose up -d
```

---

### Acessar

Abra no navegador:

```
http://localhost
```

(ou o domínio configurado no `.env`)

---

##  Atualizações do Projeto Oficial

O repositório oficial está configurado como **upstream**.

```bash
git remote -v
```

Para buscar atualizações do Jitsi oficial:

```bash
git fetch upstream
git rebase upstream/main
```

Depois:

```bash
git push origin main
```

Conflitos podem ocorrer se houver customizações profundas.

---

## Segurança

* Arquivos sensíveis (`.env`) **não devem ser versionados**
* Recomenda-se configurar:

  * HTTPS
  * Firewall
  * Autenticação de usuários
  * Tokens de acesso

---

##  Observações

* Este projeto é mantido para **uso educacional, laboratório e implantação própria**
* Não há vínculo oficial com a equipe do Jitsi
* Para contribuições oficiais, utilize o repositório original

---

##  Licença

Este projeto segue a licença **Apache 2.0**, conforme o projeto original.

---

##  Autor / Mantenedor

Repositório mantido por **rnd-s**
Customizações e organização próprias

```


