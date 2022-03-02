# Qué son?

- Cómo asegurarnos que un Pod esté disponible siempre, que escale, etc?

- Es el Pod + Update Strategy + History + Scalability

- Utiliza ``labels`` para controlar todo esto.

- Desde el Dashboard se crean Deployments por defecto.

- kubectl run lanza Deployments por defecto.

## Deployment spec

- Lleva varias propiedades importantes:
    - replicas
    - strategy: Cómo deben hacerse los upgrades
    - selector: usa matchLabels para ver cómo las etiquetas matchean contra el Pod
    - template: contiene la especificación del Pod

## Comandos

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create deployment httpd-run --image=httpd --replicas=3
deployment.apps/httpd-run created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps httpd-run  -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
.
.
spec:
  progressDeadlineSeconds: 600
  **replicas: 3**
  revisionHistoryLimit: 10
  selector:
    **matchLabels:**
      app: httpd-run
  **strategy:**
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: httpd-run
    spec:
      containers:
      - image: httpd
        imagePullPolicy: Always
        name: httpd
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get all
NAME                             READY   STATUS    RESTARTS   AGE
pod/httpd-run-7cff6b6b4b-725vt   1/1     Running   0          2m21s
pod/httpd-run-7cff6b6b4b-qlx7l   1/1     Running   0          2m21s
pod/httpd-run-7cff6b6b4b-z64fv   1/1     Running   0          2m21s

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/httpd-run   3/3     3            3           2m21s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/httpd-run-7cff6b6b4b   3         3         3       2m21s