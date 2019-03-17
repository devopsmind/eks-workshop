---
title: "Atualize o Repositório do Chart"
date: 2018-08-07T08:30:11-07:00
weight: 100
---

Helm usa um formato de embalagem chamado [Charts](https://github.com/helm/helm/blob/master/docs/charts.md).  Um Chart é uma coleção de arquivos que descrevem recursos do k8s.

Os Charts podem ser simples, descrevendo algo como um servidor da Web independente (que é o que vamos criar), mas eles também podem ser mais complexos, por exemplo, um charts que representa uma stack completa de aplicativos da Web, incluindo servidores da Web, bancos de dados, proxys etc.

Em vez de instalar os recursos do k8s manualmente por meio do kubectl, podemos usar o Helm para instalar charts pré-definidos mais rapidamente, com menos chances de erros de digitação ou outros erros do operador.

Quando você instala o Helm, você recebe um repositório padrão de Charts do [Repositório Oficial de Chart do Helm](https://github.com/helm/charts/tree/master/stable).

Esta é uma lista muito dinâmica que sempre muda devido a atualizações e novas adições.  Para manter a lista local de Helm atualizada com todas essas mudanças, precisamos executar ocasionalmente [atualização de repositório]comando (https://docs.helm.sh/helm/#helm-repo-update).

Para atualizar a lista local de charts do Helm, execute:

```
helm repo update
```

E você deve ver algo semelhante a:

```
Aguarde enquanto pegamos as últimas novidades dos seus repositórios de charts...
...Obteve com sucesso uma atualização do "stable" chart Atualização do repositório concluída. ⎈ Feliz Helming!⎈
```

Em seguida, procuraremos o gráfico do servidor da Web NGINX.
