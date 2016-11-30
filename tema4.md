## Ejercicios tema 4

## Ejercicio 1

### Instala LXC en tu versión de Linux favorita. Normalmente la versión en desarrollo, disponible tanto en GitHub como en el sitio web está bastante más avanzada; para evitar problemas sobre todo con las herramientas que vamos a ver más adelante, conviene que te instales la última versión y si es posible una igual o mayor a la 1.0.

Para instalarlo tendremos que utilizar el comando `sudo apt install lxc1` para comprobar que este correctamente instalado ejecutaremos `lxc-checkconfig`

![imagen1](http://i68.tinypic.com/34sgn0h.png)

## Ejercicio 2

###Comprobar qué interfaces puente se han creado y explicarlos.
 
Tendremos que crear la máquina con la orden `sudo lxc-create -t ubuntu -n caja` una vez creado el contenedor lo arracamos con `sudo lxc-start -n nubecilla` en este momento podremos ver que la máquina esta corriendo con el comando `sudo lxc-ls -f` podremos ver todas las máquinas que tenemos y el estado de las mismas

![nube](http://i67.tinypic.com/dw3pf5.png)

Por último vemos las dos interfaces puente que se han creado correspondientes a la máquina que esta en ejecución con `ifconfig`

```
lxcbr0    Link encap:Ethernet  direcciónHW fe:65:ae:f9:0b:8b  
          Direc. inet:10.0.3.1  Difus.:0.0.0.0  Másc:255.255.255.0
          Dirección inet6: fe80::70f1:42ff:feca:21ac/64 Alcance:Enlace
          ACTIVO DIFUSIÓN FUNCIONANDO MULTICAST  MTU:1500  Métrica:1
          Paquetes RX:59 errores:0 perdidos:0 overruns:0 frame:0
          Paquetes TX:244 errores:0 perdidos:0 overruns:0 carrier:0
          colisiones:0 long.colaTX:1000 
          Bytes RX:7412 (7.4 KB)  TX bytes:33858 (33.8 KB)

vethLGHSJ8 Link encap:Ethernet  direcciónHW fe:65:ae:f9:0b:8b  
          Dirección inet6: fe80::fc65:aeff:fef9:b8b/64 Alcance:Enlace
          ACTIVO DIFUSIÓN FUNCIONANDO MULTICAST  MTU:1500  Métrica:1
          Paquetes RX:11 errores:0 perdidos:0 overruns:0 frame:0
          Paquetes TX:244 errores:0 perdidos:0 overruns:0 carrier:0
          colisiones:0 long.colaTX:1000 
          Bytes RX:1374 (1.3 KB)  TX bytes:31045 (31.0 KB)


```

## Ejercicios 3

### 1-Crear y ejecutar un contenedor basado en Debian.
Para generar un contenedor con debian ejecutaremos `sudo lxc-create -t debian -n caja_debian` y para arrancarlo lo haremos de la misma forma que en el ejercicio anterior `sudo lxc-start -n caja_debian`

### 2-Crear y ejecutar un contenedor basado en otra distribución, tal como Fedora. Nota En general, crear un contenedor basado en tu distribución y otro basado en otra que no sea la tuya. Fedora, al parecer, tiene problemas si estás en Ubuntu 13.04 o superior, así que en tal caso usa cualquier otra distro. Por ejemplo, Óscar Zafra ha logrado instalar Gentoo usando un script descargado desde su sitio, como indica en este comentario en el issue.



Para crear un contenedor en CentOS, instalaremos yum para ello ejecutamos `sudo apt-get install yum` una vez que tengamos yum instalado podremos crear nuestro contenedor con centOS tal y como veniamos haciendolo en ejercicios anteriores es decir `sudo lxc-create -t centos -n caja_centos`
Tras instalar la caja nos aparecerá un mensaje indicándonos la ruta donde se encuentra almacenada temporalmente la contraseña y la ruta donde tendremos que acceder para establecer la contraseña que queramos, para ello introduciremos en la termminal `sudo chroot /var/lib/lxc/caja_centos/rootfs passwd` ya tendremos listo el contenedor para utilizarlo.

Podemos ver que los dos contenedores se han creado de forma correcta mostrando todos los contenedores que tenemos creados:

![imagen_todos_contenedores](http://i66.tinypic.com/2vbotco.png)


## Ejercicios 4

### 1-Instalar lxc-webpanel y usarlo para arrancar, parar y visualizar las máquinas virtuales que se tengan instaladas.

En modo superusuario ejecutaremos el comando `wget https://lxc-webpanel.github.io/tools/install.sh -O - | bash` , y se nos mostrará tras realizar la instalación que podremos acceder a través de nuestro navegador con la url ´http://your-ip-address:5000/´ cuando accedemos nos pedirá que introduzcamos nuestro nombre de usuario y contraseña que por defecto será admin admin. Como podemos ver desde esta interfaz web podremos gestionar facilmente todos nuestros contenedores 
![imagenlxc](http://i67.tinypic.com/2it1ob7.png)

### 2-Desde el panel restringir los recursos que pueden usar: CPU shares, CPUs que se pueden usar (en sistemas multinúcleo) o cantidad de memoria.

Cuando hacemos click en el contenedor que queremos administrar nos saldrán todas las opciones que podemos configurar para dicho contenedor tal y como podemos ver en esta imagen hemos hecho click en el contenedor una-caja y podremos poner en marcha o parar el contenedor, además de poder cambiar el nombre del mismo configuraciones de red y CPUs y memoria como se especifica en este ejercicio concretamente

![una-caja-imagen-config](http://i65.tinypic.com/34t93k3.png)

## Ejercicio 5

### Comparar las prestaciones de un servidor web en una jaula y el mismo servidor en un contenedor. Usar nginx.

En primer lugar tendremos que instalar nginx (tanto en la jaula como en el contenedor) para ello tendremos que parar el servico apache2 en caso de que lo tengamos en nuestro pc ya que si no tendremos conflictos al instalar nginx:

`sudo systemctl stop apache2.service`

Una vez que hemos detenido apache podremos instalar nginx con el comando `sudo apt-get update` y `sudo apt-get install nginx`

Una vez que hemos instalado nginx instalaremos deboostrap si es necesario `sudo apt-get install deboostrap`


Para empezar activaremos nuestro contenedor `sudo lxc-start -n una-caja` , en caso de que no haga auto_start ejecutaremos el comando `sudo lxc-console -n una-caja` ,e instalaremos nginx como hemos comentado anteriormente.

![caja_img](http://i68.tinypic.com/2mxlqw6.png)

Es el momento de instalar una jaula con la orden `sudo debootstrap --arch=amd64 xenial /home/jaula/` , para acceder a la jaula ejecutaremos `sudo chroot /home/jaula`

##### Mediciones:

Para comparar haremos mediciones utilizando siege :

Lo instalaremos con `sudo apt-get install siege`

Una vez tengamos todo correctamente configurado tendremos que ejecutar apache benchmark con el comando `siege -b -c 1000 -t 20S 127.0.0.1` 

![nging_ b_jaula](http://i64.tinypic.com/21obqpt.png)

En el caso del contenedor procederemos de la misma forma `sudo siege -b -c 1000 -t 20S 10.0.3.220`

![nginx caja_siege](http://i65.tinypic.com/29mspdh.png)

Como podemos ver los resultados son parecidos en ambos.



# Ejercicio 6

### Instalar docker.

Para instalar docker tendremos que introducir en la consola el comando `sudo apt-get install docker.io`

# Ejercicios 7

### 1- Instalar a partir de docker una imagen alternativa de Ubuntu y alguna adicional, por ejemplo de CentOS.

Para instalar una imagen de ubuntu tendremos que ejecutar el comando `sudo docker pull ubuntu`

![docker](http://i63.tinypic.com/2uztgjp.png)

En el caso de querer instalar una imagen de centOS lo que tendremos que ejecutar será `sudo docker pull centos`

![centOS](http://i67.tinypic.com/2ly0bh1.png)

### 2-Buscar e instalar una imagen que incluya MongoDB.

Para instalar una imagen que contenga mongo tendremos que ejecutar en la terminal `sudo docker pull mongo`

![mongo](http://i66.tinypic.com/53aiye.png)

# Ejercicio 8

### Crear un usuario propio e instalar nginx en el contenedor creado de esta forma.

En primer lugar lanzamos el contenedor que creamos anteriormente `sudo docker run -i -t ubuntu /bin/bash`

Tendremos que introducir el siguiente comando para que nos permita ejecutar sudo ya que tiene un problema con ubuntu `apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*`

Crearemos un usuario como se haria normalmente en un sistema UNIX:
- `useradd -d /home/miguel -m miguel`
Establecemos la contraseña:
- `passwd miguel`
Añadimos al nuevo usuario al grupo sudo
- `adduser miguel sudo`

Ya que tenemos creado el usuario accederemos a su cuenta e instalaremos nginx con `apt-get install nginx`

# Ejercicio 9

### Crear a partir del contenedor anterior una imagen persistente con commit.

Tendremos que arrancar el contenedor si no lo teniamos ya abierto con el comando `sudo docker run -i -t ubuntu /bin/bash` en este momento tendremos que consultar cual es la ID del contenedor con el comando `sudo docker ps -a=false` ya que tenemos la ID podremos realizar el commit `sudo docker commit 7b19da8f5791 contenedor`

![imagendocker](http://i68.tinypic.com/28lx9wm.png)


# Ejercicio 10

### Crear una imagen con las herramientas necesarias para el proyecto de la asignatura sobre un sistema operativo de tu elección.


Podemos ver la documentación para este ejercicio en la rama de [documentación](https://miguelmoral.github.io/IV/)

























