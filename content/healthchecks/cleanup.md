---
title: "Limpar"
chapter: false
weight: 25
---
{{% notice info %}}
Nosso exemplo do Liveness Probe usou a solicitação HTTP e o Readiness Probe executou um comando para verificar a integridade de um pod. O mesmo pode ser feito usando uma solicitação TCP, conforme descrito em [documentation](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).
{{% /notice %}}
```
kubectl delete -f ~/environment/healthchecks/liveness-app.yaml
kubectl delete -f ~/environment/healthchecks/readiness-deployment.yaml
```


