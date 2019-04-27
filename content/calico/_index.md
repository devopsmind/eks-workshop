---
title: "Criar políticas de rede usando o Calico"
chapter: true
weight: 50
---

# Criar políticas de rede usando o Calico

Neste capítulo, criaremos algumas políticas de rede usando o [Calico] (https://www.projectcalico.org/) e veremos as regras em ação.

As políticas de rede permitem que você defina regras que determinam que tipo de tráfego pode fluir entre diferentes serviços. Usando políticas de rede, você também pode definir regras para restringir o tráfego. Eles são um meio de melhorar a segurança do seu cluster.

Por exemplo, você só pode permitir o tráfego do frontend para o back-end em seu aplicativo.

As políticas de rede também ajudam a isolar o tráfego nos namespaces. Por exemplo, se você tiver namespaces separados para desenvolvimento e produção, poderá impedir o fluxo de tráfego entre eles, restringindo a comunicação do pod para o pod dentro do mesmo namespace.


![calico](/images/Project-Calico-logo-1000px.png)
