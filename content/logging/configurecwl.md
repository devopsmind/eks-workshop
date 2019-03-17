---
title: "Configurar logs do CloudWatch e Kibana"
date: 2018-08-07T08:30:11-07:00
weight: 40
---

### Configurar Subscrição de Logs do CloudWatch

Os logs do CloudWatch podem ser entregues a outros serviços, como o Amazon Elasticsearch, para processamento personalizado. Isso pode ser obtido assinando um feed em tempo real de eventos de log. Um filtro de assinatura define o padrão de filtro a ser usado para filtrar quais eventos de log são entregues ao Elasticsearch, bem como informações sobre para onde enviar eventos de log correspondentes.

Nesta seção, inscreveremos os eventos de log do CloudWatch do fluxo fluent-cloudwatch do grupo de logs eks/eksworkshop-eksctl. Esse feed será transmitido para o cluster do Elasticsearch.

As instruções originais para isso estão disponíveis em:

http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_ES_Stream.html

Crie a role de execução básica do Lambda

```
cat <<EoF > ~/environment/iam_policy/lambda.json
{
   "Version": "2012-10-17",
   "Statement": [
   {
     "Effect": "Allow",
     "Principal": {
        "Service": "lambda.amazonaws.com"
     },
   "Action": "sts:AssumeRole"
   }
 ]
}
EoF

aws iam create-role --role-name lambda_basic_execution --assume-role-policy-document file://~/environment/iam_policy/lambda.json

aws iam attach-role-policy --role-name lambda_basic_execution --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

Vá ao [console de logs CloudWatch](https://console.aws.amazon.com/cloudwatch/home?#logs:)

Selecione o grupo de log `/eks/eksworkshop-eksctl/containers`. Clique em `Actions` e selecione` Stream to Amazon ElasticSearch Service`.
![Stream to ElasticSearch](/images/logging_cwl_es.png)

Selecione o cluster do ElasticSearch `kubernetes-logs` e a role do IAM `lambda_basic_execution`

![Subscribing to logs](/images/logging-cloudwatch-es-subscribe-iam.png)

Clique em 'Next'

Selecione `Common Log Format` e clique em` Next`

![ES Log Format](/images/logging-cloudwatch-es-subscribe-log-format.png)

Revise a configuração. Clique em `Next` e depois em 'Start Streaming'

![Review ES Subscription](/images/logging-cloudwatch-es-subscribe-confirmation.png)

A página do Cloudwatch é atualizada para mostrar que o filtro foi criado com sucesso

### Configurar o Kibana

No console do Amazon Elasticsearch, selecione o [kubernetes-logs sob Meus domínios](https://console.aws.amazon.com/es/home?#domain:resource=kubernetes-logs;action=dashboard)

![Detalhes do ElasticSearch](/images/logging_es_details.png)

Abra o dashboard do Kibana no link. Após alguns minutos, os logs começarão a ser indexados pelo ElasticSearch. Você precisará configurar um padrão de índice no Kibana.

Configure `Index Pattern` como **cwl-\*** e clique `Next`

![Index Pattern](/images/logging_index_pattern.png)

Select `@timestamp` from the dropdown list and select `Create index pattern`

![Index Pattern](/images/logging_time_filter.png)

![Kibana Summary](/images/logging_kibana.png)

Clique em 'Discover' e explore seus logs

![Kibana Dashboard](/images/logging_kibana_dashboard.png)
