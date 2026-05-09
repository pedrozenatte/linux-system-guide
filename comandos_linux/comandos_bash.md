# Bash e Comandos Linux

O Bash (Bourne Again SHell) é o interpretador de comandos mais utilizado em sistemas Linux.  
Ele funciona como uma interface entre o usuário e o sistema operacional, permitindo executar comandos, automatizar tarefas, manipular arquivos, instalar programas e controlar processos.
Criando um terminal: 
https://www.youtube.com/watch?v=tiDVxNCxa1g

No Linux, praticamente tudo pode ser feito pelo terminal.

---

## Estrutura básica de um comando

A maioria dos comandos segue a estrutura:

```bash
comando [opções] [argumentos]
```
---

### Navegação entre diretórios
#### 1) Mostra o diretório atual.
PWD - Print working Directory
```bash
pwd
```

#### 2) Lista arquivos e pastas.
LS - List
```bash
ls
```

#### 3) Listagem detalhada:

```bash
ls -l
```

#### 4) Mostrar arquivos ocultos:

```bash
ls -a
```

#### 5) Muda de diretório.

**Entrar em uma pasta:**
CD - Change Directory
```bash
cd Documentos
```

**Voltar uma pasta:**
```bash
cd ..
```

**Ir para a home:**
```bash
cd ~
```

### Manipulação de arquivos e pastas
#### 1) Cria diretórios.
```bash
mkdir minha_pasta
```

#### 2) Criar múltiplas pastas:
```bash
mkdir pasta1 pasta2 pasta3
```

#### 3) touch
Cria arquivos vazios.
```bash
touch arquivo.extensão
```

#### 4) Copia arquivos ou pastas.
**Copiar arquivo:**
```bash
cp origem.txt copia.txt
```

**Copiar diretório:**
```bash
cp -r pasta_origem pasta_destino
```

#### 5) Move ou renomeia arquivos.
**Renomear:**
```bash
mv antigo.txt novo.txt
```
Cuidado com esse comando, pois se renomearmos por um nome que já existe, vamos subescrevê-lo, assim o arquivo de nome original vai desaparecer e ficar apenas o que renomeamos. 

**Mover:**
```bash
mv arquivo.txt Documentos/
```

#### 6) Remove arquivos ou diretórios.
**Remover arquivo:**
RM - Remove
```bash
rm arquivo.txt
```
OBS: Não existe lixeira.

Se usarmos:
```bash
rm -i arquivos_id_*
```
Ao invés de apagar todos os arquivos que começam com arquivos_id_alguma coisa, a flag `-i` indica o iterativo, então o terminal ficará perguntando se quer remover os arquivos específicos. 

**Remover pasta:**
```bash
rm -r pasta
```

**ATENÇÃO:** arquivos removidos pelo terminal normalmente não vão para a lixeira.

### Pesquisando em arquivos
Ver o conteúdo do arquivo:
```bash
cat file.txt
```

Busca arquivos e diretórios:
```bash
find . -name "*.py"
```

Busca palavras dentro de arquivos:
 
```bash
grep "main" arquivo.py
```
OBS: Grep é um programa para pesquisar um padrão.

Ignorar maiúsculas/minúsculas:
```bash
grep -i "erro" log.txt
```

Subscrever em um arquivo: 
```bash
echo hello > file.txt
```
OBS: Isso vai subscrever tudo que estiver no arquivo file.txt

Escrever ao final:
```bash
echo alguma coisa >> file.txt
```
OBS: Se o arquivo não existir, ele vai criar. 

MINUTO 32

### Permissões
No Linux, arquivos possuem permissões de leitura, escrita e execução.

#### 1) Altera permissões.
**Dar permissão de execução:**
```bash
chmod +x script.sh
```

### Histórico de comandos
#### Ver histórico
```bash
history
```

### Arquivos ocultos
Para criar um arquivo oculto, usamos:
```bash
# Um ponto antes do arquivo
touch .file.txt
```

Para listar esses arquivos:
```bash
ls -a
```



### Aleatórios
#### Montar um pendrive sem dar trigger na regra: 
```bash
sudo systemctl start 
```

## Referência
https://www.youtube.com/watch?v=Sx9zG7wa4FA