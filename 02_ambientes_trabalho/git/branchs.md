# Branchs 


## Comandos

#### Verificar o status de um repositório
Aqui, a gente verifica o que está acontecendo com o repositório, por exemplo, o que tem de diferente. 
```bash
git status
```


#### Vamos ver o que foi modificado em um arquivo
O que tem de diferença entre o estado atual com relação ao commit anterior é o comando:   
```bash
git diff nome_arquivo
```
Esse nome_arquivo, aparece quando existe um arquivo modificado ao digitar o `git status`.


#### Retirar um arquivo/pasta do staged de envio (add)
```bash
git reset nome_arquivo/pasta
```


#### Verificar as branchs existentes
```bash
git branch
```


#### Criar uma branch e mudar para ela 
Pega o estado da branch que estamos (a main por exemplo), criando uma cópia dela e mudando o ramo: 
```bash
git checkout -b nome_branch
```
Essa branch nova carrega os commits anteriores. 
**OBS:** O `-b` é para criar a branch caso ela não exista. 


#### Voltar para alguma outra branch
```
git checkout nome_branch
```


#### Trazer as informações de uma branch secundária para uma outra, por exemplo a main (merge)
```bash
git merge nome_branch 
```
Estando na branch que queremos atualizar, o comando acima traz as informações da branch secundária para a que estamos. 


#### Deletar uma branch
```bash
git branch -D nome_branch
```
**OBS:** Não pode estar na branch para deletá-la.



## Conflitos 
Supondo que duas pessoas mexeram no mesmo arquivo, na mesma linha, e ao realizar o merge, houve o conlfito. 
Lembrando: 
- <<<<<< HEAD (commit atual)
- \>>>>>> está chegando

Ao olhar o arquivo, teremos praticamente a mesma linha, mas em duas linhas separadas, cada uma com a forma modificada. Para resolver, precisa analisar manualmente e escolher as mudanças que mais se adequam ao que deseja. 
Ou seja, para resolver esse tipo de conflito, é necessário abrir o arquivo e escolher quais alterações vão ficar e quais vão ser descartadas. 