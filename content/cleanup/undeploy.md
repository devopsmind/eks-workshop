---
title: "Undeploy the applications"
date: 2018-08-07T13:37:53-07:00
weight: 20
---

Para excluir os recursos criados pelos aplicativos, devemos excluir as implantações do aplicativo e o dashboard do kubernetes:

remover Implantação das aplicações:
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

kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml

```
