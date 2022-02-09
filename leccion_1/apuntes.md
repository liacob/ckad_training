# ¿Qué es un contenedor?

- Aplicación autocontenida lista para lanzarse != máquina virtual.
- Tiene todas las dependencias y librerías necesarias para ejecutarse son problemas.
- Necesitas algo para correr los contenedores (```container runtime```), que está en el host OS -> Todos los contenedores comparten kernel.

# Los contenedores son Linux

El hecho de compartir kernel entre host y contenedores no supone un problema porque los contenedores básicamente son linux siempre:

- Ofrecen características propias de Linux OS
- Los ```Linux Kernel Namespaces``` ofrecen aislamiento estricto entre los componentes:
    <ul>
    <li>red</li>
    <li>archivos</li>
    <li>usuarios</li>
    <li>procesos</li>
    <li>IPCs</li>
    </ul>

# Container runtimes

- Son los que permiten iniciar y correr los contenedores sobre el host.
- Llevan todos los procesos que no sean parte de la ejecución del contenedor como tal.
- Hay distintas soluciones:
    <ul>
    <li><strong>Docker</strong></li>
    <li>lxc</li>
    <li>runc</li>
    <li>cri-o</li>
    <li>rkt</li>
    </ul>

# Open Containers Initiative (OCI)

- Estandariza el uso de contenedores
    <ul>
    <li>Cómo empaquetar los contenedores en el sistema de archivos</li>
    <li>Cómo lanzar el sistema de archivos en un contenedor</li>
    </ul>

- Asegura compatibilidad entre contenedores independientemente de la solución de la que vengan.

# Docker

- Solución más famosa pero NO la única, el mercado cambia.
- Comenzó ofreciendo cómo crear, administrar, correr y compartir instancias e imágenes.

# Componentes de Docker

- ```Imágenes```: Entornos read-only que incluye la aplicación y sus dependencias.
- ```Registro```: Donde se guardan las imágenes, como DockerHub.
- ```Contenedores```: Entornos aislados donde corren las aplicaciones. Se aislan gracias a los namespaces.

