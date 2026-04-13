# Instalação do Arch Agent

Este guia explica como instalar o agente nos seus servidores Linux.
O agente permite que a equipe da ArchOLT gerencie e monitore seus equipamentos de rede remotamente.

---

## Antes de começar

Certifique-se de que o servidor possui os seguintes programas instalados:

```bash
sudo apt-get install -y expect telnet snmp
```

> Se o sistema usar `yum` no lugar de `apt-get` (CentOS/RHEL):
> ```bash
> sudo yum install -y expect telnet net-snmp-utils
> ```

---

## Opção 1 — Binário direto no Linux (recomendado)

Use esta opção em servidores **Linux Debian 12 ou 13**.

### 1. Baixar o agente

```bash
  curl -Lo /usr/local/bin/arch-agent \
 https://github.com/arch-software1/arch-agent-public/releases/download/v0.1.24/arch-

chmod +x /usr/local/bin/arch-agent
```

### 2. Registrar o agente

```bash
arch-agent register
```

O agente exibe um banner com um **código** de registro. Para confirmar:

1. Esteja **logado** em `https://app.archolt.com.br`
2. Acesse `https://app.archolt.com.br/agents`
3. Insira o **código** exibido no terminal

Você tem **60 minutos** para inserir o código. Se expirar, execute `arch-agent register` novamente.

Aguarde até ver a mensagem:

```
  ✓ Agente registrado com sucesso!
```

> O comando `arch-agent start` também inicia o registro automaticamente se o agente ainda não estiver registrado.

### 3. Iniciar o agente

```bash
arch-agent start
```

O agente vai iniciar e conectar automaticamente aos servidores da ArchOLT.

---

## Opção 2 — Docker Compose

Use esta opção se preferir usar Docker em vez do binário direto.

### 1. Instalar o Docker (se ainda não tiver)

```bash
curl -fsSL https://get.docker.com | sh
```

### 2. Criar o arquivo de configuração

```bash
mkdir -p /opt/arch-agent
cd /opt/arch-agent
```

Crie o arquivo `docker-compose.yml` com o seguinte conteúdo:

```yaml
services:
  arch-agent:
    image: ghcr.io/arch-software1/arch-agent:latest
    restart: unless-stopped
    volumes:
      - agent-data:/data/arch-agent

volumes:
  agent-data:
```

### 3. Iniciar o agente

```bash
docker compose up
```

Na primeira execução, o agente exibe um banner como este:

```
┌──────────────────────────────────────────────────────────────┐
│  Arch Agent — Registro necessário                            │
│                                                              │
│  ⚠  Você precisa estar logado na plataforma Arch.           │
│                                                              │
│  Acesse a URL e insira o código abaixo:                      │
│  https://app.archolt.com.br/agents                          │
│                                                              │
│  Código:       XXXX-XXXX                                    │
│  Válido até:   HH:MM:SS                                     │
└──────────────────────────────────────────────────────────────┘
```

**Como confirmar o registro:**

1. Esteja **logado** em `https://app.archolt.com.br`
2. Acesse `https://app.archolt.com.br/agents`
3. Insira o **código** exibido no banner

O agente verifica a confirmação automaticamente a cada 5 segundos. Você tem **60 minutos** para inserir o código antes de expirar.

Após a confirmação, você verá:

```
  ✓ Agente registrado com sucesso!
```

### 4. Rodar em segundo plano (após o registro)

Depois que o registro for concluído, pressione **Ctrl+C** para parar e rodar o agente em background:

```bash
docker compose up -d
```

Para ver os logs a qualquer momento:

```bash
docker compose logs -f
```

---

## Manter o agente ativo após reinicialização (opcional)

Se usar o binário direto, você pode configurar o agente para iniciar automaticamente com o sistema via **supervisor**:

```bash
# Instala o supervisor (se ainda não tiver)
sudo apt-get install -y supervisor

# Cria /etc/supervisor/conf.d/arch-agent.conf e ativa o serviço
arch-agent install
```

O comando `install` cria a configuração do supervisor e recarrega o serviço automaticamente.

---

## Atualizar o agente

Para baixar e aplicar a versão mais recente do agente:

```bash
arch-agent update
```

O agente também se atualiza automaticamente quando o servidor distribui um novo binário.

---

## Outros comandos úteis

```bash
# Limpar o registro local (permite re-registrar o agente)
arch-agent clear-registration
```

---

## Verificar se o agente está funcionando

Após a instalação, entre em contato com a equipe da ArchOLT para confirmar que o agente apareceu como conectado no painel.

---

## Problemas comuns

**O registro não confirma após clicar no link**
Aguarde alguns segundos e verifique se o servidor tem acesso à internet.

**Mensagem: "dependências não encontradas no PATH"**
Execute o comando de instalação de dependências do início deste guia e tente novamente.

**O agente para após reiniciar o servidor**
Use a opção Docker Compose com `restart: unless-stopped`, ou configure o systemd conforme indicado acima.

---

Em caso de dúvidas, entre em contato com o suporte da ArchOLT.
