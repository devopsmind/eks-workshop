---
title: "Instalar Calico"
date: 2018-11-08T08:30:11-07:00
weight: 1
---

Aplique o manifesto Calico do [projeto GitHub aws/amazon-vpc-cni-k8s ](https://github.com/aws/amazon-vpc-cni-k8s). Isso cria o daemon set  no namespace kube-system .


```
kubectl apply -f https://raw.githubusercontent.com/aws/amazon-vpc-cni-k8s/master/config/v1.2/calico.yaml
```
Vamos ver alguns dos principais recursos do manifesto Calico:

1) Nós vemos uma annotation throughout; [annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) são uma maneira de anexar metadados **non-identifying**  para objetos. Estes metadados não são usados ​​internamente pelo Kubernetes, então eles não podem ser usados ​​para serem identificados dentro do K8. Em vez disso, eles são usados ​​por ferramentas e bibliotecas externas. Exemplos de anotações incluem timestamps de compilação/liberação, informações da biblioteca do cliente para depuração ou campos gerenciados por uma política de rede, como o Calico, neste caso.

```
kind: DaemonSet
apiVersion: extensions/v1beta1
metadata:
  name: calico-node
  namespace: kube-system
  labels:
    k8s-app: calico-node
spec:
  selector:
    matchLabels:
      k8s-app: calico-node
  updateStrategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
  template:
    metadata:
      labels:
        k8s-app: calico-node
      annotations:
        # This, along with the CriticalAddonsOnly toleration below,
        # marks the pod as a critical add-on, ensuring it gets
        # priority scheduling and that its resources are reserved
        # if it ever gets evicted.
        *scheduler**.alpha.kubernetes.io/critical-pod: ''*
        ...
```
Em contraste, [Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors) no Kubernetes devem ser usados ​​para especificar atributos **identifying** para objetos. Eles são usados ​​por consultas de seletor ou com seletores de labels. Como eles são usados ​​internamente pelo Kubernetes, a estrutura de chaves e valores é restrita para otimizar as consultas.


2) Nós vemos que o manifesto tem um atributo de tolerância. [Contornos e tolerâncias](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) trabalhe em conjunto para garantir que os pods não sejam agendados em nós inapropriados. As impurezas são aplicadas aos nós e **apenas pods que podem tolerar a contaminação podem ser executados nesses nós.** 

Uma contaminação consiste em uma chave, um valor para isso e um efeito, que pode ser:

* PreferNoSchedule: Prefiro não agendar pods intolerantes para o nó contaminado
* NoSchedule: Não agende pods intolerantes para o nó contaminado
* NoExecute: Além de não agendar, também despeje os pods intolerantes que já estão em execução no nó.
    
Como as contaminações, as tolerâncias também têm um par de valores-chave e um efeito, com a adição do operador.
Aqui no manifesto Calico, vemos tolerâncias tem apenas um atributo: **Operator = exists**. Isso significa que o par de valores-chave é omitido e a tolerância corresponderá a qualquer contaminação, garantindo que seja executada em todos os nós.

```
 tolerations:
      - operator: Exists
```
Observe os conjuntos de daemon do kube-system e aguarde até que o daemon do nó calico esteja configurado para ter o número DESIRED de pods no estado READY.

```
kubectl get daemonset calico-node --namespace=kube-system
```
Expected Output:

```
NAME          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
calico-node   3         3         3       3            3           <none>          38s
```





