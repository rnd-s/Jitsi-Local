 Jitsi Local (Docker)

Este repositório contém uma **instalação local do Jitsi Meet usando Docker Compose**, baseada no projeto oficial [`jitsi/docker-jitsi-meet`], adaptada para **uso em rede local (LAN)**, com **autenticação habilitada** e **sem Let's Encrypt**.

O objetivo é permitir a execução do Jitsi em ambiente de testes, laboratório ou uso interno, sem depender de domínio público ou certificado HTTPS válido.

---

##  Visão Geral da Arquitetura

* Jitsi Meet (Web)
* Prosody (XMPP)
* Jicofo (Controle de conferências)
* JVB (Jitsi Videobridge)
* Docker + Docker Compose
* Rede local (LAN)

Todos os serviços rodam em contêineres Docker, orquestrados via `docker-compose.yml`.

---

##  Estrutura do Projeto

```
.
├── docker-compose.yml
├── .env                # Arquivo de configuração principal
├── base/
├── prosody/
├── jicofo/
├── jvb/
├── web/
├── jigasi/
├── jibri/
├── resources/
└── README.md
```

---

## Configurações Utilizadas (.env)

Abaixo estão as **principais configurações efetivamente utilizadas neste projeto**.

###  Rede e Acesso

* **IP do servidor:** `192.168.0.100`
* **Ambiente:** Rede local (LAN)

```env
HTTP_PORT=8000
HTTPS_PORT=8444
PUBLIC_URL=https://192.168.0.100:8444
JVB_ADVERTISE_IPS=192.168.0.100
```

O acesso ao Jitsi é feito via navegador em:

```
https://192.168.0.100:8444
```

> Como não há certificado válido, o navegador exibirá aviso de segurança.

---

###  HTTPS / Certificados

* **Let's Encrypt:** ❌ Desabilitado

```env
ENABLE_LETSENCRYPT=0
```

Motivo: ambiente local sem domínio público.

---

###  Autenticação

Este setup **exige login para criar salas**, mas **permite convidados** aguardarem no lobby.

```env
ENABLE_AUTH=1
ENABLE_GUESTS=1
AUTH_TYPE=internal
```

 Comportamento:

* Usuários autenticados → podem criar salas
* Convidados → aguardam liberação no lobby

As contas de usuários são gerenciadas internamente pelo Prosody.

---

###  Segurança (Senhas de Serviços)

As senhas internas foram geradas com o script oficial:

```bash
./gen-passwords.sh
```

Exemplo de serviços protegidos:

* Jicofo
* JVB
* Jigasi
* Jibri

> **Nunca reutilizar essas senhas em outros serviços**.

---

###  Serviços Opcionais

| Serviço      | Status            |
| ------------ | ----------------- |
| Etherpad     | ❌ Não habilitado  |
| Whiteboard   | ❌ Não habilitado  |
| Jigasi (SIP) | ❌ Não configurado |
| Jibri        | ❌ Não utilizado   |
| JWT / LDAP   | ❌ Não configurado |

---

##  Como Executar

### Pré-requisitos

* Docker
* Docker Compose

### Clonar o repositório

```bash
git clone https://github.com/rnd-s/Jitsi-Local.git
cd Jitsi-Local/docker-jitsi-meet
```

### Criar / ajustar o `.env`

Copie o exemplo e ajuste conforme necessário:

```bash
cp env.example .env
```

> O `.env` **não deve ser versionado** se contiver segredos.

### Subir os containers

```bash
docker compose up -d
```

### Acessar

Abra no navegador:

```
https://192.168.0.100:8444
```

---

## Observações de Segurança

* Não expor esse setup diretamente à internet sem:

  * Firewall
  * HTTPS válido
  * Hardening adicional
* Ideal apenas para:

  * Laboratórios
  * Ambientes educacionais
  * Testes

---

##  Base do Projeto

Este projeto é baseado no repositório oficial:

* [https://github.com/jitsi/docker-jitsi-meet](https://github.com/jitsi/docker-jitsi-meet)

Com adaptações para uso local.

---

##  Autor

Configuração e adaptação para ambiente local por **rnd-s**.

---

##  Licença

Este projeto segue a mesma licença do projeto original do Jitsi Meet.
