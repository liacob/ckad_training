# Entendiendo la arquitectura:

- K8s master (unidad pensante, controller, head node):
    - kube-scheduler
    - etcd
    - kube-controller manager (Lo que habla con la nube)
    - kube-apiserver (Que comunica todo lo anterior con los minions)

- K8s minions (los que ejecutan las cosas):
    - kube-proxy (Que permite la comunicación con el máster)
    - kubelet (Lo que permite ejecutar las cosas y lleva el conector a la cloud))