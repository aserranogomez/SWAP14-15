#EJERCICIOS TEMA 3

##Ejercicio 3.1

* Para configurar el enrutamiento en Linux podemos utilizar el comando **iptables**, que permite, entre otras cosas, filtrar paquetes. Esta herramienta está construida sobre Netfilter, un framework disponible en el núcleo de Linux que permite interceptar y manipular paquetes de red.

* Para manipular las tablas de enrutamiento se puede utilizar también el comando **route**. Además, para que el ordenador funcione como un router y permita realizar el reenvío de paquetes es necesario modificar la variable **ip_forward** del sistema y ponerla a 1.

* Para configurar el enrutamiento en Windows se puede utilizar también el comando **route**. Sin embargo, también podemos utilizar herramientas gráficas como el servicio de enrutamiento y acceso remoto. Existe una tercera opción, que es el contexto IP de enrutamiento Netsh, que incluye varios comandos para mostrar y administrar la tabla de enrutamiento IP. 

##Ejercicio 3.2

**Windows**

Para agregar un filtro de paquetes en "Windows" seguimos los siguientes pasos:

* Abrimos Enrutamiento y acceso remoto.
* En el árbol de consola, hacemos clic en General. (Enrutamiento y acceso remoto/nombre de servidor/[IPv4 o IPv6]/General)
* En el panel de detalles, hacemos clic con el botón secundario en la interfaz donde deseamos un filtro y, a continuación, hacemos clic en Propiedades.
* En la ficha General, hacemos clic en Filtros entrantes o Filtros salientes.
* En el cuadro de diálogo Filtros entrantes o Filtros salientes, hacemos clic en Nuevo.
* En el cuadro de diálogo Agregar filtro IP, escribimos la configuración del filtro y, a continuación, hacemos clic en Aceptar.
* En Acción de filtrado, seleccione la acción de filtrado apropiada y, a continuación, haga clic en Aceptar.

**Linux**

En linux podemos utilizar "netfilter" para el filtrado y bloqueo de paquetes"

Esta utilidad es interna en el kernel de Linux e incorpora las siguientes tres tablas o listas de reglas:

* filter La tabla por defecto para el manejo de paquetes de red.

* nat Usada para alterar paquetes que crean una nueva conexión y utilizada para la Traducción de direcciones de red (Network Address Translation, NAT).

* mangle Usada por tipos específicos de alteración de paquetes.

Cada una de estas tablas tiene un grupo de cadenas incorporadas que corresponden a las acciones llevadas a cabo en el paquete por netfilter.

Las cadenas internas para la tabla filtro son las siguientes:

* INPUT Aplica a los paquetes recibidos a través de una interfaz de red.

* OUTPUT Esta cadena sirve para paquetes enviados por medio de la misma interfaz de red que recibió los paquetes.

* FORWARD Esta cadena sirve para paquetes recibidos en una interfaz de red y enviados en otra.

El comando iptables configura estas tablas, así como también configura nuevas tablas si es necesario.
