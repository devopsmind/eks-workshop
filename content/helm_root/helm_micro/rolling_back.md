---
title: "Retrocedendo"
date: 2018-08-07T08:30:11-07:00
weight: 50
---

Erros acontecerão durante a deploy e, quando isso acontecer, o Helm facilitará o desfazer ou "roll back" para a versão implementada anteriormente.

#### Atualize o chart do aplicativo de demonstração com uma alteração de quebra

Abrir **values.yaml** e modificar o nome da imagem em `nodejs.image` para **brentley/ecsdemo-nodejs-non-existing**. Esta imagem não existe, então isso vai quebrar a nosso deploy.

Faça o Deploy do aplicativo de demonstração atualizado chart:
```sh
helm upgrade workshop ~/environment/eksdemo
```

A atualização contínua começará criando um novo pod de nodejs com a nova imagem. O novo Pod `ecsdemo-nodejs` deve deixar de puxar a imagem não existente. Execute o comando `helm status` para ver o erro` ImagePullBackOff`:

```
helm status workshop
```

#### Reverter a atualização com falha

Agora, vamos reverter o aplicativo para a revisão de versão  anterior.

Primeiro, liste as revisões da versão do Helm:

```
helm history workshop
```

Em seguida, reverter para a revisão de aplicativo anterior (pode reverter para qualquer revisão também):

```sh
# rollback to the 1st revision
helm rollback workshop 1
```

Valide o status de release do `workshop` com:

```
helm status workshop
```
