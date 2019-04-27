---
title: "Assegure-se de que a função de serviço do ELB exista"
date: 2018-09-18T17:40:09-05:00
weight: 29
---

Nas contas da AWS que nunca criaram um balanceador de carga antes, é possível que a função de serviço para o ELB ainda não exista.

Podemos verificar o papel e criá-lo se estiver faltando.

Copie/cole os seguintes comandos no seu workspace Cloud9 :

```
aws iam get-role --role-name "AWSServiceRoleForElasticLoadBalancing" || aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"
```
