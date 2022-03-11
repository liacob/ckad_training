# Integración con DNS

- Kubernetes incorpora un DNS interno. Está montado para que actualice cada vez que se añade un servicio puesto que los puertos son dinámicos y esto supone un reto.

- Lanzamos un pod con busybox para poder hacer lo siguiente:

```
base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl exec -it multipod -- nslookup proxy
Defaulted container "busy" out of: busy, nginx
Server:         10.96.0.10
Address:        10.96.0.10:53

Name:   proxy.default.svc.cluster.local
Address: 10.108.103.60

*** Can't find proxy.svc.cluster.local: No answer
*** Can't find proxy.cluster.local: No answer
*** Can't find proxy.default.svc.cluster.local: No answer
*** Can't find proxy.svc.cluster.local: No answer
*** Can't find proxy.cluster.local: No answer
```
- Tenemos que tener lanzado **kubectl proxy** para que funcione la cosa.

- Si el nslookup no encuentra lo mejor para empezar el troubleshooting es ir al **selector** que es lo que sirve de comunicación servicio-deployment.