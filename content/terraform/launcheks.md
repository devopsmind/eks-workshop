---
title: "Launch EKS Cluster"
date: 2018-08-07T13:03:16-07:00
weight: 30
draft: false
---

Começamos inicializando o estado do Terraform:
```
terraform init
```

Agora podemos executar o plan do  nosso deployment:
```
terraform plan -var 'cluster-name=eksworkshop-tf' -var 'desired-capacity=3' -out eksworkshop-tf
```

E se quisermos aplicar esse plan:
```
terraform apply "eksworkshop-tf"
```
{{% notice info %}}
A aplicação do novo plan do terraform levará aproximadamente 15 minutos
{{% /notice %}}
