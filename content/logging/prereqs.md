---
title: "Configurar política do IAM para Worker Nodes"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

Nós estaremos implantando o Fluentd como um DaemonSet, ou um pod node por worker node. O Daemon de Log Fluentd irá coletar logs e encaminhar para o CloudWatch Logs.. Isso exigirá que os nodes tenham permissões para enviar logs e criar grupos de log e stream de logs. Isso pode ser feito com um usuário do IAM, uma Role do IAM ou usando uma ferramenta como `Kube2IAM`.

Em nosso exemplo, criaremos uma política do IAM e anexaremos a role do  Worker.

Colete o perfil da instância e o NOME da role na Stack do CloudFormation
```
INSTANCE_PROFILE_PREFIX=$(aws cloudformation describe-stacks | jq -r .Stacks[].StackName | grep eksctl-eksworkshop-eksctl-nodegroup)
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep $INSTANCE_PROFILE_PREFIX)
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
```
Crie uma nova política do IAM e anexe-a a Role do Worker Node.
```
mkdir ~/environment/iam_policy
cat <<EoF > ~/environment/iam_policy/k8s-logs-policy.json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
EoF
aws iam put-role-policy --role-name $ROLE_NAME --policy-name Logs-Policy-For-Worker --policy-document file://~/environment/iam_policy/k8s-logs-policy.json
```

Valide se a política está anexada à role.
```
aws iam get-role-policy --role-name $ROLE_NAME --policy-name Logs-Policy-For-Worker
```
