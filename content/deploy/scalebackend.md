---
title: "Escale os serviços de back-end"
date: 2018-09-18T17:40:09-05:00
weight: 50
---

Quando lançamos nossos serviços, lançamos apenas um contêiner de cada um. Podemos confirmar isso visualizando os pods em execução:
```
kubectl get deployments
```

Agora vamos escalar os serviços de back-end:
```
kubectl scale deployment ecsdemo-nodejs --replicas=3
kubectl scale deployment ecsdemo-crystal --replicas=3
```
Confirme acompanhando o deployment novamente:
```
kubectl get deployments
```

Além disso, verifique a guia do navegador onde podemos ver nosso aplicativo em execução. Agora você deve ver o tráfego fluindo para vários serviços de back-end.
