 # Detalles de la autorización y la auntenticación

 - Authentication es de dónde vienen los usuarios que vemos en kubectl config view.

 - Authorization es lo que pueden hacer. Detrás de ella está RBAC para tratar las diferentes opciones.
    - Para ver lo que se puede hacer o no está:
    > kubectl auth can-i ...