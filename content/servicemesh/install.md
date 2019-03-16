---
title: "Instale o Istio"
date: 2018-11-13T16:36:55+09:00
weight: 30
draft: false
---

### Instalar o CRD do Istio
O [Custom Resource Definition, também conhecido como CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions), é um recurso da API que permite definir recursos personalizados. 
```
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
```

### Instale o Istio
Helm é necessário para os exemplos a seguir.  Se você ainda não instalou o Helm, [por favor, primeiro referenciar o capítulo Helm](/helm_root) Antes de prosseguir.

```
kubectl create -f install/kubernetes/helm/helm-service-account.yaml

helm template install/kubernetes/helm/istio --name istio --namespace istio-system --set global.configValidation=false --set sidecarInjectorWebhook.enabled=false --set grafana.enabled=true --set servicegraph.enabled=true > istio.yaml

kubectl create namespace istio-system

kubectl apply -f istio.yaml
```

Veja o progresso de instalação usando:

```
kubectl get pod -n istio-system -w
```

E pressione CTRL-C quando estiver pronto para prosseguir.

```
NAME                                    READY     STATUS      RESTARTS   AGE
grafana-9cfc9d4c9-csvw7                 1/1       Running     0          3m
istio-citadel-6d7f9c545b-w7hjs          1/1       Running     0          3m
istio-cleanup-secrets-vrkm5             0/1       Completed   0          3m
istio-egressgateway-866885bb49-cz6jr    1/1       Running     0          3m
istio-galley-6d74549bb9-t8sqb           1/1       Running     0          3m
istio-grafana-post-install-4bgxv        0/1       Completed   0          3m
istio-ingressgateway-6c6ffb7dc8-dnmqx   1/1       Running     0          3m
istio-pilot-685fc95d96-jhfhv            2/2       Running     0          3m
istio-policy-688f99c9c4-pb558           2/2       Running     0          3m
istio-security-post-install-5dw8n       0/1       Completed   0          3m
istio-telemetry-69b794ff59-spkp2        2/2       Running     0          3m
prometheus-f556886b8-cxb9n              1/1       Running     0          3m
servicegraph-778f94d6f8-tfmp6           1/1       Running     0          3m
```
