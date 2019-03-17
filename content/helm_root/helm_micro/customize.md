---
title: "Personalizar Padrões"
date: 2018-08-07T08:30:11-07:00
weight: 20
---

Se você olhar no diretório **eksdemo** recém-criado, verá vários arquivos e diretórios. Especificamente, dentro do diretório /templates, você verá:

* NOTES.txt: O 'texto de ajuda' do seu chart. Isso será exibido para seus usuários quando eles executarem a instalação do helm.
* deployment.yaml: Um manifesto básico para criar uma implantação do Kubernetes
* service.yaml: Um manifesto básico para criar um terminal de serviço para seu deploy
* _helpers.tpl: Um lugar para colocar template helpers que você pode reutilizar em todo o chart

Na verdade, vamos criar nossos próprios arquivos, então vamos excluir esses arquivos clichês
```
rm -rf ~/environment/eksdemo/templates/
rm ~/environment/eksdemo/Chart.yaml
rm ~/environment/eksdemo/values.yaml
```
Crie um novo arquivo Chart.yaml que irá descrever o Chart
```
cat <<EoF > ~/environment/eksdemo/Chart.yaml
apiVersion: v1
appVersion: "1.0"
description: A Helm chart for EKS Workshop Microservices application
name: eksdemo
version: 0.1.0
EoF
```

Em seguida, copiaremos os arquivos de manifesto de cada um dos nossos microservices no diretório de templates *servicename*.yaml
```
#criar subpastas para cada tipo de template
mkdir -p ~/environment/eksdemo/templates/deployment
mkdir -p ~/environment/eksdemo/templates/service

# Copiar e renomear manifestos do frontend
cp ~/environment/ecsdemo-frontend/kubernetes/deployment.yaml ~/environment/eksdemo/templates/deployment/frontend.yaml
cp ~/environment/ecsdemo-frontend/kubernetes/service.yaml ~/environment/eksdemo/templates/service/frontend.yaml

# Copiar e renomear manifestos de cristal
cp ~/environment/ecsdemo-crystal/kubernetes/deployment.yaml ~/environment/eksdemo/templates/deployment/crystal.yaml
cp ~/environment/ecsdemo-crystal/kubernetes/service.yaml ~/environment/eksdemo/templates/service/crystal.yaml

# Copie e renomeie os manifestos de nodejs
cp ~/environment/ecsdemo-nodejs/kubernetes/deployment.yaml ~/environment/eksdemo/templates/deployment/nodejs.yaml
cp ~/environment/ecsdemo-nodejs/kubernetes/service.yaml ~/environment/eksdemo/templates/service/nodejs.yaml
```

Todos os arquivos no diretório de templates são enviados pelo mecanismo de template. Atualmente, esses arquivos YAML simples são enviados ao Kubernetes no estado em que se encontram.

#### Substituir valores codificados por diretivas de template
Vamos substituir alguns dos valores por 'diretivas de template' para permitir mais personalização removendo os valores codificados.

Abra  ~/environment/eksdemo/templates/deployment/frontend.yaml no seu editor Cloud9.

{{% notice info %}}
As etapas a seguir devem ser concluídas separadamente **frontend.yaml**, **crystal.yaml**, e **nodejs.yaml**.
{{% /notice %}}

Under `spec`, find **replicas: 1**  and replace with the following:
```
replicas: {{ .Values.replica }}
```
Debaixo `spec.template.spec.containers.image`, substitua a imagem pelo valor de template correto da tabela abaixo:

|Filename | Value |
|---|---|
|frontend.yaml|image: {{ .Values.frontend.image }}:{{ .Values.version }}|
|crystal.yaml|image: {{ .Values.crystal.image }}:{{ .Values.version }}|
|nodejs.yaml|image: {{ .Values.nodejs.image }}:{{ .Values.version }}|

#### Crie um arquivo values.yaml com nossos padrões de template

Este arquivo irá preencher nossas `diretivas de template` com os valores padrão.
```
cat <<EoF > ~/environment/eksdemo/values.yaml
# Valores padrão para o eksdemo.
# Este é um arquivo formatado em YAML.
# Declare variáveis ​​a serem passadas para seus templates.

# Valores de todo o realease
replica: 3
version: 'latest'

# Valores Específicos de Serviço
nodejs:
  image: brentley/ecsdemo-nodejs
crystal:
  image: brentley/ecsdemo-crystal
frontend:
  image: brentley/ecsdemo-frontend
EoF
```
