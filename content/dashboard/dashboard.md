---
title: "Implantar o Dashboard oficial do Kubernetes"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

O Dashboard oficial do Kubernetes não é implantado por padrão, mas há instruções em [a documentação oficial](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

Podemos implantar o Dashboard com o seguinte comando:
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

Como isso é implantado em nosso cluster privado, precisamos acessá-lo por meio de um proxy.
O Kube-proxy está disponível para fazer proxy de nossos pedidos para o serviço do dashboard.  No seu
workspace, execute o seguinte comando:
```
kubectl proxy --port=8080 --address='0.0.0.0' --disable-filter=true &
```

Isso iniciará o proxy, escutará na porta 8080, escutará todas as interfaces e desativará a filtragem de solicitações não localhost.

Este comando continuará sendo executado em segundo plano na sessão do terminal atual.

{{% notice warning %}}
Estamos desativando a filtragem de solicitações, um recurso de segurança que protege contra ataques XSRF.
Isso não é recomendado para um ambiente de produção, mas é útil para nosso ambiente de desenvolvimento.
{{% /notice %}}
