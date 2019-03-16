---
title: "Instalar o CLI do Helm"
date: 2018-08-07T08:30:11-07:00
weight: 5
---

Antes de começarmos a configurar `helm` precisaremos primeiro instalar as ferramentas de linha de comando com as quais você irá interagir. Para fazer isso, execute o seguinte.

```
cd ~/environment

curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh

chmod +x get_helm.sh

./get_helm.sh
```

{{% notice info %}}
Depois de instalar o helm, o comando solicitará que você execute 'helm init'. **Do not run 'helm init'.** Siga as instruções para configurar o helm usando **Kubernetes RBAC** e, em seguida, instale o helm como especificado abaixo Se você acidentalmente executar 'helm init', você pode desinstalar com segurança o helm executando 'helm reset --force'
{{% /notice %}}

### Configurar o acesso ao Helm com o RBAC

Helm conta com um serviço chamado **tiller** que requer permissão especial no cluster do kubernetes, então precisamos construir um _**Service Account**_ para o **tiller** to use. We'll then apply this to the cluster.

Para criar um novo manifesto de conta de serviço:
```
cat <<EoF > ~/environment/rbac.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EoF
```

Em seguida, aplique a configuração:
```
kubectl apply -f ~/environment/rbac.yaml
```

Então podemos instalar **helm** usando a ferramenta helm **helm** 

```
helm init --service-account tiller
```

Isso instalará o  **tiller** no cluster, que lhe dá acesso para gerenciar recursos em seu cluster.
