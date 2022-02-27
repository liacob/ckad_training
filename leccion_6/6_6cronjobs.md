# Qué hace un cronjob

- El propósito de k8s es lanzar pods que usar contenedores.

- Hay varias formas de hacerlo
    - Directamente
    - Lanzando un job
    - Lanzando un deployment
    - Cronjobs

- jobs que se lanzan un número específico de veces a cierta hora (por ejemplo backups). El orden es:
    - cronjob -> job -> pod

- Para ver los detalles:
>> kubectl explain cronjob.spec

- Inspeccionando el cronjob:

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get jobs.batch 
NAME                     COMPLETIONS   DURATION   AGE
hello-cronjob-27432758   1/1           3s         3m26s
hello-cronjob-27432760   1/1           3s         86s


(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME                           READY   STATUS      RESTARTS   AGE
hello-cronjob-27432758-thp9q   0/1     Completed   0          98s
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods hello-cronjob-27432758-thp9q -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2022-02-27T12:38:00Z"
  generateName: hello-cronjob-27432758-
  **labels:**
    controller-uid: 608b1b2e-eccc-4b6b-88f5-d6ff63dc78da
    job-name: hello-cronjob-27432758

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME                           READY   STATUS      RESTARTS   AGE
hello-cronjob-27432758-thp9q   0/1     Completed   0          4m
hello-cronjob-27432760-z77wm   0/1     Completed   0          2m
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl logs hello-cronjob-27432758-thp9q
Sun Feb 27 12:38:02 UTC 2022
hello from the K8s cluster
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl logs hello-cronjob-27432760-z77wm  
Sun Feb 27 12:40:02 UTC 2022
hello from the K8s cluster

- Si queremos pararlo

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl delete cronjobs.batch hello-cronjob 
cronjob.batch "hello-cronjob" deleted