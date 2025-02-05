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
        - sudo systemctl restart flask_app
    
    Y con esto nuestra aplicacion con flask estaria arrancada y lista para usarse.


### ¿Qué es Flask?

Flask es un framework ligero de Python para hacer aplicaciones web rápido y fácil. Te deja manejar rutas, peticiones y respuestas sin tanta complicación. Es ideal para proyectos pequeños, pero escalable mediante extensiones.

### ¿Qué es Gunicorn?

Gunicorn (Green Unicorn) es un servidor WSGI para aplicaciones Python, rápido y eficiente. Se encarga de ejecutar Flask (u otros frameworks) en producción, manejando múltiples conexiones a la vez; a dichas conexiones podemos establecer cuantos hilos queremos que se ejecute basados en nuestra cpu, estos hilos son usualmente llamados **workers**. Básicamente, es el que hace que tu app no se rompa cuando llegan muchas visitas.


### Pasos que he seguido 

- Pasos previos
    - Hacer sudo apt-get update para actualizar nuestra maquina

    - Instalar nginx como viene en el vagrant file



Primero instalo pip mediante el comando:

`sudo apt-get install -y python3-pip`


Una vez hecho eso, instalé pipenv para crear y gestionar el entorno virtual, y python-dotenv para cargar las variables de entorno desde un archivo .env de manera sencilla.  Para ello los comandos: 

`pip3 install pipenv`
`pip3 install python-dotenv`

Despues creo la carpeta de mi aplicacion con:

`sudo mkdir -p /var/www/app`

y le pongo los permisos necesarios con: 

`sudo chown -R vagrant:www-data /var/www/app`
`sudo chmod -R 775 /var/www/app` 


copio todo el contenido de mi aplicacion  la carpeta /var/www/app con: 

`sudo cp -vr /vagrant/app_provision/.env /var/www/app`  
`sudo cp -vr /vagrant/app_provision/application.py /var/www/app`  
`sudo cp -vr /vagrant/app_provision/wsgi.py /var/www/app`

#### Recomendacion

**Puedes probar el servidor mediante el servidor de prueba que trae ya el propio flask,  arrancalo usando**  

`pipenv run flask run --host 0.0.0.0`

**O mediante el entorno virtual de pipenv**
- ejecutar el entorno virtual: `pipenv shell`

`flask run --host 0.0.0.0`


copio el contenido con la informacion del socket o servidor de flask que vamos a ejecutar en /etc/systemd/system con: 

`sudo cp -vr /vagrant/app_provision/flask_app.service  /etc/systemd/system/`

Una vez hecho esto habilito el servidor flask con:  

`sudo systemctl enable flask_app`


Por ultimo habilito la pagina en nginx/sites-available y le creo el enlace simbolico a nginx/sites-enabled y por ultimo recargo:
`sudo cp -vr /vagrant/app_provision/app.conf  /etc/nginx/sites-available/`
`sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/`
`sudo systemctl restart nginx`


### Vista de la aplicacion desplegada

![alt text](captures/image.png)