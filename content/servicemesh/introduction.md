---
title: "Introdução"
date: 2018-11-13T16:36:24+09:00
weight: 10
draft: false
---

### Istio

![Istio Architecture](/images/servicemesh-intro2.png)

O Istio é um service mesh  completamente aberta que se espalha de forma transparente em aplicativos distribuídos existentes. É também uma plataforma, incluindo APIs, que permite a integração em qualquer plataforma de registro, telemetria ou sistema de políticas..

Vamos revisar com mais detalhes o que cada um dos componentes que compõem essa service mesh é.

* <span style="color:orange">**Envoy**</span>
  * Processa o tráfego de entrada / saída de serviços intercalados e de serviço para serviço externo de forma transparente.

* <span style="color:orange">**Pilot**</span>
  * Pilot fornece descoberta de serviço para os sidecars Envoy, capacidades de gerenciamento de tráfego para roteamento inteligente (e.g., A/B tests, canary deployments, etc.), e resiliência (timeouts, retries, circuit breakers, etc.)

* <span style="color:orange">**Mixer**</span>
  * O Mixer aplica as políticas de controle de acesso e uso no service mesh e coleta dados de telemetria do proxy Envoy e de outros serviços.

* <span style="color:orange">**Citadel**</span>
  * O Citadel fornece uma forte autenticação entre serviço e usuário final, com gerenciamento integrado de identidade e credenciais.
