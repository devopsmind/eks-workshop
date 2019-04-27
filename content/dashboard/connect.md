---
title: "Acesse o Dashboard"
date: 2018-08-07T08:30:11-07:00
weight: 30
---

Agora podemos acessar o Dashboard do Kubernetes

1. Em seu ambiente Cloud9, clique **Preview / Preview Running Application**
1. Vá até **the end of the URL** e acrescente:

```
/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
```

Abra uma nova guia Terminal e digite
```
aws-iam-authenticator token -i eksworkshop-eksctl --token-only
```

Copie a saída deste comando e, em seguida, *click* o botão em formato radio ao lado
*Token* em seguida, no campo de texto abaixo, cole a saída do último comando.

![Token page](/images/dashboard-connect.png)

Então aperte *Sign In*.

Se você quiser ver o Dashboard em uma guia completa, clique no botão **Pop Out** , como abaixo:
![popout](/images/popout.png)
