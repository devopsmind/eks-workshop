---
title: "Implantar a API de back-end do NodeJS"
date: 2018-09-18T17:39:30-05:00
weight: 10
---

Vamos implantar a API do Backend do NodeJS!

Copie/cole os seguintes comandos no seu workspace Cloud9 :

```
cd ~/environment/ecsdemo-nodejs
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
```

Podemos acompanhar o progresso observando o status de deployment:
```
kubectl get deployment ecsdemo-nodejs
```
