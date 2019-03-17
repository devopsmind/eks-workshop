---
title: "Teste o cluster"
date: 2018-08-07T13:36:57-07:00
draft: true
weight: 30
---

Confirme seu Nodes:

```bash
kubectl get nodes
```

Exportar o nome da função do Worker para uso em todo o workshop

```bash
INSTANCE_PROFILE_PREFIX=$(aws cloudformation describe-stacks | jq -r .Stacks[].StackName | grep eksctl-eksworkshop-eksctl-nodegroup)
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep $INSTANCE_PROFILE_PREFIX)
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
echo "export ROLE_NAME=${ROLE_NAME}" >> ~/.bash_profile

```

#### Parabéns!

Agora você tem um cluster do Amazon EKS totalmente funcional pronto para uso!
