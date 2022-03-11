# Labels

- Se puede usar como un par clave/valor para **identificación** y **localización** posterior de recursos de k8s.

- También las usa el propio k8s para elegir un Pod por parte de deployments y services
    - A través de la run label se monitoriza si hay suficiente cantidad de pods.
    - Al hacer kubectl expose se añade una label automáticamente.

- Se puede atacar mediante la opción --selector
>> kubectl get pods --selector='run=httpd'

- Se crean de forma manual o automática.

# Annotations

- Son para dar metadata detallada en un objeto.

- No se puede usar en queries, solo dan información adicional (licencias, mantenimiento, etc).


# Comandos

## FORMA MANUAL

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl label deployment httpd-run nl=spook
deployment.apps/httpd-run labeled

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps --show-labels 
NAME        READY   UP-TO-DATE   AVAILABLE   AGE    LABELS
httpd-run   3/3     3            3           106s   app=httpd-run,nl=spook

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps --selector nl=spook
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
httpd-run   3/3     3            3           2m43s
```

## FORMA AUTOMÁTICA

- Al lanzar un deployment nos da una label por defecto en app=...
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps --show-labels 
NAME         READY   UP-TO-DATE   AVAILABLE   AGE     LABELS
httpd-run    3/3     3            3           6m20s   **app=httpd-run**,nl=spook
ngingx-run   1/1     1            1           29s     **app=ngingx-run**

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe deployments.apps ngingx-run 
Name:                   ngingx-run
Namespace:              default
CreationTimestamp:      Tue, 01 Mar 2022 19:23:24 +0100
**Labels:                 app=ngingx-run**
Annotations:            deployment.kubernetes.io/revision: 1
**Selector:               app=ngingx-run**
```
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps ngingx-run -o yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
  labels:
    app: ngingx-run
  name: ngingx-run
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: ngingx-run
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ngingx-run
```
- Quitamos la label automática y al momento se nos crea otro pod porque "ha muerto" el pod con esa label
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl label pod ngingx-run-8db74747f-j4h9j app-
pod/ngingx-run-8db74747f-j4h9j unlabeled

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods --show-labels 
NAME                         READY   STATUS    RESTARTS   AGE     LABELS
httpd-run-7cff6b6b4b-l7sd6   1/1     Running   0          12m     app=httpd-run,pod-template-hash=7cff6b6b4b
httpd-run-7cff6b6b4b-sq8qq   1/1     Running   0          12m     app=httpd-run,pod-template-hash=7cff6b6b4b
httpd-run-7cff6b6b4b-tp9cf   1/1     Running   0          12m     app=httpd-run,pod-template-hash=7cff6b6b4b
ngingx-run-8db74747f-j4h9j   1/1     Running   0          6m26s   pod-template-hash=8db74747f
ngingx-run-8db74747f-sjzgf   1/1     Running   0          13s     app=ngingx-run,pod-template-hash=8db74747f

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get all --selector app=ngingx-run
NAME                             READY   STATUS    RESTARTS   AGE
pod/ngingx-run-8db74747f-sjzgf   1/1     Running   0          110s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ngingx-run   1/1     1            1           8m3s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/ngingx-run-8db74747f   1         1         1       8m3s
```