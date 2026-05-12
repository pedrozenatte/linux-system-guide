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
Verifica as permissões
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


### Arquivos de paginação
Uma foma melhor para ler aquivos no terminal
```bash
less arquivo_de_leitura
```

### Man (manual) Pages 
Gerar um manual do comando
```bash
# Exemplo
man ls
```


### Variáveis Básicas
```bash
echo $HOME
echo $USER
echo $PWD
echo $SHELL
echo $UID
```

Além disso, também podemos criar nossas próprias variáveis válidas apenas para a sessão do terminal. 
```bash
nome=pedro # Sem espaços
echo $nome
```
**OBS:** Cuidado com aspas duplas e aspas simples, pois a dupla retorna a string original:

```bash
pedro@pedro-Aspire-A515-54G:~$ nome="pedro      zenatte"
pedro@pedro-Aspire-A515-54G:~$ echo $nome
pedro zenatte
pedro@pedro-Aspire-A515-54G:~$ echo "$nome"
pedro      zenatte
```

Para desativar a variável:
```bash
unset nome_da_variavel
```

Nome do usuário e do computador:
```bash
pedro@pedro-Aspire-A515-54G:~$ whoami
pedro
pedro@pedro-Aspire-A515-54G:~$ uname
Linux
pedro@pedro-Aspire-A515-54G:~$ uname -a
Linux pedro-Aspire-A515-54G 7.0.0-15-generic #15-Ubuntu SMP PREEMPT_DYNAMIC Wed Apr 22 16:06:43 UTC 2026 x86_64 GNU/Linux
```

Atribuir um comando a uma variável (guardar a saída de um comando):
```bash
pedro@pedro-Aspire-A515-54G:~$ thing=`uname -a`
pedro@pedro-Aspire-A515-54G:~$ echo "$thing"
Linux pedro-Aspire-A515-54G 7.0.0-15-generic #15-Ubuntu SMP PREEMPT_DYNAMIC Wed Apr 22 16:06:43 UTC 2026 x86_64 GNU/Linux
```
OU
```bash
thing=$(uname -a)
echo "$thing"
```


### Permissões de arquivos
No Linux, arquivos possuem permissões de leitura, escrita e execução.

#### 1) Altera permissões.
**Dar permissão de execução:**
```bash
chmod +x script.sh
```

---

Ao fazer um script.sh, na primeira linha do script colocar shebang ou hashbang. Isso é uma linha especial usada no início de scripts em sistemas Unix que inidica ao SO qual interpretador usar para rodar o script. 

Exemplo:
```sh
#!/usr/bin/env bash

comandos
...
...
```

Dessa forma, estamos dizendo que o script deve ser executado usando o Bash, independentemente de qual versão do Bash ou do sistema esteja sendo usada.

---

MINUTO 1:22:45

### Histórico de Comandos
#### Ver histórico
```bash
history
```

### Arquivos Ocultos
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

#### Verificar onde algo está localizado no sistema:
```bash
which python3
```

#### Identificar o tipo de comando ou se é um alias:
```bash
# Exemplo
type ls
```

#### Identificar o tipo de um arquivo:
Esse comando analisa o conteúdo, dizendo se o arquivo é um texto ASCII, uma imagem, um executável, um diretório, entre outros.
```bash
file algum_arquivo/diretório
```

#### Translate (tr):
Traduzir, deletar ou cumprir caracteres de entrada padrão
```bash
echo $PATH | tr ":" "\n"
```
Nesse caso, os dois pontos virarão '\n'. 

#### Fornecer informações sobre pacotes:
```bash
sudo dpkg -l
```

---

### PATH
O \$PATH é uma variável de ambiente no Linux que define os diretórios onde o sistema procura por executáveis. Quando você digita um comando no terminal, o sistema verifica o conteúdo do $PATH na ordem dos diretórios listados para encontrar o programa correspondente e executá-lo, facilitando o uso de comandos sem precisar especificar seu caminho completo.

Sendo assim, podemos criar comandos personalizados.

---

## Referência
https://www.youtube.com/watch?v=Sx9zG7wa4FA