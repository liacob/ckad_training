# Veamos cómo manejar los recursos

- Parte de los ```Cgroups``` de Linux: cpu / memory / ...

- k8s -> docker run -> linux Cgroups

- Por defecto un Pod usará toda la CPU y memoria que necesite para hacer su tarea.

- Se puede manejar con pod.spec.containers.resources:
    - CPU se expresa en milicore: 1/1000 de un CPU Core
    - Memoria es quivalente al --memory del docker run

- La cosa está en que el scheduler asegura que no se lance el pod a menos que tenga los recursos necesarios.

spec:
  containers:
  - name: db
    image: mysql
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "password"
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pods frontend
Name:         frontend
Containers:
  db:
    Container ID:   docker://dd120f30ff2214d6b1e1fccc034c73970fc15b2ab10bec25259f7986f58747d5
    Image:          mysql
    **Limits:**
      cpu:     500m
      memory:  128Mi
    **Requests:**
      cpu:     250m
      memory:  64Mi
    Environment:
      MYSQL_ROOT_PASSWORD:  password