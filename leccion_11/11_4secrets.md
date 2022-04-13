# Secrets

## Conceptos básicos

- Se usan para guardar datos sensibles como contraseñas, claves SSH, auth keys y evitar las exposición accidental de estos.

- Hay algunos secretos que se crean automáticamente por el sistema.

- Los secrets se configuran de la misma manera que los configMaps

## Tipos de secrets

- docker-registry: Para conectarse a un registro de Docker
- TLS
- Generic: Mismo comportamiento que un ConfigMap pero codificado

## Secrets built-in

- Kubernetes nos crea automáticamente secretos para conectarnos a la API  y modifica los Pods automáticamente para que los puedan usar.


## Ejemplo

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl describe pods nginx-cm 
Name:         nginx-cm

  nginx-cm:
    Container ID:   docker://2ae087d62743bc57d1ab0a9402db6ec8ee71be773e11a3938c21bcf6dcdd1f2b
    Image:          nginx
    Mounts:
      /etc/nginx/conf.d from conf (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-n82xl (ro) -------------------------

  ```