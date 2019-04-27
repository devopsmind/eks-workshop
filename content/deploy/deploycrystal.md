---
title: "Implantar a API do Crystal Backend"
date: 2018-09-18T17:40:03-05:00
weight: 20
---

Vamos trazer a API do Crystal Backend!

Copie/cole os seguintes comandos no seu workspace Cloud9 :

```
cd ~/environment/ecsdemo-crystal
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
```

Podemos acompanhar o progresso observando o status do deployment:
```
kubectl get deployment ecsdemo-crystal
```
