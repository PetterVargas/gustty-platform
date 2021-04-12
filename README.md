# gustty-platform
Gustty - La ayuda para empresas en camino al fracaso.
Todos los servicios de Gustty comenzaron desde un único repositorio

*Tenga en cuenta que este repositorio es solo para desarrollo local y no está diseñado para implementarse en ningún entorno de producción. Si no es un desarrollador y solo quiere probar Gustty puede consultar nuestra [demostración en vivo] (https://demo.gustty.com/).*

## Requerimientos
1. [Docker](https://docs.docker.com/install/)
2. [Docker Compose](https://docs.docker.com/compose/install/)

## How to run it?
1. Clone el repositorio:

```
$ git clone https://github.com/PetterVargas/gustty-platform.git --recursive --jobs 3
```

2. Estamos utilizando carpetas compartidas para permitir la recarga de código en vivo. Sin esto, Docker Compose no se iniciará:
    - Windows / MacOS: Agregue el directorio clonado `gustty-platform` a los directorios compartidos de Docker (Preferencias -> Recursos -> Uso compartido de archivos).
    - Windows / MacOS: Asegúrese de que en las preferencias de Docker haya dedicado al menos 5 GB de memoria (Preferencias -> Recursos -> Avanzado).
    - Linux: No se requiere ninguna acción, el uso compartido ya está habilitado y la memoria para el motor Docker no está limitada.

3. Ingrese al repositorio clonado:
```
$ cd gustty-platform
```

4. Cree la aplicación:
```
$ docker-compose build
```

5. Aplicar migraciones de Django:
```
$ docker-compose run --rm api python3 manage.py migrate
```

6. Recopile archivos estáticos:
```
$ docker-compose run --rm api python3 manage.py collectstatic --noinput
```

7. Complete la base de datos con datos de ejemplo y cree el usuario administrador:
```
$ docker-compose run --rm api python3 manage.py populatedb --createsuperuser
```
*Tenga en cuenta que el argumento `--createsuperuser` crea una cuenta de administrador para `admin@example.com` con la contraseña establecida en `admin`.*

8. Ejecute la aplicación:
''
$ docker-compose up
''
*Tanto el storefront como el dashboard son proyectos frontend bastante grandes y pueden tardar unos minutos en compilarse dependiendo de su CPU. Si no aparece nada en el puerto 3000 o 9000, espere hasta que aparezca `Compiled successfully` en la salida de la consola.*

## ¿Cómo actualizar los subproyectos a las últimas versiones?
Este repositorio contiene las versiones estables más recientes.
Cuando aparezca una nueva versión, extraiga la nueva versión de este repositorio.
Para actualizarlos todos a sus versiones más recientes, ejecute:
```
$ git submodule update --remote
```

Puede encontrar la última versión de Gustty, el Storefront y el Dashboard en sus repositorios individuales:

- https://github.com/PetterVargas/gustty
- https://github.com/PetterVargas/gustty-dashboard
- https://github.com/PetterVargas/gustty-storefront

## Cómo resolver problemas por falta de espacio disponible o errores de compilación después de la actualización

La mayoría de las veces, ambos problemas se pueden resolver limpiando el espacio que ocupan los contenedores viejos. Después de eso, volvemos a construir la plataforma completa.


1. Asegúrate de que la pila de Docker no se esté ejecutando
```
$ docker-compose stop
```

2. Eliminar volúmenes existentes
**¡Advertencia!** ¡Al dar en continuar, eliminará también el contenedor de su base de datos! Si necesita datos existentes, elimine solo los servicios que causan problemas. https://docs.docker.com/compose/reference/rm/
```
docker-compose rm
```

3. Construya contenedores nuevos
```
docker-compose build
```

4. Ahora puede ejecutar un entorno nuevo utilizando los comandos de la sección `¿Cómo ejecutarlo?`. ¡Hecho!

### Aún no hay espacio disponible

Si tiene problemas con la falta de espacio disponible, considere podar su caché de la ventana acoplable:

**¡Advertencia!** Esto eliminará:
  - Todos los contenedores detenidos.
  - Todas las redes no utilizadas por al menos un contenedor.
  - Todas las imágenes colgantes.
  - Todo el caché de compilación colgante.
  
  Más información: https://docs.docker.com/engine/reference/commandline/system_prune/
  
```
$ docker system prune
```

## ¿Cómo ejecutar partes de la aplicación?
  - `docker-compose up api worker` para servicios de backend solamente.
  - `docker-compose up` para servicios de backend y frontend.

## ¿Dónde están corriendo las aplicaciones?
- Gustty Core (API) - http://localhost:8000
- Gustty Storefront - http://localhost:3000
- Gustty Dashboard - http://localhost:9000
- Jaeger UI (APM) - http://localhost:16686
- Mailhog (Test email interface) - http://localhost:8025

Si usted tiene preguntas o feedback, puede contactarnos en https://PeterVargas.com

#### Creado con ❤️ por [Peter Vargas](http://PeterVargas.com)