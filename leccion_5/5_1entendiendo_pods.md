# Qué son los pods?

- Es una abstracción de un server, donde se corren las aplicaciones.
    - Puede llevar varios contenedores dentro un mismo NameSpace (como una LAN), expuesto por una sola IP.
  
-  Es la unidad mínima manejable por Kubernetes.

- Los pods se suelen lanzar mediante Deployments ya que lanzados solos no se relanzan en caso de fallar.

# Manejando pods con kubectl

- Para lanzar un Deployment basado en imagen default
> kubectl run ghost --image=gost:0.9

- Lo más habitual:
> kubectl create -f <name.yaml>

> kubectl get pods [-o yaml]
> kubectl describe pods
> kubectl edite pods 