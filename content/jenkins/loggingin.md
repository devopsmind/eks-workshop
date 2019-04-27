---
title: "Fazendo login"
date: 2018-08-07T08:30:11-07:00
weight: 30
draft: true
---

Agora que temos o endereço ELB de sua instância `jenkins`, podemos ir para navegar para esse endereço em outra janela.

![Login do Jenkins](/images/jenkins-login.png)

A partir daqui, podemos fazer login usando:

| Username | Password             |
|----------|----------------------|
| admin    | `command from below` |


```
printf $(kubectl get secret --namespace default cicd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

A saída deste comando lhe dará a senha padrão para o seu usuário `admin`. Faça o login na tela de login do `jenkins` usando estas credenciais.
