---
title: "Limpar"
date: 2018-08-07T08:30:11-07:00
weight: 500
---

Para remover todos os objetos que o Helm Chart criou, podemos usar [Helm delete](https://docs.helm.sh/helm/#helm-delete).

Antes de excluí-lo, podemos verificar o que temos em execução por meio do [Helm list](https://docs.helm.sh/helm/#helm-list) command:

```
helm list
```

Você deve ver uma saída semelhante à abaixo, que mostra que o mywebserver está instalado:

```
NAME            REVISION        UPDATED                         STATUS          CHART           APP VERSION     
mywebserver     1               Tue Nov 13 19:55:25 2018        DEPLOYED        nginx-1.1.2     1.14.1          
```

Foi muito divertido; Tivemos ótimos momentos enviando HTTP para frente e para trás, mas agora é hora de excluir essa implantação. Deletar:

```
helm delete --purge mywebserver
```

E você deve encontrar a saída:

```
release "mywebserver" deleted
```

O kubectl também demonstrará que nossos pods e serviços não estão mais disponíveis:

```
kubectl get pods -l app=mywebserver-nginx
kubectl get service mywebserver-nginx -o wide
```

Como tentaria acessar o serviço através do navegador da web através de uma recarga de página.

Com isso, a limpeza está completa.
