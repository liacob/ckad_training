# Escalabilidad en K8s

- Se basa en ReplicaSets, útiles para manejar fuera del deployment.

- Desde dentro del deployment, se maneja en spec.replicas. Mejor tocar este parámetro que complicarse.
>> kubectl explain deployment.spec.replicas

- Para escalar:

>> kubectl scale deployment <```my-deployment``> --replicas=n
>> kubectl edit deployment <``my-deployment``>


## Comandos

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get rs
NAME                   DESIRED   CURRENT   READY   AGE
httpd-run-7cff6b6b4b   3         3         3       43m
redis-696dfdfbfc       1         1         1       4m47s

- Lo bonito de los deployments

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
httpd-run-7cff6b6b4b-725vt   1/1     Running   0          43m
httpd-run-7cff6b6b4b-qlx7l   1/1     Running   0          43m
httpd-run-7cff6b6b4b-z64fv   1/1     Running   0          43m
**redis-696dfdfbfc-kcmhk       1/1     Running   0          4m57s**

**(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl delete pods redis-696dfdfbfc-kcmhk** 
pod "redis-696dfdfbfc-kcmhk" deleted

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
httpd-run-7cff6b6b4b-725vt   1/1     Running   0          44m
httpd-run-7cff6b6b4b-qlx7l   1/1     Running   0          44m
httpd-run-7cff6b6b4b-z64fv   1/1     Running   0          44m
**redis-696dfdfbfc-vsqvn       1/1     Running   0          4s**