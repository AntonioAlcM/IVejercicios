## Ejercicios tema 6

# Ejercicio 1 Instalar chef en la máquina virtual que vayamos a usar:

Para instalar chef tendremos que instalar `curl -L https://www.opscode.com/chef/install.sh | sudo bash`, para comprobar que funciona correctamente ejecutaremos `chef-solo -v` que nos mostrará la versión de chef que tenemos.

![img1](http://i64.tinypic.com/24dow3r.png)

# Ejercicio 4 Instalar una máquina virtual Debian usando vagrant y conectar con ella

Instalaremos vagrant con el comando `sudo apt-get install vagrant`

A continuación ejecutamos `vagrant box add debian/jessie64`

![img2](http://i66.tinypic.com/23igfpx.png)

Con `vagrant up` levantaremos la máquina.
![img3](http://i66.tinypic.com/cye9.png)


Para conectarnos a la máquina ejecutamos `vagrant ssh`

![img4](http://i66.tinypic.com/vwu03m.png)

# Ejercicio 5 Crear un script para provisionar `nginx` o cualquier otro servidor web que pueda ser útil para alguna otra práctica.

Dentro de vagrant file tendremod que introducir este script

```
Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.provision "shell", path: "provision.sh"
end
```

# Ejercicio 6 Configurar tu máquina virtual usando vagrant con el provisionador chef.
