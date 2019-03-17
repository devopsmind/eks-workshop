---
title: "Autoscaling our Applications and Clusters"
chapter: true
weight: 41
---

# Implement AutoScaling with HPA and CA

Neste capítulo, mostraremos padrões para dimensionar automaticamente os node worker e as implantações de aplicativos. O escalonamento automático no K8s vem em duas formas:

* **Horizontal Pod Autoscaler (HPA)** dimensiona os pods em um conjunto de implantação ou réplica. Ele é implementado como um recurso de API do K8s e um controlador. O gerenciador do controlador consulta a utilização de recursos em relação às métricas especificadas em cada definição de HorizontalPodAutoscaler. Obtém as métricas da API de métricas de recursos (para métricas de recurso por pod) ou da API de métricas personalizadas (para todas as outras métricas).

* **Cluster Autoscaler (CA)** é o componente K8s padrão que pode ser usado para executar dimensionamento de pod e dimensionar nós em um cluster. Aumenta automaticamente o tamanho de um grupo Auto Scaling para que os pods tenham um lugar para ser executado. E tenta remover nós inativos, ou seja, nós sem pods em execução.
