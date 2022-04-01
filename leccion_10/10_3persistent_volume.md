# Veamos los volúmenes persistentes

- Son objetos independientes que conectan con almacenamiento externo.
    - Creados con persistentVolume
    - Apuntan a cualquier tipo de almacenamiento
    - Necesitan de los persistentVolumeClaims (indican tipo y tamaño) para el binding con el almacenamiento externo (se hace según lo que se haya pedido)
    - Los persistentVolumeClaims son los que se encargan de la comunicación con el backend para ver lo que hay disponible

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/pv.yaml 
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-volume
  labels:
      type: local
spec:2
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath: -----------------
    path: "/mydata"

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl create -f materials/pv.yaml 
persistentvolume/pv-volume created

(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl get pv
NAME        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-volume   2Gi        RWO            Retain           Available                                   5s

```