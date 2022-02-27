# Opciones de los pods

- Tenemos etcd, la base de datos.

- Tenemos los pods con su(s) container(s)

- Tenemos varios comandos:

>> kubectl describe <``objeto``> <``nombre``>
Que apunta a etcd y nos da todos los settings del objeto. Viene bien porque muchos atributos se ponen a valor por defecto.

>> kubectl logs
Que apunta a los pods dándote el stdout

>> kubectl exec
Que te da una shell. Apunta al container