# Bash e Comandos Linux

O Bash (Bourne Again SHell) é o interpretador de comandos mais utilizado em sistemas Linux.  
Ele funciona como uma interface entre o usuário e o sistema operacional, permitindo executar comandos, automatizar tarefas, manipular arquivos, instalar programas e controlar processos.

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

```bash
pwd
```

#### 2) Lista arquivos e pastas.

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

**Mover:**
```bash
mv arquivo.txt Documentos/
```

#### 6) Remove arquivos ou diretórios.
**Remover arquivo:**
```bash
rm arquivo.txt
```

**Remover pasta:**
```bash
rm -r pasta
```

**ATENÇÃO:** arquivos removidos pelo terminal normalmente não vão para a lixeira.

### Busca e localização
#### 1) Busca arquivos e diretórios.
```bash
find . -name "*.py"
```

#### 2) Busca palavras dentro de arquivos.
```bash
grep "main" arquivo.py
```

#### 3) Ignorar maiúsculas/minúsculas:
```bash
grep -i "erro" log.txt
```

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