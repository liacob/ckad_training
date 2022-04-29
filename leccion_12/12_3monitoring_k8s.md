# Monitorizacicón de las aplicaciones

- La idea es obtener métricas sobre la infraestructura que tenemos.

- Para ello debemos utilizar otras aplicaciones CNCF:
    - Heapster: Para monitorizar múltiples pods
    - Prometheus: Para métricas de varios clústers
    - Fluentd: Para agregar logs.

- No obstante hay algunos comandos que nos pueden servir para ver el estado del clúster:
    - kubectl config current-context
    - kubectl config view
    - kubectl get all -o wide

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl config current-context 
minikube

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://kubernetes.docker.internal:6443
  name: docker-desktop
- cluster:
    certificate-authority: /home/liacob/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Thu, 28 Apr 2022 21:31:42 CEST
        provider: minikube.sigs.k8s.io
        version: v1.25.1
      name: cluster_info
    server: https://127.0.0.1:60917
  name: minikube
contexts:
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-desktop
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Thu, 28 Apr 2022 21:31:42 CEST
        provider: minikube.sigs.k8s.io
        version: v1.25.1
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: docker-desktop
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: minikube
  user:
    client-certificate: /home/liacob/.minikube/profiles/minikube/client.crt
    client-key: /home/liacob/.minikube/profiles/minikube/client.key
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ 
```