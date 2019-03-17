---
title: "Crie o ConfigMap"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

#### Introdução
[ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) permite dissociar artefatos de configuração e secrets do conteúdo da imagem para manter os aplicativos em contêiner portáteis. Usando o ConfigMap, você pode controlar independentemente a configuração do MySQL. 

#### Crie o ConfigMap
Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/templates
wget https://eksworkshop.com/statefulset/configmap.files/mysql-configmap.yml

```
Verifique a configuração do arquivo mysql-configmap.yml, seguindo o comando.
```
cat ~/environment/templates/mysql-configmap.yml
```
ConfigMap armazena master.cnf, slave.cnf e passa a inicializar os pods master e slave definidos no statefulset. **master.cnf** é para o pod master do MySQL que possui opção de log binário(log-bin) para fornece um registro das alterações de dados a serem enviadas para servidores slaves **slave.cnf** com pods slaves que tem opção super-read-only .
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-config
  labels:
    app: mysql
data:
  master.cnf: |
    # Apply this config only on the master.
    [mysqld]
    log-bin
  slave.cnf: |
    # Apply this config only on slaves.
    [mysqld]
    super-read-only
```

Crie o configmap 'mysql-config' seguindo o comando.
```
kubectl create -f ~/environment/templates/mysql-configmap.yml
```

{{%attachments title="Related files" pattern=".yml"/%}}
