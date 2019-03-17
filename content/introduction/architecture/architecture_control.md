---
title: "Plano de controle"
date: 2018-10-03T10:18:27-07:00
draft: false
weight: 100
---

{{<mermaid>}}
graph TB
kubectl{kubectl}
  subgraph ControlPlane
    api(API Server)
    controller(Controller Manager)
    scheduler(Scheduler)
    etcd(etcd)
  end

  kubectl-->api
  controller-->api
  scheduler-->api
  api-->kubelet
  api-->etcd

  classDef green fill:#9f6,stroke:#333,stroke-width:4px;
  classDef orange fill:#f96,stroke:#333,stroke-width:4px;
  classDef blue fill:#6495ed,stroke:#333,stroke-width:4px;
  class api blue;
  class internet green;
  class kubectl orange;
{{< /mermaid >}}

* Um ou mais servidores de API: ponto de entrada para REST / kubectl

* etcd: Armazenamento de chave/valor distribuído

* Controller-manager: Sempre avaliando o estado atual vs desejado

* Scheduler: Agendamentos de pods para worker nodes

Confira [a documentação oficial do Kubernetes](https://kubernetes.io/docs/concepts/overview/components/#master-components) para uma explicação mais detalhada dos componentes do plano de controle.
