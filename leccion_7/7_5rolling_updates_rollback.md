# Estrategias de update

- Al hacer un cambio en un deployment, esto se aplica al momento en los pods.

- Nosotros deaseamos que el servicio esté en continuo funcionamiento.

- Hasta que no se tiene el nuevo servicio corriendo el viejo no se puede borrar.

>> kubectl rollout history
>> kubectl rollout undo

- Hay dos estrategias de upgrade en los pods:
    - RollingUpgrade: Se van actualizando los pods uno a uno de manera que siempre hay alguno funcionando para no perder servicio en ningún momento.
    - Recreate: Se sube la versión de todos los pods a la vez. Implica corte de servicio. A veces no se puede tener una misma app en versiones diferentes.

- El ``RollingUpgrade`` tiene opciones como:
    - MaxUnavailable: Máximo número de Pods que se están actualizando a la vez.
    - MaxSurge: Máximo por encima del número de réplicas especificado.

## Comandos
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/rolling.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rolling-nginx
spec:
  **replicas: 4**
  **strategy:**
    **type: RollingUpdate**
    **rollingUpdate:**
      **maxSurge: 2**
      **maxUnavailable: 1**
```
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl rollout history deployment rolling-nginx 
deployment.apps/rolling-nginx 
REVISION  CHANGE-CAUSE
1         <none>

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl edit deployments.apps rolling-nginx 
```
- Cambiamos la versión de la imagen a una más moderna
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps 
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
rolling-nginx   **3/4**     3            3           2m1s

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl rollout history deployment rolling-nginx 
deployment.apps/rolling-nginx 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl rollout history deployment rolling-nginx **--revision=2**
deployment.apps/rolling-nginx with revision #2
Pod Template:
  Labels:       app=nginx
        pod-template-hash=5d5cc69b9
  Containers:
   nginx:
    **Image:      nginx:1.15**
    Port:       <none>
    Host Port:  <none>
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get rs
NAME                       DESIRED   CURRENT   READY   AGE
rolling-nginx-5d5cc69b9    4         4         4       4m26s **NUEVO**
rolling-nginx-**77875989ff**   0         0         0       6m21s **VIEJO**

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl rollout undo deployment rolling-nginx --to-revision=1
deployment.apps/rolling-nginx rolled back

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get rs
NAME                       DESIRED   CURRENT   READY   AGE
rolling-nginx-5d5cc69b9    0         0         0       6m52s
rolling-nginx-**77875989ff**   4         4         4       8m47s
```
**VUELVE A LOS PODS ANTERIORES QUE SE CREARON EN LA V1**