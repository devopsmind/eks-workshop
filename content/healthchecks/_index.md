---
title: "Health Checks"
chapter: true
weight: 40
---

# Health Checks

Por padrão, o Kubernetes irá reiniciar um contêiner se ele falhar por algum motivo. Ele usa os probes Liveness e Readiness, que podem ser configurados para executar um aplicativo robusto, identificando os contêineres saudáveis ​​para enviar tráfego e reiniciando os quando necessário.

Nesta seção, vamos entender como [liveness e readiness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) são definidos e testam o mesmo contra estados diferentes de um pod. Abaixo está a descrição de alto nível de como estas sondas funcionam.

**Liveness probes** são usados ​​em Kubernetes para saber quando um pod está vivo ou morto. Um pod pode estar em um estado morto por diferentes razões, enquanto o Kubernetes mata e recria o pod quando a sonda de liveness não passa.

**Readiness probes** são usados ​​no Kubernetes para saber quando um pod está pronto para atender ao tráfego. Somente quando o probe de prontidão passar, um pod receberá tráfego do serviço. Quando o probe de prontidão falhar, o tráfego não será enviado a um pod até que seja aprovado.

Vamos revisar alguns exemplos neste módulo para entender as diferentes opções de configuração de probes de disponibilidade e disponibilidade.
