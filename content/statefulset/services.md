---
title: "Criar serviços"
date: 2018-08-07T08:30:11-07:00
weight: 15
---
#### Introdução
[Serviço Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/) define um conjunto lógico de pods e uma política pela qual acessá-los. O serviço pode ser exposto de diferentes formas, especificando um [tipo](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/) no serviceSpec. StatefulSet atualmente requer um [Headless Service](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services) para controlar o domínio de seus Pods, alcançar diretamente cada Pod com entradas de DNS estáveis. Especificando  **"None"** para o clusterIP, você pode criar o serviço Headless.
#### Criar serviços
Copie/Cole os seguintes comandos no seu terminal Cloud9.
```
cd ~/environment/templates
wget https://eksworkshop.com/statefulset/services.files/mysql-services.yml
```
Verifique a configuração do mysql-services.yml, com o seguinte comando.
```
cat ~/environment/templates/mysql-services.yml
```
Você pode ver que o serviço **mysql** é para resolução de DNS, de modo que quando os pods são colocados pelo controlador StatefulSet, pods podem ser resolvidos usando pod-name.mysql. **mysql-read** é um serviço de cliente que faz o balanceamento de carga para todos slaves. 
```
# Serviço Headless de DNS estáveis ​​de membros do StatefulSet.
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  ports:
  - name: mysql
    port: 3306
  clusterIP: None
  selector:
    app: mysql
---
# Serviço de cliente para conectar-se a qualquer instância do MySQL para leituras.
# Para gravações, você deve se conectar ao master: mysql-0.mysql.
apiVersion: v1
kind: Service
metadata:
  name: mysql-read
  labels:
    app: mysql
spec:
  ports:
  - name: mysql
    port: 3306
  selector:
    app: mysql
```
Criar serviço mysql e mysql-read com o seguinte comando
```
kubectl create -f ~/environment/templates/mysql-services.yml
```
{{%attachments title="Related files" pattern=".yml"/%}}
