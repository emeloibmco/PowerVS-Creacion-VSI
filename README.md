# PowerVS-Creacion-VSI

En esta guía se muestra la creación de una instancia virtual de PowerVS.


## Índice  📰
1. [Pre-Requisitos](#Pre-Requisitos-pencil)
2. [Crear un Power Systems Virtual Server](#crear-un-power-systems-virtual-server)
3. [Crear una SSH key](#crear-una-ssh-key)
4. [Crear una subred](#crear-una-subred)
5. [Crear una Virtual Server instance](#crear-una-virtual-server-instance)
6. [Resizing instancia VSI](#resizing-de-la-instancia)
7. [Crear cloud connection](#creación-del-cloud-connection)
8. [Acceder a la consola del SO](#acceder-al-sistema-operativo-por-consola)
9. [Referencias](#Referencias-mag)
10. [Autores](#Autores-black_nib)
<br />

## Pre Requisitos :pencil:
* Contar con una cuenta en <a href="https://cloud.ibm.com/"> IBM Cloud</a>.
* Contar con un grupo de recursos específico para el despliegue de los recursos
<br />

## Crear un Power Systems Virtual Server
Para crear y configurar un servidor virtual Power Systems, tenga en cuenta los siguientes pasos:

1. Desde el catálogo busque Power Systems Virtual Server y haga click sobre el icono.
2. Esto lo llevara a una pestaña de configuración, aquí ingrese la siguiente información:
  * ```Select a location: ``` Seleccione la ubicación donde desea desplegar el servidor.
  * ```Service name: ``` Asígnele un nombre el servidor.
  * ```Select a resource group: ``` Seleccione el grupo de recursos con el cual quiere desplegar el servidor.
  * ```Tags :``` Asígnele etiquetas al servidor si lo considera necesario.
3. Luego de esto de click en el botón ```Crear```.

<p align="center">
<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/PSVS.gif>
</p>
<br />

## Crear una SSH key
1. Para crear una SSH key es necesario obtener primero una llave publica, a continuación, se muestra una de las opciones en las que se puede generar el par de llaves SSH para esto tenga en cuenta los siguientes pasos:
   * Acceda al *IBM Cloud Shell* e ingrese el siguiente comando: 
   ```
   ssh-keygen -t rsa -C "user_id" 
   ```
   * Al colocar el comando anterior, en la consola se pide que especifique la ubicación, en este caso oprima la tecla Enter para que se guarde en la ubicación sugerida. Posteriormente, cuando se pida la ```Passphrase ```coloque una contraseña que pueda recordar o guárdela, ya que se utilizará más adelante.
   * Muévase con el comando 
   ```cd .ssh``` a la carpeta donde están los archivos ```id_rsa.pub``` y ```id_rsa```. Estos archivos contienen las claves públicas y privadas respectivamente.
   * Copie el valor de la clave pública, utilice el comando: 
   ```
   cat id_rsa.pub
   ```
   
4. Una vez obtenida la llave publica en la Lista de recursos, seleccione su servicio de Servidor virtual de Power Systems.
5. Siga la ruta ```SSH keys > Create SSH key```.
6. Ingrese un nombre para la llave y pegue la llave publica creada anteriormente. 
<br />

## Crear una subred
Tenga en cuenta los siguientes pasos para crear una subred en el servidor virtual creado anteriormente:
1. En la Lista de recursos, seleccione su servicio de Servidor virtual de Power Systems.
2. Siga la ruta ```Subnets > Create subnet```
3. Esto abrirá una ventana de configuración, aquí ingrese la siguiente información:
  * ```Name```: Ingrese un nombre distintivo para la subred.
  * ```CIDR```: Ingrese el valor exacto de una dirección o un rango de direcciones IP tenga en cuenta la siguiente <a href="https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-configuring-subnet"> guía </a>  para evitar tener solapamientos con los segmentos de IBM Cloud al momento de crear la subred .
4. De click sobre el botón ```Crear```. 

<p align="center">
<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/SN.gif>
</p>
<br />

## Crear una Virtual Server instance
Para esto tenga en cuenta los siguientes pasos:
1. En la Lista de recursos, seleccione su servicio de Servidor virtual de Power Systems.
2. Siga la ruta ```Virtual server instances > Create instance```
3. Esto lo llevara a una pestaña de configuración, aquí ingrese la siguiente información:
  * ```Instance name```: Ingrese un nombre distintivo para la instancia.
  * ```Number of instances```: Seleccione el numero de instancias que desee.
  * ```VM pinning```: Seleccione la opción que desee para controlar el movimiento de las VM durante desastres y otros eventos de reinicio.
  * ```SSH key```: Seleccione la llave SSH creada anteriormente.
  * ```Operating system```: Seleccione el sistema operativo que desee.
  * ```Image```: Seleccione la versión de la imagen que desee.
  * ```Tier```: Seleccione el storage tier que necesite según su aplicación.
  * ```Machine Type```: Seleccione el tipo de maquina según su aplicación.
  * ```Core type```: Seleccione el tipo de core que desee.
  * ```Attached storage volumes (optional)```: Cree un volumen de almacenamiento de 20GB en el Tier que selecciono
  * ```Public networks```: Seleccione la opción ```OFF``` para no permitir acceso a la interfaz mediante redes publicas
  * ```Private networks```: De click sobre ```Attach existing``` y seleccione la subred privada creada anteriormente.
4. De click sobre ```Crear```.

## Resizing de la instancia

Acceder a la instancia de PowerVS que desea editar y seleccionar la opción de edit details, para cambiar características de la VSI, tales como número de cores o almacenamiento, no es necesario apagar la máquina, solamente se debe ingresar a la instancia y seleccionar el botón ```Edit details```, modificar las características deseadas y dar clic en ```Save Edits and order```

<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/edit-server-details.png>

Luego de realizar esto aparecerá un anuncio de edición exitosa e iniciará automaticamente la actualización de la VSI.

<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/server-edit-succesful.png>

## Creación del cloud connection
Seleccione la opción de cloud connection en el banner de la izquierda donde podrá configurar resources details,virtual connections y subnets.Ademas En resource details no es necesario activar el global routing.

<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/1-resource-details.png>

En la sección de virtual connections es posible establecer una conexión con infraestructura clásica sin la necesidad de utilizar un tunel GRE.

<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/2-virtual-connections.png>

La subnet debe pertenecer a la misma región y pod que la VSI.

<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/3-subnets.png>

## Acceder al Sistema operativo por consola

Debe acceder a las instancias virtuales en el banner de la izquierda y seleccionar la instacia en la cual desea abrir la consola. Luego en el menú de 3 puntos a la derecha seleccionar la opción de ```Open console```

<p align="center">
<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/open-console.png >
</p>
<br />

Luego se abrirá una nueva pestaña en el navegador que tendrá acceso al sistema operativo de la VSI y podrá interactuar con el servidor.

Ademas si es la primera vez que inicia la consola debera proporcionar credenciales de usuario y contraseña para poner conectarse posteriormente.

<p align="center">
<img width="800" alt="autoscale" src=https://github.com/emeloibmco/PowerVS-Creacion-VSI/blob/main/Images/consola.png>
</p>
<br />



## Referencias :mag:
* <a href="https://github.com/emeloibmco/VPC-Conexion-VPN"> VPC Conexión VPN</a>. 
* <a href="https://github.com/emeloibmco/Gateway-Appliance-Juniper-vSRX-version-20.4"> Gateway-Appliance-Juniper-vSRX-version-20.4 </a>.

<br />

## Autores :black_nib:
Equipo IBM Cloud Tech Sales Colombia.
<br />
