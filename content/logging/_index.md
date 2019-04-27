---
title: "Registrando logs com Elasticsearch, Fluentd, and Kibana (EFK)"
chapter: true
weight: 51
---

# Implemente o log com o EFK

Neste capítulo, vamos implantar um padrão de registro em log comum do Kubernetes, que consiste no seguinte:

* [Fluentd](https://www.fluentd.org/) é um coletor de dados de código aberto que fornece uma camada de registro unificada, suportada por 500 plugins conectados a vários tipos de sistemas.
* [Elasticsearch](https://www.elastic.co/products/elasticsearch) é um mecanismo de pesquisa e análise distribuído e RESTful.
* [Kibana](https://www.elastic.co/products/kibana) permite visualizar seus dados do Elasticsearch.

Juntos, Fluentd, Elasticsearch e Kibana também são conhecidos como “stack” EFK . O Fluentd encaminhará os logs das instâncias individuais no cluster para um back-end de log centralizado (CloudWatch Logs), onde eles serão combinados para relatórios de nível mais alto usando o ElasticSearch e o Kibana.
