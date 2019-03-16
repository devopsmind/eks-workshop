---
title: "Monitorar & Visualizar"
date: 2018-11-13T22:42:01+09:00
weight: 60
draft: false
---

### Coletando novos dados de telemetria

Em seguida, faça o download de um arquivo YAML para manter a configuração da nova métrica e do fluxo de logs que o Istio irá gerar e coletar automaticamente.

```
curl -LO https://eksworkshop.com/servicemesh/deploy.files/istio-telemetry.yaml

kubectl apply -f istio-telemetry.yaml
```

Certifique-se de que Prometheus e Grafana estejam funcionando

```
kubectl -n istio-system get svc prometheus

kubectl -n istio-system get svc grafana
```

Configurar o encaminhamento de porta para o Grafana executando o seguinte comando:

```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 8080:3000 &
```

Abra o Painel do Istio através da interface Grafana 

1. Em seu ambiente Cloud9, click **Preview / Preview Running Application**
1. Vá até  **the fim da URL** e acrescente:

```
/dashboard/db/istio-mesh-dashboard
```

Abra uma nova guia de terminal e insira para enviar um tráfego para o mesh

```
export SMHOST=$(kubectl get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname} ' -n istio-system)

SMHOST="$(echo -e "${SMHOST}" | tr -d '[:space:]')"

while true; do curl -o /dev/null -s "${SMHOST}/productpage"; done
```

Você verá que o tráfego está distribuído uniformemente entre <span style="color:orange">reviews:v1</span> e <span style="color:blue">reviews:v3</span>

![Grafana Dashabord](/images/servicemesh-visualize1.png)

Encorajamos você a explorar outros painéis do Istio que estão disponíveis clicando em **Istio Mesh Dashboard** no menu do canto superior esquerdo da página, e selecionando um painel diferente.
