# Trabajando con CURL para manejar objetos de Kubernetes

- Lo primero es iniciar el proxy con algo como:
> kubectl proxy --port=8001 &

- Esto ya te mete el TLS solo así que no worries.

- De ahí ya podemos pegarnos con el curl a muerte:
> curl http://localhost:8001
> curl http://localhost:8001/api/v1/namespaces/default/pods
