---
title: "Modificar aws-auth ConfigMap"
date: 2018-10-087T08:30:11-07:00
weight: 11
draft: false
---

Agora que temos o papel do IAM criado, vamos adicionar o role ao [aws-auth ConfigMap](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html)
para o cluster EKS.

Depois que o ConfigMap incluir essa nova role, o kubectl no estágio CodeBuild do pipeline poderá interagir com o cluster EKS por meio da role IAM.

```
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

ROLE="    - rolearn: arn:aws:iam::$ACCOUNT_ID:role/EksWorkshopCodeBuildKubectlRole\n      username: build\n      groups:\n        - system:masters"

kubectl get -n kube-system configmap/aws-auth -o yaml | awk "/mapRoles: \|/{print;print \"$ROLE\";next}1" > /tmp/aws-auth-patch.yml

kubectl patch configmap/aws-auth -n kube-system --patch "$(cat /tmp/aws-auth-patch.yml)"
```


{{% notice tip %}}
Se você gostaria de editar o aws-auth ConfigMap manualmente, você pode executar: $ kubectl edit -n kube-system configmap/aws-auth
{{% /notice %}}

