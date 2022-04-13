# Qué son los configMaps

- Son un tipo especial de volumen que se usan para separar datos dinámicos (generalmente Clave:Valor) de los estáticos dentro de un Pod.

- Hay varias formas de usarlos:
    - Para pasar variables a un Pod
    - Para pasarle argumentos a la línea de comandos
    - Al crearlo como volumen, los ficheros creados donde se indica reciben el nombre de la Clave y su contenido es el Valor

- Los Secrets son ConfigMaps pero codificados para llevar datos sensibles (no encriptados)

- Deben crearse antes que los Pods que los quieren usar.


# Fuentes de los configsMaps

- Directorios: Usar los distintos archivos de un directorio.
- Archivos: Usar los contenidos del fichero como tal.
- Valores literales: Sirven para nombrar variables o pasar a la línea de comandos.

- Existe el kustomization.yaml generator que te crea ConfigMap según lo que le pongas