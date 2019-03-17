---
title: "Faça deploy de nossos aplicativos de exemplo"
date: 2018-09-18T16:01:14-05:00
weight: 5
---

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ecsdemo-nodejs
  labels:
    app: ecsdemo-nodejs
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ecsdemo-nodejs
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: ecsdemo-nodejs
    spec:
      containers:
      - image: brentley/ecsdemo-nodejs:latest
        imagePullPolicy: Always
        name: ecsdemo-nodejs
        ports:
        - containerPort: 3000
          protocol: TCP
```
No arquivo de exemplo acima, descrevemos o serviço e  *como* deve ser implantado.
Vamos escrever esta descrição para a API do kubernetes usando o kubectl, e o kubernetes irá garantir que nossas preferências sejam atendidas à medida que o aplicativo for implementado.

Os contêineres escutam na porta 3000 e a descoberta de serviço nativo será usada para localizar os contêineres em execução e se comunicar com eles..
