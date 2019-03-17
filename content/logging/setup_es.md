---
title: "Provisionar um cluster do Elasticsearch"
date: 2018-08-07T08:30:11-07:00
weight: 20
---

Este exemplo cria um cluster de duas instâncias do Amazon Elasticsearch chamado kubernetes-logs. Esse cluster é criado na mesma região que o cluster do Kubernetes e o grupo de log do CloudWatch. 

{{% notice warning %}}
Observe que esse cluster tem uma política de acesso aberto que precisa ser bloqueada em ambientes de produção.
{{% /notice %}}

```
aws es create-elasticsearch-domain \
  --domain-name kubernetes-logs \
  --elasticsearch-version 6.3 \
  --elasticsearch-cluster-config \
  InstanceType=m4.large.elasticsearch,InstanceCount=2 \
  --ebs-options EBSEnabled=true,VolumeType=standard,VolumeSize=100 \
  --access-policies '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"AWS":["*"]},"Action":["es:*"],"Resource":"*"}]}'
```

Demora um pouco para o cluster ser criado e chegar a um estado ativo. O AWS Console deve mostrar o seguinte status quando o cluster estiver pronto.

![Elasticsearch Dashboard](/images/logging_es_dashboard.png)

Você também pode verificar isso via AWS CLI:
```
aws es describe-elasticsearch-domain --domain-name kubernetes-logs --query 'DomainStatus.Processing'
```
Se o valor de saída for falso, significa que o domínio foi processado e agora está disponível para uso.

Sinta-se à vontade para seguir para a próxima seção por enquanto.
