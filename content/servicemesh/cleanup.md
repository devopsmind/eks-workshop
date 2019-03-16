---
title: "Limpeza"
date: 2018-11-13T23:59:44+09:00
weight: 70
draft: false
---

Para limpar, siga os passos abaixo.

Para remover o processo de configuração de telemetria / port-forward de porta

```
kubectl delete -f istio-telemetry.yaml
```

Para remover os serviços virtuais / regras de destino do aplicativo

```
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml

kubectl delete -f samples/bookinfo/networking/destination-rule-all.yaml
```

Para remover o gateway / aplicativo

```
kubectl delete -f samples/bookinfo/networking/bookinfo-gateway.yaml

kubectl delete -f samples/bookinfo/platform/kube/bookinfo.yaml
```

Para remover o Istio

```
kubectl delete -f istio.yaml

kubectl delete -f install/kubernetes/helm/istio/templates/crds.yaml

kubectl delete namespace istio-system
```

