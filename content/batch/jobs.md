---
title: "Kubernetes Jobs"
date: 2018-11-18T00:00:00-05:00
weight: 20
draft: false
---

### Kubernetes Jobs

Um _job_ cria um ou mais pods e garante que um número especificado deles termine com sucesso. À medida que os pods são concluídos com êxito, o trabalho rastreia as conclusões bem-sucedidas. Quando um número especificado de conclusões bem-sucedidas é atingido, o trabalho em si é concluído. Excluir um Job limpará os pods criados.

Salve o manifesto abaixo como 'job-whalesay.yaml' usando seu editor favorito.

```
apiVersion: batch/v1
kind: Job
metadata:
  name: whalesay
spec:
  template:
    spec:
      containers:
      - name: whalesay
        image: docker/whalesay
        command: ["cowsay",  "This is a Kubernetes Job!"]
      restartPolicy: Never
  backoffLimit: 4
```

Execute um exemplo de job do Kubernetes usando a imagem `whalesay` .

```
kubectl apply -f job-whalesay.yaml
```

Wait until the job has completed successfully.

```bash
kubectl get job/whalesay
```

```output
NAME       DESIRED   SUCCESSFUL   AGE
whalesay   1         1            2m
```

Confirm the output.

```bash
kubectl logs -l job-name=whalesay
```

```output
 ___________________________ 
< This is a Kubernetes Job! >
 --------------------------- 
    \
     \
      \     
                    ##        .            
              ## ## ##       ==            
           ## ## ## ##      ===            
       /""""""""""""""""___/ ===        
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~   
       \______ o          __/            
        \    \        __/             
          \____\______/   
```