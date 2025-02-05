# Proyecto: Despliegue de Aplicación Flask con Vagrant, Gunicorn y Nginx

## Descripción
Este proyecto describe los pasos necesarios para configurar un entorno de desarrollo y despliegue de una aplicación Flask utilizando Vagrant, Gunicorn y Nginx en una máquina virtual.

## Requisitos Previos
- Vagrant instalado en la máquina anfitrión.
- VirtualBox u otro proveedor de virtualización compatible.
- Instalar nginx de manera apropiada(mirar mis otros enlaces de github)


### Que debo hacer para que funcione?

    1. Copiar el contenido que viene en mi vagrant file para instalar todas las dependencias y paquetes necesarios del proyecto
    2. Acceder a nuestra maquina virtual mediante:
        - vagrant ssh
    3. Una vez en nuestra maquina virtual ejecutamos el comando:
        - sudo systemctl start flask_app
    
    Y con esto nuestra aplicacion con flask estaria arrancada y lista para usarse.



