---
title: "Limpar o Workspace"
chapter: false
weight: 50
---

Vamos deletar nossa chave SSH:
```
aws ec2 delete-key-pair --key-name "eksworkshop"
```

Como não precisamos mais da instância Cloud9 para ter acesso de administrador à nossa conta, podemos excluir a role que criamos:

  - Vamos para [Console do IAM  ](https://console.aws.amazon.com/iam/home?#/roles/eksworkshop-admin)
  - Clique **Delete a role** no canto superior direito

Por fim, vamos excluir nossa instância do Cloud9 EC2:

- Vá para sua[Cloud9 Environment](https://console.aws.amazon.com/cloud9/home)
- Selecione o ambiente chamado **eksworkshop** e escolha **delete**
