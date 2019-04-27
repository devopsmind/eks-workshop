---
title: "Delete the EKSCTL Cluster"
date: 2018-08-07T13:37:53-07:00
weight: 30
---

Para excluir os recursos criados para esse cluster EKS, execute os seguintes comandos:

Exclua o cluster:
```
eksctl delete cluster --name=eksworkshop-eksctl
```

{{% notice tip %}}
O grupo de nós terá que concluir o processo de exclusão antes que o cluster EKS possa ser excluído. O processo total levará aproximadamente 15 minutos e pode ser monitorado pelo
[Console do CloudFormation ](https://console.aws.amazon.com/cloudformation/home)
{{% /notice %}}
