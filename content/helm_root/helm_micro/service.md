---
title: "Teste o serviço"
date: 2018-08-07T08:30:11-07:00
weight: 40
---

Para testar o serviço criado pelo nosso eksdemo Chart, precisaremos obter o nome do endpoint do ELB que foi gerado quando implantamos o Chart:

```
kubectl get svc ecsdemo-frontend -o jsonpath="{.status.loadBalancer.ingress[*].hostname}"; echo
```

Copie esse endereço e cole-o em uma nova guia no seu navegador. Você deve ver algo semelhante a:

![Example Service](/images/helm_micro/micro_example.png)
