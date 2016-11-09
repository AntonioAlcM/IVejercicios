## Ejercicios tema 3

## Ejercicio 1

### Darse de alta en algún servicio PaaS tal como Heroku, Nodejitsu, BlueMix u OpenShift.

Para este ejercicio voy a utilizar la plataforma Heroku. Tras rellenar un formulario de datos personales como nombre, compañia y lenguaje de programación principal nos llegará un correo al mail para que establezcamos una contraseña.

Una vez completemos este registro Heroku nos redirigirá a una página como esta para poder empezar con nuestros proyectos en la plataforma

![Imagen 1](http://i67.tinypic.com/103itmr.png)

## Ejercicio 2

### Crear una aplicación en OpenShift o en algún otro PaaS en el que se haya dado uno de alta. Realizar un despliegue de prueba usando alguno de los ejemplos.

para realizar este ejercicio voy a utilizar wordpress en Heroku, se va a usar una built de Wordpress que viene con PostgreSQL.

Tendremos que instalar Heroku en nuestro PC con el comando:

```
wget -O- https://toolbelt.heroku.com/install-ubuntu.sh | sh
```

Una vez instalado tendremos que hacer un clone del repositorio de Github wordpress-heroku

```
git clone git@github.com:bkvirendra/wordpress-heroku.git
```

Tras hacer el clone de este repositorio tendremos que crear nuestra aplicación con el comando `heroku create`

Llegados a este paso nos pedirá nuestro mail y contraseña y creará nuestra aplicación proporcionándonos el siguiente enlace https://serene-plateau-23024.herokuapp.com/

El siguiente paso será añadir el add-on de base de datos a la aplicación de la siguiente forma `heroku addons:add heroku-postgresql:dev`





Haremos un clone del repositorio de github `git clone git://github.com/mhoofman/wordpress-heroku.git`

Tras lo que crearemos nuestra aplicación con el comando `heroku create` estando en el directorio wordpress-heroku . Y nos generará el enlace de nuestra app https://arcane-refuge-51918.herokuapp.com/

El siguiente paso es añadir un add-on de base de datos a la aplicación : ` heroku addons:create heroku-postgresql`

A continuación ejecutamos `heroku pg:promote postgresql-elliptical-18615` siendo postgresql-elliptical-18615 la base de datos creada con el comando anterior.

Una vez hecho esto crearemos una nueva rama destinada a la producción `git checkout -b production`

Llegados a este paso tan solo nos quedará desplegar con el comando `git push heroku production:master
`  
Esto nos devolverá la URL de nuestro sitio web https://arcane-refuge-51918.herokuapp.com/ desde donde podremos gestionar nuestro sitio web. La primera vez que entremos nos pedirá que nos registremos con nuestra información personal y el nombre de la página. Tras realizar esta configuración obtendremos una simple web con un Hola mundo con el siguiente aspecto http://arcane-refuge-51918.herokuapp.com/wp-admin/customize.php

![Imagenholamundo](http://i65.tinypic.com/27xp5zn.png)

## Ejercicio 3

### Realizar una app en express (o el lenguaje y marco elegido) que incluya variables como en el caso anterior.

En este ejercicio voy hacer uso de [flask](http://flask.pocoo.org/) para hacer una sencilla aplicación como la siguiente.

```
# -*- coding: utf-8 -*-
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return 'Esto es una aplicación de prueba para el ejercicio de Infraestuctura Virtual'

@app.route("/usuario/<user>")
def usuario(user):
    return 'Bienvenido usuario ' + user

@app.route("/test")
def test():
    return 'Este es el test de la aplicación para la asignatura IV'

if __name__ == "__main__":
    app.run()

```

Para ejecutarlo tan solo tendremos que ejecutar el comando `python nombre_del_archivo.py` y nos dará la URL que tenemos que introducir en el navegador.

![imagenapp](http://i66.tinypic.com/w2m461.png)

## Ejercicio 4

### Crear pruebas para las diferentes rutas de la aplicación.

Para los test vamos a utilizar la propia herramienta que nos proporciona [Flask](http://flask.pocoo.org/docs/0.10/testing/) quedando el test para nuestra aplicación de la siguiente forma.
```
# -*- coding: utf-8 -*-
import os
import ej3
import unittest
import tempfile

class ej3TestCase(unittest.TestCase):

    def setUp(self):
        self.db_fd, ej3.app.config['DATABASE'] = tempfile.mkstemp()
        ej3.app.config['TESTING'] = True
        self.app = ej3.app.test_client()
        #ej3.init_db()

    def tearDown(self):
        os.close(self.db_fd)
        os.unlink(ej3.app.config['DATABASE'])

    def test1(self):
        rv = self.app.get('/')
        assert 'Esto es una aplicación de prueba para el ejercicio de Infraestuctura Virtual' in rv.data

    def test2(self):
        rv = self.app.get('/usuario/prueba')
        assert 'Bienvenido usuario' in rv.data

    def test3(self):
        rv = self.app.get('/test')
        assert 'Este es el test de la aplicación para la asignatura IV' in rv.data


if __name__ == '__main__':
    unittest.main()
```

Tras ejecutarlo vemos como se ha ejecutado con éxito 
![imgtest](http://i67.tinypic.com/dxd8cl.png)

## Ejercicio 5

### Instalar y echar a andar tu primera aplicación en Heroku.

En primer lugar tendremos que loguearnos en Eroku con `heroku login` y nos pedirá nuestro mail con el que nos registramos en eroku y contraseña. A continuación tendremos que hacer un clone del siguiente repositorio de github `git clone https://github.com/heroku/node-js-getting-started.git` . Ahora tan solo tendremos que crear una nueva aplicación en eroku tendremos que estar en la carpeta node-js-getting-started que generamos haciendo el clone con el comando `heroku create`. Por último tendremos que ejecutar los siguientes comandos y todo quedará configurado de manera correcta:

```
npm install ejs
npm install -g expressrm -fr node_modules
npm i
git add -f node_modules
git commit -m "Fix node_modules dependencies"
git push heroku master

```

Finalmente la ejecutamos con `nodejs index.js`

Podemos comprobar que todo funciona correctamente:

![imagennodejs](http://i67.tinypic.com/34zj7ty.png)

## Ejercicio 6

### Usar como base la aplicación de ejemplo de heroku y combinarla con la aplicación en node que se ha creado anteriormente. Probarla de forma local con foreman. Al final de cada modificación, los tests tendrán que funcionar correctamente; cuando se pasen los tests, se puede volver a desplegar en heroku.

Tendremos que crear dos nuevos documentos en el fichero de nuestra aplicación estos son Procfile y requirements.txt

En el documento Procfile tendremos que añadir la linea `web: python ej3.py` y en el requirements algunas dependencias necesarias para Flask

requirements.txt:

´´´
Werkzeug==0.8.3
Jinja2==2.6
distribute==0.6.31
wsgiref==0.1.2

´´´
Ahora tan solo nos quedará ejecutar con el comando `foreman start`. De esta forma vemos como podemos acceder a nuesta app realizada en el ejercicio 3:

![imagenej6](http://i63.tinypic.com/28lrq6b.png)

Finalmente nos quedará desplegarlo con:

´´´
git init
git add .
git commit "subiendo ejercicio IV"
heroku create
git push heroku master
´´´

## Ejercicio 7

### Haz alguna modificación a tu aplicación en node.js para Heroku, sin olvidar añadir los tests para la nueva funcionalidad, y configura el despliegue automático a Heroku usando Snap CI o alguno de los otros servicios, como Codeship, mencionados en StackOverflow.

Ya hemos visto como funciona el despliegue automatico, ahora nos quedará configurar Heroku para que realice el despliegue unicamente cuando hayan pasado los test unitarios. Para que esto ocurra tendremos que marcar esta casilla que se encuentra en los settings del proyecto:

![herokupic](http://i64.tinypic.com/2yuye09.png)



