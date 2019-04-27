---
title: "Configure Horizontal Pod AutoScaler (HPA)"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

### Implantar o Metrics Server
O Metrics Server é um agregador de dados de uso de recursos em todo o cluster. Essas métricas conduzirão o comportamento de dimensionamento do [deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/). Vamos implantar o servidor de métricas usando o `Helm` configurado anteoriormente [module](/helm_root/helm_intro/install/index.html)

```
helm install stable/metrics-server \
    --name metrics-server \
    --version 2.0.4 \
    --namespace metrics
```
### Confirme se a API de métricas está disponível.

Retornar ao terminal no ambiente Cloud9
```
kubectl get apiservice v1beta1.metrics.k8s.io -o yaml
```
Se tudo estiver bem, você deve ver uma mensagem de status semelhante à abaixo na resposta
```
status:
  conditions:
  - lastTransitionTime: 2018-10-15T15:13:13Z
    message: all checks passed
    reason: Passed
    status: "True"
    type: Available
```

#### Agora estamos prontos para escalar um aplicativo implantado
