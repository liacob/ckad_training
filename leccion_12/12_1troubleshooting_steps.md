# Pasos generales de troubleshooting

- Verificar que la sintaxis es correcta antes de añadir cualquier cosa

- Usar herramientas estándar de Linux para el troubleshooting. A unas malas si es a nivel de Pod usar busybox

- kubectl describe es fundamental pues es una query a la etcd para ver lo que pasa

- kubectl logs para ver en el pod lo que ha ido pasando

- Hay herramientas como Fluentd o Prometheus que monitorizan todo en todos los nodos pero eso es externo a k8s

# Estados de los Pods

- Los Pods pasan por diferentes estandos en su ciclo de vida: kubectl get pods 

    - pending: Se validado con la API y se ha creado la entrada en la etcd. Pero la imagen aun tiene que ser traida (fetched)
    - running: Todo ok
    - succeeded: Ha hecho lo que tenía que hacer y se ha parado
    - failed: Ha terminado de ejecutarse pero ha fallado durante el proceso
    - unknow: No se ha podido obtener el estado (Muy raro verlo, ocurra más al haber varios clústeres y falle la comunicación entre ellos)

# Sintaxis

- kubectl explain nos vale si no tenemos acceso a internet

- https://kubernets.io/docs y buscar la feature que toque, sección tasks


# Ejemplo

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create configmap variables --from-file=materials/variables 
configmap/variables created
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/cm-test-pod.yaml 
pod/test1 created
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME    READY   STATUS      RESTARTS   AGE
test1   0/1     Completed   0          5s
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME    READY   STATUS             RESTARTS      AGE
test1   0/1     CrashLoopBackOff   1 (11s ago)   16s
```

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pods test1 
Name:         test1
Containers:
  test1:

    Command:
      /bin/sh
      -c
      env
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated ----------------------------
      Reason:       Completed
      Exit Code:    0
      Started:      Mon, 18 Apr 2022 12:36:28 +0200
      Finished:     Mon, 18 Apr 2022 12:36:28 +0200
    Ready:          False
    Restart Count:  4

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl logs test1 
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.96.0.1:443
HOSTNAME=test1
SHLVL=1
HOME=/root
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
variables=VAR1=Hello ---------------------------
VAR2=World -------------------------------------

KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/
```

- Nos fijamos en Last State y parece que en verdad ha ido bien, ha lanzado los comandos y se ha cerrado. Por ello quizás sea mejor crear un Job antes que un Pod para esto

- Cambiamos la restart policy y listo. Sabíamos que esto existía gracias a kubectl explain pod.spec