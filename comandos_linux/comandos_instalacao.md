# Comandos de instalação

## Atualização
#### 1) Atualizar lista de pacotes
```bash
sudo apt update
```

#### 2) Atualizar programas instalados
```bash
sudo apt upgrade
```

## Instalação de programas
#### 1) Instalar um programa via repositório ofocial do Ubuntu
```bash
sudo apt install git
```

#### 2) Instalar um programa via arquivo .deb
```bash
sudo apt install ./nome_arquivo.deb
```

OBS: Nesse caso, o arquivo precisa estar na pasta pessoal, se não é necessário cd pasta_do_arquivo. 

## Remover programas
```bash
sudo apt remove git
```