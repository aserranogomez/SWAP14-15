# Práctica 5 - Replicación de bases de datos MySQL

## Crear una BD e insertar datos

* En primer lugar, en la máquina 1 que será la principal, usamos el comando **mysql –u root -p**, y pulsamos "enter", y ya estaremos dentro de mysql.

* Una vez dentro, crearemos la base de datos de contactos: **create database contactos;**
* Cuando este creada, debemos seleccionar la base de datos creada para, ello hacemos **use contactos;**
* Hacemos un **show tables** para mostrar las tablas, pero como solo hemos creado la base de datos no habrá ninguna tabla aún.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/1-maquina1.png)

* Ahora pasaremos a crear tablas y volveremos a realizar un **show tables** para comprobar que se han creado.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/2-maquina1.png)

* Ahora vamos a proceder a incluir datos.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/3-maquina1.png)

* Ahora haremos que nos muestre todo lo que contenga: **select * from datos**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/4-maquina1.png)

* Comprobamos los valores.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/5-maquina1.png)

* Salimos de mysql con: **quit**

## Replicar una BD MySQL con mysqldump

* La sintaxis de uso es: 
**mysqldump ejemplodb -u root -p > /root/ejemplodb.sql** . 
Esto puede ser suficiente, pero tenemos que tener en cuenta que los datos pueden estar actualizándose constantemente en el servidor de BD principal. En este caso, antes de hacer la copia de seguridad en el archivo .SQL debemos evitar que se acceda a la BD para cambiar nada. Así, en el servidor de BD principal (maquina1) hacemos: **mysql -u root –p** y despues ponemos: **FLUSH TABLES WITH READ LOCK;**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/6-maquina1.png)

* Ahora ya sí podemos hacer el **mysqldump** para guardar los datos. En el servidor principal (maquina1) hacemos: 
**mysqldump contactos -u root -p > /root/contactos.sql**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/7-maquina1.png)

* Como habíamos bloqueado las tablas, debemos desbloquearlas (quitar el “LOCK”):
```
mysql -u root –p
UNLOCK TABLES;
quit
```

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/8-maquina1.png)

## AHORA EN LA SEGUNDA MÁQUINA

* Ya podemos ir a la máquina esclavo (maquina2, secundaria) para copiar el archivo .SQL con todos los datos salvados desde la máquina principal (maquina1):
**scp root@maquina1:/root/contacot.sql /root/**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/1-maquina2.png)

* Es importante destacar que el archivo .SQL de copia de seguridad tiene formato de texto plano, e incluye las sentencias SQL para restaurar los datos contenidos en la BD en otra máquina. Sin embargo, la orden mysqldump no incluye en ese archivo la sentencia para crear la BD (es necesario que nosotros la creemos en la máquina secundaria en un primer paso, antes de restaurar las tablas de esa BD y los datos contenidos en éstas). Con el archivo de copia de seguridad en el esclavo ya podemos importar la BD completa en el MySQL. Para ello, en un primer paso creamos la BD: 
```
mysql -u root –p
CREATE DATABAS ejemplodb;
quit
```

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/2-maquina2.png)

* Y en un segundo paso restauramos los datos contenidos en la BD (se crearán las tablas en el proceso): 
**mysql -u root -p ejemplodb < /root/contactos.sql**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/3-maquina2.png)

* A continuación, vamos a comprobar que realmente se ha realizado. Para ello, haré dentro de "mysql" un **describe datos** y un **select * from datos**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/4-maquina2.png)

## Replicación de BD mediante una configuración maestro-esclavo (opcional)

* La opción anterior funciona perfectamente, pero es algo que realiza un operador a mano. MySQL tiene la opción de configurar el demonio para hacer replicación de las BD sobre un esclavo a partir de los datos que almacena el maestro. Se trata de un proceso automático que resulta muy adecuado en un entorno de producción real. Implica realizar algunas configuraciones, tanto en el servidor principal como en el secundario. En las siguientes URLs se detallan estas configuraciones:

* Lo primero que debemos hacer es la configuración de mysql del maestro. Para ello editamos, como root, el /etc/mysql/my.cnf para realizar las modificaciones que se describen a continuación. Comentamos el parámetro bind-address que sirve para que escuche a un servidor: **bind- address 127.0.0.1** y le añadimos:
```
log_bin = /var/log/mysql/mysql-bin.log
log_error= /var/log/mysql/error.log
server-id=1
```

* Y lo guardamos y lo reiniciamos con **sudo /etc/ini.d/mysql restart**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd3-maquina1.png)

* En siguiente lugar, vamos a proceder a crear el usuario. Para ello, ejecutaremos los siguientes comandos (dentro de mysql):
```
CREATE USER esclavo IDENTIFIED BY 'slave_user';
GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%' IDENTIFIED BY 'usuario';
FLUSH PRIVILEGES;
FLUSH TABLES;
FLUSH TABLES WITH READ LOCK;
```

* Para finalizar con la configuración en el maestro obtenemos los datos de la base de datos que vamos a replicar para posteriormente usarlos en la configuración del esclavo: **SHOW MASTER STATUS;**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd2-maquina1.png)

* Volvemos a la máquina esclava, entramos en mysql y le damos los datos del maestro. Como indicábamos antes, estos datos se pueden introducir  directamente en el archivo de configuración si trabajamos con versiones inferiores a mysql 5.5. Si no es así, en el entorno de mysql ejecutamos la siguiente sentencia (ojo con la IP, "master_log_file" y del "master_log_pos" del maestro):
```
CHANGE MASTER TO MASTER_HOST='192.168.1.101', MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='bin.000001', MASTER_LOG_POS=501, MASTER_PORT=3306;
```

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd2-maquina2.png)

* Por último, arrancamos el esclavo y ya está todo listo para replicar los datos que se introduzcan/modifiquen/borren en el servidor maestro:
**START SLAVE;**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd3-maquina2.png)

* Ahora, podemos hacer pruebas en el maestro y deberían replicarse en el esclavo automáticamente. Por último, volvemos al maestro y volvemos a activar las tablas para que puedan meterse nuevos datos en el maestro: **UNLOCK TABLES;**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd4-maquina1.png)

* Ahora si queremos saber que todo funciona perfectamente y que el esclavo no tiene ningún problema para acceder al maestro y copiar la base de datos, nos vamos al esclavo y con la siguiente orden: **SHOW SLAVE STATUS\G** y viendo que el valor de la variable "Seconds_Behind_Master" es distinto de null todo funciona perfectamente, sino, es que existen errores.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd5-maquina2.png)

* Para comprobar que todo funciona, debemos ir al maestro e introducir nuevos datos a la base de datos y luego ir al esclavo para reavisar que las dos máquinas tienen la misma base de datos.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd5-maquina1.png)
![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica5/bd6-maquina2.png)





