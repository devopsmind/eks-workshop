---
title: "Configure Readiness Probe"
chapter: false
weight: 10
---

#### Configurar o probe

Salve o texto do bloco a seguir como **~/environment/healthchecks/readiness-deployment.yaml**. A definição readinessProbe explica como um comando linux pode ser configurado como healthcheck. Criamos um arquivo vazio ***/tmp/healthy** para configurar o probe de prontidão e usamos o mesmo para entender como o kubelet ajuda a atualizar uma implantação apenas com pods saudáveis.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: readiness-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: readiness-deployment
  template:
    metadata:
      labels:
        app: readiness-deployment
    spec:
      containers:
      - name: readiness-deployment
        image: alpine
        command: ["sh", "-c", "touch /tmp/healthy && sleep 86400"]
        readinessProbe:
          exec:
            command:
            - cat
            - /tmp/healthy
          initialDelaySeconds: 5
          periodSeconds: 3

```

Vamos agora criar uma implantação para testar readiness probe

```
kubectl apply -f ~/environment/healthchecks/readiness-deployment.yaml
```

O comando acima cria uma implantação com 3 réplicas e prontidão do probe, conforme descrito no início

```
kubectl get pods -l app=readiness-deployment
```

A saída parece semelhante ao abaixo

```

NAME                                    READY     STATUS    RESTARTS   AGE
readiness-deployment-7869b5d679-922mx   1/1       Running   0          31s
readiness-deployment-7869b5d679-vd55d   1/1       Running   0          31s
readiness-deployment-7869b5d679-vxb6g   1/1       Running   0          31s
```

Vamos também confirmar que todas as réplicas estão disponíveis para servir o tráfego quando um serviço é apontado para este deployment.

```
kubectl describe deployment readiness-deployment | grep Replicas:
```

A saída parece abaixo

```
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
```

#### Introduzir um falha
Escolha um dos pods acima de 3 e emita um comando como abaixo para excluir o arquivo **/tmp/healthy**, o que faz com que o probe readiness falhe.

```
kubectl exec -it readiness-deployment-<YOUR-POD-NAME> -- rm /tmp/healthy
```

**readiness-deployment-7869b5d679-922mx** foi escolhido em nosso cluster de exemplo. O arquivo **/tmp/healthy** foi excluído. Este arquivo deve estar presente para que a verificação de disponibilidade passe. Abaixo está o status após a emissão do comando.

```
kubectl get pods -l app=readiness-deployment
```

A saída parece semelhante a abaixo:
```
NAME                                    READY     STATUS    RESTARTS   AGE
readiness-deployment-7869b5d679-922mx   0/1       Running   0          4m
readiness-deployment-7869b5d679-vd55d   1/1       Running   0          4m
readiness-deployment-7869b5d679-vxb6g   1/1       Running   0          4m
```
O tráfego não será roteado para o primeiro pod na implantação acima. A coluna pronta confirma que o probe de preparação para este pod não foi aprovado e, portanto, foi marcado como não pronto. 

Agora, verificaremos as réplicas disponíveis para atender ao tráfego quando um serviço for apontado para essa implantação.

```
kubectl describe deployment readiness-deployment | grep Replicas:
```

A saída parece abaixo

```
Replicas:               3 desired | 3 updated | 3 total | 2 available | 1 unavailable
```

Quando o probe de prontidão para um pod falhar, o controlador de pontos de extremidade remove o pod da lista de pontos de extremidade de todos os serviços que correspondem ao pod.

#### Desafio: 
**Como você restauraria o pod para o status Ready??**
{{%expand "Expanda aqui para ver a solução" %}}
Execute o comando abaixo com o nome do pod para recriar o arquivo **/tmp/healthy**. Depois que o pod passa pela sonda, ela é marcada como pronta e começará a receber tráfego novamente.

```
kubectl exec -it readiness-deployment-<YOUR-POD-NAME> -- touch /tmp/healthy
```
```
kubectl get pods -l app=readiness-deployment
```
{{% /expand %}}
