---
title: "Cleanup"
date: 2018-08-07T13:15:13-07:00
weight: 60
draft: true
---
Para excluir os recursos criados para esse cluster EKS, execute os seguintes comandos:

Veja o plan:
```
terraform plan -destroy -out eksworkshop-destroy-tf
```

Execute o plan:
```
terraform apply "eksworkshop-destroy-tf"
```

{{% notice info %}}
Destruir todos os recursos levará aproximadamente 15 minutos
{{% /notice %}}
