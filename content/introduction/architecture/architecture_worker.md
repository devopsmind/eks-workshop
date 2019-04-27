---
title: "Plano de dados"
date: 2018-10-03T10:18:27-07:00
draft: false
weight: 110
---

{{<mermaid>}}
graph TB
internet((internet))
    subgraph worker1
      kubelet1(kubelet)
      kube-proxy1(kube-proxy)
      subgraph docker1
        subgraph podA
          containerA[container]
        end
        subgraph podB
          containerB[container]
        end
      end
    end

  internet-->kube-proxy1
  api-->kubelet1
  kubelet1-->containerA
  kubelet1-->containerB
  kube-proxy1-->containerA
  kube-proxy1-->containerB

  classDef green fill:#9f6,stroke:#333,stroke-width:4px;
  classDef orange fill:#f96,stroke:#333,stroke-width:4px;
  classDef blue fill:#6495ed,stroke:#333,stroke-width:4px;
  class api blue;
  class internet green;
  class kubectl orange;
{{< /mermaid >}}

* Feito de worker nodes

* kubelet: Atua como um canal entre o servidor da API e o node

* kube-proxy: Gerenciamento de  tradução e roteamento de IP

Confira [a documentação oficial do Kubernetes](https://kubernetes.io/docs/concepts/overview/components/#node-components) para uma explicação mais detalhada dos componentes do plano de dados.
