## Ejercicios tema 2

## Ejercicio 1

### Instalar alguno de los entornos virtuales de node.js (o de cualquier otro lenguaje con el que se esté familiarizado) y, con ellos, instalar la última versión existente, la versión minor más actual de la 4.x y lo mismo para la 0.11 o alguna impar (de desarrollo).


instalamos virtualenv con el comando

```
sudo pip install virtualenv

```
para ver las versiones disponibles introducimos el comando

```
virtualenv -p /usr/bin/python
```
una vez tengamos instalado virtual env tan solo nos quedará crearnos nuestro propio entorno virtual con el comando

```
virtualenv miencv
```

Para poder poner en marcha este entorno virtual ejecutaremos:

```
. bin/activate
```

![Imagen 1](http://i66.tinypic.com/2ia30vo.png)

## Ejercicio 2

### Como ejercicio, algo ligeramente diferente: una web para calificar las empresas en las que hacen prácticas los alumnos.

Para realizar esta web se ha utilizado Djando, podemos ver esta web en este [enlace](https://github.com/Miguelmoral/IV_web_empresa). Tal y como se especifica en el enunciado en esta web están implementadas las funcionalidades para insertar nuevas empresas con sus correspondientes datos como pueden ser la clasificación de la propia empresa, la fecha y la persona la cuál ha introducido dicha información. Además de poder borrar alguna de estas empresas que se han introducido y poder consultar una lista ordenada de menor a mayor puntuación.

## Ejercicio 3

### Ejecutar el programa en diferentes versiones del lenguaje. ¿Funciona en todas ellas?

Para comprobar la ejecución del programa en diferentes versiones de python utilizaremos virtualenv y ejecutaremos: 

```
python3 manage.py runserver

```

o 

```
python manage.py runserver

```

En mi caso tan solo se ejecuta con la segunda versión de python y no con la primera.

![Imagen 2](http://i65.tinypic.com/2hecbja.png)

## Ejercicio 4

### Crear una descripción del módulo usando package.json. En caso de que se trate de otro lenguaje, usar el método correspondiente.

Tendremos que instalar el paquete npm introduciendo en la terminal: 
```
sudo apt-get install npm

```

Lo siguiente que tendremos que hacer es ejecutar en la carpeta correspondiente al repositorio de github donde tenemos subido el contenido que queremos documentar con package.json, el comando:
```
npm init

```
![imagen 4](http://i66.tinypic.com/245ebs0.png)

## Ejercicio 5

### Automatizar con grunt y docco (o algún otro sistema) la generación de documentación de la librería que se cree. Previamente, por supuesto, habrá que documentar tal librería.

Para la automatización de la documentación he utilizado sphinx el cuál instalaremos con `sudo easy_install sphinx` , tras la instalación podremos ejecutarlo en la carpeta donde tengamos nuestro proyecto introduciendo `sphinx-quickstart ` , el programa nos hará una serie de preguntas a cerda de nuestro proyecto y esto generará una documentación index.srt y un makefile.

## Ejercicio 6

### Para la aplicación que se está haciendo, escribir una serie de aserciones y probar que efectivamente no fallan. Añadir tests para una nueva funcionalidad, probar que falla y escribir el código para que no lo haga (vamos, lo que viene siendo TDD).

Crearemos en la carpeta empresa un nuevo documento llamado test.py donde haremos nuestros test. El primer test consistirá simplemente en la introducción de una nueva empresa. Quedándonos este test de la siguiente forma:

```
#/empresa/test.py
class TestIntroduccionValores(TestCase):

    def test_formulario(self):
        e = Datos(nombre_empresa = "Empresatest", persona = "Personaprueba", pub_date = timezone.now(), puntuacion = "5")
        e.save()
        self.assertEqual(e.nombre_empresa, "Empresatest")
```

![imagen ej6.1](http://i66.tinypic.com/1gkqiw.png)

Tras la ejecución de este test mediante el comando `python manage.py test empresa` vemos que la ejecución del test ha finalizado de forma exitosa.

Para ver si realmente está funcionando de una forma adecuada vamos a crear un test que nos devuelva un fallo, lo haremos viendo si el nombre de alguna otra de las empresas que hay en la base de datos es igual al nombre de la empresa la cuál acabamos de registrar en el test en la base de datos:

```
#/empresa/test.py
class TestIntroduccionValores(TestCase):

    def test_formulario(self):
        e = Datos(nombre_empresa = "Empresatest", persona = "Personaprueba", pub_date = timezone.now(), puntuacion = "5")
        e.save()
        self.assertEqual(e.nombre_empresa[3], "Empresatest")
```

Tras la ejecución `python manage.py test empresa` vemos como efectivamente nos da un error ya que no son iguales. 

![imagen 6.2](http://i64.tinypic.com/244a9gz.png)

## Ejercicio 7

### Convertir los tests unitarios anteriores con assert a programas de test y ejecutarlos desde mocha, usando descripciones del test y del grupo de test de forma correcta. Si hasta ahora no has subido el código que has venido realizando a GitHub, es el momento de hacerlo, porque lo vas a necesitar un poco más adelante.

Ya se ha hecho en el paso anterior ya que los test que he escrito son test unitarios preparador para ejecutarse en aplicaciones de test.

## Ejercicio 8

### Haced los dos primeros pasos antes de pasar al tercero.

Tendremos que subir a nuestro repositorio de la aplicación el fichero .travis.yml con el siguiente contenido:

```
language: python
python:
 - "2.7"

install:
 - sudo apt-get install python-dev
 - pip install --upgrade pip 
 - pip install Django 

script:
 - python manage.py test
```

Nos registraremos en travis con nuestra cuenta de github y habilitaremos que haga test de este repositorio. Tras la ejecución del test nos mostrará el siguiente resultado si todo ha ido de forma correcta incluyendo los resultados de los test como cuando lo ejecutamos en local.

[![Build Status](https://travis-ci.org/Miguelmoral/IV_web_empresa.svg?branch=master)](https://travis-ci.org/Miguelmoral/IV_web_empresa)


![imagen8](http://i68.tinypic.com/25qevyf.png)

















