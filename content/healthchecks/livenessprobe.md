---
title: "Configure Liveness Probe"
chapter: false
weight: 5
---

#### Configurar o probe


Use o comando abaixo para criar um diretório

```
mkdir ~/environment/healthchecks
```

Salve o manifesto como liveness-app.yaml usando seu editor favorito. Você pode revisar o manifesto descrito abaixo. No arquivo de configuração, o livenessProbe determina como o kubelet deve verificar o contêiner para considerar se ele está em boas condições ou não. O kubelet usa o campo periodSeconds para fazer verificações freqüentes no contêiner. Nesse caso, o kubelet verifica a sonda de atividade a cada 5 segundos. O campo initialDelaySeconds é para dizer ao kubelet que ele deve esperar por 5 segundos antes de fazer o primeiro teste. Para realizar uma análise, o kubelet envia uma solicitação HTTP GET para o servidor que hospeda esse pod e, se o manipulador dos servidores / integridade retornar um código de sucesso, o Contêiner será considerado íntegro. Se o manipulador retornar um código de falha, o kubelet mata o contêiner e o reinicia.

```
apiVersion: v1
kind: Pod
metadata:
  name: liveness-app
spec:
  containers:
  - name: liveness
    image: brentley/ecsdemo-nodejs
    livenessProbe:
      httpGet:
        path: /health
        port: 3000
      initialDelaySeconds: 5
      periodSeconds: 5
```

Vamos criar o pod usando o manifesto

```
kubectl apply -f ~/environment/healthchecks/liveness-app.yaml
```

O comando acima cria um pod com sonda de vivacidade

```
kubectl get pod liveness-app
```
A saída parece abaixo. Observe o ***RESTARTS***

```
NAME           READY     STATUS    RESTARTS   AGE
liveness-app   1/1       Running   0          11s
```

O comando `kubectl describe` mostrará um histórico de eventos que mostrará qualquer falha ou reinicialização da sonda.
```bash
kubectl describe pod liveness-app
```

```text
Events:
  Type    Reason                 Age   From                                    Message
  ----    ------                 ----  ----                                    -------
  Normal  Scheduled              38s   default-scheduler                       Successfully assigned liveness-app to ip-192-168-18-63.ec2.internal
  Normal  SuccessfulMountVolume  38s   kubelet, ip-192-168-18-63.ec2.internal  MountVolume.SetUp succeeded for volume "default-token-8bmt2"
  Normal  Pulling                37s   kubelet, ip-192-168-18-63.ec2.internal  pulling image "brentley/ecsdemo-nodejs"
  Normal  Pulled                 37s   kubelet, ip-192-168-18-63.ec2.internal  Successfully pulled image "brentley/ecsdemo-nodejs"
  Normal  Created                37s   kubelet, ip-192-168-18-63.ec2.internal  Created container
  Normal  Started                37s   kubelet, ip-192-168-18-63.ec2.internal  Started container
```


#### Introduzir um fracasso
Vamos executar o próximo comando para enviar um sinal SIGUSR1 para o aplicativo nodejs. Ao emitir este comando, enviaremos um sinal de kill ao processo de aplicativo no tempo de execução do docker.

```
kubectl exec -it liveness-app -- /bin/kill -s SIGUSR1 1
```

Descreva o pod depois de esperar por 15-20 segundos e você notará ações do Kubelet de matar o Contêiner e reiniciá-lo. 
```
Events:
  Type     Reason                 Age                From                                    Message
  ----     ------                 ----               ----                                    -------
  Normal   Scheduled              1m                 default-scheduler                       Successfully assigned liveness-app to ip-192-168-18-63.ec2.internal
  Normal   SuccessfulMountVolume  1m                 kubelet, ip-192-168-18-63.ec2.internal  MountVolume.SetUp succeeded for volume "default-token-8bmt2"
  Warning  Unhealthy              30s (x3 over 40s)  kubelet, ip-192-168-18-63.ec2.internal  Liveness probe failed: Get http://192.168.13.176:3000/health: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
  Normal   Pulling                0s (x2 over 1m)    kubelet, ip-192-168-18-63.ec2.internal  pulling image "brentley/ecsdemo-nodejs"
  Normal   Pulled                 0s (x2 over 1m)    kubelet, ip-192-168-18-63.ec2.internal  Successfully pulled image "brentley/ecsdemo-nodejs"
  Normal   Created                0s (x2 over 1m)    kubelet, ip-192-168-18-63.ec2.internal  Created container
  Normal   Started                0s (x2 over 1m)    kubelet, ip-192-168-18-63.ec2.internal  Started container
  Normal   Killing                0s                 kubelet, ip-192-168-18-63.ec2.internal  Killing container with id docker://liveness:Container failed liveness probe.. Container will be killed and recreated.
```

Quando o aplicativo nodejs entrou em um modo de depuração com sinal SIGUSR1, ele não respondeu aos pings de verificação de integridade e o kubelet interrompeu o contêiner. O contêiner estava sujeito à política de reinicialização padrão.

```
kubectl get pod liveness-app
```

The output looks like below

```
NAME           READY     STATUS    RESTARTS   AGE
liveness-app   1/1       Running   1          12m
```

#### Desafio:
**Como podemos verificar o status das verificações de integridade do contêiner?**

{{%expand "Expanda aqui para ver a solução" %}}
```bash
kubectl logs liveness-app
```
Você também pode usar o `kubectl logs` para recuperar logs de uma instanciação anterior de um container com`flag --previous `, caso o container tenha caído
```bash
kubectl logs liveness-app --previous
```
```text
<Output omitted>
Exemplo de aplicativo ouvindo na porta 3000!
::ffff:192.168.43.7 - - [20/Nov/2018:22:53:01 +0000] "GET /health HTTP/1.1" 200 16 "-" "kube-probe/1.10"
::ffff:192.168.43.7 - - [20/Nov/2018:22:53:06 +0000] "GET /health HTTP/1.1" 200 17 "-" "kube-probe/1.10"
::ffff:192.168.43.7 - - [20/Nov/2018:22:53:11 +0000] "GET /health HTTP/1.1" 200 17 "-" "kube-probe/1.10"
Starting debugger agent.
Debugger listening on [::]:5858
```
{{% /expand %}}