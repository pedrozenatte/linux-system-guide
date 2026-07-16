# Alterar Commits Git

## 1. Configurar `user.name` e `user.email`

#### Apenas no repositório atual: 
```bash
git config user.name "nome"
git config user.email "email@exemplo.com"
```

#### De forma global:
```bash
git config --global user.name "nome"
git config --global user.email "email@exemplo.com"
```

#### Verificar configuração:
```bash
git config user.name
git config user.email
```

## 2. Alterar nome e e-mail do último commit
Vamos considerar uma situação em que foi realizado um commit, mas o nome e e-mail vieram da configuração global ou apenas estavam com uma configuração local errada.

### Situação 1: Commit ainda não foi enviado para o repositório remoto
Primeiro, configure os dados corretos: 

```bash
git config user.name "Nome Correto"
git config user.email "emailcorreto@exemplo.com"
```

Depois, recrieo último commit usando o novo autor: 
```bash
git commit --amend --reset-author --no-edit
```
- `--amend`: altera o último commit.
- `--reset-author`: usa o `user.name` e `user.email` configurados atualmente.
- `--no-edit`: mantém a mesma mensagem do commit.

### Situação 2: Commit foi enviado para o repositório remoto `git push`
Faremos as mesmas alterações realizadas na situação 1, porém vamos forçar a atualização do repositório remoto:
```bash
git push --force-with-lease
```

OBS: Da para utilizar `git push --force`, porém o `--force-with-lease` verifica se outra pessoa alterou a branch antes de sobrescrevê-la.


## Alterar a mensagem do último commit
Agora, a situação é que queremos mudar a mensagem do último commit.
```
git commit --amend -m "Nova mensagem do commit"
```

**ATENÇÃO:** Lembrando que se o commit foi enviado para o repositório remoto, tem que forçar o push com `git push --force-with-lease`

--- 
### OBS: Se tivermos vários commits errados, da para fazer o seguinte: 

Ver quantos commits precisam ser alterados:
```bash
git log --oneline
```

Se os três últimos commits estão errados:
```bash
git rebase -i HEAD~3 
```
OBS: O número 3 é a quantidade de commits que serão mostrados, e isso serve para caso queiramos alterar algum commit mais antigo. 

Após isso, irá abrir um editor, então terá algo semelhante a: 
```edit
pick h7i8j9k Commit 1 (mais recente)
pick d4e5f6g Commit 2 
pick a1b2c3d Commit 3 (mais antigo)
```

Troque esse `pick` por `edit`. Salve e feche o editor. 

**ATENÇÃO:** O Git vai parar no primeiro commit marcado. Nesse caso, configure normalmente o commit com a alteração desejada e, ao final, digite:
```bash
git rebase --continue
```

Pois assim, o Git vai para o próximo commit, mas sem dizer nada, então precisa lembrar da sequência de commits vista com `git rebase -i HEAD~X` e entender que no rebase interativo `git rebase --continue`, a lista será em ordem do **commit mais antigo para o mais novo** (a ordem de alteração é a inversa vista no arquivo de edição).  

Para cancelar o rebase:
```bash
git rebase --abort
```

---