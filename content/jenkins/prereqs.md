---
title: "Configurar Storage Class"
date: 2018-08-07T08:30:11-07:00
weight: 10
draft: true
---

Antes de podermos implantar nossa instância `Jenkins`, precisamos configurar um Storage Class padrão para nosso cluster. O Storage Class informam ao Kubernetes como elas devem configurar os Volumes Persistentes que serão usados ​​com o cluster.

Em nosso exemplo, vamos configurar o `gp2` ou EBS de uso geral como nosso suporte a
`pvc`'s

Primeiro, precisamos criar um novo manifesto de armazenamento:
```
cat <<EoF > ~/environment/storage-class.yaml
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: gp2
  annotations:
    "storageclass.kubernetes.io/is-default-class": "true"
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Retain
mountOptions:
  - debug
EoF
```

Por fim, aplique a configuração.

```
kubectl apply -f ~/environment/storage-class.yaml
```
