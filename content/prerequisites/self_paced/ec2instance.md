---
title: "Anexe a role do IAM ao seu Workspace"
chapter: false
weight: 26
---

1. Siga [este link direto para encontrar sua instância do Cloud9 EC2](https://console.aws.amazon.com/ec2/v2/home?#Instances:tag:Name=aws-cloud9-eksworkshop*;sort=desc:launchTime)
1. Selecione a instância e escolha **Actions / Instance Settings / Attach/Replace IAM Role**
![c9instancerole](/images/c9instancerole.png)
1. Choose **eksworkshop-admin** from the **IAM Role** drop down, and select **Apply**
![c9attachrole](/images/c9attachrole.png)
