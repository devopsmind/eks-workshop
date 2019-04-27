---
title: "Crie uma chave SSH"
chapter: false
weight: 21
---

{{% notice info %}}
A partir daqui, quando você ver o comando a ser inserido, como abaixo, você inserirá esses comandos na IDE do Cloud9 . Você pode usar o recurso **Copiar para área de transferência**  (canto superior direito) para simplesmente copiar e colar na Cloud9. Para colar, você pode usar Ctrl V para Windows ou Command V para Mac.
{{% /notice %}}

Por favor, execute este comando para gerar a chave SSH na Cloud9. Essa chave será usada nas instâncias dos worker node para permitir o acesso ssh, se necessário.

```bash
ssh-keygen
```

{{% notice tip %}}
Pressione `enter` 3 vezes para escolher as opções padrão
{{% /notice %}}

Carregue a chave pública na sua região EC2:

```bash
aws ec2 import-key-pair --key-name "eksworkshop" --public-key-material file://~/.ssh/id_rsa.pub
```