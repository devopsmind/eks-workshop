---
title: "Limpar"
date: 2018-10-14T21:03:30-04:00
weight: 25
draft: false
---

#### Excluir Prometheus e grafana
```
helm delete prometheus
helm del --purge prometheus
helm delete grafana
helm del --purge grafana
```
