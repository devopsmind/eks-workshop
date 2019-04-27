---
title: "Configure Cluster Autoscaler (CA)"
date: 2018-08-07T08:30:11-07:00
weight: 30
---
Autoscaler do Cluster,  A AWS fornece integração com grupos do Auto Scaling. Ele permite que os usuários escolham entre quatro opções diferentes de deployment:

* **One Auto Scaling group** - This is what we will use
* Multiple Auto Scaling groups
* Auto-Discovery
* Master Node setup

### Configure the Cluster Autoscaler (CA)
We have provided a manifest file to deploy the CA. Copy the commands below into your Cloud9 Terminal.

```
mkdir ~/environment/cluster-autoscaler
cd ~/environment/cluster-autoscaler
wget https://eksworkshop.com/scaling/deploy_ca.files/cluster_autoscaler.yml
```

### Configure the ASG
We will need to provide the name of the Autoscaling Group that we want CA to manipulate. Collect the name of the Auto Scaling Group (ASG) containing your worker nodes. Record the name somewhere. We will use this later in the manifest file.

You can find it in the console by following this [link](https://console.aws.amazon.com/ec2/autoscaling/home?#AutoScalingGroups:id=eksctl-eksworkshop-eksctl-nodegroup-0-NodeGroup-SQG8QDVSR73G;view=details;filter=eksworkshop).

![ASG](/images/scaling-asg.png)

Check the box beside the ASG and click `Actions` and `Edit`

Change the following settings:

* Min: **2**
* Max: **8**

![ASG Config](/images/scaling-asg-config.png)

Click `Save`

### Configurar o autoescalador do cluster

Usando o navegador de arquivos à esquerda, abra cluster_autoscaler.yml

Procure por `command:` e dentro deste bloco, substitua o texto do placeholder `<AUTOSCALING GROUP NAME>` com o nome do ASG que você copiou na etapa anterior. Além disso, atualize o valor AWS_REGION para refletir a região que você está usando e **Salve ** o arquivo.

```
command:
  - ./cluster-autoscaler
  - --v=4
  - --stderrthreshold=info
  - --cloud-provider=aws
  - --skip-nodes-with-local-storage=false
  - --nodes=2:8:eksctl-eksworkshop-eksctl-nodegroup-0-NodeGroup-SQG8QDVSR73G
env:
  - name: AWS_REGION
    value: us-east-1
```
Este comando contém toda a configuração do Autoescalador do cluster. A configuração principal é a flag `--nodes` . Isso especifica o mínimo de  nodes **(2)**, máximo de nodes **(8)** e **Nome do ASG**.

Embora o Autoescalador de cluster seja o padrão de fato para o dimensionamento automático em K8s, não faz parte do release principal. Nós o implantamos como qualquer outro pod no namespac do  kube-system, semelhante a outros pods de gerenciamento.

### Crie uma política do IAM
Precisamos configurar uma política embutida e adicioná-la ao perfil da instância do EC2 do worker nodes

Colete o perfil da instância e o NOME da role na stack do CloudFormation
```
INSTANCE_PROFILE_PREFIX=$(aws cloudformation describe-stacks | jq -r .Stacks[].StackName | grep eksctl-eksworkshop-eksctl-nodegroup)
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep $INSTANCE_PROFILE_PREFIX)
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
```
```
mkdir ~/environment/asg_policy
cat <<EoF > ~/environment/asg_policy/k8s-asg-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeAutoScalingInstances",
        "autoscaling:SetDesiredCapacity",
        "autoscaling:TerminateInstanceInAutoScalingGroup"
      ],
      "Resource": "*"
    }
  ]
}
EoF
aws iam put-role-policy --role-name $ROLE_NAME --policy-name ASG-Policy-For-Worker --policy-document file://~/environment/asg_policy/k8s-asg-policy.json
```

Valide se a política está anexada a role
```
aws iam get-role-policy --role-name $ROLE_NAME --policy-name ASG-Policy-For-Worker
```

### Implantar o autoescalador do cluster

```
kubectl apply -f ~/environment/cluster-autoscaler/cluster_autoscaler.yml
```

Acompanhe os logs
```
kubectl logs -f deployment/cluster-autoscaler -n kube-system
```

#### Agora estamos prontos para escalar nosso cluster

{{%attachments title="Related files" pattern=".yml"/%}}
