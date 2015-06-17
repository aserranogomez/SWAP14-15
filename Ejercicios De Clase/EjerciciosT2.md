#EJERCICIOS TEMA 2

##Ejercicio 2.1
```
-> Disponibilidad Web:

· Disponibilidad Web1 = 85%
· Disponibilidad Web2 = 85%+(1-85%)*85% = 97.75%
· Disponibilidad Web3 = 97.75%+(1-97.75%)*85% = 99.6625%

-> Application:

· Application1 = 90%
· Application2 = 90%+(1-90)*90% = 99%
· Application3 = 99%+(1-99%)*90% = 99.9%

-> Database:

· Database1 = 99.9%
· Database2 = 99.9%+(1-99.9%)*99.9% = 99.9999%
· Database3 = 99.9999%+(1-99.9999%)*99.9% = 99.99999999%

-> DNS:

· DNS1 = 98%
· DNS2 = 98%+(1-98%)*98% = 99.96%
· DNS3 = 99.96%+(1-99.6%)*98% = 99.9992%

-> Firewall:

· Firewall1 = 85%
· Firewall2 = 85%+(1-85%)*85% = 97.75%
· Firewall3 = 97.75%+(1-97.75%)*85% = 99.6625%

-> Switch:

· Switch1 = 99%
· Switch2 = 99%+(1-99%)*99% = 99.99%
· Switch3 = 99.99%+(1-99.99%)*99% = 99.9999%

-> Data center:

· Data Center1 = 99.99%
· Data Center2 = 99.99%+(1-99.99%)*99.99% = 99.999999%
· Data Center3 = 99.999999%+(1-99.999999%)*99.99% = 100%

-> ISP:

· ISP1 = 95%
· ISP2 = 95%+(1-95%)*95% = 99.75%
· ISP3 = 99.75%+(1-99.75%)*95% = 99.9875%

-> Disponibilidad Total:

· Disponibilidad total = 99.6625% \* 99.9% \* 99.99999999% \* 99.9992% \* 99.6625% \* 99.9999% \* 100% \* 99.9875% = 99.21351663%
```

##Ejercicio 2.2

* Zope:

Zope es un framework escrito en python que puede configurarse para escenarios donde se requieren funcionamiento de alta disponibilidad a través de configuraciones con Servidores Web como Apache, Nginx, Zope; con Proxies / Balanceador de Carga como HAProxy, Pound, Squid; con servidor de Cacheo Web Externo como Varnish, Squid, Apache y Memcache; replicación de Base de Datos con la librería Relstorage o Neopod.

* webControl CMS:

webControl CMS está preparado para entornos de alta disponibilidad y alto rendimiento tales como servidores balanceados, clusters, granjas de servidores y entornos distribuidos.

* MySQL Fabric:

MySQL Fabric es un sistema para gestionar granjas de servidores MySQL para lograr una Alta Disponibilidad

##Ejercicio 2.3

* Top: Sirve para conocer en tiempo real la actividad del procesador, así como una lista con los procesos que hacen un mayor uso de la CPU.
Gnome-System-Monitor: es una aplicación (no instalada previamente) que permite monitorizar tanto los procesos como los sistemas de memoria y red.
* Munin: es un sistema de monitorización de código libre, que permite monitorizar la infraestructura y la red, y avisa de cualquier cambio o problema. Presenta además toda la información en gráficos a través de una interfaz web.
* Nagios: es un sistema de monitorización de código libre, que permite monitorizar servicios de red y recursos de sistemas hardware.
* Ganglia: permite monitorizar sistemas de cómputo distribuidos y permite una gran escalabilidad.
* Zabbix: es también de código libre y permite monitorizar el sistema.
* Awstats: es un monitor específico de servidores web.


##Ejercicio 2.4
```
* Balanceadores Software:
        VLM-200
        VLM-1000

* Balanceadores Hardware:
        LM-2400 Server Load Balancer
        LM-5400 Server Load Balancer

* Servidores de Aplicaciones:
        Oracle WebLogic Server
        WebSphere Application Server

* Servidores de Almacenamiento:
        HP P2000 G3
        IBM Storwize V3700
```
