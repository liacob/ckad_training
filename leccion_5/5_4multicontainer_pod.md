# Recomendaciones

- En general es un contenedor por pod al ser más fáciles de crear y mantener.

- Hay algunos casos en los que hay varios en un pod:
    - Sidecar container: contenedor que solo es una mejora del principal, como por ejemplo un logging, monioring, syncing.
        - Acceso a recursos compartidos para intercambiar información.
    - Ambassador container: contenedor que presenta el principal al exterior, como proxy.
    - Adapter container: contenedor que adopta el tráfico o los datos para cuadrarlo con algún patrón en otras aplicaciones del clúster.

- Suele haber almacenamiento compartido.