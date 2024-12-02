# Apuntes de Git y tal
Apuntes sobre diferentes comandos de Git, ya sean para; crear un repositorio, subir cambios realizados (tanto en el back como en el front), arreglar diferentes fallos que hayamos podido hacer...

## Indice
- [Crear nuevo repositorio](#crear-nuevo-repositorio)
- [Vincular repositorios](#vincular-repositorios)
- [Estado del repositorio](#estado-del-repositorio)
- [Subir cambios](#subir-cambios)
  - [Subir front](#subir-front)
  - [Subir back](#subir-back)
- [Cambiar de rama](#cambiar-de-rama)
- [Crear migracion vacia](#crear-migracion-vacia)
- [Deshacer cambios y errores](#deshacer-cambios-y-errores)
  - [Jugando con el stash](#jugando-con-el-stash)
  - [Sacar archivos del git add](#sacar-archivos-del-git-add)
- [Rebasear](#rebasear)
- [Pull request](#pull-request)
- [Arrancar servicios](#arrancar-el-front)
  - [Arrancar front](#arrancar-el-front)
  - [Arrancar back](#arrancar-back)
  - [Redis](#redis)
  - [Celery](#celery)
  - [Arrancar test](#arrancar-test)
- [Trabajando con Poetry](#poetry)
- [Referencias](#referencias)
##

### Crear nuevo repositorio
El primer paso es tener un repositorio, ya sea creandolo desde cero o con uno existente.

[Creacion de repositorio](https://coderefinery.github.io/github-without-command-line/_images/creating.png)

<br><br>

### Vincular repositorios
Teniendo nuestro repositorio, tendremos que vincularlo con nuestro repositorio local.

[Url repositorio](https://www.bluej.org/tutorial/git/GitHub-afterCreatedRepository.png)

<br><br>

### Estado del repositorio

Para comprobar los archivos que hemos cambiado y cuales estan en la fase del `git add` podemos comprobarlo usando:
```bash
  git status
```

<br><br>

## Subir cambios

### Subir front
Al haber hecho cambios en los diferentes archivos, tendremos que pasar el formateador:

```bash
  npx nx format:write
```
<br>
A continuación añadiremos esos mismos archivos al `stage`, que es marcarlos para subirlos:

```bash
  git add .
```
- Pasandole "." al git add, añadimos TODOS los archivos en los que hayamos hecho cambios

<br>

Si solo queremos añadir algún archivo en concreto tendremos que especificarle:

```bash
  git add ./archivo.html
```
<br><br>
Después haremos un commit, que es el ultimo paso antes de subir los cambios hechos:

```bash
  git commit -m "mensaje"
```
- Al poner `-m` le estamos especificando que queremos ponerle un mensaje al commit, el cual tendrá que ser conciso y descriptivo, para que el resto pueda ver de manera sencilla los cambios que hemos realizado.

<br><br>
Para finalizar subiremos los cambios a nuestra rama:

```bash
  git push origin [rama]
```
- [rama] es la rama local en la que nos encontramos en ese momento.

<br><br>

### Subir back
Al haber hecho cambios en los diferentes archivos, tendremos que pasar los diferentes formateadores.

```bash
  black .
  flake8 .
  isort .
```
Si alguno de estos nos indica que hay algo mal y tenemos que hacer algun cambio, lo haremos antes de continuar hasta que los tres formateadores pasen sin dar problemas.

<br><br>
A continuación añadiremos esos mismos archivos al `stage`, que es marcarlos para subirlos.

```bash
  git add .
```
- Pasandole "." al git add, añadimos TODOS los archivos en los que hayamos hecho cambios.

<br>

Si solo queremos añadir algún archivo en concreto tendremos que especificarle:
```bash
  git add ./archivo.py
```

<br><br>
Después haremos un commit, que es el ultimo paso antes de subir los cambios hechos:

```bash
  git commit -m "mensaje"
```
- Al poner `-m` le estamos especificando que queremos ponerle un mensaje al commit, el cual tendrá que ser conciso y descriptivo, para que el resto pueda ver de manera sencilla los cambios que hemos realizado.

<br><br>
Para finalizar subiremos los cambios a nuestra rama:

```bash
  git push origin [rama]
```
- [rama] es la rama local en la que nos encontramos en ese momento.

<br><br>

##

### Cambiar de rama

Si queremos cambiarnos a una rama local que sabes que existe tendremos que:

```bash
  git switch [rama]
```
Siendo [rama] el nombre de a donde vamos a cambiarnos.

<br><br>
Si por algun casual queremos ir a una rama que no existe podremos hacerlo mediante:

```bash
  git switch -c [rama]
```
Así directamente crearemos una nueva rama con el nombre [rama], si por algún casual esa rama existe, simplemente nos moveremos a ella.

<br><br>

### Crear migracion vacia

Si en algún momento necesitamos una migración vacía para añadir alguna funcionalidad de conversión de datos o para llenar nuestra base de datos con datos falsos para poder ver que lo que hemos hecho nos devuelva algo, podremos hacerlo facilmente con el siguiente comando.

```bash
  python manage.py makemigrations [carpeta] --name [nombre] --empty
```
- [carpeta] => el lugar donde queremos crear nuestra nueva migración.
- [nombre] => como se le va a llamar a la migración, hay que intentar que sea descriptivo para saber que va a hacer sin tener que meterse a mirarlo.
- --empty => indica que solo queremos que tenga las referencias a anteriores migraciones de esa misma carpeta, a partir de ahí tendremos que hacerlo todo nosotros.

<br><br>

## Deshacer cambios y errores

Podemos ver el historial de commits que se han hecho, ordenados de manera descendente:
```bash
  git log --graph
```
<br>
Al usar esto podremos personalizarlo como nosotros queramos para que sea más facil ver los identificadores, que quite información que no queramos o muchas otras cosas, aquí hay varias paginas que hablan sobre el tema:

- [Pretty Git Branch - StackOverflow](https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs)
- [Customizing git log - Justin Joyce](https://justinjoyce.dev/customizing-git-log-format/)
- [Pretty formats - git](https://git-scm.com/docs/pretty-formats)

<br>
Si queremos visualizar los comandos que se han realizado, con los movimientos especificos entre ramas y su identificador, podremos usar:

```bash
  git reflog
```

<br>
Ahora, sabiendo el identificador (suele ser lo primero que aparece, a menos que se haya editado para cambiar el orden) del punto al que queremos retroceder tendremos dos opciones para hacerlo:

<br>

```bash
  git reset --soft [id]
```
Haciendo esto volvemos al punto especifico y recuperamos los cambios que se habían hecho.

<br>

```bash
  git reset --hard [id]
```
Con este otro comando volvemos al punto especificado, pero todos los cambios que haya desde donde estabamos hasta este punto se borran

<br><br>

### Jugando con el stash

Cuando estamos en una rama y queremos cambiarnos a otra, o para tener un "punto de control" o por cualquier otro motivo, podemos usar el `stash` que es una especie de almacenamiento.

Para poder ver todo lo que está guardado en él, podemos hacerlo directamente desde nuestro entorno de desarrollo o mediante consola:
```bash
  git stash list
```

<br>

Para poder añadir nuevos cambios al stash, primero tendremos que marcarlos:

```bash
  git add .
  git stash
```
- Si no queremos añadir todos los archivos que hemos editado, siempre podremos especificar los archivos en vez de pasarles todos.

<br>

Después, para recuperar los los cambios del stash tendremos que:

```bash
  git stash pop stash@{0}
```
- pop => después de recuperar los elementos, se borran permanentemente del stash.
- 0 => indica la posicion, entre los elementos del stash que queramos recuperar.

<br>

Esto también se puede hacer mediante el entorno de desarrollo con los menus, que pueden llegar a ser más visuales.

<br><br>

### Sacar archivos del "git add"

Si a la hora de marcar archivos para subirlos, nos damos cuenta que hay alguno que no queremos o no debamos subir tendremos que sacarlo:

```bash
  git restore --staged [nombre]
```
- [nombre] => siendo el nombre del archivo que queramos sacar, para no subirlo.

<br><br>

### Rebasear

Primero tendremos que movernos a la rama a la que queremos hacer el rebase, que es mover nuestra rama al final de la rama objetivo para que parezca que sale de esa, haciendo esto tendremos los últimos cambios y no nos dará problemas de conflictos cuando queramos juntar las ramas:

```bash
  git switch [rama]
  git pull origin [rama]
```
Primero tendremos que cambiarnos a [rama] y bajarnos los cambios que tenga.

<br>

Después volveremos a la rama en la que estabamos antes:
```bash
  git switch [rama_antigua]
```

<br>

Abrimos un archivo para seleccionar los commits que queremos subir.
```bash
  git rebase -i --keep-base [rama]
```
[rama] => Aunque normalmente se vaya a hacer a develop, se puede hacer a cualquier rama que queramos.

Al abrir el archivo; el primero tiene que ser del tipo `pick`, el resto tiene que ser `s`. Si por algún casual queremos eliminar alguno de los commits podemos hacer tanto: indicarle `d` o comentarlo `#`.

<br>

Después de haber seleccionado los commits deseados, tendremos que hacer el rebase per sé.
```bash
  git rebase [rama]
```
[rama] => Aunque normalmente se vaya a hacer a develop, se puede hacer a cualquier rama que queramos.

Si en este momento nos da algún tipo de problema, por problemas de confrontación de errores, tendremos que solucionarlos antes de continuar, para ello existen dos maneras:

1. <u>Pantalla visual</u>, desde la parte superior izquierda del pycharm, nos aparecerá una flecha verde, que al seleccionar nos saldrá los archivos conflictivos. Desde ahí tendremos que seleccionar con que nos queremos quedar y al finalizar todos ellos continuar.
2. <u>Accediendo a los archivos</u>, tendremos que ir a los archivos conflictivos de manera manual y hacer los cambios desde ahí. Al terminar tendremos que añadir los archivos manualmente y continuar con el rebase:
```bash
  git add .
  git rebase --continue
```

<br>

Al terminar todo esto y querer subirlo, tendremos que forzar la subida, ya que la historia en remoto y local son diferentes.
```bash
  git push origin [rama] -f
```
[rama] => rama en la que hemos estado haciendo los cambios.
-f => para forzar la subida, si después de hacer el rebase intentamos subirlo sin esto, nos dará error.

<br><br>

### Pull request

A la hora de hacer las pull request, lo mejor es hacer los pasos del rebaseo para subir un solo commit y facilitar el trabajo.

Al subir los cambios a GitHub habremos hecho todo lo que podemos desde pycharm, visual o nuestro ED. Ahora iremos a GitHub para finalizar. Desde ahí tendremos que ir a la pestaña `Pull requests` desde el proyecto en el que hayamos estado trabajando.

Entonces crearemos una nueva PR, lo más seguro es que nos marque en la parte superior la rama que acabamos de subir, aunque si no es el caso, al seleccionar `New pull request` en la parte de `base` seleccionaremos a la que queremos añadir los cambios, y en el `compare` nuestra rama que hemos subido.

Después tendremos que seleccionar a un reviewer desde la parte de la derecha (quien queremos que compruebe que esta todo bien antes de juntarlo) y nos asignaremos a nosotros mismos en assignees (para que se sepa quien a sido el que ha subido y trabajado en esa rama).

<br><br>

## Arrancar servicios

### Arrancar back

Para hacer que funcione la parte del back de nuestra aplicación tendremos que, desde el entorno de desarrollo que usemos para editar los archivos, poner en la consola o crear una configuración el siguiente comando:
```bash
  python manage.py runserver
```
<br><br>

### Arrancar el front

Para hacer que funcione la parte del front de nuestra aplicación tendremos que, desde el entorno de desarrollo que usemos para editar los archivos, poner en la consola el siguiente comando:
```bash
  npx nx run biolanglobal:serve
```

<br><br>

### Arrancar Redis

Para arrancar redis, tendremos que abrir una nueva terminal fuera de nuestro entorno de desarrollo e introducir el siguiente comando:
```bash
  sudo service redis-server start
```

<br>

Para ver el estado de redis:
```bash
  sudo service redis-server status
```

<br>

Aunque a la hora de apagar el ordenador el servidor de redis se detenga, siempre podremos detenerlo nosotros mismos:
```bash
  sudo service redis-server stop
```

<br><br>

### Arrancar Celery

Celery es una biblioteca basada en Python que se usa para ejecutar tareas asíncronas, programadas y distribuidas. Usando esto no bloquea la ejecución principal del programa.

Después de haber arrancado redis, tendremos que arrancarlo desde nuestro proyecto, para ello hay que arrancar tanto el `beat` como el `worker`:

- Arrancar beat
```bash
  celery -A [carpeta] beat -l INFO
```
[carpeta] => es la carpeta donde se encuentra el archivo `celery.py`.

`INFO` => nos dará información de todos los procesos que va realizando, también se puede cambiar por `DEBUG` para arrancarlo en el modo debug para poder ir haciendo pausas y comprobar mejor los datos que nos llegan en un momento en concreto.

- Arrancar worker
```bash
  celery -A [carpeta] worker -l INFO
```
[carpeta] => es la carpeta donde se encuentra el archivo `celery.py`.

`INFO` => nos dará información de todos los procesos que va realizando, también se puede cambiar por `DEBUG` para arrancarlo en el modo debug para poder ir haciendo pausas y comprobar mejor los datos que nos llegan en un momento en concreto.

<br><br>

### Arrancar test

Para arrancar todos los test que tengamos en nuestra aplicación:
```bash
  python manage.py test
```

<br>

Para arrancar los test de una sola carpeta en específico:
```bash
  python manage.py test [carpeta]
```
- [carpeta] => especificar la carpeta de la cual queremos arrancar los test

<br>

Desde pycharm podremos añadir una nueva configuración para que se encargue de arrancar los tests de manera más automática. Además haciendo esto se pueden arrancar los test en el modo `debug` para comprobar de mejor manera donde da error o que devuelve los test.

<br><br>


## Poetry

Si se esta usando poetry en el proyecto, que es una herramienta para trabajar con las dependencias que pueda tener nuestro proyecto, se puede ir directamente a la información de poetry o buscar la información necesaria por internet. 

<br>

Si se quiere instalar algo nuevo, en vez de instalarlo directamente con sudo, es mejor añadirlo a poetry, para que el resto de miembros también lo tengan y no les den problemas.
```bash
  poetry add [paquete]
```
- [paquete] => especificar el paquete que queramos añadir a poetry


### Referencias

- Aitor Basarrate
- [Pretty Git Branch - StackOverflow](https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs)
- [Customizing git log - Justin Joyce](https://justinjoyce.dev/customizing-git-log-format/)
- [Pretty formats - git](https://git-scm.com/docs/pretty-formats)
- [Poetry](https://python-poetry.org/docs/)
