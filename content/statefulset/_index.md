---
title: "Contêineres com informações de estado usando StatefulSets"
chapter: true
weight: 54
---

# Contêineres com informações de estado usando StatefulSets

[StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) gerenciar deploy e o dimensionamento de um conjunto de pods e fornecer garantias sobre o pedido e a exclusividade desses pods, adequados para aplicativos que exigem um ou mais dos seguintes.

* **Stable, unique network identifiers**
* **Stable, persistent storage**
* **Ordered, graceful deployment and scaling**
* **Ordered, automated rolling updates**

Neste capítulo, revisaremos como implantar o banco de dados MySQL usando StatefulSets e EBS como[PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/). O exemplo é uma topologia de mestre único do MySQL com vários escravos executando replicação assíncrona.
