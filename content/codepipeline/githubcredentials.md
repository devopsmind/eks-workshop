---
title: "Token de Acesso do GitHub"
date: 2018-10-087T08:30:11-07:00
weight: 13
draft: false
---

Para que o CodePipeline receba retornos de chamada do GitHub, precisamos gerar um token de acesso pessoal.

Uma vez criado, um token de acesso pode ser armazenado em um código seguro e reutilizado, portanto, essa etapa só é necessária durante a primeira execução ou quando você precisar gerar novas chaves.

Abra uma [nova página de acesso pessoal](https://github.com/settings/tokens/new) no GitHub.

{{% notice info %}}
Você pode ser solicitado a digitar sua senha do GitHub
{{% /notice %}}

Insira um valor para **Token description**, verifique o escopo de permissão **repo** e role para baixo e clique no botão ** Gerar token **

![Generate New](/images/codepipeline/github_token_name.png)

Copie o **token de acesso pessoal** e salve-o em um local seguro para a próxima etapa

![Generate New](/images/codepipeline/github_copy_access.png)

