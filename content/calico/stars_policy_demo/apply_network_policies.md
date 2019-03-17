---
title: "Aplicar políticas de rede"
date: 2018-08-07T08:30:11-07:00
weight: 3
---
Em um cluster de nível de produção, não é seguro ter uma comunicação aberta de pod para pod. Vamos ver como podemos isolar os serviços uns dos outros.

Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/calico_resources
wget https://eksworkshop.com/calico/stars_policy_demo/apply_network_policies.files/default-deny.yaml
```

Vamos examinar nosso arquivo executando `cat default-deny.yaml`.

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: default-deny
spec:
  podSelector:
    matchLabels: {}
```

Vamos repassar a política de rede. Aqui vemos que o podSelector não tem nenhum matchLabels, essencialmente bloqueando todos os pods de acessá-lo.

Aplique a política de rede no namespace **stars** (serviços frontend e backend) e o namespace **client** (client service):

```
kubectl apply -n stars -f default-deny.yaml
kubectl apply -n client -f default-deny.yaml
```

Ao atualizar seu navegador, você vê que a Interface de gerenciamento não pode alcançar nenhum dos nós, portanto, nada aparece na interface do usuário.

As políticas de rede no Kubernetes usam rótulos para selecionar pods e definir regras sobre o tráfego permitido para alcançar esses pods. Eles podem especificar entrada ou saída ou ambos. Cada regra permite o tráfego que corresponde às seções de e portas.

Crie duas novas políticas de rede.

Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/calico_resources
wget https://eksworkshop.com/calico/stars_policy_demo/apply_network_policies.files/allow-ui.yaml
wget https://eksworkshop.com/calico/stars_policy_demo/apply_network_policies.files/allow-ui-client.yaml
```

Mais uma vez, podemos examinar o conteúdo dos nossos arquivos executando: `cat allow-ui.yaml`

```
kind: NetworkPolicy
apiVersion: extensions/v1beta1
metadata:
  namespace: stars
  name: allow-ui
spec:
  podSelector:
    matchLabels: {}
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              role: management-ui
```

`cat allow-ui-client.yaml`
```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: client
  name: allow-ui
spec:
  podSelector:
    matchLabels: {}
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              role: management-ui
```

#### Desafio:
**Como aplicamos nossas políticas de rede para permitir o tráfego que desejamos?**

{{%expand "Expanda aqui para ver a solução" %}}
```
kubectl apply -f allow-ui.yaml
kubectl apply -f allow-ui-client.yaml
```
{{% /expand %}}

Ao atualizar seu navegador, você pode ver que a interface do usuário de gerenciamento pode alcançar todos os serviços, mas eles não podem se comunicar uns com os outros.

![Interface de gerenciamento acessar todos os serviços](/images/calico-mgmtui-access.png)
