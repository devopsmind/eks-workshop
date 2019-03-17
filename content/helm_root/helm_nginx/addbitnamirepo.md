---
title: "Adicione o repositório Bitnami"
date: 2018-08-07T08:30:11-07:00
weight: 300
---

No último slide, vimos que o NGINX oferece muitos produtos diferentes através do repositório Helm Chart padrão, mas o servidor web autônomo NGINX não é um deles.

Depois de uma rápida pesquisa na web, descobrimos que existe um gráfico para o servidor da Web autônomo NGINX disponível por meio do [repositório Chart Bitnami](https://github.com/bitnami/charts).

Para adicionar o repositório do Bitnami Chart à nossa lista local de gráficos pesquisáveis:

```
adicionar repositório helm bitnami https://charts.bitnami.com/bitnami
```

Depois disso, podemos pesquisar todos Bitnami Charts:

```
helm search bitnami
```

O que resulta em:

```
NAME                                    CHART VERSION   APP VERSION             DESCRIPTION                                                 
bitnami/bitnami-common                  0.0.3           0.0.1                   Chart with...        
bitnami/apache                          2.1.2           2.4.37                  Chart for Apache...                              
bitnami/cassandra                       0.1.0           3.11.3                  Apache Cassandra...
...
```

Procure novamente pelo NGINX:

```
helm search nginx
```

Agora estamos vendo mais opções do NGINX em todos os repositórios:

```
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                                                 
bitnami/nginx                           1.1.2           1.14.1          Chart for the nginx server                                  
bitnami/nginx-ingress-controller        2.1.4           0.20.0          Chart for the nginx Ingress...                    
stable/nginx-ingress                    0.31.0          0.20.0          An nginx Ingress controller ...
```

Ou até mesmo pesquise o repositório Bitnami, apenas para NGINX:

```
helm search bitnami/nginx
```

O que limita isso ao NGINX do Bitnami:

```
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                           
bitnami/nginx                           1.1.2           1.14.1          Chart for the nginx server            
bitnami/nginx-ingress-controller        2.1.4           0.20.0          Chart for the nginx Ingress...
```

Em ambas as duas últimas pesquisas, vemos

```
bitnami/nginx
```

como um resultado de pesquisa. Esse é o que estamos procurando, então vamos usar o Helm para instalá-lo no cluster EKS.
