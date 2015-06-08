# Práctica 2-Clonar la información de un sitio web

## CREAR UN TAR CON FICHEROS LOCALES EN UN EQUIPO

* En primer lugar, crearé un archivo llamado prueba en la primera máquina virtual. Para ello, ejecutaré el comando **touch prueba**.

* Una vez creado el archivo, ejecutaremos: **tar czf - prueba | ssh equipodestino 'cat > ~/tar.tgz'** . Cuando lo ejecute, me pedirá la contraseña de la máquina virtual correspondiente a la Ip a través de la cual me conecto, en este caso, a la segunda máquina que tenemos creada.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/tar.jpg)

* Y tras introducir la contraseña, el proceso se realizará. Ya solo nos queda comprobarque realmente se ha enviado.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/tar2.jpg)


## INSTALAR LA HERRAMIENTA RSYNC

* Instalamos la herramienta "rsync" ejecutando **sudo apt-get install rsync**

* Una vez que hemos instalado dicha herramienta, vamos a proceder a ejecutarla. Ahora, podemos probar el funcionamiento, clonando una carpeta cualquiera, por ejemplo, la carpeta con el contenido del servidor web. En la máquina 2 (secundaria) ejecutaremos:                        **rsync -avz -e ssh root@maquina1:/var/www/ /var/www/**

* Y comprobamos que el directorio /var/www/html/ se ha clonado correctamente con **´ls -la /var/www/html/**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/clonado-maquina2.jpg)

## ACCESO SIN CONTRASEÑA PARA SSH

Los pasos que seguiremos para acceder a “ssh” sin necesidad de introducir la contraseña serán los siguientes:

* Ejecutaremos en la máquina secundaria el siguiente comando: **ssh-keygen -t dsa**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/clave-maquina2.jpg)

* Después copiamos la clave pública al equipo remoto, para ello ejecutamos: **ssh-copy-id -i .ssh/id_dsa.pub root@maquina1**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/ssh-maquina2.jpg)

* Una vez hecho esto, ya podremos acceder desde la máquina 2 a la máquina 1 sin necesidad de introducir la contraseña.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/ssh2-maquina2.jpg)

## PROGRAMAR TAREAS CON CRONTAB

* Por último, nos quedará programar una tarea con “crontab”. La tarea que vamos a programar es la de actualizar el directorio “/var/www” de nuestra máquina 2, copiando la de la máquina 1. Para ello, lo único que tendremos que hacer es ir al fichero “/etc/crontab” de la máquina 2, tal y como se observa en la imagen.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/cron-maquina2.jpg)

* Y añadiremos una nueva línea: **01 *    * * * root rsync -avz -e ssh root@maquina1:/var/www/html/ /var/www/html/**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica2/cron2-maquina2.jpg)

