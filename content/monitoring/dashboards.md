---
title: "Dashboards"
date: 2018-10-14T21:03:43-04:00
weight: 20
draft: false
---

#### Criar Dashboards

Faça o login no Dashboards do Grafana usando as credenciais fornecidas durante a configuração
Você vai notar que **'Install Grafana'** & **'crie sua primeira fonte de dados'** já estão concluídas. Vamos importar o Dashboards criado pela comunidade para este tutorial

Clique '+'no  botão no painel esquerdo e selecione 'Import'

Enter **3131** dashboard id under Grafana.com Dashboard & click **'Load'**.

Leave the defaults, select **'Prometheus'** as the endpoint under prometheus data sources drop down, click **'Import'**.

This will show monitoring dashboard for all cluster nodes

![grafana-all-nodes](/images/grafana-all-nodes.png)

For creating dashboard to monitor all pods, repeat same process as above and enter **3146** for dashboard id

![grafana-all-pods](/images/grafana-all-pods.png)
