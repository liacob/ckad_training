# ConfigMaps for ConfigFiles

- Los ConfigMaps sirven también para pasar un archivo entero de configuración.

- En esta caso se usa el ConfigMap como un volumen que simplemente contenga el fichero.

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/nginx-custom-config.conf 
server {
    listen       8888;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create cm nginx --from-file=materials/nginx-custom-config.conf 
configmap/nginx created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get cm nginx -o yaml 
apiVersion: v1
data:
  nginx-custom-config.conf: |
    server {
        listen       8888;
        server_name  localhost;
        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
    }
kind: ConfigMap
metadata:
  creationTimestamp: "2022-04-08T10:31:53Z"
  name: nginx
  namespace: default
  resourceVersion: "4674"
  uid: 03ddf894-2700-4316-b0a7-36df7938fe68
```

```
base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$  cat materials/nginx-cm.yml 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-cm
  labels:
    role: web
spec:
  containers:
  - name: nginx-cm
    image: nginx
    volumeMounts:
    - name: conf
      mountPath: /etc/nginx/conf.d
  volumes:
  - name: conf
    configMap:
      name: nginx-cm -----------------------------------> No se llama como el configmap que hemos creado antes, no se va a arrancar el pod si no hacemos algo
      items:
      - key: nginx-custom-config.conf
        path: default.conf

```

- Comprobación tras crearlo

``` 
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl exec -it nginx-cm /bin/bash
root@nginx-cm:/# cat /etc/nginx/conf.d/default.conf 
server {
    listen       8888;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}
```