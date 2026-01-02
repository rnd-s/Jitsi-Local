
# Jitsi Meet Local (Docker) — Guia Final de Configuração

Este repositório documenta toda a jornada real de configuração de um Jitsi Meet local, rodando com Docker / Docker Compose, incluindo autenticação, convidados, sala de espera (Lobby) e correções de estabilidade para rede local (LAN).

O objetivo é deixar claro o comportamento esperado do Jitsi, evitar falsas interpretações de erro e facilitar futuras manutenções.

---

## Visão Geral

Este ambiente fornece:

* Jitsi Meet rodando localmente (HTTPS na porta 8444)
* Autenticação via Prosody (XMPP)
* Suporte a usuários autenticados e convidados
* Sala de espera (Lobby) funcional
* Controle de moderador
* Correções de estabilidade para uso em rede local

Importante:
O Jitsi **não bloqueia usuários autenticados pelo Lobby**.
Se o usuário está autenticado, ele entra diretamente na sala.
O Lobby é aplicado apenas a convidados.

---

## Arquitetura dos Containers

O ambiente é composto por:

* jitsi-web
  Interface Web (HTTPS/8444)

* prosody
  Autenticação e XMPP (componente central)

* jicofo
  Controle das conferências

* jvb
  Media Bridge (áudio/vídeo via UDP)

Todos os serviços são orquestrados via `docker-compose.yml`.

---

## Pré-requisitos

* Docker e Docker Compose
* Navegador moderno (Chrome, Firefox ou Edge)
* IP fixo no servidor (exemplo: 192.168.0.100)

### Portas necessárias

| Porta | Protocolo | Função                  |
| ----- | --------- | ----------------------- |
| 8444  | TCP       | HTTPS (Web)             |
| 8000  | TCP       | HTTP                    |
| 10000 | UDP       | Áudio e vídeo (crítico) |

---

## Estrutura do Projeto

```text
Jitsi-Local/
├── docker-compose.yml
├── .env                  # Senhas e ajustes críticos
├── README.md             # Documentação
└── config/               # Persistência de dados
```

---

## Configuração de Segurança e Rede (.env)

As opções abaixo são fundamentais para funcionamento correto e estável em rede local:

```env
# Segurança
ENABLE_AUTH=1
ENABLE_GUESTS=1
AUTH_TYPE=internal
XMPP_DOMAIN=meet.jitsi

# Rede e Estabilidade
JVB_ADVERTISE_IPS=192.168.0.100
ENABLE_XMPP_WEBSOCKET=0
PUBLIC_URL=https://192.168.0.100:8444
```

### Explicação das opções

| Opção                   | Função                                 |
| ----------------------- | -------------------------------------- |
| ENABLE_AUTH=1           | Exige login para criar e moderar salas |
| ENABLE_GUESTS=1         | Permite convidados                     |
| AUTH_TYPE=internal      | Autenticação interna do Prosody        |
| JVB_ADVERTISE_IPS       | Corrige problemas de vídeo em LAN      |
| ENABLE_XMPP_WEBSOCKET=0 | Evita quedas com SSL autoassinado      |

Essa combinação resolve a maioria dos problemas comuns em ambientes locais.

---

## Criação de Usuários (Prosody)

Para criar usuários autenticados (administradores), utilize o comando abaixo:

```bash
sudo docker compose exec prosody \
prosodyctl --config /config/prosody.cfg.lua \
register admin meet.jitsi 'SuaSenhaForte'
```

Parâmetros:

* admin: nome do usuário
* meet.jitsi: domínio interno (não alterar)
* 'SuaSenhaForte': senha do usuário

Após isso, se necessário, reinicie os containers:

```bash
sudo docker compose down && sudo docker compose up -d
```

---

## Comportamento de Entrada na Sala

### Usuário autenticado

* Realiza login como anfitrião
* Entra diretamente na sala
* Torna-se moderador automaticamente

### Convidado

* Não realiza login
* Entra na sala de espera se o Lobby estiver ativado
* Aguarda aprovação do moderador

Esse é o comportamento oficial do Jitsi Meet.

---

## Sala de Espera (Lobby)

A sala de espera:

* Não bloqueia usuários autenticados
* Aplica-se apenas a convidados
* Não é ativada automaticamente

### Como ativar

Dentro da sala, como moderador:

1. Clique no ícone de segurança
2. Ative a opção "Habilitar sala de espera"

---

## Esclarecimento Importante

Mensagem comum:
"O lobby não está funcionando"

Na prática:

* Usuário autenticado entra direto: comportamento correto
* Convidado entra direto: o Lobby não foi ativado pelo moderador

O Lobby é um recurso de moderação, não de autenticação.

---

## Configuração do Navegador (Chrome / Edge)

Para permitir compartilhamento de tela com certificado HTTPS autoassinado:

1. Acesse
   `chrome://flags/#unsafely-treat-insecure-origin-as-secure`
2. Altere para Enabled
3. Adicione:
   `https://192.168.0.100:8444`
4. Reinicie o navegador

---

## Como Testar o Funcionamento

1. Entre na sala autenticado como administrador
2. Ative o Lobby
3. Abra uma aba anônima ou outro dispositivo
4. Entre na mesma sala

Resultado esperado:

* Convidado permanece aguardando permissão
* Administrador recebe solicitação para liberar a entrada

---
