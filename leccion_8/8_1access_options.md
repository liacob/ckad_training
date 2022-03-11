# Veamos cómo exponer los pods al exterior

- Los pods no se pueden acceder directamente desde el exterior.

- Surge el concepto de ``Service`` que es una especie de balanceador de carga.
    - Te genera una IP que al conectarte a ella te dirige a uno de los pods que haya asociados.

- Al service se puede acceder directa o indirectamente, lo segundo conlleva trabajar con el ``ingress`` (DNS).

- Para acceder en local se puede tirar de ``port forwarding`` y ya está.

