---
title: "Atualizar para o mais recente CLI da AWS"
chapter: false
weight: 45
draft: true
comment: instalação padrão agora inclui aws-cli/1.15.83
---

{{% notice tip %}}
Para este workshop, por favor, ignore os avisos sobre a versão do pip sendo usada.
{{% /notice %}}

1. Execute o seguinte comando para visualizar a versão atual do aws-cli:
```
aws --version
```

1. Atualize para a versão mais recente:
```
pip install --user --upgrade awscli
```

1. Confirme que você tem uma versão mais recente:
```
aws --version
```
