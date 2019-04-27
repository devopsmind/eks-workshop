---
title: "Nodes do Kubernetes"
date: 2018-10-03T10:15:55-07:00
draft: false
weight: 40
---

As máquinas que formam um cluster Kubernetes são chamadas **nodes**.

Os nós em um cluster do Kubernetes podem ser físicos ou virtuais.  

Existem dois tipos de nós:

* Um tipo  Master-node, que compõe o [Plano de controle](../../architecture/architecture_control), age como o 'cérebro' do cluster.

* um tipo Worker-node , que compõe o [Plano de dados](../../architecture/architecture_worker), executa as imagens reais do contêiner (via pods.)

Vamos mergulhar mais fundo na forma como os nós interagem uns com os outros mais tarde na apresentação.
