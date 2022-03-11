# Uso de yaml para servicios

- Podemos tener un archivo como este

```
(base) liacob@DESKTOP-9HJ41J9:~/repos/ckad_training$ cat materials/service.yml 
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```
