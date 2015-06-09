#Comprobar el rendimiento de servidores web

##Comprobar el rendimiento de servidores web con Apache Benchmark

* En primer lugar, vamos a comenzar comprobando el rendimiento con Apache
Benchmark de una de nuestras máquinas servidoras, en mi caso en mi máquina 1,
haciendo peticiones IP a dicha máquina. Pero antes de comenzar, instalaré una cuarta
máquina que utilizaré para hacer las mediciones. Una vez hecho esto, crearemos un
pequeño documento html la máquina 1 (en el directorio /var/www), tal y como muestro 
en la siguiente imagen:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/1.png)

* Ahora desde la nueva máquina que he creado, comenzaremos a medir la máquina 1,
tecleando el siguiente comando:
**ab -n 1000 -c 5 http://192.168.56.101/pruebamedicion.html**
donde -n 1000 hace que se solicite mil veces esta página y -c 5 hace que se pidan de
cinco en cinco.

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/2.png)

* Nos quedaremos con las medidas de "Time taken for tests", "Failed requests", "Requests per
second", "Time per request".

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/3.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/4.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/5.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/6.png)

##MEDICIONES AL BALANCEADOR CON NGINX ARRANCADO

* A “pruebamedicion.html”
 Ahora deberemos ejecutar el mismo comando, pero pondremos la ip del balanceador.
```
**ab -n 1000 -c 5 http://192.168.56.103/ pruebamedicion.html**
```
![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/7.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/8.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/9.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/10.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/11.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/12.png)

##MEDICIONES AL BALANCEADOR CON HAPROXY ARRANCADO
```
* Para arrancar haproxy hacemos lo siguiente:
**sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg**
y apagamos nginx con:
**sudo /etc/ini.d/nginx stop**
Empezamos enviando peticiones a pruebamedicion.html
**ab -n 1000 -c 5 http://192.168.56.103/ pruebamedicion.html**
```
![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/13.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/14.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/15.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/16.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/17.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/18.png)

##MEDICIONES AL BALANCEADOR CON SIEGE

Para la ejecución de esta bateria de pruebas se ha usado el comando 
```
 **siege -b -t60S -v http://192.168.56.103/index.php**
```
Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/19.jpg)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/20.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/21.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/22.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/23.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/24.png)

##MEDICICIONES CON HTTPERF

* Lo haremos a pruebamedicion.html en la maquina 1. 
Ejecutaremos el comando que muestro a continuación en la imagen:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/25.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/26.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/27.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/28.png)

##MEDICICIONES CON HTTPERF CON HAPROXY ARRANCADO
```
* Para arrancar haproxy hacemos lo siguiente:
**sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg**
y apagamos nginx con:
**sudo /etc/ini.d/nginx stop**
```
Empezamos enviando peticiones a pruebamedicion.html, tal como muestro en la
imagen:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/29.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/30.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/31.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/32.png)

##MEDICICIONES CON HTTPERF CON NGINX ARRANCADO
```
* Para parar haproxy ejecutamos
**sudo service haproxy stop**
y arrancamos nginx con
**sudo service nginx start**
```
* Y empezamos a enviar peticiones a pruebamedicion.html, tal como muestro en la imagen:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/33.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/34.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/35.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/36.png)

##MEDICICIONES CON OPENWEBLOAD

* Comenzaremos haciendo peticiones a la máquina 1 mediante el comando que muestro a continuación.
```
**openload -l 20 http://192.168.56.101/pruebamedicion.html 10**
```
![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/37.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/38.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/39.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/40.png)

##OPENWEBLOAD CON NGINX ARRANCADO

* Hacemos peticiones a la ip del balanceador con el mismo comando usado antes
```
**openload -l 20 http://192.168.56.103/pruebamedicion.html 10**
```
![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/41.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/42.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/43.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/44.png)

##OPENWEBLOAD CON HAPROXY ARRANCADO

* Hacemos peticiones a la ip del balanceador con el mismo comando usado antes
```
**openload -l 20 http://192.168.56.103/pruebamedicion.html 10**
```
![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/45.png)

* Y estos son los resultados que obtenemos:

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/46.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/47.png)

![img](https://github.com/aserranogomez/SWAP14-15/blob/master/Imagenes/Practica%204/48.png)


