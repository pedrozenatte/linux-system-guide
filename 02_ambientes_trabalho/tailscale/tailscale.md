# Tailscale

## O que é?
O Tailscale é uma ferramenta de rede privada virtual (VPN) baseada no protocolo WireGuard que permite conectar dispositivos de forma segura e simples, como se todos estivessem na mesma rede local, mesmo estando em lugares diferentes da internet.

Na prática, ele cria uma rede privada virtual entre seus dispositivos:
- computador;
- notebook;
- servidor;
- Raspberry Pi;
- Jetson;
- celular;
- máquinas na nuvem.

Assim, é possível acessar remotamente máquinas usando SSH, compartilhar serviços e trocar arquivos de forma segura sem precisar configurar roteador, abrir portas ou mexer com NAT manualmente.

### Como ele funciona
Quando um dispositivo entra na rede Tailscale:

- ele instala o cliente Tailscale;
- autentica com uma conta (Google, GitHub, Microsoft etc.);
- recebe um IP virtual privado da rede Tailscale.

O Tailscale utiliza:
- WireGuard para criptografia e tunelamento;
- NAT traversal para atravessar roteadores automaticamente;
- coordenação via servidores da própria Tailscale.

A comunicação entre as máquinas normalmente é:
**peer-to-peer**
ou seja, direta entre os dispositivos.

## Guia para instalação e autenticação de máquinas no Tailscale

#### Documentação: 
https://tailscale.com/docs

**Linux:**
https://tailscale.com/docs/install/linux
**20.04 LTS:**
https://tailscale.com/docs/install/ubuntu/ubuntu-2004

## Instalação:
**Modo rápido:**
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

**Modo específico:**
```bash
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null

curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.tailscale-keyring.list | sudo tee /etc/apt/sources.list.d/tailscale.list

sudo apt-get update

sudo apt-get install tailscale
```

**Autenticação da própria máquina no navegador:**
```bash
sudo tailscale up
```

---

**Colocar o computador dentro de uma rede Tailscale:**
```bash
sudo tailscale up --authkey SUA_CHAVE
```

**Verificar as máquinas remotas:**
```bash
tailscale status
```

**Acessar a máquina:**
```bash
# Via SSH
ssh usuario@100.x.x.x

# Usando chave SSH
ssh -i chave.pem usuario@100.x.x.x
```

**Desconectar temporariamente:**
```bash
sudo tailscale down
```

**Para voltar:**
```bash
sudo tailscale up
```
*OBS:* Quando estamos com a chave cadastrada, é entendido que ao fazer `tailscale up` será conectado na rede da chave, e não na própria rede. 
Para esquecer a rede temos:

**Fazer logout da tailnet:**
```bash
sudo tailscale logout
```

---

**Verificar se alguém pode acessar o meu computador via SSH:**
```bash
sudo systemctl status ssh
```


## Exemplo de conexão entre duas máquinas

### Requisitos
Em ambas as máquinas:
- Tailscale instalado.

Na máquina que será acessada remotamente:
- `openssh-server` instalado;
- serviço SSH ativo.

### Instalando o SSH Server

#### Verificar se já está instalado
```bash
dpkg -l | grep openssh-server
```
ou:
```bash
systemctl status ssh
```
Caso não tiver instalado:

#### Instalar
```bash
sudo apt update
sudo apt install openssh-server
```

### Ativando o SSH

#### Iniciar o serviço agora
```bash
sudo systemctl start ssh
```

#### Iniciar automaticamente ao ligar o computador
```bash
sudo systemctl enable ssh
```

#### Fazer os dois ao mesmo tempo
```bash
sudo systemctl enable --now ssh
```

### Verificando se o SSH está ativo
```bash
systemctl status ssh
```
ou:
```bash
sudo systemctl is-active ssh
```
Se retornar:
```text
active
```
o SSH está funcionando.

### Desativando o SSH

#### Parar temporariamente
```bash
sudo systemctl stop ssh
```

#### Impedir inicialização automática
```bash
sudo systemctl disable ssh
```

### Método 1 — Mesma conta Tailscale

#### Objetivo
As duas máquinas entram na mesma Tailnet utilizando a mesma conta Tailscale.

#### Em cada computador
Autenticar no Tailscale:
```bash
sudo tailscale up
```
Será aberta uma autenticação no navegador.
Faça login com a mesma conta nos dois dispositivos.

#### Verificar máquinas conectadas
```bash
tailscale status
```
Exemplo:
```text
100.82.185.18    notebook
100.91.239.94    desktop
```

#### Descobrir IP da máquina remota
```bash
tailscale ip
```

#### Conectar via SSH
No computador cliente:
```bash
ssh usuario@100.x.x.x
```

### Método 2 — Utilizando Auth Key

#### Objetivo
Adicionar uma máquina na Tailnet sem realizar login manual pelo navegador.

Muito utilizado para:
- servidores;
- Jetsons;
- containers Docker;
- pipelines CI/CD;
- laboratórios;
- máquinas headless.

#### Gerando a Auth Key
No painel do Tailscale:
```text
Tailscale Admin Console
```
Ir em:
```text
Settings → Keys
```
e gerar uma chave de autenticação.

#### Na máquina remota
Autenticar utilizando a chave:
```bash
sudo tailscale up --authkey SUA_CHAVE
```
A máquina será adicionada automaticamente na Tailnet associada àquela chave.

#### Verificar conexão
```bash
tailscale status
```

#### Descobrir IP
```bash
tailscale ip
```

#### Conectar via SSH
```bash
ssh usuario@100.x.x.x
```