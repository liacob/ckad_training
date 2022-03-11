```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl expose deployment proxy --port=80 --type=ClusterIP
service/proxy exposed
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get service proxy 
NAME    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
proxy   ClusterIP   10.102.30.116   <none>        80/TCP    7s
```

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe service proxy
Name:              proxy
Namespace:         default
Labels:            app=proxy
Annotations:       <none>
Selector:          app=proxy
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.102.30.116
IPs:               10.102.30.116
Port:              <unset>  80/TCP
TargetPort:        80/TCP
Endpoints:         172.17.0.5:80,172.17.0.6:80,172.17.0.7:80 + 2 more...
Session Affinity:  None
Events:            <none>
```
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pods -o wide
NAME                     READY   STATUS             RESTARTS        AGE     IP            NODE       NOMINATED NODE   READINESS GATES
proxy-5857d9564d-6h7v7   1/1     Running            0               110m    172.17.0.5    minikube   <none>           <none>
proxy-5857d9564d-drn6c   1/1     Running            0               110m    172.17.0.7    minikube   <none>           <none>
proxy-5857d9564d-fszd5   1/1     Running            0               110m    172.17.0.9    minikube   <none>           <none>
proxy-5857d9564d-gqsrx   1/1     Running            0               110m    172.17.0.6    minikube   <none>           <none>
proxy-5857d9564d-scqx2   1/1     Running            0               110m    172.17.0.8    minikube   <none>           <none>
```

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get endpoints
NAME          ENDPOINTS                                               AGE
kubernetes    192.168.49.2:8443                                       3h44m
mywebserver   <none>                                                  98m
proxy         172.17.0.5:80,172.17.0.6:80,172.17.0.7:80 + 2 more...   6m6s
```

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ minikube ssh 
docker@minikube:~$ curl http://10.102.30.116
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
docker@minikube:~$ 
```


```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl edit service proxy
 ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 32000 -------
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: proxy
  sessionAffinity: None
  type: NodePort -----------

status:
service/proxy edited
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get svc
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP        3h47m
mywebserver   NodePort    10.109.54.158   <none>        80:32376/TCP   101m
proxy         NodePort    10.102.30.116   <none>        80:32000/TCP   9m59s
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ 

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ minikube ip
192.168.49.2

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ curl http://192.168.49.2:32000
```