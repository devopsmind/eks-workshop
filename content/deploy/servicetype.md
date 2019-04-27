---
title: "Vamos checar tipos de serviço"
date: 2018-09-18T17:40:09-05:00
weight: 25
---

Antes de abrirmos o serviço frontend, vamos dar uma olhada nos tipos de serviço que estamos usando:
Isto é o `kubernetes/service.yaml` para nosso serviço de frontend:
```
apiVersion: v1
kind: Service
metadata:
  name: ecsdemo-frontend
spec:
  selector:
    app: ecsdemo-frontend
  type: LoadBalancer
  ports:
   -  protocol: TCP
      port: 80
      targetPort: 3000
```
Observe `type: LoadBalancer` Isso configurará um ELB para manipular o tráfego de entrada para este serviço.

Compare isso com `kubernetes/service.yaml` para um dos nossos serviços de back-end:
```
apiVersion: v1
kind: Service
metadata:
  name: ecsdemo-nodejs
spec:
  selector:
    app: ecsdemo-nodejs
  ports:
   -  protocol: TCP
      port: 80
      targetPort: 3000
```
Observe que não há nenhum tipo de serviço específico descrito. Quando nós verificamos[a documentação do kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)
descobrimos que o tipo padrão é `ClusterIP`. Isso expõe o serviço em um IP interno do cluster.
A escolha desse valor torna o serviço acessível somente dentro do cluster.
