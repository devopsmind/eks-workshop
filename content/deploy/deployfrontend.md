---
title: "Implantar o serviço de front-end"
date: 2018-09-18T17:40:09-05:00
weight: 30
---

Vamos implantar o Frontend Ruby !

Copie/cole os seguintes comandos no seu workspace Cloud9 :

```
cd ~/environment/ecsdemo-frontend
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
```

Podemos acompanhar o progresso observando o status da implantação:
```
kubectl get deployment ecsdemo-frontend
```
