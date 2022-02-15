# Entendiendo namespaces

- Es lo que nos da aislamiento a nivel de kernel.

- Kubernetes nos da objetos de tipo NameSpace para separar recursos entre clientes.

- Para cambiar de namespace en el que trabajamos:

> kubectl ... -n namespace

- Para ver todo:

> kubectl get ... --all-namespaces

- Se especifica a nivel de metadata ( kubectl explain pods.metadata.namespace)