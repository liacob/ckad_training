# Entendamos las reglas del ingress

Toda regla de ingress contiene lo siguiente:

- Un host específico. Si no la regla se aplica a todo el tráfico HTTP

- Una lista de paths (e.g. /testpath). Con cada path teniendo su backend.

- El backend contiene el serviceName y el servicePort al que apunta. Se suele crear un backend default para tráfico que no cuadra con ningún path.

# Tipos de backend

- Simple fanout: El tráfico va a múltiples backends. Reduciendo el número de los balanceadores de carga.

- Name based virtual hosting: Tráfico viniendo en un nombre específico va enrutado a un servicio en específico.

- TLS ingress: Usa un secreto TLS para asegurar seguridad en el balanceador de carga.

# Ejemplos

- SIMPLE
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/example-ingress.yaml 
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - host: hello-world.info
    http:
      paths:
      - path: /|/(.+)
        backend:
          serviceName: web
          servicePort: 8080
```
- FANOUT
```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: example-ingress-fanout
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foo
        backend:
          serviceName: service1
          servicePort: 4200
     - path: /bar
        backend:
          serviceName: service2
          servicePort: 4201
```

- VIRTUAL HOSTING

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/ingress-virtual-hosting.yaml 
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: name-virtual-host-ingress
spec:
  rules:
  - host: first.bar.com
    http:
      paths:
      - backend:
          serviceName: service1
          servicePort: 80
  - host: second.foo.com
    http:
      paths:
      - backend:
          serviceName: service2
          servicePort: 80
  - http:
      paths:
      - backend: 
          serviceName: service3
          servicePort: 80
```