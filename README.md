# arch-agent

Agente de gerenciamento remoto de rede da plataforma [Archolt](https://app.archolt.com.br).

## Download

Baixe a versão mais recente para Linux x86_64:

```bash
curl -LO https://github.com/arch-software1/arch-agent-public/releases/latest/download/arch-agent
chmod +x arch-agent
sudo mv arch-agent /usr/local/bin/arch-agent
```

## Pré-requisitos

```bash
# Debian/Ubuntu
sudo apt-get install -y expect telnet snmp
```

## Uso

### Iniciar o agente

```bash
arch-agent start
```

Na primeira execução, o agente inicia o fluxo de registro automaticamente:

```
┌─────────────────────────────────────────────────────────────┐
│  Arch Agent — Registro necessário                           │
│                                                             │
│  Acesse a URL abaixo para registrar este agente:            │
│                                                             │
│  https://app.archolt.com.br/agent/register/<uuid>           │
│                                                             │
│  Aguardando confirmação...                                  │
└─────────────────────────────────────────────────────────────┘
```

Após confirmar no portal, o agente conecta ao servidor automaticamente.

### Registrar e iniciar separadamente

```bash
arch-agent register
arch-agent start
```

### Instalar como serviço (systemd)

```bash
arch-agent install-systemd
```

### Instalar como serviço (supervisor)

```bash
sudo arch-agent install-supervisor
```

### Atualizar manualmente

```bash
arch-agent update
```

### Remover dados de registro

```bash
arch-agent clear-registration
```

## Opções do comando `start`

| Flag | Descrição |
|------|-----------|
| `--server <host:porta>` | Endereço do servidor (padrão: `agente.archolt.com.br:443`) |
| `--daemon` | Executa em background |
| `--no-tls` | Desabilita TLS (uso local/dev) |
| `--max-concurrency <n>` | Máximo de comandos em paralelo (padrão: `40`) |

## Self-update

O agente verifica e aplica atualizações automaticamente ao iniciar e quando notificado pelo servidor. Nenhum comando em execução é interrompido durante o update.
