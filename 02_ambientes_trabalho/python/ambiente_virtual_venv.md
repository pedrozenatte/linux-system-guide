# Ambiente Virtual Python (venv)

Um ambiente virtual é um ambiente isolado de Python. Isso nos permite que cada projeto tenha: 
- Suas próprias bibliotecas
- Sua própria versão de pacotes

E isso não vai interferir no sistema ou em outros projetos. 

## O que é o `venv`? 
O `venv` é o módulo oficial do Python para criar ambientes virtuais.

Ele cria uma pasta contendo:

- um Python isolado
- um pip isolado
- bibliotecas isoladas

Normalmente, o Python já vem com o venv instalado, mas no caso do Python que vem com o sistema Linux, nem sempre vem instalado. 

#### Instalando o venv no Python do sistema
```bash
sudo apt update
sudo apt install python3-venv
```

**Para versões específicas:**
```bash
sudo apt install python3.8-venv
```

**OBS:** Para o Python istalado via pyenv, o `venv` normalmente já vem instalado automaticamente. 

## Como criar um ambiente virtual
1) Primeiro, escolhe-se uma versão do python, por exemplo:
```bash
pyenv local 3.11.4
```

2) Criar o ambiente
```bash
python3 -m venv venv
```
Esse comando vai usar o Python atual, executar o módulo `venv`, o qual também vai criar um ambiente virtual na pasta chamada `venv`.

**OBS1:** Essa pasta venv é criada no repositório onde foi executado o comando, por isso executar na pasta do projeto. 
**OBS2:** O segundo `venv` do comando é o nome da pasta, mas por convenção é deixado venv mesmo.

3) Ativar o ambiente virtual
```bash
source venv/bin/activate
```

Quando ativamos o ambiente virtual, o terminal passa a usar o Python e pip do ambiente virtual, ou seja, tudo que instalarmos vai ficar restrito na pasta `venv` criada. 

4) Desativar o ambiente virtual
```bash
deactivate
```

**OBS:** Para apagar um ambiente virtual é só apagar a pasta **venv**. 


## Utitlizando o ambiente virtual e repassando para o git
Não é comum passar a pasta do ambiente virtual para o git (pasta venv), pois ao mudar de sistema operacional pode dar problema. Nesse caso, o que fazemos é criar o arquivo `.gitignore` e acrescentar o nome da pasta do ambiente virtual `venv/`. 
Dessa forma, utilziamos um arquivo requirements.txt (não precisa ser esse nome necessariamente, mas por padronização, usa-se esse) para especificar as dependências do projeto. Esse arquivo é criado com:
```bash
pip freeze > requirements.txt
```

Após isso, uma outra máquina pega esse arquivo, cria o seu próprio ambiente virtual, ativa o ambiente e roda o comando:
```bash
pip install -r requirements.txt # Ou o nome do arquivo que contém o que deve ser instalado
```