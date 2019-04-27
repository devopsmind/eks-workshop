---
title: "Implantar aplicativos de exemplo"
date: 2018-11-13T16:37:17+09:00
weight: 40
draft: false
---

Agora que temos todos os recursos instalados para o Istio, usaremos um aplicativo de exemplo chamado BookInfo para analisar os principais recursos da service mesh, como o roteamento inteligente, e analisar os dados de telemetria usando o Prometheus.

### Sample Apps


![Sample Apps](/images/servicemesh-deploy1.png)

O aplicativo Bookinfo é dividido em quatro microsserviços separados:

* <span style="color:orange">**productpage**</span>
  * O microsserviço da página do produto chama os detalhes e revisa microsserviços para preencher a página.

* <span style="color:orange">**details**</span>
  * O microsserviço de detalhes contém informações do livro.

* <span style="color:orange">**reviews**</span>
  * O microserviço de avaliações contém resenhas de livros. Ele também chama o microsserviço de classificações.

* <span style="color:orange">**ratings**</span>
  * O microsserviço de classificações contém informações de classificação de livros que acompanham uma resenha do livro.

Existem 3 versões do <span style="color:orange">*reviews*</span>microsserviço:

* Version v1
  * não chama o serviço de classificações.

* Version v2
  * chama o serviço de classificação e exibe cada classificação como 1 a 5<span style="color:black">**black stars**</span>.

* Version v3
  * chama o serviço de classificação e exibe cada classificação como 1 a 5 <span style="color:red">**red stars**</span>.

---

### Implantar aplicativos de exemplo

Implante aplicativos de exemplo injetando manualmente o istio proxy e confirme os pods, os serviços estão sendo executados corretamente

```
kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml)
```

Para verificar o resultado

```
kubectl get pod,svc
```

Deve ser semelhante a:

```
NAME                              READY     STATUS    RESTARTS   AGE
details-v1-64558cf56b-dxbx2       2/2       Running   0          14s
productpage-v1-5b796957dd-hqllk   2/2       Running   0          14s
ratings-v1-777b98fcc4-5bfr8       2/2       Running   0          14s
reviews-v1-866dcb7ff-k69jm        2/2       Running   0          14s
reviews-v2-6d7959c9d-5ppnc        2/2       Running   0          14s
reviews-v3-7ddf94f545-m7vls       2/2       Running   0          14s

NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
details       ClusterIP   10.100.102.153   <none>        9080/TCP   17s
kubernetes    ClusterIP   10.100.0.1       <none>        443/TCP    138d
productpage   ClusterIP   10.100.222.154   <none>        9080/TCP   17s
ratings       ClusterIP   10.100.1.63      <none>        9080/TCP   17s
reviews       ClusterIP   10.100.255.157   <none>        9080/TCP   17s
```

Em seguida, vamos definir o serviço virtual e o gateway de ingress:

```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

Em seguida, vamos consultar o nome DNS do gateway de ingress e usá-lo para se conectar através do navegador.

```
kubectl get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}' -n istio-system ; echo
```

Isso pode levar um ou dois minutos, primeiro para o Ingress ser criado e, em segundo lugar, para o Ingress conectar-se aos serviços que ele expõe.

Para testar, faça o seguinte:

1. Abra uma nova guia do navegador
2. Cole o endpoint DNS retornado do comando anterior ```get service istiogateway``` 
3. Adicionar /productpage ao final desse DNS endpoint
4. Pressione Enter para recuperar a página.

{{% notice info %}}
Lembre-se de adicionar **/productpage** até o final do URI no navegador para ver a página de exemplo!
{{% /notice %}}

![Sample Apps](/images/servicemesh-deploy2.png)

Clique em recarregar várias vezes para ver como o layout e o conteúdo das revisões são alterados como versões diferentes (v1, v2, v3) do aplicativo são chamados.
