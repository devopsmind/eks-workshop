---
title: "Create Kubeconfig File"
date: 2018-08-07T13:07:50-07:00
weight: 40
draft: false
---
Agora que temos o cluster em execução, precisamos criar o arquivo KubeConfig que será usado para gerenciar o cluster.

O módulo terraform armazena as informações do kubeconfig em seu armazenamento de estado.
Podemos visualizá-lo com este comando:
```
terraform output kubeconfig
```
E podemos salvá-lo para uso com este comando:
```
terraform output kubeconfig > ${HOME}/.kube/config-eksworkshop-tf
```

Agora precisamos adicionar essa nova configuração à lista de configuração do KubeCtl:
```
export KUBECONFIG=${HOME}/.kube/config-eksworkshop-tf:${HOME}/.kube/config
echo "export KUBECONFIG=${KUBECONFIG}" >> ${HOME}/.bashrc
```
