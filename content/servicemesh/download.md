---
title: "Baixe e instale o Istio CLI"
date: 2018-11-13T16:36:43+09:00
weight: 20
draft: false
---

Antes de podermos começar a configurar o Istio, precisamos primeiro instalar as ferramentas de linha de comando com as quais você irá interagir. Para fazer isso, execute o seguinte.
```
cd ~/environment

curl -L https://git.io/getLatestIstio | sh -

// versão pode ser diferente como istio é atualizado
cd istio-1.0.3

sudo mv -v bin/istioctl /usr/local/bin/
```
