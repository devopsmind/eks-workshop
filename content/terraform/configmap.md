---
title: "Crie o Worker ConfigMap"
date: 2018-08-07T12:00:40-07:00
weight: 50
draft: true
---

O estado terraform também contém um configmap que podemos usar para o nosso EKS workers.

Veja o configmap:
```
terraform output config-map
```

Salve o config-map:
```
terraform output config-map > /tmp/config-map-aws-auth.yml
```

Aplique o config-map:
```
kubectl apply -f /tmp/config-map-aws-auth.yml
```

Confirme seu Nodes:
```
kubectl get nodes
```

#### Parabéns!
Agora você tem um cluster do Amazon EKS totalmente funcional pronto para uso!
