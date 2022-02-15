- Podemos crear los yaml nosotros con ejemplos y cosas

- Otra opción es lanzarlo de manera imperativa y pasarlo a yaml
> kubectl create ns production -o yaml
> kubectl run  nginx-prod --image=nginx -o yaml > lab5_2.yaml