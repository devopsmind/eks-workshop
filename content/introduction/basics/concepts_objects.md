---
title: "Visão Geral dos Objetos K8s"
date: 2018-10-03T10:15:55-07:00
draft: false
weight: 50
---

Objetos do Kubernetes são entidades usadas para representar o estado do cluster.

Um objeto é um “registro de intenção”  - uma vez criado, o cluster faz o possível para garantir que ele exista conforme definido. Isso é conhecido como o cluster “desired state.”

O Kubernetes está sempre trabalhando para tornar o “Estado atual” de um objeto igual ao objeto “Estado desejado.”  Um estado desejado pode descrever:

* Quais pods (contêineres) estão sendo executados e em quais nodes
* IP de endpoints  que mapeiam para um grupo lógico de contêineres
* Quantas réplicas de um contêiner estão sendo executadas
* E muito mais...

Vamos explicar esses objetos k8s com mais detalhes..
