---
title: "Limpar"
weight: 50
draft: false
---
Limpar nossa implantação de Microservices

```bash
cd ~/environment/ecsdemo-frontend
kubectl delete -f kubernetes/service.yaml
kubectl delete -f kubernetes/deployment.yaml

cd ~/environment/ecsdemo-crystal
kubectl delete -f kubernetes/service.yaml
kubectl delete -f kubernetes/deployment.yaml

cd ~/environment/ecsdemo-nodejs
kubectl delete -f kubernetes/service.yaml
kubectl delete -f kubernetes/deployment.yaml
```

Limpar o Daemonset Manipulador de Manchas

```bash
kubectl delete -f ~/environment/spot/spot-interrupt-handler-example.yml
```

Para limpar o  worker criado por este módulo, execute os seguintes comandos:

Remova os nodes Worker do EKS:

```bash
aws cloudformation delete-stack --stack-name "eksworkshop-spot-workers"
```
