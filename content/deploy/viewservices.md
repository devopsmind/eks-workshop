---
title: "Encontre o endereço do serviço"
date: 2018-09-18T17:40:09-05:00
weight: 40
---

Agora que temos um serviço em execução que é `type: LoadBalancer`, precisamos encontrar o endereço do ELB.  Podemos fazer isso usando a operação `get services` do kubectl:

```
kubectl get service ecsdemo-frontend
```

Observe que o campo não é largo o suficiente para mostrar o FQDN do ELB. Podemos ajustar o formato de saída com este comando:
```
kubectl get service ecsdemo-frontend -o wide
```

Se quiséssemos usar os dados pragmaticamente, também poderíamos produzir via json. Este é um exemplo de como podemos usar a saída json:
```
ELB=$(kubectl get service ecsdemo-frontend -o json | jq -r '.status.loadBalancer.ingress[].hostname')

curl -m3 -v $ELB
```
{{% notice tip %}}
Levará vários segundos para o ELB ficar saudável e começar a passar o tráfego para os pods do frontend.
{{% /notice %}}

Você também deve poder copiar/colar o nome do host loadBalancer em seu navegador e ver o aplicativo em execução.
Mantenha esta aba aberta enquanto escalamos os serviços na próxima página.
