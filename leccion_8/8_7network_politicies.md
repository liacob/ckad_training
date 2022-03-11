# Politicas de red

- Por defecto todos los pods dentro de un mismo namespace se pueden comunicar.

- Una forma de bloquear tráfico es mandando pods a namespaces dedicados.

- Tenemos un objeto llamado **NetworkPolicy** que se comporta como un firewall bloqueando ingress/egress.

- Depende del internet provider (puede no ser efectiva si no se soporta).

- Las las conexiones son statefull así que con una dirección nos vale.

- Para saber dónde se aplica ser tira de labels para variar

>> kubectl explain NetworkPolicy.spec

# Uso de las políticas

- Si una dirección sale en el manifiesto pero no se especifica nada -> No hay limitación

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/pods-with-nw-policy.yaml 
kind: Pod
apiVersion: v1
metadata:
  name: database-pod
  namespace: default
  labels:
    app: database
spec:
  containers:
    - name: database
      image: alpine
---

kind: Pod
apiVersion: v1
metadata:
  name: web-pod
  namespace: default
  labels:
    app: web
spec:
  containers:
    - name: web
      image: alpine

---

kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: db-networkpolicy
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: database -> Quiero aplicar una política en los pods que lleven la label app-database
  policyTypes:
  - Ingress
  - Egress 
  ingress: -> Quiero que la política de ingreso sea la siguiente
    - from:
      - podSelector:
          matchLabels:
            app: web -> Quiero que entre el tráfico del pod con label app-web
```

