---
title: "Comunicação padrão Pod-to-Pod"
date: 2018-08-07T08:30:11-07:00
weight: 2
---
No Kubernetes, os  **pods por padrão pode se comunicar com outros pods**, independentemente de qual host eles se comunicam. Cada pod recebe seu próprio endereço IP para que você não precise criar explicitamente links entre os pods. Isso é demonstrado pelo **management-ui**.

```
kind: Service
metadata:
  name: management-ui
  namespace: management-ui
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 9001
```

Para abrir a interface do usuário de gerenciamento usando, recupere o nome DNS da interface do usuário de gerenciamento usando:

```
kubectl get svc -o wide -n management-ui
```

Copie o **EXTERNAL-IP** da saída e cole em um navegador.
A coluna EXTERNAL-IP contém um valor que termina com "elb.amazonaws.com” - o valor total é o endereço DNS.

```output
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP                                                              PORT(S)        AGE       SELECTOR
management-ui   LoadBalancer   10.100.239.7   a8b8c5f77eda911e8b1a60ea5d5305a4-720629306.us-east-1.elb.amazonaws.com   80:31919/TCP   9s        role=management-ui
```

A interface do usuário aqui mostra o comportamento padrão, de todos os serviços sendo capazes de alcançar um ao outro.

![full access](/images/calico-full-access.png)
