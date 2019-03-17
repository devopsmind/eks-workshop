---
title: "Implantar o Jenkins"
date: 2018-08-07T08:30:11-07:00
weight: 20
draft: true
---

Com nossa Classe de Armazenamento configurada, precisamos criar nossa configuração `jenkins`. Para fazer isso, vamos apenas usar o `helm` cli com alguns flags.

{{% notice info %}}
Em um sistema de produção, você deve estar usando um arquivo `values.yaml` para poder gerenciar o desvio à medida que precisar atualizar as versões
{{% /notice %}}

#### Instalar o Jenkins

```
helm install stable/jenkins --set rbac.install=true --name cicd
```

A saída desse comando fornecerá algumas informações adicionais, como a senha `admin` e a maneira de obter o nome do host do ELB que foi provisionado.

Vamos dar um tempo para provisionar e, enquanto isso, vamos observar os pods de inicialização.

```
kubectl get pods -w
```

Você deveria ver os pods no status `init`, `pending` ou `running`.

Uma vez que isso mude para `running`, podemos obter o endereço do `load balancer`.

```
export SERVICE_IP=$(kubectl get svc --namespace default cicd-jenkins --template "{{ range (index .status.loadBalancer.ingress 0) }}{{ . }}{{ end }}")
echo http://$SERVICE_IP:8080/login
```
