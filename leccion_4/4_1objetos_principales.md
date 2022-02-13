# Objetos principales de Kubernetes

- **Contenedores**: Que son orquestados como ya sabemos.
- **Pods**: Objetos que pueden contener varios contenedores que comparten volúmenes, son los que llevan la IP. Son la unidad básica de Kubernetes.
- **Deployment**: Es la aplicación en sí, contiene uno o varios pods. En ellos se especifica la replicación y la estrategia de actualización.
- **Service**: Para acceder a los deployments. Se basan en ```labels```. Sirve como balanceadores de carga.
    - El usuario se conecta al servicio. Que distribuye la carga entre las réplicas del deployment con round-robin por defecto.
    - Da combinación IP/puerto única para acceder.
- **PV**: Persistent Volume, donde guardar los datos de la app ya que si no se pierden. Se relaciona mediante el Persisten Volume Claim a los volúmenes del pod.
- **Ingress**: Capa por encima del Service que propociona accesibilidad flexible a los servicios.