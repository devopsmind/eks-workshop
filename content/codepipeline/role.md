---
title: "Criar Role IAM "
date: 2018-10-087T08:30:11-07:00
weight: 10
draft: false
---

Em um CodePipeline da AWS, vamos usar o AWS CodeBuild para implantar um serviço de exemplo do Kubernetes.
Isso requer uma Role [Gerenciamento de identidade e acesso da AWS](https://aws.amazon.com/iam/) (IAM) capaz de interagir com o cluster EKS.

Nesta etapa, criaremos uma role do IAM e adicionaremos uma política in-line que usaremos no estágio CodeBuild para interagir com o cluster EKS por meio do kubectl.

Crie o Role:

```
cd ~/environment

ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

TRUST="{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Effect\": \"Allow\", \"Principal\": { \"AWS\": \"arn:aws:iam::$ACCOUNT_ID:root\" }, \"Action\": \"sts:AssumeRole\" } ] }"

echo '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "eks:Describe*", "Resource": "*" } ] }' > /tmp/iam-role-policy

aws iam create-role --role-name EksWorkshopCodeBuildKubectlRole --assume-role-policy-document "$TRUST" --output text --query 'Role.Arn'

aws iam put-role-policy --role-name EksWorkshopCodeBuildKubectlRole --policy-name eks-describe --policy-document file:///tmp/iam-role-policy
```
