---
title: "Escale o frontend"
date: 2018-09-18T17:40:09-05:00
weight: 60
---

Vamos também dimensionar nosso serviço frontend da mesma forma:
```
kubectl get deployments
kubectl scale deployment ecsdemo-frontend --replicas=3
kubectl get deployments
```

Verifique a guia do navegador, onde podemos ver nosso aplicativo em execução. Agora você deve ver o tráfego fluindo para vários serviços frontend.
