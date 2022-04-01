# Cómo configurar los volúmenes

- Lo primero, ¿Volumen Pod-local o Volumen Persistente?

- Empezando con lo sencillo, Pod-local.

    - Definimos el volumen a nivel de pod en spec.volumes
    - El contenedor monta el volumen en spec.containers.volumemounts

- El Volumen Persistente necesita de otros objetos

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/morevolumes.yaml 
apiVersion: v1
kind: Pod
metadata: 
  name: morevol2
spec:
  containers:
  - name: centos1
    image: centos:7
    command:
      - sleep
      - "3600" 
    volumeMounts:
      - mountPath: /centos1
        name: test --------------
  - name: centos2
    image: centos:7
    command:
      - sleep
      - "3600"
    volumeMounts:
      - mountPath: /centos2
        name: test --------------
  volumes: 
    - name: test --------------
      emptyDir: {}

```

Tras lanzarlo:

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pod morevol2 
Name:         morevol2
Namespace:    default

Containers:
  centos1:
    .
    .
    .
    Image:         centos:7
    Mounts:
      /centos1 from test (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-kgzz5 (ro)
  centos2:
    .
    .
    Mounts:
      /centos2 from test (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-kgzz5 (ro)
Volumes:
  test:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
```
Testeamos que funcione creando un archivo en un contenedor y mirando en el otro:

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl exec -it morevol2 -c centos1 -- touch /centos1/test
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl exec -it morevol2 -c centos2 -- ls -l /centos2
total 0
-rw-r--r-- 1 root root 0 Mar 30 20:08 test
```

# Volume types

- Hay muchos tipos:
    - emptyDir: Tenemos un directorio vacío en el host.
    - hostPath: Conexión persistente con el host.
    - azureDisk / awsPersistentDisk / nube
    - nfs
    - gitrepo

Para más detalles:

>> kubectl explain pod.spec.volumes