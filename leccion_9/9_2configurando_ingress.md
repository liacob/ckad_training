# Primeros pasos ingres

- Vamos a la documentación de Google y copiamos el primer ejemplo:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80
```

- Tras lanzar el addon de minikube lo podemos comprobar

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ minikube ssh
Last login: Fri Mar 11 22:50:35 2022 from 192.168.49.1
docker@minikube:~$ docker ps | grep docker
docker@minikube:~$ docker ps | grep ingress
b2e778ce5c45   k8s.gcr.io/ingress-nginx/controller   "/usr/bin/dumb-init …"   33 seconds ago   Up 32 seconds                                              k8s_controller_ingress-nginx-controller-6d5f55986b-rx6gp_ingress-nginx_4c9442ee-3939-4593-91f6-670ccd70d831_0
0e02edcdbfd0   k8s.gcr.io/pause:3.6      
```

- Creamos un deployment sencillo y lo exponemos

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl expose deployment nginx --port=80 --type=NodePort
service/nginx exposed
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        43m
nginx        NodePort    10.105.110.195   <none>        80:32272/TCP   6s
```

- Otra forma de acceder con minikube

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ minikube service nginx --url
🏃  Starting tunnel for service nginx.
|-----------|-------|-------------|------------------------|
| NAMESPACE | NAME  | TARGET PORT |          URL           |
|-----------|-------|-------------|------------------------|
| default   | nginx |             | http://127.0.0.1:39521 |
|-----------|-------|-------------|------------------------|
http://127.0.0.1:39521
```
- Tras crear el ingress nos debería funcionar con:
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ curl http://ingress.example.com/testpath
curl: (6) Could not resolve host: ingress.example.com
```