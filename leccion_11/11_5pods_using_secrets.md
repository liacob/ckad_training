# Uso de Secrets

- Pues como los ConfigMaps:
    - Montados como volúmenes
    - Pasando variables

# Montados como volúmenes

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create secret generic secretstuff --from-literal=password=password --from-literal=user=linda
secret/secretstuff created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get secrets secretstuff -o yaml
apiVersion: v1
data:
  password: cGFzc3dvcmQ=
  user: bGluZGE=
kind: Secret
metadata:
  creationTimestamp: "2022-04-13T20:16:26Z"
  name: secretstuff
  namespace: default
  resourceVersion: "7256"
  uid: df46822b-728f-404d-ab22-23666973df65
type: Opaque
```

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/pod-secret.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: secretbox2
  namespace: default
spec:
  containers:
  - name: secretbox
    image: busybox
    command:
      - sleep
      - "3600" 
    volumeMounts:
    - mountPath: /secretstuff
      name: secret
  volumes:
  - name: secret
    secret:
      secretName: secretstuff
```

Tras crear el Pod

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pod secretbox2 
Name:         secretbox2
Namespace:    default

Containers:
  secretbox:
    Mounts:
      /secretstuff from secret (rw) ------------------------------------------------------------
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-6xtmq (ro)

Volumes:
  secret:
    Type:        Secret (a volume populated by a Secret) ---------------------------------------
    SecretName:  secretstuff
    Optional:    false

```

# Como variable

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create secret generic --from-literal=password=root

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/pod-secret-as-var.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: mymysql
  namespace: default
spec:
  containers:
  - name: mysql
    image: mysql:latest
    env:
    - name: MYSQL_ROOT_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysql
          key: password
```

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl exec -it mymysql -- /bin/bash
root@mymysql:/# env
MYSQL_MAJOR=8.0
HOSTNAME=mymysql
PWD=/
MYSQL_ROOT_PASSWORD=root --------------------------------------------------------
HOME=/root
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
MYSQL_VERSION=8.0.28-1debian10
GOSU_VERSION=1.14
TERM=xterm
SHLVL=1
KUBERNETES_PORT_443_TCP_PROTO=tcp

```