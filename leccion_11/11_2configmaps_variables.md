# Vista general del proceso:

- Definir en ConfigMap teniendo en cuenta las distinas fuentes que puede tener:

```
kubectl create cm variables --from-env-file=variables
kubectl create cm special --from-literal=VAR=red --from-literal=VAR4=blue

kubectl describe cm <cmname>
```

- Para utilizarlo en un Pod es lo mismo siempre:

```
envFrom:
  - configMapRef:
    name: ConfigMapName
```

- Creación de uno mediante fichero y su descripción
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/variables 
VAR1=Hello
VAR2=World

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create cm variables --from-env-file=materials/variables -o yaml --dry-run 
apiVersion: v1
data:
  VAR1: Hello
  VAR2: World
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: variables

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe cm variables 
Name:         variables
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
VAR1:
----
Hello
VAR2:
----
World
```

- Creación de un pod que lo utilice:

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/cm-test-pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: test1
spec:
  containers:
  - name: test1
    image: cirros
    command: ["/bin/sh", "-c", "env"]
    envFrom:
      - configMapRef:
          name: variables

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/cm-test-pod.yaml 
pod/test1 created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl logs test1 
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.96.0.1:443
HOSTNAME=test1
SHLVL=1
HOME=/root
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
VAR1=Hello
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
VAR2=World
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/
```

- De manera literal


```
kubctl creat cm morevars --from-literal=VAR=red --from-literal=VAR4=green

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get cm morevars -o yaml
apiVersion: v1
data:
  VAR: red
  VAR4: green
kind: ConfigMap
metadata:
  creationTimestamp: "2022-04-08T10:23:23Z"
  name: morevars
  namespace: default
  resourceVersion: "4241"
  uid: bb0b61c5-de7a-4b24-a131-17f2260c69c6
```