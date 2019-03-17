---
title: "Create the Worker ConfigMap"
date: 2018-08-07T12:00:40-07:00
weight: 70
draft: true
---

O arquivo encontrado no repositório em *assets/worker-configmap.yml* contém um mapa de configuração que podemos usar para nossos funcionários da EKS. Precisamos substituir nossa Role de instância ARN no template:

Veja o template:
```
cd ${HOME}/environment/howto-launch-eks-workshop/

cat assets/worker-configmap.yml
```
Pesquise e armazene o ARN da instância:
```
export INSTANCE_ARN=$(aws cloudformation describe-stacks --stack-name "eksworkshop-cf-worker-nodes" --query "Stacks[0].Outputs[?OutputKey=='NodeInstanceRole'].OutputValue" --output text)

echo INSTANCE_ARN=$INSTANCE_ARN
```

Teste modifique o template para ver quais alterações:
```
sed "s@.*rolearn.*@    - rolearn: $INSTANCE_ARN@" assets/worker-configmap.yml
```
Agota realmente aplique o configmap:
```
sed "s@.*rolearn.*@    - rolearn: $INSTANCE_ARN@" assets/worker-configmap.yml | kubectl apply -f /dev/stdin
```
