# Secure Shell (SSH)

## O que é? 
O Secure Shell (SSH) é um protocolo de comunicação de rede utilizado para realizar conexões remotas de forma segura entre computadores. Ele permite enviar comandos, acessar terminais e transferir arquivos através de uma rede, mesmo que essa rede não seja confiável, utilizando mecanismos de criptografia para proteger os dados transmitidos.
A ideia geral do SSH, é ser possível acessar remotamente outro computador como se estivéssemos utilizando o terminal da própria máquina. Isso é muito utilizado em servidores Linux, dispositivos embarcados, robôs, drones, máquinas na nuvem e sistemas de desenvolvimento.

Além do acesso remoto ao terminal, o SSH também permite:
- Configuração e gerenciamento de sistemas remotamente;
- Instalação e atualização de programas e pacotes;
- Administração de servidores;
- Execução de scripts e aplicações;
- Transferência segura de arquivos;
- Autenticação segura em serviços como o GitHub.

No caso do GitHub, por exemplo, o SSH é frequentemente utilizado para autenticar o usuário sem precisar digitar login e senha a cada operação. Isso é feito através de um sistema de chaves criptográficas (chave pública e chave privada), tornando operações como git clone, git pull e git push mais seguras e práticas.

## Criando a conexão
Para se conectar a um servidor remotamente utilizando SSH pelo terminal, utilizamos o comando:
```bash
ssh usuario@host
```
OBS: O host pode ser um IP ou um hostname. 
Na primeira conexão com um servidor, o SSH normalmente perguntará se você deseja confiar naquele host. Isso acontece porque o computador ainda não possui a identidade criptográfica do servidor salva localmente.

#### 1) Autenticação por senha
Se não tivermos uma chave de autenticação configurada, será preciso digitar a senha do usuário:
```bash
password:
```
Porém isso **não é uma prática segura**, já que permite o acesso de forma muito fácil, pois senhas podem ser descobertas por ataques de força bruta ou vazamentos.
Por isso, normalmente utiliza-se autenticação por chaves SSH.

#### 2) Chave de autenticação SSH
A chave de autenticação é criptografada (criptografia assimétrica), então não há o risco de alguém interceptar a informação pelo caminho, e é baseada em duas chaves:
- Chave privada: fica armazenada somente no seu computador e nunca deve ser compartilhada;
- Chave pública: pode ser enviada para servidores e serviços como GitHub.

Quando tentamos nos conectar, o servidor verifica se a chave privada correspondente à chave pública cadastrada está presente na máquina cliente, permitindo uma autenticação segura sem transmitir senhas pela rede

Para criar um par de chaves SSH, utilizamos:
```bash
ssh-keygen -t rsa -b 4096 -C "meu@email.com"
```
**Parâmetros:**
- t --> Tipo de chave (nesse caso é do tipo rsa - algoritmo utilizado para criação da chave)
- b --> Tamanho da chave (nesse caso é 4096 bits)
- c --> Apenas um comentário de identificação da chave (normalmente utiliza o e-mail).

---

Outro tipo de chave muito utilizado é:
`ed25519`
Esse modelo já vem por padrão com um tamanho certo de bits (256), e para o github é o mais indicado atualmente, mas não significa que não possa utilizar outros tipos de chave. 

---

Após executar o comando, o terminal fará algumas perguntas automaticamente.
**1 - Escolha do local da chave.**
Se apenas pressionarmos Enter, o SSH utilizará o caminho padrão.
```bash
# Caminho padrão, no meu caso:
# /home/pedro/.ssh

cd ~/.ssh/
```

**2 - Depois disso, será perguntado se desejamos proteger a chave com uma senha adicional.**
Essa senha é opcional, mas aumenta bastante a segurança caso alguém tenha acesso ao arquivo da chave privada.
Se não quiser definir uma senha, basta pressionar Enter duas vezes.

#### Estrutura típica da pasta .ssh
Estando tudo OK, será gerado dois arquivos, sendo um sendo a chave privada e um com a extensão .pub, o qual contém a chave pública.
```bash
~/.ssh/
├── id_rsa
├── id_rsa.pub
├── known_hosts
└── config
```
- known_hosts: guarda os servidores já conhecidos/confiáveis;
- config: permite criar atalhos e configurações personalizadas para conexões SSH.

---

### Arquivo Config
O arquivo config do SSH serve para criar configurações personalizadas e atalhos para conexões SSH.


**Exemplo:**
No meu caso, como uso dois githubs, preciso configurar o host como um Alias: 
```text
# Conta pessoal
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/git_pessoal
  IdentitiesOnly yes

# Conta Empresa
Host github-empresa
  HostName github.com
  User git
  IdentityFile ~/.ssh/git_empresa # Chave privada
  IdentitiesOnly yes
```

Dessa forma, se eu for usar o link do SSH quando quero acessar um repositório do github da conta segundária, precisa trocar a forma que vier de:
```bash
git@github.com:.....git 
```
Para
```bash
git@github-empresa:......git
```

#### Estrutura 
**Para o git:**
```text
Host apelido
  HostName github.com
  User git
  IdentityFile ~/.ssh/git_empresa
```

**Mais geral:**
```text
Host apelido
    HostName endereco_do_servidor
    User usuario
    Port porta
    IdentityFile caminho_da_chave
```

OBS: Se não especificar a porta, será utilizada a porta padrão 22. 

---

### Exemplo
Vamos criar uma chave de teste:
```bash
ssh-keygen -t ed25519 -C "Teste"
```

Após executar o comando, aparecerá algo parecido com:
```bash
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/pedro/.ssh/id_ed25519): /home/pedro/.ssh/minha_chave_teste
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/pedro/.ssh/minha_chave_teste
Your public key has been saved in /home/pedro/.ssh/minha_chave_teste.pub
The key fingerprint is:
SHA256:wtfXEqeAelgmNsxOU6zAXmAfLKC5SxP3rBZF6Pa0LqU Teste
The key's randomart image is:
+--[ED25519 256]--+
|  .o++...        |
| o o=+oo..       |
|o..o =X.+ . . .  |
| .oo=*.O . . =   |
|.o..ooB S . + .  |
|... o+ +   . .   |
|.  o+            |
|  .E .           |
|    .            |
+----[SHA256]-----+
```

Perceba esta parte:
```bash
Enter file in which to save the key (/home/pedro/.ssh/id_ed25519): /home/pedro/.ssh/minha_chave_teste
```
Aqui estamos definindo manualmente:
- o local onde a chave será salva;
- e o nome da chave.

Se apenas pressionássemos `Enter`, a chave seria criada automaticamente na pasta:

```bash
~/.ssh
```

com o nome padrão:

```bash
id_ed25519
```

Como informamos:

```bash
/home/pedro/.ssh/minha_chave_teste
```

a chave foi criada com o nome:

```bash
minha_chave_teste
```

dentro da pasta `~/.ssh`. 


#### Observação importante
Se tivéssemos digitado apenas:

```bash
minha_chave_teste
```

sem informar um caminho completo, os arquivos seriam criados na pasta atual do terminal (normalmente a pasta pessoal `~`).
Embora seja possível armazenar chaves SSH em qualquer diretório, o ideal é mantê-las dentro da pasta:
```bash
~/.ssh
```
pois este é o local padrão utilizado pelo SSH.

Nada impede o uso de chaves em outras pastas, desde que elas sejam informadas manualmente no arquivo de configuração SSH (`~/.ssh/config`) utilizando:

```text
IdentityFile
```

**Exemplo:**
```text
Host meu_servidor
    HostName 192.168.0.10
    User pedro
    IdentityFile ~/minhas_chaves/minha_chave_teste
```


#### Verificando permissões da chave
Sempre que uma chave for criada fora da pasta `~/.ssh`, é importante verificar se as permissões continuam corretas.

Para visualizar as permissões:
```bash
ls -l nome_da_chave
```

Exemplo:
```bash
pedro@pedro-Aspire-A515-54G:~/.ssh$ ls -l minha_chave_teste
-rw------- 1 pedro pedro 387 mai  8 12:16 minha_chave_teste

pedro@pedro-Aspire-A515-54G:~/.ssh$ ls -l minha_chave_teste.pub
-rw-r--r-- 1 pedro pedro 87 mai  8 12:16 minha_chave_teste.pub
```

Perceba que:
- a chave privada possui permissão apenas para o dono;
- a chave pública pode ser lida pelos demais usuários.

Isso é exatamente o esperado.

Caso as permissões não tiverem ideais, torna-se necessário mudá-las.
```bash
chmod 600 minha_chave_teste
chmode 644 minha_chave_teste.pub
```

---

#### Permissões no Linux
No Linux, os arquivos possuem permissões divididas em 3 grupos:

| Grupo | Significado |
|---|---|
| owner | dono do arquivo |
| group | grupo do usuário |
| others | outros usuários |

E cada grupo pode possuir:

| Permissão | Valor |
|---|---|
| leitura (r) | 4 |
| escrita (w) | 2 |
| execução (x) | 1 |

Os números utilizados no comando `chmod` representam a soma dessas permissões.

##### Exemplo: 600

```bash
chmod 600 ~/.ssh/minha_chave_teste
```
O `600` significa:

```text
6 0 0
dono   → 6 = 4 + 2 = rw-
grupo  → 0 = ---
outros → 0 = ---
```

Resultado:

```text
rw-------
```

Ou seja:

- somente o dono pode ler e modificar a chave privada;
- ninguém mais pode acessá-la.

##### Exemplo: 644

```bash
chmod 644 ~/.ssh/minha_chave_teste.pub
```
O `644` significa:

```text
6 4 4
dono   → rw-
grupo  → r--
outros → r--
```

Resultado:

```text
rw-r--r--
```

Ou seja:

- o dono pode ler e editar;
- os demais apenas podem ler.

Isso é aceitável para a chave pública, pois ela não é secreta.

---

### Referências
https://www.youtube.com/watch?v=e4Yt-UlsJG4