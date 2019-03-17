---
title: "Limpar o cluster do CloudFormation"
date: 2018-08-07T12:37:34-07:00
weight: 10
draft: true
---
Para limpar os recursos da sua conta da AWS criada por este workshop.
Run the following commands:

Remove the Worker nodes from EKS:
```
aws cloudformation delete-stack --stack-name "eksworkshop-cf-worker-nodes"
```
Exclua o cluster EKS:
```
aws eks delete-cluster --name "eksworkshop-cf"
```
Confirme se o cluster foi excluído antes de remover o cluster VPC:
```
aws eks describe-cluster --name eksworkshop-cf --query "cluster.status"
```
Remova o Cluster VPC:
```
aws cloudformation delete-stack --stack-name "eksworkshop-cf"
```
Remover políticas associadas a Role do IAM:
```
aws iam detach-role-policy --role-name "eks-service-role-workshop" --policy-arn "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
aws iam detach-role-policy --role-name "eks-service-role-workshop" --policy-arn "arn:aws:iam::aws:policy/AmazonEKSServicePolicy"
```
Remova a Role IAM criada para o cluster EKS:
```
aws iam delete-role --role-name "eks-service-role-workshop"
```
