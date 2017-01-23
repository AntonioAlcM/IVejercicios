# Ejercicios tema 5

## Ejercicio 1: Instalar los paquetes necesarios para usar KVM. Se pueden seguir estas instrucciones. Ya lo hicimos en el primer tema, pero volver a comprobar si nuestro sistema está preparado para ejecutarlo o hay que conformarse con la paravirtualización.

En caso de no tener los paquetes instalados los instalaremos con `sudo apt-get install qemu-kvm libvirt-bin`

Una vez estemos seguros de tener los paquetes instalados podremos comprobar si nuestro equipo soporta KVM con el comando `sudo kvm-ok`

En el caso de mi equipo me muestra que se puede utilizar KVM:
```
INFO: /dev/kvm exists
KVM acceleration can be used

```

## Ejercicio 2:
### 1-Crear varias máquinas virtuales con algún sistema operativo libre tal como Linux o BSD. Si se quieren distribuciones que ocupen poco espacio con el objetivo principalmente de hacer pruebas se puede usar CoreOS (que sirve como soporte para Docker) GALPon Minino, hecha en Galicia para el mundo, Damn Small Linux, SliTaz (que cabe en 35 megas) y ttylinux (basado en línea de órdenes solo).

#### Instalación de Slitaz:

En este caso voy a instalar Slitaz, para empezar bajamos la imagen de su [web](http://www.slitaz.org/en/) la cuál pesa tan solo 36,4 MB.

Lo primero que haremos será crear el disco duro `qemu-img create -f qcow2 slitaz.qcow2 2G`

Y después tendremos que instalar la imagen que nos descargamos inicialmente `qemu-system-x86_64 -machine accel=kvm -hda slitaz.qcow2 -cdrom slitaz-4.0.iso -m 1G -boot d`

Tras ejecutar el último comando se nos abrirá la máquina virtual directamente con una configuración inicial típica de un sistema basado en UNIX, tras completar esta configuración tendremos el sistema operativo funcionando correctamente.

![imagen1](http://i66.tinypic.com/16733d.png)

#### Instalación de Damm Small Linux:
Descargamos la imagen de la [página oficial](http://distro.ibiblio.org/damnsmall/release_candidate/) la cuál tan solo pesa 50,6 MB

Repetimos los pasos anteriores de crear un disco duro de dos gigas como en el caso anterior `qemu-img create -f qcow2 dsl.qcow2 2G`

Una vez que tenemos creado el disco duro podremos instalar la imagen que descargamos `qemu-system-x86_64 -machine accel=kvm -hda dsl.qcow2 -cdrom dsl-4.11.rc1.iso -m 1G -boot d`

Podremos ver después de una configuración básica que todo funciona correctamente:

![imagen2](http://i66.tinypic.com/vhx8b9.png)

### 2-Hacer un ejercicio equivalente usando otro hipervisor como Xen, VirtualBox o Parallels.

Para realizar este ejercicio he utilizado virtualBox, tan solo tendremos que abrir el programa y crear una nueva máquina virtual dándole el espacio tanto de RAM como de disco duro que deseemos. Una vez creada la máquina añadimos una nueva unidad óptica en el apartado de configuración/almacenamiento y montamos la imagen .iso del sistema operativo que queramos, en mi caso de Slitaz.

![imagen3](http://i66.tinypic.com/2hda9ue.png)

Una vez hecho esto tan solo tendremos que ejecutar la máquina y tras una configuración inicial todo funcionará correctamente como podemos ver en la imagen:

![imagen4](http://i67.tinypic.com/29ervcw.png)

# Ejercicio 4: Crear una máquina virtual Linux con 512 megas de RAM y entorno gráfico LXDE a la que se pueda acceder mediante VNC y ssh.

Para realizar este ejercicio voy a utilizar lubuntu, nos podremos bajar su imagen de la [web](https://help.ubuntu.com/community/00000000000Lubuntu/GetLubuntu/LTS).

Una vez tengamos la imagen descargada procederemos como en los ejercicios anteriores a reservar espacio en disco y a instalarla posterioremente:
- Reservar espacio 5GB en este caso: `qemu-img create -f qcow2 lubuntu.qcow2 5G`
- Iniciar la instalación (especificando los 512 mb de RAM con -m): `qemu-system-x86_64 -hda lubuntu.qcow2 -cdrom lubuntu-14.04.5-desktop-amd64.iso -m 512M
`

Una vez tengamos todo instalado correctamente necesitaremos conectarnos a la máquina que acabamos de crear, para ello voy a utilizar vinagre, lo instalaremos con el comando `sudo apt-get install vinagre`

Vamos a ver que todo este funcionando, para ello arrancamos la máquina con ` qemu-system-x86_64 -machine accel=kvm -hda lubuntu.qcow2 -m 512M -vnc :1`, una vez que la máquina esta ejecutándose podremos conectarnos a ella con vinagre abriendo otra terminal y ejecutando `vinagre localhost:1` en la siguiente imagen vemos como podemos acceder perfectamente a la máquina:

![imagen7](http://i68.tinypic.com/5o81s1.png)

### Conectar por ssh

Tendremos que instalar ssh en la máquina virtual ejecutando `sudo apt-get install ssh`.

Ya que tenemos ssh corriendo en la máquina virtual tan solo tendremos que ejecutarla de la siguiente forma para poder modificar los puertos y que ssh no tenga problemas por los puertos `qemu-system-x86_64 -machine accel=kvm -hda lubuntu.qcow2 -m 512M -redir tcp:8526::22 -vnc :1`

Llegados a este punto podremos ejecutar ssh desde otra terminal `ssh miguel@127.0.0.1 -p 8526`

![imagen8](http://i66.tinypic.com/24l1tgz.png)

# Ejercicio 6: Instalar una máquina virtual con Linux Mint para el hipervisor que tengas instalado.

Descargamos la imagen de linux mint de la [web oficial](https://www.linuxmint.com/edition.php?id=226), una vez que la tengamos descargada la montaremos en VirtualBox tal y como hicimos en el ejercicio 2

En primer lugar creamos una nueva máquina que tenga más de 10 GB de disco para poder instalar Linux Mint, una vez creada la máquina nos vamos a configuración y montamos la imagen .iso como un nuevo controlador

![imagen5](http://i67.tinypic.com/2wm1bhz.png)

Al arrancar la máquina nos pedirá una configuración inicial tras la cuál tendremos el sistema operativo funcionando perfectamente:

![imagen6](http://i66.tinypic.com/eakr3t.png)
