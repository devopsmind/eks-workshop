---
title: "Permitir tráfego direcional"
date: 2018-08-07T08:30:11-07:00
weight: 4
---
Vamos ver como podemos permitir o tráfego direcional do cliente para frontend e backend.

Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/calico_resources
wget https://eksworkshop.com/calico/stars_policy_demo/directional_traffic.files/backend-policy.yaml
wget https://eksworkshop.com/calico/stars_policy_demo/directional_traffic.files/frontend-policy.yaml
```

Vamos examinar essa política de back-end com `cat backend-policy.yaml`:
```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: stars
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      role: backend
  ingress:
    - from:
        - <EDIT: UPDATE WITH THE CONFIGURATION NEEDED TO WHITELEST FRONTEND USING PODSELECTOR>
            <EDIT: UPDATE WITH THE CONFIGURATION NEEDED TO WHITELEST FRONTEND USING PODSELECTOR>
              <EDIT: UPDATE WITH THE CONFIGURATION NEEDED TO WHITELEST FRONTEND USING PODSELECTOR>
      ports:
        - protocol: TCP
          port: 6379
```
#### Challenge:
**Depois de analisar o manifesto, você verá que deixamos intencionalmente alguns dos campos de configuração para você EDITAR. Por favor, edite a configuração conforme sugerido. Você pode encontrar informações úteis nesta [Documentação do kubernetes](https://kubernetes.io/docs/concepts/services-networking/network-policies/)**

{{% expand "Expanda aqui para ver a solução"%}}
```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: stars
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      role: backend
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 6379
```
{{%/expand%}}

Vamos examinar a política de frontend com`cat frontend-policy.yaml`:

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: stars
  name: frontend-policy
spec:
  podSelector:
    matchLabels:
      role: frontend
  ingress:
    - from:
        - <EDIT: UPDATE WITH THE CONFIGURATION NEEDED TO WHITELEST CLIENT USING NAMESPACESELECTOR>
            <EDIT: UPDATE WITH THE CONFIGURATION NEEDED TO WHITELEST CLIENT USING NAMESPACESELECTOR>
              <EDIT: UPDATE WITH THE CONFIGURATION NEEDED TO WHITELEST CLIENT USING NAMESPACESELECTOR>
      ports:
        - protocol: TCP
          port: 80
```
#### Desafio:
**Por favor, edite a configuração conforme sugerido. Você pode encontrar informações úteis nesta [Documentação do kubernetes](https://kubernetes.io/docs/concepts/services-networking/network-policies/)**

{{% expand "Expanda aqui para ver a solução"%}}
```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: stars
  name: frontend-policy
spec:
  podSelector:
    matchLabels:
      role: frontend
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              role: client
      ports:
        - protocol: TCP
          port: 80
```
{{%/expand%}}
Para permitir o tráfego do serviço de frontend para o serviço de back-end, aplique o seguinte manifesto:

```
kubectl apply -f backend-policy.yaml
```

E permitir tráfego do namespace do cliente para o serviço frontend:

```
kubectl apply -f frontend-policy.yaml
```
Ao atualizar seu navegador, você poderá ver as políticas de rede em ação:

![directional traffic](/images/calico-client-f-b-access.png)

Vamos dar uma olhada na política de backend. Sua especificação tem um podSelector que seleciona todos os pods com a label **role:backend**, e permite a entrada de todos os pods que têm o label **role:frontend** e na porta TCP **6379**, mas não o contrário. O tráfego é permitido em uma direção em um número de porta específico.

```
spec:
  podSelector:
    matchLabels:
      role: backend
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
      ports:
        - protocol: TCP
          port: 6379
```

O frontend-policy é semelhante, exceto pelo fato de permitir a entrada de ** namespaces ** que têm o label **role: client** na porta TCP **80**.

```
spec:
  podSelector:
    matchLabels:
      role: frontend
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              role: client
      ports:
        - protocol: TCP
          port: 80
```
