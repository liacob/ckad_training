# kubectl expose

- La forma más fácil de comunicar un deployment con el exterior es usar **kubectl expose**

- Quita mucha flexibilidad ya que los servicios trabajan con **labels** y **selectors**. Lo que te permite un service para varios deployments, cosa que no pasa con expose, pero para empezar va guay.

- Los comandos serían:

>> kubectl expose deployment nginx --port=80 --type=NodePort (--targetPort=n para que no se asigne a un puerto random de los nodos)

- Luego pues como siempre:

>> kubectl get svc (name -o yaml)

# Comandos

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl expose deployment proxy --port=80 --type=NodePort
service/proxy exposed

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        112m
proxy        NodePort    10.108.103.60   <none>        80:30493/TCP   20s

