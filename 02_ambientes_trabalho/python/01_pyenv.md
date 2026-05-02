# Pyenv 

**O que é o Pyenv?**
É uma ferramenta para gerenciar múltiplas versões do Python apenas em sistemas Unix-like, como no Linux e no macOS.
Com o Pyenv podemos escolher a versão que será usada no sistema inteiro, em um projeto específico e apenas no terminal atual. 
Dessa forma, evitamos até conflitos entre os projetos. 

## Como o Pyenv funciona internamente
O pyenv não substitui o Python do sistema. Na realidade ele funciona colocando um diretório chamado `shims` no começo do `PATH`. Dessa forma, quando rodamos:
```bash
python3
```
O Linux vai procurar os executáveis seguindo a ordem que está no `PATH`.
**OBS:** Se quiser ver a ordem do `PATH`, utilize o comando `echo $PATH`
No meu caso, temos: 
```bash
/home/pedro/.pyenv/shims:/home/pedro/.pyenv/bin:/opt/ros/noetic/bin:/home/pedro/.nvm/versions/node/v24.15.0/bin:/home/pedro/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Perceba que, como `~/.pyenv/shims` vem primeiro, o comando `python3` cai em um atalho do pyenv que vai redirecionar para a versão correta. 

## Instalação do Pyenv no Linux
Para a instalação, podemos analisar as recomendações do repositório oficial do pyenv: https://github.com/pyenv/pyenv

### 1 - Instalar as dependências de compilação do Python
```bash
sudo apt update

sudo apt install -y \
build-essential \
curl \
git \
libssl-dev \
zlib1g-dev \
libbz2-dev \
libreadline-dev \
libsqlite3-dev \
wget \
llvm \
libncurses5-dev \
libncursesw5-dev \
xz-utils \
tk-dev \
libffi-dev \
liblzma-dev \
python3-openssl
```

### 2 - Instalar o pyenv
```bash
curl -fsSL https://pyenv.run | bash
```

### 3 - Configurar o shell
Rode o comando:
```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc
```

**Recarregue o shell** executando o comando:
```bash
source ~/.bashrc
```

Ou **fechando e abrindo o terminal**. 

Dessa forma, ao abrir o conteúdo do arquivo com:
```bash
nano ~/.bashrc
```

Ao final do arquivo teremos:
```text
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
```

### 4 - Verificar instalação
**Verificar a versão do pyevn:**
```bash
pyenv --version
```

**Verificar onde o Linux está procurando o Python:**
```bash
which python3
```

---

## Comandos importantes do pyenv
**Verificar versões do Python disponíveis para instalação:**
```bash
pyenv install --list | less

# ou

pyenv install -l | less
```

**Ver versões instaladas:**
```bash
pyenv versions
```

**Instalar uma versão das possíveis listadas:**
```bash
pyenv install 3.11.9
```

**Definir uma versão de uso:**
1) Selecionar uma versão do Python para usar no terminal
```bash
pyenv shell <version> 
```

2) Selecionar uma versão para o repositório local
```bash
pyenv local <version> 
```

3) Selecionar uma versão global do Python
```bash
pyenv global <version>
```

**OBS:** Para colocar o Python fornecido pelo sistema,  usar "system" como nome da versão.

**Desistalar uma versão do Python do pyevn:**
```bash
pyenv uninstall <versions>
```

Também é possível desistalar removendo o diretório da versão. 

**Outros comandos:**
```bash
pyevn --help
```

**Atualizar o pyenv:**
```bash
pyenv update
```

---

## Instalando um pacote no Python
Quando mudamos de versão com o pyenv, ao utilizar o comando:
```bash
pip install pacote
```
Talvez de problema na versão que você instalou o pacote, por exemplo, instalar na versão do sistema ao invés da versão que o terminal está utilizando no momento.

Dessa forma, para contornar esse problema, a melhor solução e mais segura é utilizar o comando:
```bash
python -m pip install pacote
```

Isso garante que a versão do Python que terá o pacote instalado é a que o terminal está utilizando. 