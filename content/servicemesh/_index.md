---
title: "Service Mesh com Istio"
date: 2018-11-13T16:32:30+09:00
weight: 53
draft: false
---

### Service Mesh

Um service mesh é uma camada de infraestrutura dedicada para manipulação **service-to-service communication**. Ele é responsável pela entrega confiável de solicitações por meio da complexa topologia de serviços que compõem um aplicativo nativo moderno na nuvem.

As soluções de malha de serviço têm dois componentes distintos que se comportam de maneira um pouco diferente: 1) um plano de dados e 2) um plano de controle. O diagrama a seguir ilustra a arquitetura básica.

![Service Mesh Architecture](/images/servicemesh-intro1.png)

* O <span style="color:orange">**plano de dados**</span> é composto por um conjunto de proxies inteligentes (Envoy) implementados como sidecars. Esses proxies servem de mediador e controlam toda a comunicação de rede entre microsserviços junto com o Mixer, uma política de propósito geral e hub de telemetria.

* A<span style="color:orange">**control plane**</span>gerencia e configura os proxies para rotear o tráfego. Além disso, o plano de controle configura Mixers para aplicar políticas e coletar telemetria.
