# YAML

- Muy fácil de leer y utilizar.

- Basado en indentaciones (espacios, no tabs) para crear las relaciones.

- Consejo para hacerlo más fácil en vim, en ~/.vimrc:

> autocmd FileType yaml setlocal ai ts=2 sw=2 et

- Los manifest ingredents son: apiVersion / kind / metadata / spec

- Para contenedores se necesitan estas specs:
    - name
    - image
    - command
    - args
    - env

- Se pueden ver con:
> kubectl epxlain pods.<spec_que_sea>
