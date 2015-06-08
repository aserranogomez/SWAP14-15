# Práctica 6 - Discos en RAID

## Configuración del RAID por software

Partimos de una máquina virtual ya instalada y configurada a la que, estando apagada, añadiremos dos discos del mismo tipo y capacidad. Para añadir los dos discos haremos lo siguiente:
*Pincharemos con el botón derecho sobre nuestra máquina virtual y pulsaremos *configuración*.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/1.png)

* Seguidamente, pulsaremos sobre *Almacenamiento* y donde pone *Controlador:SATA* pulsaremos con el botón derecho y le daremos a *Agregar disco duro*. Los dos discos duros que agregaremos deberán ser del mismo tipo y de la misma capacidad.
* No saldrá una ventana emergente en donde deberemos pulsar sobre *Crear nuevo disco*.

!img[img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/2.png)

* En la primera ventana pulsaremos sobre *Next* tal cual aparece en la imagen.
* Dejaremos marcado *Reservado dinámico* y pulsaremos sobre *Next*.
* Le daremos 8 GB de tamaño al igual que el disco principal y le daremos a *Crear*.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/3.png)

* Y ya tendremos creado el primer disco.
* Ahora para crear el segundo seguiremos los mismos pasos anteriores, así que no pondré los pasos, a continuación muestro una imagen con los dos discos creados.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/4.png)

- Ahora arrancamos nuestra máquina virtual y entramos para instalar el software necesario para configurar el RAID. Para ello, seguiremos los siguientes pasos: 

* En primer lugar, ejecutaremos el comando **sudo apt-get install mdadm** .
* En la siguiente imagen, pulsaremos *aceptar* dejando la opción de *Sitio de internet*.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/5.png)

* Dejaremos el nombre por defecto y pulsaremos sobre *aceptar*.

* Una vez terminada la instalación, debemos buscar la información (identificación asignada por Linux) de ambos discos. Vamos a ejecutar el comando **sudo fdisk -l**. Al ejecutar este comando obtendremos la información de los discos.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/7.png)

* Ahora ya podemos crear el RAID 1, usando el dispositivo /dev/sdb, indicando el número de dispositivos a utilizar (2), así como su ubicación. Deberemos ejecutar el siguiente comando:
**sudo mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc** .

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/8.png)

* Ahora que ya hemos creado el dispositivo RAID, le damos formato ejecutando: **sudo mkfs /dev/md0** .

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/9.png)

* Ahora ya podemos crear el directorio en el que se montará la unidad del RAID. Hacemos lo siguiente:
```
sudo mkdir /datos
sudo mount /dev/md0 /datos
```
* Y para comprobar el estado del RAID, ejecutamos: **sudo mdadm --detail /dev/md0**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/11.png)

* Para finalizar el proceso, conviene configurar el sistema para que monte el dispositivo RAID creado al arrancar el sistema. Para ello debemos editar el archivo  */etc/fstab* y añadiremos la siguiente línea: **/dev/md0	/datos  ext2	defaults	0	0** .

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/12.png)

* Ahora reiniciaremos la máquina. En este momento nos dará un error de que no ha encontrado el dispositivo datos, así que pulsaremos “S” y seguiremos arrancando el sistema.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/13.png)

* A continuación, entraremos como root haciendo **sudo su**, y listaremos los dispositivos *md* que existen.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/15.png)

* Con esta información volveremos a editar el fichero */etc/fstab* y modificamos la línea que nos montaba el sistema RAID. Lo que haremos será cambiar *md0* por *md127*. La línea deberá quedar tal y como muestro a continuación.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/16.png)

* Volvemos a montarlo utilizando **mount -a**. Y lo comprobamos utilizando el comando **mdadm -detail /dev/md127** .

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/17.png)
![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/20.png)

* Ya podremos reiniciar el sistema y comprobar que ya no nos sale el error de antes y cada vez que se arranca se monta automáticamente.

## Simular fallo en uno de los discos del RAID (opcional)

* En caso de que uno de los discos fallase, habría que sacar del raid el disco fallido y reemplazarlo por otro, con el cual habría que seguir todos los pasos anteriores para prepararlo.
 
Forzamos el disco como fallido para hacer la prueba:
* Primero vamos a comprobar que todos funcionan tal y como muestro en la imagen ejecutando **cat /proc/mdstat**.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/19.png)

* Ahora vamos a producir un fallo ejecutando el comando: **mdadm --manage /dev/md127 --fail /dev/sdc1** .

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/21.png)

* Ahora volvemos a ejecutar **sudo mdadm --detail /dev/md127** y tras mostrar la información podemos comprobar en la imagen que el disco *sdc* tiene un fallo.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/22.png)

* Ahora que el disco ha fallado, vamos a proceder a borrarlo. Para ello, ejecutaremos lo siguiente: **sudo mdadm --remove /dev/md127 /dev/sdc** .
* Y volvemos a mostrar los detalles para comprobar que aparece *removed*.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/23.png)

* Limpiamos cualquier información previa de un disco RAID. Para ello ejecutamos la orden: **sudo mdadm --zero-superblock /dev/sdc**.

* Aún retirado el disco podremos acceder a la información ya que aún disponemos del disco *sdb*. Ahora vamos a proceder a volver a añadirlo. Para ello ejecutamos lo siguiente: **sudo mdadm --add /dev/md127 /dev/sdc**

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/24.png)

* Si volvemos a comprobar los detalles veremos que el disco vuelve a estar, pero está en estado *spare rebuilding*.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/25.png)

* Por último, volverá a estar activo.

![img](https://github.com/manolotello7/SWAP14-15/blob/master/Im%C3%A1genes/Pr%C3%A1ctica6/26.png)










