# Uso de yaml para servicios

- Podemos tener un archivo como este

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/service.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```
- Podemos lanzar el servicio anterior sin que haya un deployment llamado como se especifica, tendremos al selector esperando a que haya uno
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/service.yaml 
service/mywebserver created
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get svc
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP        126m
mywebserver   NodePort    10.109.54.158   <none>        80:32376/TCP   9s
proxy         NodePort    10.108.103.60   <none>        80:30493/TCP   14m
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
proxy-5857d9564d-6h7v7   1/1     Running   0          14m
proxy-5857d9564d-drn6c   1/1     Running   0          14m
proxy-5857d9564d-fszd5   1/1     Running   0          14m
proxy-5857d9564d-gqsrx   1/1     Running   0          14m
proxy-5857d9564d-scqx2   1/1     Running   0          14m
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get deployments.apps 
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
proxy   5/5     5            5           14m
```