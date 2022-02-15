# Hay que usar kubectl de manera declarativa

- La cosa es escribir tu manifiesto y lanzarlo con kubectl create|apply -f manifest.yaml

- Esto da mucho más control.

# Generación sencilla de .yaml

- Obtener el estado actual de un objeto:
> kubectl get deployments nginx --export -o yaml
    - Habría que borrar cosas como el status o timestamp
    
- Push de la nueva configuración:
> kubectl replace -f nginx.yaml

- Aplicar configuración de un manifiesto:
> kubectl apply -f nginx.yaml

- Otra opción es https://kubernetes.io/docs y hacer copia pega.