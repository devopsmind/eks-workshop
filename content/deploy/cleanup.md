---
title: "Limpar os aplicativos"
date: 2018-08-07T13:37:53-07:00
weight: 90
---

Para excluir os recursos criados pelos aplicativos, devemos excluir os deployments de aplicativos:

Remover implantação dos aplicativos:
```
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
