---
title: "Limpar"
date: 2018-08-07T08:30:11-07:00
weight: 40
---
Primeiro exclua o StatefulSet. Isso também excluirá os pods. 
Pode demorar um pouco.
```
kubectl delete statefulset mysql
```
Verifique se não há pods em execução executando o comando.
```
kubectl get pods -l app=mysql
```
```
No resources found.
```

Exclua o ConfigMap, Service e PVC, executando o comando.
```
kubectl delete configmap,service,pvc -l app=mysql
```
```
configmap "mysql-config" deleted
service "mysql" deleted
service "mysql-read" deleted
persistentvolumeclaim "data-mysql-0" deleted
persistentvolumeclaim "data-mysql-1" deleted
persistentvolumeclaim "data-mysql-2" deleted
```
#### Parabéns! Você terminou o laboratório StatefulSets.
