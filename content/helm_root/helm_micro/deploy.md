---
title: "Implantar o eksdemo Chart"
date: 2018-08-07T08:30:11-07:00
weight: 30
---

#### Use a flag dry-run  para testar nossos templates

Para testar a sintaxe e a validade do chart sem realmente implementá-lo, usaremos a flag dry-run .

O comando a seguir criará e exibirá os templates renderizados sem instalar oChart:

```sh
helm install --debug --dry-run --name workshop ~/environment/eksdemo
```
Confirme se os valores criados pelo template estão corretos.


#### Implante o chart
Agora que testamos nosso template, vamos instalá-lo.
```
helm install --name workshop ~/environment/eksdemo
```

Semelhante ao que vimos anteriormente no [NGINX Helm Chart example](/helm_root/helm_nginx/index.html), uma saída dos objetos Deployment, Pod e Service é gerada, semelhante a essa:

```
NAME:   workshop
LAST DEPLOYED: Fri Nov 16 21:42:00 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Service
NAME              AGE
ecsdemo-crystal   0s
ecsdemo-frontend  0s
ecsdemo-nodejs    0s

==> v1/Deployment
ecsdemo-crystal   0s
ecsdemo-frontend  0s
ecsdemo-nodejs    0s

==> v1/Pod(related)

NAME                               READY  STATUS             RESTARTS  AGE
ecsdemo-crystal-764b9cb9bc-4dwqt   0/1    ContainerCreating  0         0s
ecsdemo-crystal-764b9cb9bc-hcb62   0/1    ContainerCreating  0         0s
ecsdemo-crystal-764b9cb9bc-vl7nr   0/1    ContainerCreating  0         0s
ecsdemo-frontend-67876457f6-2xrtb  0/1    ContainerCreating  0         0s
ecsdemo-frontend-67876457f6-bfnc5  0/1    ContainerCreating  0         0s
ecsdemo-frontend-67876457f6-rb6rg  0/1    ContainerCreating  0         0s
ecsdemo-nodejs-c458bf55d-994cq     0/1    ContainerCreating  0         0s
ecsdemo-nodejs-c458bf55d-9qtbm     0/1    ContainerCreating  0         0s
ecsdemo-nodejs-c458bf55d-s9zkh     0/1    ContainerCreating  0         0s

```
