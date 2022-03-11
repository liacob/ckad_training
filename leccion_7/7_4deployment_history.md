# Historial de deployments

- Un cambio mayor que cree un nuevo ReplicaSet que usa nuevas propiedades.

- El viejo ReplicaSet se mantiene pero con el numero de pods a 0.

- Esto facilita el deshacer cambios

>> kubectl rollout history

## Comandos
```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ kubectl rollout history deployment ngingx-run 
deployment.apps/ngingx-run 
REVISION  CHANGE-CAUSE
1         <none>
```