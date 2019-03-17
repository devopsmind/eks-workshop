---
title: "Implantar Fluentd"
date: 2018-08-07T08:30:11-07:00
weight: 30
---

```
mkdir ~/environment/fluentd
cd ~/environment/fluentd
wget https://eksworkshop.com/logging/deploy.files/fluentd.yml
```
Explore o fluentd.yml para ver o que está sendo implantado. Existe um link na parte inferior desta página. A configuração do agente de log do Fluentd está localizada no Kubernetes ConfigMap. O Fluentd será implantado como um DaemonSet, ou seja, um pod por work node. No nosso caso, um cluster de 3 node é usado e, portanto, serão exibidos 3 pods na saída quando implantarmos.

{{% notice warning %}}
Atualize a REGION no fluentd.yml conforme necessário. Está definido para us-west-2 por padrão.
{{% /notice %}}

```
kubectl apply -f ~/environment/fluentd/fluentd.yml
```

Cuidado com todos os pods para mudar para o status running

```
kubectl get pods -w --namespace=kube-system
```

Agora estamos prontos para verificar se os logs estão chegando [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/home?#logStream:group=/eks/eksworkshop-eksctl/containers)

Selecione a região mencionada em fluentd.yml para procurar o Grupo de Logs do Cloudwatch, se necessário.

{{%attachments title="Related files" pattern=".yml"/%}}
