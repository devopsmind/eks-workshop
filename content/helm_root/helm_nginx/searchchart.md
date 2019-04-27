---
title: "Pesquisar o repositório de charts"
date: 2018-08-07T08:30:11-07:00
weight: 200
---

Agora que a nossa lista de charts do repositório foi atualizada, podemos [procurar por charts](https://docs.helm.sh/helm/#helm-search).

Para listar todos de Charts:

```
helm search
```

Isso deve resultar em algo parecido com:

```
NAME                                    CHART VERSION   APP VERSION                     DESCRIPTION                                                 
stable/acs-engine-autoscaler            2.2.0           2.1.1                           Scales worker...
stable/aerospike                        0.1.7           v3.14.1.2                       A Helm chart...
...
```

Você pode ver na saída que ele despejou a lista de todos os charts que ele conhece. Em alguns casos, isso pode ser útil, mas uma pesquisa ainda mais útil envolveria um argumento de palavra-chave. Então, em seguida, vamos procurar apenas por NGINX:

```
helm search nginx
```

Isso resulta em:

```
NAME                            CHART VERSION   APP VERSION     DESCRIPTION                                                 
stable/nginx-ingress            0.31.0          0.20.0          An nginx Ingress ...
stable/nginx-ldapauth-proxy     0.1.2           1.13.5          nginx proxy ...
stable/nginx-lego               0.3.1                           Chart for...
stable/gcloud-endpoints         0.1.2           1               DEPRECATED Develop...
...
```

Esta nova lista de Charts é específica para nginx, porque passamos o argumento **nginx** para o comando de pesquisa.
