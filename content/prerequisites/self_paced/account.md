---
title: "Crie uma conta da AWS"
chapter: false
weight: 1
---

{{% notice warning %}}
Sua conta deve ter a capacidade de criar novas roles do IAM e escopo de outras permissões do IAM.
{{% /notice %}}

1. Se você ainda não tem uma conta da AWS com acesso de administrador: [crie uma agora clicando aqui](https://aws.amazon.com/getting-started/)

1. Depois de ter uma conta da AWS, verifique se você está seguindo as etapas restantes da oficina como um usuário do IAM com acesso de administrador à conta da AWS:
[Criar um novo usuário do IAM para usar no workshop](https://console.aws.amazon.com/iam/home?#/users$new)

1. Digite os detalhes do usuário:
![Criar usuário](/images/iam-1-create-user.png)

1. Anexe a Política do IAM AdministratorAccess:
![Anexar Política](/images/iam-2-attach-policy.png)

1. Clique para criar o novo usuário:
![Confirme o usuário](/images/iam-3-create-user.png)

1. Anote o URL de login e salve:
![Login URL](/images/iam-4-save-url.png)
