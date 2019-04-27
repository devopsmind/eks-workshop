---
title: "Criar recursos"
date: 2018-08-07T08:30:11-07:00
weight: 1
---

Antes de criar políticas de rede, vamos criar os recursos necessários.
Crie uma nova pasta para os arquivos de configuração.

```
mkdir ~/environment/calico_resources
cd ~/environment/calico_resources
```

####Namespace Stars

Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/calico_resources
wget https://eksworkshop.com/calico/stars_policy_demo/create_resources.files/namespace.yaml
```

Vamos examinar nosso arquivo executando `cat namespace.yaml`.

```
kind: Namespace
apiVersion: v1
metadata:
  name: stars
```

Crie um namespace chamado stars:

```
kubectl apply -f namespace.yaml
```

Vamos criar frontend e backend [controladores de replicação](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/) e [services](https://kubernetes.io/docs/concepts/services-networking/service/) neste namespace em etapas posteriores.


Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/calico_resources
wget https://eksworkshop.com/calico/stars_policy_demo/create_resources.files/management-ui.yaml
wget https://eksworkshop.com/calico/stars_policy_demo/create_resources.files/backend.yaml
wget https://eksworkshop.com/calico/stars_policy_demo/create_resources.files/frontend.yaml
wget https://eksworkshop.com/calico/stars_policy_demo/create_resources.files/client.yaml
```

`cat management-ui.yaml`:

```
apiVersion: v1
kind: Namespace
metadata:
  name: management-ui
  labels:
    role: management-ui
---
apiVersion: v1
kind: Service
metadata:
  name: management-ui
  namespace: management-ui
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 9001
  selector:
    role: management-ui
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: management-ui
  namespace: management-ui
spec:
  replicas: 1
  template:
    metadata:
      labels:
        role: management-ui
    spec:
      containers:
      - name: management-ui
        image: calico/star-collect:v0.1.0
        imagePullPolicy: Always
        ports:
        - containerPort: 9001
```

Criar um namespace management-ui , com um serviço do namespace management-ui e um controlador de replicação dentro desse namespace:
```
kubectl apply -f management-ui.yaml
```

`cat backend.yaml` para ver como o serviço de back-end é construído:

```
apiVersion: v1
kind: Service
metadata:
  name: backend
  namespace: stars
spec:
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    role: backend
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: backend
  namespace: stars
spec:
  replicas: 1
  template:
    metadata:
      labels:
        role: backend
    spec:
      containers:
      - name: backend
        image: calico/star-probe:v0.1.0
        imagePullPolicy: Always
        command:
        - probe
        - --http-port=6379
        - --urls=http://frontend.stars:80/status,http://backend.stars:6379/status,http://client.client:9000/status
        ports:
        - containerPort: 6379
```

Vamos examinar o serviço de frontend com `cat frontend.yaml`:

```
apiVersion: v1
kind: Service
metadata:
  name: frontend
  namespace: stars
spec:
  ports:
  - port: 80
    targetPort: 80
  selector:
    role: frontend
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: frontend
  namespace: stars
spec:
  replicas: 1
  template:
    metadata:
      labels:
        role: frontend
    spec:
      containers:
      - name: frontend
        image: calico/star-probe:v0.1.0
        imagePullPolicy: Always
        command:
        - probe
        - --http-port=80
        - --urls=http://frontend.stars:80/status,http://backend.stars:6379/status,http://client.client:9000/status
        ports:
        - containerPort: 80
```

Crie controladores e serviços de replicação frontend e backend dentro do namespace stars:

```
kubectl apply -f backend.yaml
kubectl apply -f frontend.yaml
```

Por fim, vamos examinar como o namespace  client e um serviço de client para um controlador de replicação.
são construídos. `cat client.yaml`:

```
kind: Namespace
apiVersion: v1
metadata:
  name: client
  labels:
    role: client
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: client
  namespace: client
spec:
  replicas: 1
  template:
    metadata:
      labels:
        role: client
    spec:
      containers:
      - name: client
        image: calico/star-probe:v0.1.0
        imagePullPolicy: Always
        command:
        - probe
        - --urls=http://frontend.stars:80/status,http://backend.stars:6379/status
        ports:
        - containerPort: 9000
---
apiVersion: v1
kind: Service
metadata:
  name: client
  namespace: client
spec:
  ports:
  - port: 9000
    targetPort: 9000
  selector:
    role: client
```

Aplique a configuração do cliente.

```
kubectl apply -f client.yaml
```
Verifique seu status e aguarde até que todos os pods alcancem o status de execução:

```
$ kubectl get pods --all-namespaces
```
Sua saída deve ficar assim:

```
NAMESPACE       NAME                                                  READY   STATUS    RESTARTS   AGE
client          client-nkcfg                                          1/1     Running   0          24m
kube-system     aws-node-6kqmw                                        1/1     Running   0          50m
kube-system     aws-node-grstb                                        1/1     Running   1          50m
kube-system     aws-node-m7jg8                                        1/1     Running   1          50m
kube-system     calico-node-b5b7j                                     1/1     Running   0          28m
kube-system     calico-node-dw694                                     1/1     Running   0          28m
kube-system     calico-node-vtz9k                                     1/1     Running   0          28m
kube-system     calico-typha-75667d89cb-4q4zx                         1/1     Running   0          28m
kube-system     calico-typha-horizontal-autoscaler-78f747b679-kzzwq   1/1     Running   0          28m
kube-system     kube-dns-7cc87d595-bd9hq                              3/3     Running   0          1h
kube-system     kube-proxy-lp4vw                                      1/1     Running   0          50m
kube-system     kube-proxy-rfljb                                      1/1     Running   0          50m
kube-system     kube-proxy-wzlqg                                      1/1     Running   0          50m
management-ui   management-ui-wzvz4                                   1/1     Running   0          24m
stars           backend-tkjrx                                         1/1     Running   0          24m
stars           frontend-q4r84                                        1/1     Running   0          24m
```

{{% notice note %}}
Pode levar vários minutos para baixar todas as imagens necessárias do Docker.
{{% /notice %}}

Para resumir os diferentes recursos que criamos:

* Um namespace chamado **stars**
* **frontend** e **backend** controladores de replicação e serviços dentro do namespace **stars** 
* Um namespace chamado **management-ui**
* Controlador e serviço de replicação **management-ui** para a interface do usuário visto no navegador, No namespace **management-ui**
* Um namespace chamado **client**
* **client** controlador de replicação e serviço no namespace **client** 
