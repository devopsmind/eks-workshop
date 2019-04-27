---
title: "Roteamento Inteligente"
date: 2018-11-13T21:49:32+09:00
weight: 50
draft: false
---

### Roteamento Inteligente

A implantação de um aplicativo baseado em microsserviço em uma service mesh do Istio permite controlar externamente o monitoramento e o rastreamento de serviços, o roteamento de solicitação (versão), testes de resiliência, segurança e aplicação de políticas e mais de maneira consistente em todos os serviços e no aplicativo.

Antes de usar o Istio para controlar o roteamento da versão Bookinfo, você precisará definir as versões disponíveis, chamadas <span style="color:orange">**subsets**</span>, nas regras de destino.

{{% notice info %}}
Versões de serviço (a.k.a. subsets) - Em um cenário de implantação contínua, para um determinado serviço, pode haver subconjuntos distintos de instâncias executando diferentes variantes do binário do aplicativo. Essas variantes não são necessariamente versões diferentes da API. Podem ser alterações iterativas para o mesmo serviço, implantadas em diferentes ambientes (prod, staging, dev, etc.). Cenários comuns onde isso ocorre incluem A/B testing, canary rollouts, etc. A escolha de uma determinada versão pode ser decidida com base em vários critérios (cabeçalhos, url, etc.) e / ou por pesos atribuídos a cada versão. Cada serviço tem uma versão padrão que consiste em todas as suas instâncias.
{{% /notice %}}

```
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml

kubectl get destinationrules -o yaml
```

Para rotear para apenas uma versão, você aplica serviços virtuais que definem a versão padrão dos microsserviços. Nesse caso, os serviços virtuais rotearão todo o tráfego para<span style="color:orange">**reviews:v1**</span> de microserviço.

```
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

kubectl get virtualservices reviews -o yaml
```

O subconjunto é definido como v1 para todas as solicitações de comentários.

```
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
```

Tente agora recarregar a página várias vezes e observe como somente a versão 1 das resenhas é exibida a cada vez.

Em seguida, alteraremos a configuração da rota para que todo o tráfego de um usuário específico seja roteado para uma versão de serviço específica. Nesse caso, todo o tráfego de um usuário chamado <span style="color:orange">*Jason*</span> será encaminhado para o serviço <span style="color:orange">**reviews:v2**</span>.

```
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml

kubectl get virtualservices reviews -o yaml
```

O subconjunto é definido como v1 no padrão e rota v2 se o usuário logado for compatível com 'jason' para solicitação de comentários.

```
spec:
  hosts:
  - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```

Para testar, clique **Sign in** no canto superior direito da página e faça o login usando **jason** como nome de usuário com uma senha em branco. Você só verá resenhas: v2 o tempo todo. Outros verão revisões: v1.

Para testar a resiliência, injete um atraso de 7 s entre os comentários: v2 e microsserviços de classificação para o usuário jason. Este teste irá descobrir um bug que foi intencionalmente introduzido no aplicativo Bookinfo.

```
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml

kubectl get virtualservice ratings -o yaml
```

O subset é definido como v1 no padrão e adicionado 7s de atraso para toda a solicitação, se o usuário logado for compatível com 'jason' para classificações.

```
spec:
  hosts:
  - ratings
  http:
  - fault:
      delay:
        fixedDelay: 7s
        percent: 100
    match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: ratings
        subset: v1
  - route:
    - destination:
        host: ratings
        subset: v1
```

Logout e clique em **Sign in** no canto superior direito da página, usando **jason** como o nome de usuário com uma senha em branco. Você verá os atrasos e o erro de exibição será exibido nos comentários. Outros verão comentários sem erros.

O tempo limite entre a página de produto e o serviço de comentários é de 6 segundos - codificado como 3s 1 para 6s no total.

Para testar outra resiliência, introduza um abort de HTTP nos microsserviços de classificações para o usuário de teste jason. A página exibirá imediatamente “<span style="color:orange">*Ratings service is currently unavailable*</span>”

```
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-abort.yaml

kubectl get virtualservice ratings -o yaml
```

O subset é definido como v1 e, por padrão, retorna uma mensagem de erro de"Ratings service is currently unavailable" abaixo do nome do revisor se o nome de usuário registrado corresponder 'jason'.

```
spec:
  hosts:
  - ratings
  http:
  - fault:
      abort:
        httpStatus: 500
        percent: 100
    match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: ratings
        subset: v1
  - route:
    - destination:
        host: ratings
        subset: v1
```

Para testar, clique **Sign in** no canto superior direito da página e faça o login usando **jason** para o nome de usuário com uma senha em branco. Como **jason** você verá a mensagem de erro. Outros (não logados como **jason**) não verão mensagem de erro.

Em seguida, demonstraremos como migrar gradualmente o tráfego de uma versão de um microsserviço para outro. Em nosso exemplo, vamos enviar <span style="color:orange">50% de tráfego para reviews:v1</span>e <span style="color:blue">50% para reviews:v3</span>.

```
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml

kubectl get virtualservice reviews -o yaml
```

O subset está definido para 50% do tráfego para v1 e 50% do tráfego para v3 para todos os pedidos de comentários.
```
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 50
    - destination:
        host: reviews
        subset: v3
      weight: 50
```

Para testá-lo, atualize seu navegador várias vezes, e você verá apenas reviews:v1 e reviews:v3.
