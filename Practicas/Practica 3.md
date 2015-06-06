#PRÁCTICA 3

##Balanceo de carga

			##El servidor web nginx
* Lo primero que vamos a hacer, una vez hayamos instalado nuestra tercera máquina
  virtual, es proceder a instalar nginx sobre dicha máquina. Para ello, seguiremos los
  siguientes pasos:

* Lo primero que debemos hacer es importar la clave del repositorio de software.
  Para ello, ejecutamos los comandos siguientes:
   * cd /tmp/
   * wget http://nginx.org/keys/nginx_signing.key
   * apt-key add /tmp/nginx_signing.key
   * rm -f /tmp/nginx_signing.key

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/1.jpg)


* A continuación, añadiremos el repositorio. Para ello, editaremos el fichero
	/etc/apt/sources.list y añadiremos al final las siguientes líneas:
* echo "deb http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list
* echo "deb-src http://nginx.org/packages/ubuntu/ lucid nginx" >> /etc/apt/sources.list

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/2.png)

* Ahora ya podremos proceder a la instalación del nginx. Tan solo ejecutaremos
  los comandos “sudo apt-get update” y seguidamente “sudo apt-get install nginx”.
* Ahora que ya que lo hemos instalado, vamos a proceder a su configuración como
  balanceador de carga. Para ello, editaremos el fichero “/etc/nginx/conf.d/default.conf”,
  borraremos todo lo que haya, e introduciremos lo siguiente:

upstream apaches {
	server 192.168.56.101;
	server 192.168.56.102;
}

server{
	listen 80;
	server_name m3lb;
	access_log /var/log/nginx/m3lb.access.log;
	error_log /var/log/nginx/m3lb.error.log;
	root /var/www/;
	location /
	{
		proxy_pass http://apaches;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header	X-Forwarded-For
		$proxy_add_x_forwarded_for;
		proxy_http_version 1.1;
		proxy_set_header Connection "";
	}
}

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/3.png)

* Una vez hayamos acabado de configurar dicho fichero, lanzaremos el servicio nginx
con el comando **“sudo service nginx restart”.** Si no hay ningún error en lo que hemos
configurado anteriormente, deberá iniciarse sin problemas, tal y como se observa en la
siguiente imagen.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/4.png)

* Una vez todo esté funcionando correctamente, probaremos el balanceador haciendo
  peticiones a la IP de esta máquina:
  **curl http://192.168.56.103**

* Esto nos saldrá cuando lo hagamos a la Ip de nuestro balanceador.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/5.png)

* Vemos que en cada peticion va cambiando de máquina.

* Por otro lado, si sabemos que alguna de las máquinas finales es más potente, podemos
  modificar la definición del upstream para pasarle más tráfico que al resto de las del
  grupo. Para ello, tenemos un modificador llamado weight, al que le damos un valor
  numérico que indica la carga que le asignamos. Por defecto tiene el valor 1, por lo que
  si no modificamos ninguna máquina del grupo, todas recibirán la misma cantidad de
  carga. Para probar esto, daré más prioridad a la máquina 2, suponiendo que es más
  potente que la dos. Para ello, pondremos el código de la siguiente manera:

upstream apaches {
	server 192.168.56.101 weight=2;
	server 192.168.56.102 weight=1;
}

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/6.png)

* Volvemos a hacer peticiones a la ip del balanceador y vemos como la máquina 2 da mas respuestas

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/7.png)

* Para que todo el tráfico que venga de una IP se sirva durante toda la sesión por el
  mismo servidor final, usaremos la directiva ip_hash al definir el upstream y para especificar a nginx 
  que use keepalive, debemos modificar nuestro upstream añadiendo la directiva keepalive y un tiempo de 
  mantenimiento de la conexión en segundos:

upstream apaches {
	ip_hash;
	server 192.168.56.101 weight=2;
	server 192.168.56.102 weight=1;
	keepalive 3;
}

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/8.png)

* Volvemos a hacer peticiones a la ip del balanceador y vemos como siempre nos sirve la máquina 2.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/9.png)

			##Balanceo de carga con haproxy
* Primero ejecutaremos el comando **“sudo apt-get install haproxy”**.
* Una vez instalado, debemos modificar el archivo **/etc/haproxy/haproxy.cfg** ya que la
  configuración que trae por defecto no nos vale. Así pues, tras consultar cuál es la IP de
  la máquina balanceadora, y anotar las IP de las máquinas servidoras, editamos el fichero
  de configuración de haproxy, Para ello, ejecutamos el comando **“sudo nano /etc/haproxy/haproxy.cfg”**

* Por defecto, el archivo de configuración de haproxy no nos vale, lo borraremos y lo
sustituiremos por el siguiente código:

global
	daemon
	maxconn 256

defaults
	mode http
	contimeout 4000
	clitimeout 42000
	srvtimeout 43000

frontend http-in
	bind *:80
	default_backend servers

backend servers
	server m1 192.168.56.101:80 maxconn 32
	server m2 192.168.56.102:80 maxconn 32
 
![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/10.png)

* Antes de comenzar con haproxy, pararemos el servicio de nginx, porque si no no funcionará haproxy. 
  Para detenerlo, usaremos el comando “sudo service nginx stop”.
* Ahora lanzaremos el servicio haproxy mediante el comando:
  **sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg**

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/11.png)

* Si no nos sale ningún error o aviso, todo ha ido bien, y ya podemos comenzar a hacer peticiones a la IP del balanceador.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%203/maquina%203/12.png)
