# Proyecto: Despliegue de Aplicación Flask con Vagrant, Gunicorn y Nginx

## Descripción
Este proyecto describe los pasos necesarios para configurar un entorno de desarrollo y despliegue de una aplicación Flask utilizando Vagrant, Gunicorn y Nginx en una máquina virtual.

## Requisitos Previos
- Vagrant instalado en la máquina anfitrión.
- VirtualBox u otro proveedor de virtualización compatible.
- Instalar nginx de manera apropiada(mirar mis otros enlaces de github)


### ¿Qué debo hacer para que funcione?

    1. Copiar el contenido que viene en mi vagrant file para instalar todas las dependencias y paquetes necesarios del proyecto
    2. Acceder a nuestra maquina virtual mediante:
        - vagrant ssh
    3. Una vez en nuestra maquina virtual ejecutamos el comando:
        - sudo systemctl start flask_app
    
    Y con esto nuestra aplicacion con flask estaria arrancada y lista para usarse.


### ¿Qué es Flask?

Flask es un framework ligero de Python para hacer aplicaciones web rápido y fácil. Te deja manejar rutas, peticiones y respuestas sin tanta complicación. Es ideal para proyectos pequeños, pero escalable mediante extensiones.

### ¿Qué es Gunicorn?

Gunicorn (Green Unicorn) es un servidor WSGI para aplicaciones Python, rápido y eficiente. Se encarga de ejecutar Flask (u otros frameworks) en producción, manejando múltiples conexiones a la vez; a dichas conexiones podemos establecer cuantos hilos queremos que se ejecute basados en nuestra cpu, estos hilos son usualmente llamados **workers**. Básicamente, es el que hace que tu app no se rompa cuando llegan muchas visitas.

