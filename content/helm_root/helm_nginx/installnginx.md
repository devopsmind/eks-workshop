---
title: "Instalar o bitnami/nginx"
date: 2018-08-07T08:30:11-07:00
weight: 400
---

Instalando o chart do servidor  Web NGINX autônomo Bitnami nos envolve usando o comando [helm install](https://docs.helm.sh/helm/#helm-install) command.

Quando instalamos usando o Helm, precisamos fornecer um nome de implantação ou um nome aleatório será atribuído à implantação automaticamente.

#### Desafio:
**Como você pode usar o Helm para implantar o chart bitnami/nginx?**

**SUGESTÃO:** Use o utilitário **helm** para **instalar** o chart **bitnami/nginx**  e especifique o nome **mywebserver** para a implantação do Kubernetes. Consulte a documentação do [helm install](https://docs.helm.sh/helm/#helm-install)  ou executar o comando ```helm install --help```  para descobrir a sintaxe

{{%expand "Expanda aqui para ver a solução" %}}
```
helm install --name mywebserver bitnami/nginx
```
{{% /expand %}}

Depois de executar este comando, a saída confirma os tipos de objetos k8s que foram criados como resultado:

```
NAME:   mywebserver
LAST DEPLOYED: Tue Nov 13 19:55:25 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME               AGE
mywebserver-nginx  0s

==> v1/Pod(related)

NAME                                READY  STATUS             RESTARTS  AGE
mywebserver-nginx-85985c8466-tczst  0/1    ContainerCreating  0         0s

==> v1/Service

NAME               AGE
mywebserver-nginx  0s
```

{{% notice info %}}
Nos seguintes exemplos de comandos do  **kubectl** , pode levar um minuto ou dois para cada um desses objetos **DESIRED** e **CURRENT** valores para corresponder; se eles não corresponderem na primeira tentativa, aguarde alguns segundos e execute o comando novamente para verificar o status.
{{% /notice %}}

O primeiro objeto mostrado nesta saída é um [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).  Um objeto de implantação gerencia distribuições (e reversões) de diferentes versões de um aplicativo.

Você pode inspecionar esse objeto de implementação mais detalhadamente executando o seguinte comando:

```
kubectl describe deployment mywebserver-nginx
```

O próximo objeto mostrado criado pelo gráfico é um [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/).  Um Pod é um grupo de um ou mais contêineres.

Para verificar se o objeto Pod foi implantado com sucesso, podemos executar o seguinte comando:

```
kubectl get pods -l app=mywebserver-nginx
```
E você deve ver uma saída semelhante a:

```
NAME                                 READY     STATUS    RESTARTS   AGE
mywebserver-nginx-85985c8466-tczst   1/1       Running   0          10s
```

O terceiro objeto que este chart cria para nós é um [Service](https://kubernetes.io/docs/concepts/services-networking/service/)  O Serviço nos permite entrar em contato com esse servidor da Web NGINX pela Internet, por meio de um Elastic Load Balancer (ELB). 

Para obter o URL completo deste serviço, execute:

```
kubectl get service mywebserver-nginx -o wide
```

Isso deve produzir algo semelhante a:

```
NAME                TYPE           CLUSTER-IP      EXTERNAL-IP                                                              
mywebserver-nginx   LoadBalancer   10.100.223.99   abc123.amazonaws.com
```

Copie o valor para **EXTERNAL-IP**, abra uma nova guia no seu navegador da Web e cole-a.
{{% notice info %}}
Pode levar alguns minutos para que o ELB e seu nome DNS associado fiquem disponíveis; Se você receber um erro, aguarde um minuto e clique em Atualizar.
{{% /notice %}}

Quando o Serviço fica on-line, você deve ver uma mensagem de boas-vindas semelhante a:

![Helm Logo](/images/helm-nginx/welcome_to_nginx.png)

Parabéns! Agora você implantou com sucesso o servidor da Web autônomo NGINX em seu cluster EKS!
