---
title: "Atualizar as configurações do IAM para sua área de trabalho"
chapter: false
weight: 30
---

{{% notice info %}}
O Cloud9 normalmente gerencia as credenciais do IAM dinamicamente. No momento, isso não é compatível com o plug-in aws-iam-authenticator, por isso, vamos desativá-lo e confiar na role do IAM.
{{% /notice %}}

- Retorne ao seu espaço de trabalho e clique na roda dentada ou inicie uma nova guia para abrir a guia Preferências.
- Selecione **AWS SETTINGS**
- Desligar **A AWS gerencia credenciais temporárias**
- Feche a aba Preferências
![c9disableiam](/images/c9disableiam.png)

- Para garantir que as credenciais temporárias ainda não estejam em vigor, também removeremos qualquer arquivo de credenciais existente:
```
rm -vf ${HOME}/.aws/credentials
```

- Nós devemos configurar nosso aws cli com nossa região atual como padrão:
```
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r .region)
echo "export AWS_REGION=${AWS_REGION}" >> ~/.bash_profile
aws configure set default.region ${AWS_REGION}
aws configure get default.region
```

### Validar a Role do IAM

Use o comando da CLI [GetCallerIdentity](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html)  para validar se o Cloud9 IDE está usando o IAM role.

Primeiro, obtenha o nome da Role do IAM na AWS CLI.
```bash
INSTANCE_PROFILE_NAME=`basename $(aws ec2 describe-instances --filters Name=tag:Name,Values=aws-cloud9-${C9_PROJECT}-${C9_PID} | jq -r '.Reservations[0].Instances[0].IamInstanceProfile.Arn' | awk -F "/" "{print $2}")`
aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME --query "InstanceProfile.Roles[0].RoleName" --output text
```

A saída é o nome da role.

```output
modernizer-workshop-cl9
```

Compare isso com o resultado de

```bash
aws sts get-caller-identity
```

#### VALID

Se o _Arn_ contiver o nome da role acima e um ID da instância, você poderá continuar.

```output
{
    "Account": "123456789012", 
    "UserId": "AROA1SAMPLEAWSIAMROLE:i-01234567890abcdef", 
    "Arn": "arn:aws:sts::123456789012:assumed-role/modernizer-workshop-cl9/i-01234567890abcdef"
}
```

#### INVALID

Se o _Arn contiver `TeamRole`, `MasterRole`, ou não corresponde ao nome da role, <span style="color: red;">**DO NOT PROCEED**</span>. Volte e confirme as etapas nesta página.

```output
{
    "Account": "123456789012", 
    "UserId": "AROA1SAMPLEAWSIAMROLE:i-01234567890abcdef", 
    "Arn": "arn:aws:sts::123456789012:assumed-role/TeamRole/MasterRole"
}
```