---
title: "Limpar"
date: 2018-08-07T08:30:11-07:00
weight: 5
---
Limpe a demonstração excluindo os namespaces:

```
kubectl delete ns client stars management-ui
```

<!---
Nós não vamos desinstalar Calico por causa de questões relacionadas ao IpTables. Há um truque sobre como limpar, mas isso não funciona na plataforma da AWS
https://github.com/projectcalico/calico/blob/master/hack/remove-calico-policy/remove-policy.md

Desinstalar Calico:

```
kubectl delete -f https://raw.githubusercontent.com/aws/amazon-vpc-cni-k8s/master/config/v1.2/calico.yaml
```
-->
