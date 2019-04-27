---
title: "O que vamos fazer"
date: 2018-08-07T08:30:11-07:00
weight: 1
draft: true
---

Vamos implantar um aplicativo simples de microsserviço multicamada. Isso usará um bucket do S3 para servir ativos estáticos de, um trabalho Kubernetes para hidratar o s3 com ativos e, em seguida, um aplicação backend que lê e escreve a partir do DynamoDB.

![Architecture Diagram](/images/aws-operator-demo.png)
