---
title: "Dimensionar um cluster com CA"
date: 2018-08-07T08:30:11-07:00
weight: 40
---

### Implantar um aplicativo de exemplo

Vamos implantar um exemplo de aplicativo nginx como `ReplicaSet` of 1 `Pod`

```
cat <<EoF> ~/environment/cluster-autoscaler/nginx.yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-to-scaleout
spec:
  replicas: 1
  template:
    metadata:
      labels:
        service: nginx
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx-to-scaleout
        resources:
          limits:
            cpu: 500m
            memory: 512Mi
          requests:
            cpu: 500m
            memory: 512Mi
EoF
kubectl apply -f ~/environment/cluster-autoscaler/nginx.yaml
kubectl get deployment/nginx-to-scaleout
```

### Escalar nosso ReplicaSet

OK, vamos escalar o replicaset para 10
```
kubectl scale --replicas=10 deployment/nginx-to-scaleout
```
Alguns pods estarão no estado `Pending`, que aciona o escalonamento automático do cluster para escalonar a frota do EC2.

```
kubectl get pods -o wide --watch
```

```
NAME                                 READY     STATUS    RESTARTS   AGE

nginx-to-scaleout-7cb554c7d5-2d4gp   0/1       Pending   0          11s
nginx-to-scaleout-7cb554c7d5-2nh69   0/1       Pending   0          12s
nginx-to-scaleout-7cb554c7d5-45mqz   0/1       Pending   0          12s
nginx-to-scaleout-7cb554c7d5-4qvzl   0/1       Pending   0          12s
nginx-to-scaleout-7cb554c7d5-5jddd   1/1       Running   0          34s
nginx-to-scaleout-7cb554c7d5-5sx4h   0/1       Pending   0          12s
nginx-to-scaleout-7cb554c7d5-5xbjp   0/1       Pending   0          11s
nginx-to-scaleout-7cb554c7d5-6l84p   0/1       Pending   0          11s
nginx-to-scaleout-7cb554c7d5-7vp7l   0/1       Pending   0          12s
nginx-to-scaleout-7cb554c7d5-86pr6   0/1       Pending   0          12s
nginx-to-scaleout-7cb554c7d5-88ttw   0/1       Pending   0          12s
```

Visualizar os logs do cluster autoscaler
```
kubectl logs -f deployment/cluster-autoscaler -n kube-system
```
Você observará eventos do Autoescalador de cluster semelhantes ao abaixo
![CA Scale Up events](/images/scaling-asg-up2.png)

Verifique o AWS Management Console para confirmar se os grupos do Auto Scaling estão sendo dimensionados para atender à demanda. Isso pode levar alguns minutos. Você também pode acompanhar a implantação do pod a partir da linha de comando. Você deve ver a transição dos pods de pendente para execução, à medida que os nós são dimensionados.

![Scale Up](/images/scaling-asg-up.png)
