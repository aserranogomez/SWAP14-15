#PRÁCTICA 1

##Preparación de las herramientas

* Para esta primera práctica, crearemos dos máquinas virtuales con Ubuntu Server
12.04.1 LTS, y en ambas instalaremos un servidor web: Apache + PHP + MySQL.
Simplemente para esta práctica pondremos algunos pantallazos de las máquinas.
También instalaremos y probaremos cURL.

* Seleccionamos OpenSSh server y LAMP server para su instalación.

* Activamos la cuenta de “root” con *sudo passwd root*

* Y comprobamos la versión del servidor en la máquina 1.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%201/apache2-maquina1.png)

* Comprobamos que el servidor está en ejecución.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%201/apache-servidor-maquina1.png)

* Instalamos cURL, y comprobamos que funciona creando un archivo html en el directorio /var/www/ y ejecutamos curl http://192.168.71.131/hola.html

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%201/curlmaquina1.png)

* Ahora hacemos lo mismo con la máquina 2 y comprobamos la versión del servidor.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%201/maquina2%20Apache.png)

* Comprobamos que el servidor está en ejecución.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%201/maquina2-servidor-apache.png)

* Instalamos cURL, y comprobamos que funciona creando un archivo html en el directorio /var/www/ y ejecutamos curl http://192.168.71.130/hola.html

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%201/curl%20maquina2.png)
