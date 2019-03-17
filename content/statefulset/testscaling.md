---
title: "Teste de escalonamento"
date: 2018-08-07T08:30:11-07:00
weight: 35
---
Mais slaves podem ser adicionados ao MySQL Cluster para aumentar a capacidade de leitura. Isso pode ser feito seguindo o comando.
```
kubectl scale statefulset mysql --replicas=5
```
Você pode ver a mensagem que statefulset 'mysql' escalou.
```
statefulset "mysql" scaled
```
Veja o progresso da escala ordenada e graciosa.
```
kubectl get pods -l app=mysql -w
```
```
NAME      READY     STATUS     RESTARTS   AGE
mysql-0   2/2       Running    0          1d
mysql-1   2/2       Running    0          1d
mysql-2   2/2       Running    0          24m
mysql-3   0/2       Init:0/2   0          8s
mysql-3   0/2       Init:1/2   0         9s
mysql-3   0/2       PodInitializing   0         11s
mysql-3   1/2       Running   0         12s
mysql-3   2/2       Running   0         16s
mysql-4   0/2       Pending   0         0s
mysql-4   0/2       Pending   0         0s
mysql-4   0/2       Init:0/2   0         0s
mysql-4   0/2       Init:1/2   0         10s
mysql-4   0/2       PodInitializing   0         11s
mysql-4   1/2       Running   0         12s
mysql-4   2/2       Running   0         17s

```

{{% notice note %}}
Pode demorar alguns minutos para iniciar todos os pods.
{{% /notice %}}

Pressione Ctrl C para parar de acompanhar.
Abra outro terminal para Verificar se você fechou o loop .
```
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never --\
   bash -ic "while sleep 1; do mysql -h mysql-read -e 'SELECT @@server_id,NOW()'; done"
```
You will see 5 servers are running.
```
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         101 | 2018-11-14 13:56:42 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         102 | 2018-11-14 13:56:43 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         104 | 2018-11-14 13:56:44 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         100 | 2018-11-14 13:56:45 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         104 | 2018-11-14 13:56:46 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         101 | 2018-11-14 13:56:47 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         100 | 2018-11-14 13:56:48 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         103 | 2018-11-14 13:56:49 |
+-------------+---------------------+
```
Verifique se o slave recém-implementado (mysql-3) possui o mesmo conjunto de dados pelo comando a seguir.
```
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never --\
 mysql -h mysql-3.mysql -e "SELECT * FROM test.messages"
```
Ele mostrará os mesmos dados que o master possui.
```
+--------------------------+
| message                  |
+--------------------------+
| hello, from mysql-client |
+--------------------------+
```
Reduza as réplicas para 3 executando o comando.
```
kubectl scale statefulset mysql --replicas=3
```
Você pode ver o 'mysql' statefulset escalonado 
```
statefulset "mysql" scaled
```
Observe que a escala não exclui os dados ou PVCs anexados aos pods. Você tem que excluí-los manualmente.
Verifique se a escala é finalizada pelo seguinte comando.
```
kubectl get pods -l app=mysql
```
```
NAME      READY     STATUS    RESTARTS   AGE
mysql-0   2/2       Running   0          1d
mysql-1   2/2       Running   0          1d
mysql-2   2/2       Running   0          35m
```

Verifique 2 PVCs (data-mysql-3, data-mysql-4) ainda existem executando o comando.
```
kubectl get pvc -l app=mysql
```
```
NAME           STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-mysql-0   Bound     pvc-83e9dfeb-e721-11e8-86c5-069628ef0c9c   10Gi       RWO            mysql-gp2            1d
data-mysql-1   Bound     pvc-977e7806-e721-11e8-86c5-069628ef0c9c   10Gi       RWO            mysql-gp2            1d
data-mysql-2   Bound     pvc-b3009b02-e721-11e8-86c5-069628ef0c9c   10Gi       RWO            mysql-gp2            1d
data-mysql-3   Bound     pvc-de14acd8-e811-11e8-86c5-069628ef0c9c   10Gi       RWO            mysql-gp2            34m
data-mysql-4   Bound     pvc-e916c3ec-e812-11e8-86c5-069628ef0c9c   10Gi       RWO            mysql-gp2            26m
```


#### Desafio:
**Por padrão, a exclusão de um PersistentVolumeClaim excluirá seu volume persistente associado. E se você quisesse manter o volume? Alterar a política de recuperação do PersistentVolume associado ao PVC "data-mysql-3" para "Retain". Por favor, veja [Documentação do kubernetes](https://kubernetes.io/docs/tasks/administer-cluster/change-pv-reclaim-policy/) para ajudá-lo**

{{% expand "Expand here to see the solution"%}}
Alterar a política de reclaim:
```
kubectl patch pv <your-pv-name> -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}'
```
Agora, se você excluir o Data-mysql-3 do PersistentVolumeClaim, você ainda pode ver o volume do EBS no seu console do AWS EC2, com seu status "available".

Vamos alterar a política de reclaim de volta para 'Excluir' para evitar volumes órfãos:
```
kubectl patch pv <your-pv-name> -p '{"spec":{"persistentVolumeReclaimPolicy":"Delete"}}'
```
{{%/expand%}}

Delete data-mysql-3, data-mysql-4 by following command.
```
kubectl delete pvc data-mysql-3
kubectl delete pvc data-mysql-4
```
```
persistentvolumeclaim "data-mysql-3" deleted
persistentvolumeclaim "data-mysql-4" deleted
```
