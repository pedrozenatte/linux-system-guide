# Guia para instalação e autenticação de máquinas no Tailscale

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