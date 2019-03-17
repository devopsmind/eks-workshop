---
title: "Instalar ferramentas do Kubernetes"
chapter: false
weight: 22
---

Os clusters do Amazon EKS requerem binários kubectl e kubelet e o binário aws-iam-authenticator para permitir a autenticação do IAM para seu cluster do Kubernetes.

{{% notice tip %}}
Neste workshop nós lhe daremos os comandos para baixar os binários do Linux. Se você estiver executando o Mac OSX / Windows, por favor [consulte os documentos oficiais do EKS para os links de download.](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)
{{% /notice %}}

#### Crie o diretório ~/.kube padrão para armazenar a configuração do kubectl
```
mkdir -p ~/.kube
```

#### Instalar o kubectl
```
sudo curl --silent --location -o /usr/local/bin/kubectl "https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/linux/amd64/kubectl"
sudo chmod +x /usr/local/bin/kubectl
```

#### Instale o AWS IAM Authenticator
```
go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator
sudo mv ~/go/bin/aws-iam-authenticator /usr/local/bin/aws-iam-authenticator
```

#### Verifique os binários
```
kubectl version --short --client
aws-iam-authenticator help
```

#### Install JQ
```
sudo yum -y install jq
```
