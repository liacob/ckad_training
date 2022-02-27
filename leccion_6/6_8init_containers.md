# Qué son los init containers?

- Contenedores adicionales en un Pod que completan una tarea previa a la tarea regular para lo que se ha creado el pod.

- La tarea regular sólo se iniciará después de que el init container se inicie.

- Un ejemplo:

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/init-example1.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: init-demo1
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - name: workdir
      mountPath: /usr/share/nginx/html
  **initContainers:**
  - name: install
    image: busybox
    command:
    - wget
    - "-O"
    - "/work-dir/index.html"
    - http://info.cern.ch
    volumeMounts:
    - name: workdir
      mountPath: "/work-dir"
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}


- Segundo ejemplo:

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/init-example2.yaml 
pod/init-demo2 created
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME         READY   STATUS     RESTARTS   AGE
init-demo1   1/1     Running    0          3m53s
init-demo2   0/1     Init:0/1   0          3s