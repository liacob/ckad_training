# Entendamos los servicios

- Los (micro)servicios son un objeto más de K8s que define un conjunto lógico de pods y una política de acceso a ellos.

- La forma de apuntar a los pods por parte del servicio viene determinadas por el selector (labels).

- El controller vigila constantemente que haya pods que macheen el selector y meterlos al servicio.

# Desacople de los servicios

- Los servicios son objetos independientes de los deployments.

- Lo que hacen es buscar deployments que contengan las labels que mediante el selector se asocien al servicio.

- El servicio por lo tanto puede dar acceso a varios deployments a la vez.

# Relación con kubeproxy

- kube-proxy está presente en todos los nodos vigilando la API en búsqueda de nuevos servicios y endpoints.

- Él es que acaba haciendo de balanceador de carga que va a un endpoint u otro.

- Toda esta información está en el etcd, donde están los servicios con el nombre/IP/puertos/endpoints.

# Tipos de servicios

- ClusterIP: Por defecto, solo acceso interno.
- NodePort: Especificas el puerto que deseas exponer el FW. Quien llegue a eso entra.
- LoadBalancer: Solo para cloud pública.
- ExternalName: Redirección a nivel de DNS.
- Service without selector: Para conexiones sin endpoint, como conectarte a una base de datos.