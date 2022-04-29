# Qué es una Probe

- Son tests simples que se encargan de verificar que la aplicación reacciona. Son parte de la spec de los contenedores.

- Puede ser una readinessProbe que no publique el Pod hasta que pueda acceder a él.

- Puede ser una livenessProbe que compruebe su disponibilidad continuamente.

# Tipos de Probes

- exec: ejecuta un comando y devuelve nada
- httpGet: request HTTP con respuesta entre 200-399
- tcpSocket: verificar conexión a un socket TCP

# Ejemplo 1

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/busybox-ready.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: busybox-ready
  namespace: default
spec:
  containers:
  - name: busy
    image: busybox
    command:
      - sleep
      - "3600" 
    readinessProbe:
      periodSeconds: 10
      exec:
        command:
        - cat
        - /tmp/nothing
    resources: {}

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/busybox-ready.yaml 
pod/busybox-ready created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME            READY   STATUS      RESTARTS   AGE
busybox-ready   0/1     Running     0          30s
test1           0/1     Completed   0          8m28s

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pod busybox-ready 
Name:         busybox-ready
.
.
Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  4m45s                  default-scheduler  Successfully assigned default/busybox-ready to minikube
  Normal   Pulling    4m44s                  kubelet            Pulling image "busybox"
  Normal   Pulled     4m41s                  kubelet            Successfully pulled image "busybox" in 3.119853s
  Normal   Created    4m41s                  kubelet            Created container busy
  Normal   Started    4m41s                  kubelet            Started container busy
  Warning  Unhealthy  114s (x21 over 4m40s)  kubelet            Readiness probe failed: cat: can't open '/tmp/nothing': No such file or directory
```

- Vemos que está running pero no está ready porque no ha pasado la readiness probe ya que ese archivo no existe


# Ejemplo 2

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/nginx-probes.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-probes
  labels:
    role: web
spec:
  containers:
  - name: nginx-probes
    image: nginx
    readinessProbe: ------------------
      tcpSocket:
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe: -------------------
      tcpSocket:
        port: 80
      initialDelaySeconds: 20
      periodSeconds: 20

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pod nginx-probes 
Name:         nginx-probes
    State:          Running
      Started:      Mon, 18 Apr 2022 13:02:37 +0200
    Ready:          True
    Restart Count:  0
    Liveness:       tcp-socket :80 delay=20s timeout=1s period=20s #success=1 #failure=3 -------------
    Readiness:      tcp-socket :80 delay=5s timeout=1s period=10s #success=1 #failure=3  -------------
    Environment:    <none>
```

