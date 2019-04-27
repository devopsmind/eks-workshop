---
title: "Batch Processing with Argo"
date: 2018-11-18T00:00:00-05:00
weight: 44
draft: false
---

### Batch Processing

Neste capítulo, vamos implantar cenários comuns de processamento em lote usando Kubernetes e [Argo](https://argoproj.github.io/).

![Argo Logo](/images/argo-logo.png)

#### O que é Argo?
O Argo é um mecanismo de fluxo de trabalho nativo de contêiner de software livre para executar o trabalho no Kubernetes. 

* Definir fluxos de trabalho em que cada etapa do fluxo de trabalho é um contêiner.
* Modele fluxos de trabalho de várias etapas como uma sequência de tarefas ou capture as dependências entre tarefas usando um gráfico (DAG).
* Execute facilmente tarefas intensivas de computação para aprendizado de máquina ou processamento de dados em uma fração do tempo usando fluxos de trabalho Argo no Kubernetes.
