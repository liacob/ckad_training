# Para acceder a pods

- Hay varias formas de acceder, una muy simple es el forwarding de puertos

>> kubectl apply -f nginx.yaml
>> kubectl port-forward pod/nginx 8080:80 (Con uso de bg para ponerlo en el background)
>> curl http://localhost:8080

- Esto es útil para comprobar accesibilidad, NO para mostrarlo a usuarios externos.

- Otras formas más avanzadas son los servicios y el ingress.

