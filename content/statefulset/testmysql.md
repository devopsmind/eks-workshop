---
title: "Teste o MySQL"
date: 2018-08-07T08:30:11-07:00
weight: 25
---
Você pode usar **mysql-client** para enviar alguns dados para o master, **mysql-0.mysql**
executando o comando.
```
kubectl run mysql-client --image=mysql:5.7 -i --rm --restart=Never --\
  mysql -h mysql-0.mysql <<EOF
CREATE DATABASE test;
CREATE TABLE test.messages (message VARCHAR(250));
INSERT INTO test.messages VALUES ('hello, from mysql-client');
EOF

```
Execute o seguinte para testar slaves (mysql-read) recebeu o data.
```
kubectl run mysql-client --image=mysql:5.7 -it --rm --restart=Never --\
  mysql -h mysql-read -e "SELECT * FROM test.messages"
```
A saída deve ficar assim.
```
+--------------------------+
| message                  |
+--------------------------+
| hello, from mysql-client |
+--------------------------+
```
Para testar o balanceamento de carga entre slaves, execute o seguinte comando.
```
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never --\
   bash -ic "while sleep 1; do mysql -h mysql-read -e 'SELECT @@server_id,NOW()'; done"
```
Cada instância do MySQL é atribuída a um identificador único e pode ser recuperada usando @@server_id. Ele imprimirá o ID do servidor que atende a solicitação e o timestamp.
```
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         102 | 2018-11-14 12:44:57 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         101 | 2018-11-14 12:44:58 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         100 | 2018-11-14 12:44:59 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         100 | 2018-11-14 12:45:00 |
+-------------+---------------------+
+-------------+---------------------+
| @@server_id | NOW()               |
+-------------+---------------------+
|         101 | 2018-11-14 12:45:01 |
+-------------+---------------------+
```
Deixe isso aberto em uma janela separada enquanto você testa a falha na próxima seção.
