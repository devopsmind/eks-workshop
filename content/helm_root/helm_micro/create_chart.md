---
title: "Create a Chart"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

Os Helm charts têm uma estrutura semelhante a:

```text
/eksdemo
  /Chart.yaml  # uma descrição do chart
  /values.yaml # padrões, podem ser substituídos durante a instalação ou atualização
  /charts/ # Pode conter subcharts
  /templates/ # os próprios arquivos de template(modelo)
  ...
```

Seguiremos esse modelo e criaremos um novo chart chamado **eksdemo** com os seguintes comandos:

```sh
cd ~/environment
helm create eksdemo
```
