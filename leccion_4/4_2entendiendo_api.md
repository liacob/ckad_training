# Cómo funciona la API

- No es LA API, porque hay varias (apps, almacenamiento, redes).
- Cada 3 meses hay una nueva versión.
- Los recursos se definen mediante archivos yaml que van a los archivos de configuración.
- Comandos útiles:
    - kubectl api-resources para ver qué pertenece a qué.
    - kubectl api-versions
    - kubectl explain <```resource```> te cuenta lo que necesitas en el yaml.
- Las conexiones a la API son mediante kube-proxy que se encarga de TLS. Puedes acceder mediante curl al proxy y llegas a la API tmb.