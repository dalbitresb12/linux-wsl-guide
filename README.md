# Guía para Linux en WSL (con Ubuntu)

Esta guía está diseñada como una guía de consulta rápida para poco a poco memorizar los comandos más comunes que necesitarás cuando utilizes Linux (y más específicamente Ubuntu).

> Esta guía ha sido redactada para ser utilizada junto con Ubuntu. Muchos de los conceptos aplican también para otras distribuciones de Linux, pero otros podrían ser completamente distintos.

## Conceptos básicos

### Sistema de archivos

El sistema de archivos de Linux no funciona con letras como en Windows (por lo tanto, no existe un `C:\` o `D:\`). Todos los archivos están almacenados en forma de árbol, empezando por el `root` o `/`. **Es importante notar que en Linux se utilizar el _forward slash_ (`/`), mientras que Windows utiliza el _backward slash_ (`\`).**

> Se podría decir que `/` equivale a `C:\` en Windows.

De la misma forma que en Windows, existen dos tipos de direcciones para las carpetas. Tenemos las direcciones absolutas, la cuales definen **la dirección completa en el sistema de archivos donde un archivo está ubicado**. Un ejemplo de una dirección absoluta en Linux es la siguiente: `/home/user/Desktop/photo.png` (el significado de esta dirección será explicado luego).

> Las direcciones absolutas en Windows toman la siguiente forma: `C:\Users\user\Desktop\photo.png`.

Asimismo, tenemos también las direcciones relativas, las cuales se forman **siempre en base al directorio en el que nos encontramos**. Es decir, si nos encontramos en el directorio `/home/user/` y utilizamos la dirección relativa `Desktop/photo.png`, estaríamos haciendo referencia al mismo archivo al que hicimos referencia con la anterior dirección absoluta.

> Las direcciones relativas funcionan de la misma forma en Windows.

### El `root` o `/`

Este directorio es la base para todos los archivos en el sistema. En este podemos encontrar diversas carpetas del sistema, como por ejemplo `/etc`, `/bin`, `/usr`, `/mnt`, o `/home`. Cada carpeta almacena diferentes partes del sistema operativo.

> Por ejemplo, la carpeta `/bin` almacena los archivos ejecutables de muchos de los programas base de Linux, como por ejemplo `ls`, `echo` o `cat`, los cuales serán cubiertos más a detalle en la [siguiente sección](#herramientas-del-sistema).

Por lo general, **todas estas carpetas solo pueden ser modificadas por usuarios con [permisos de superusuario](#permisos-de-superusuario)** (los cuales serán cubiertos más a detalle luego). Entre estas carpetas, solo nos centraremos en la carpeta `/home`, ya que en esta carpeta es donde se almacenan todos los datos de cada usuario en el sistema.

> Un equivalente de la carpeta `/home` en Windows sería la carpeta `C:\Users`.

Dentro de la carpeta `/home`, encontraremos una carpeta por cada usuario. Podemos hacer referencia a la carpeta del usuario con el que hemos iniciado sesión en Linux con el signo de tilde (`~`). Si nuestro usuario es `user`, la carpeta con los datos del usuario estaría ubicada en `/home/user` y la dirección `~` apuntaría a esta dirección.

> La tilde (`~`) se puede usar para construir direcciones a carpetas relativas a la carpeta del usuario. Por ejemplo, si nuestro usuario es `user`, la dirección `~/folder/photo.png` apuntaría a un archivo PNG y su dirección absoluta sería `/home/user/folder/photo.png`.

## Herramientas del sistema

A continuación, encontrarás una lista de algunos de los comandos más útiles que están incluidos en una instalación común de Ubuntu.

> Muchos de estos comandos probablemente se encuentren también en otras distribuciones, pero no puedo asegurar esto.

La gran mayoría de los comandos en Linux siguen el siguiente formato:

```bash
command [PARÁMETROS]... [ARGUMENTOS]...
```

- Los parámetros vendrían a ser opciones que modifican el resultado del comando. Estos en su mayoría son acompañados de un guión (`-`) o (en algunos casos) doble guión (`--`) al inicio.
- Los argumentos son los valores que el comando recibe y que debe procesar para obtener un resultado.

Por ejemplo, el comando [`ls`](#ls) tendrá un comportamiento diferente si añadimos el parámetro `-l` (este parámetro será explicado más adelante):

> En los bloques de código, un comando en la terminal **siempre** está acompañado del símbolo de dólar al inicio (`$`). Si una línea **no incluye** el símbolo de dólar, representa la salida de un comando (su _output_). Un comentario está definido con un símbolo de _hashtag_ (`#`).

```bash
$ ls /home/user
file.png  folder1  folder2  folder3
$ ls -l /home/user
total 12
-rw-r--r-- 1 dab12 dab12    0 Mar 26 19:51 file.png
drwxr-xr-x 2 dab12 dab12 4096 Mar 26 19:51 folder1
drwxr-xr-x 2 dab12 dab12 4096 Mar 26 19:51 folder2
drwxr-xr-x 2 dab12 dab12 4096 Mar 26 19:51 folder3
```

> Todos los parámetros adicionales se deben incluir **antes** de los argumentos que acepte el comando. En algunos casos, también funcionará poner los parámetros luego de los argumentos, pero es más seguro ponerlos antes.

### `pwd`

Con este comando se puede obtener la dirección absoluta de la carpeta en la que la terminal se encuentra.

> El concepto de direcciones absolutas fue cubierto [en el capítulo anterior](#sistema-de-archivos).

### `cd`

Este comando se utiliza para navegar por los directorios en el [sistema de archivos](#sistema-de-archivos). Se le debe pasar como argumento una dirección absoluta o una dirección relativa a la carpeta en la que la terminal se encuentra. Se puede utilizar también la dirección especial `..`, que representa al directorio padre al directorio en el que nos encontramos.

> Si el comando `cd` es ejecutado sin una ruta, el comando nos moverá a la carpeta de inicio del usuario con el que hemos iniciado sesión.

Es importante tener en cuenta **que la terminal de Linux distingue mayúsculas y minúsculas**, por lo que debemos escribir los directorios correctamente.

Estos son algunos ejemplos de como funciona este comando (se ha utilizado también el comando [`pwd`](#pwd) para demostrar su funcionamiento):

```bash
$ pwd
/home/user
$ cd Desktop
$ pwd
/home/user/Desktop
$ cd ~/Documents
$ pwd
/home/user/Documents
$ cd ../Photos
$ pwd
/home/user/Photos
$ cd ~
$ pwd
/home/user
$ cd /usr/bin
$ pwd
/usr/bin
$ cd
$ pwd
/home/user
```

> Como se puede observar, este comando funciona de manera similar al comando con el mismo nombre que está disponible en Windows.

### `ls`

Listar el contenido de un directorio. Por defecto, listará el contenido del directorio en el que se encuentra la terminal. Si se desea ver el contenido de otra carpeta, se puede incluir después de la instrucción: `ls /home/username/folder` (dirección absoluta) o `ls folder` (dirección relativa, desde `/home/username`).

Este comando acepta también algunos parámetros que modifican su _output_. Estos son algunos de los más útiles:

- `-l`: Muestra los archivos y carpetas con información detallada como los permisos, tamaño en disco, propietario, entre otros. Por defecto, los tamaños de los archivos se muestran en _bytes_.
- `-a`: Incluye en el listado los archivos ocultos, es decir, los archivos que empiezan con un punto (`.`).
- `-h`: Muestra los tamaños de los archivos en el listado detallado como tamaños fáciles de leer para los humanos (_human-friendly sizes_).

```bash
$ ls /home/user
file.png  folder1  folder2  folder3
$ ls -l /home/user
total 12
-rw-r--r-- 1 dab12 dab12    0 Mar 26 19:51 file.png
drwxr-xr-x 2 dab12 dab12 4096 Mar 26 19:51 folder1
drwxr-xr-x 2 dab12 dab12 4096 Mar 26 19:51 folder2
drwxr-xr-x 2 dab12 dab12 4096 Mar 26 19:51 folder3
$ ls -a 
.  ..  .hiddenfile.txt  .hiddenfolder  file.png  folder1  folder2  folder3
$ ls -lah
total 24K
drwxr-xr-x  6 dab12 dab12 4.0K Mar 26 19:55 .
drwxr-xr-x 30 dab12 dab12 4.0K Mar 26 19:51 ..
-rw-r--r--  1 dab12 dab12    0 Mar 26 19:55 .hiddenfile.txt
drwxr-xr-x  2 dab12 dab12 4.0K Mar 26 19:55 .hiddenfolder
-rw-r--r--  1 dab12 dab12    0 Mar 26 19:51 file.png
drwxr-xr-x  2 dab12 dab12 4.0K Mar 26 19:51 folder1
drwxr-xr-x  2 dab12 dab12 4.0K Mar 26 19:51 folder2
drwxr-xr-x  2 dab12 dab12 4.0K Mar 26 19:51 folder3
```

> Como se puede observar, los parámetros en muchos de los comandos de Linux se pueden agrupar con un mismo guion. Por lo tanto, el resultado de `ls -l -a -h` es idéntico al de `ls -lah`.

### `cat`

Este comando es utilizado para listar los contenidos de un archivo (no un directorio) a la salida estándar (_stdout_) de la consola. Con este comando podemos obtener lo que contenga un archivo de texto directamente en la terminal.

Ejemplo:

```bash
$ cat file.txt
¡Hola mundo! Este contenido se escribió con el block de notas y se guardó en este archivo TXT.
```

### `cp`

Este comando se utiliza para copiar archivos o directorios completos de un lugar a otro. Por ejemplo, si queremos copiar el archivo `file.txt` a `/home/user/Desktop`, usaríamos la siguiente sintaxis: `cp file.txt /home/user/Desktop`.

```bash
$ pwd
/home/user/Documents
$ ls
file.txt
$ cat file.txt
¡Hola mundo! Este contenido se escribió con el block de notas y se guardó en este archivo TXT.
$ cp file.txt ~/Desktop
$ cd ~/Desktop
$ pwd
/home/user/Desktop
$ cat file.txt
¡Hola mundo! Este contenido se escribió con el block de notas y se guardó en este archivo TXT.
```

> Si se desea copiar un directorio entero, se debe añadir el parámetro `-R`.

### `mv`

Este comando principalmente se utiliza para mover archivos o carpetas, pero también se puede utilizar para renombrar. Los argumentos de este comando son similares a los del comando `cp`. En este caso, como primer argumento se recibirá la ruta al archivo que se desea mover. Como segundo argumento, se recibirá la ruta de destino del archivo.

> Para renombrar un archivo, la sintaxis sería la siguiente: `mv nombreantiguo.txt nuevonombre.txt`.

### `mkdir`

Este comando es utilizado para crear carpetas. Por ejemplo, si escribimos `mkdir Music` se creará una carpeta con nombre `Music` en el directorio en el que nos encontremos.

- Para crear un directorio dentro de otro directorio, se usaría una sintaxis como esta: `mkdir Music/NewDir`.
- En caso queramos crear las carpetas intermediarias si es que estas no existen, podemos usar el parámetro `-p`. Por ejemplo, para crear una carpeta dentro de `NewDir` sin que hayamos creado esta carpeta previamente, usaríamos la siguiente sintaxis: `mkdir -p Music/NewDir/NestedDir`.

### `rm`

Este comando se utiliza para eliminar archivos o directorios (con todos sus contenidos). Si se desea eliminar una carpeta que no está vacía, se debe añadir los parámetros `-rf`.

```bash
$ ls /home/user
file.png  folder1  folder2  folder3
$ ls /home/user/folder3
file.txt  image.png  folder
$ rm -rf /home/user/folder3
$ ls /home/user
file.png  folder1  folder2
$ ls /home/user/folder3
ls: cannot access '/home/user/folder3': No such file or directory
```

> Es importante **siempre revisar el comando antes de ejecutarlo**. El resultado de este comando **no se puede deshacer** (no existe una papelera de reciclaje con este comando).

### `touch`

Este comando se utiliza para crear archivos en blanco a través de la terminal. Por ejemplo, para crear un archivo vacío con el nombre `file.txt` en el directorio en el que nos encontramos usaríamos el siguiente comando:

```bash
$ ls
image.png
$ touch file.txt
$ ls
file.txt  image.png
$ cat file.txt
# No se obtendrá ningún resultado
```

### `echo`

Este comando se utiliza para emitir una cadena de texto (_string_) a la consola. También se puede utilizar para agregar contenido a un archivo utilizando la redirección de la salida estándar a un archivo.

```bash
$ ls
anotherfile.txt
$ cat anotherfile.txt
¡Hola mundo!
$ echo Hello World!
Hello World!
$ echo Hello World! >> file.txt
$ cat file.txt
Hello World!
$ echo Hello World! >> anotherfile.txt
$ cat anotherfile.txt
¡Hola mundo!
Hello World!
```

> La redirección de la salida estándar se cubrirá en un capítulo posterior.

### Bonus Tips

Si escribes `clear` en la terminal, se eliminará el contenido que haya en la consola y será como una consola limpia.

> Este comando es similar al comando `cls` en la consola de Windows.

La terminal de Linux acepta que presiones la tecla <kbd>TAB</kbd> para autocompletar directorios y archivos en la carpeta en la que te encuentres. Además, algunos comandos pueden autocompletar secciones de su sintaxis propia. Por ejemplo, si tenemos la carpeta `Documents` en la carpeta en la que nos encontramos, puedes escribir `cd Docu` y luego presionar <kbd>TAB</kbd> y la terminal se encargará de autocompletar lo restante.

Finalmente, con la combinación de teclas <kbd>Ctrl</kbd> + <kbd>C</kbd> podemos cancelar la ejecución de un comando en cualquier momento. Esto puede ser útil en casos que un comando se haya congelado y sea necesario cancelar su ejecución forzosamente.

---

Estos son solo algunos de los comandos más utilizados en Linux. Si deseas conocer algunos más, puedes ingresar a [este](https://www.hostinger.com/tutorials/linux-commands) enlace.

> El documento en el enlace ha sido la fuente para la redacción de algunas partes de este capítulo.

## Permisos de superusuario

En Windows, existe el diálogo para permitir un programa se ejecute con permisos de administrador. Este incluye un texto similar al siguiente:

```txt
Control de cuentas de usuario

¿Quieres permitir que esta aplicación haga cambios en el dispositivo?

[Logo] aplicación.exe

Editor comprobado: Aplicación Inc.
Origen del archivo: Unidad de disco duro en este equipo

Mostrar más detalles

Sí      No
```

A través de este diálogo se le puede permitir a una aplicación acceder a características que están bloqueadas por defecto (como por ejemplo, instalar aplicaciones, modificar ajustes del sistema, etc).

En la terminal de Linux, podemos elevar los privilegios de un programa con añadiendo al inicio de este comando la palabra `sudo`. Este comando sirve para permitirle el acceso a un programa a los permisos más elevados del sistema a cambio de la contraseña de la cuenta de usuario.

> **No todas las cuentas de usuario de Linux pueden usar el comando `sudo`.** En una instalación por defecto de Ubuntu, la primera cuenta creada podrá utilizar el comando, mientras que otras no. En el caso de un servidor compartido (como en los _hosting_), muchas veces el acceso a los privilegios elevados están bloqueados (por lo tanto, el acceso a este comando estará bloqueado).

Al utilizar este comando, veremos en la consola una salida similar a la siguiente:

```bash
$ sudo echo ¡Hola Mundo!
[sudo] password for user:
¡Hola Mundo!
```

En este caso, el comando `echo` se ha ejecutado con los más altos privilegios en el sistema. Al intentar ejecutar el comando, `sudo` nos pidió que ingresemos la contraseña para el usuario con el que hemos iniciado sesión (en este caso, el usuario es `user`).

> **La contraseña que escribamos no será visible en la consola**. Esto quiere decir que al escribir la contraseña será a ciegas. **No te preocupes si no ves que se añadan caracteres, esto está correcto**. `sudo` no muestra ningún carácter especial por cada carácter de la contraseña, solo se verá como si nada se hubiera ingresado.

Obviamente, ejecutar el comando `echo` con privilegios no brinda ningún beneficio especial. Más adelante veremos algunos comandos que requerirán usar el comnado `sudo` para que puedan funcionar (por ejemplo, al instalar paquetes con el comando `apt`).

Una vez que hayamos ingresado en la terminal la contraseña, la terminal recordará que hemos ingresado la contraseña correctamente y no la volverá a pedir por un tiempo. Esto quiere decir que no te pedirá que ingreses la contraseña para cada comando que ejecutes con `sudo`. Si es que recientemente has ejecutado otro comando con `sudo` e ingresaste la contraseña correctamente, el comando que ejecutes ya no requerirá la contraseña.

```bash
# La primera vez en la terminal siempre nos pedirá la contraseña
$ sudo echo ¡Hola Mundo!
[sudo] password for user:
¡Hola Mundo!
# Las siguientes veces ya no será necesario
$ sudo echo Hello World!
Hello World!
```

## Instalación de paquetes

Muchas veces, necesitaremos de _software_ adicional en el sistema. En Windows, tenemos que descargar el instalador (un archivo `.exe` o un `.msi`) y abrirlo, realizar todos los pasos que nos pida y esperar que se instale. En el caso de Ubuntu, tenemos un administrador de paquetes disponible incluido con el sistema, llamado `apt`.

> En el caso de otras distribuciones, habrán otros administradores como _RPM_ en el caso de _Red Hat_, o el _Pacman Package Manager_ en el caso de _Arch Linux_. Puedes leer más sobre los administradores de paquetes de Linux [aquí](https://en.wikipedia.org/wiki/Package_manager).

A través de `apt`, podremos instalar casi todos el _software_ disponible en los repositorios oficiales de Ubuntu. Asimismo, podremos añadir repositorios de terceros para conseguir acceso a otros tipos de _software_ que no están disponibles en los repositorios oficiales.

> En esta guía no se cubrirá como trabajar con repositorios de terceros.

Es importante mencionar que la tarea de instalar paquetes en el sistema **está restringida a usuarios con los mayores privilegios en el sistema**. Por lo tanto, si se desea instalar un paquete con `apt`, **tendremos que añadir el comando `sudo` al inicio de cada comando**.

> En muchas guías de Linux, se utiliza el comando `apt-get`. Estas guías fueron escritas antes del lanzamiento del comando `apt` y no han sido actualizadas. **Es preferible usar el `apt` en vez de `apt-get`** debido a que el primero ha sido diseñado para el usuario final y combina varias herramientas útiles que normalmente serían ejecutadas con otros comandos completamente diferentes. Además, el comando `apt` muestra el resultado de los comandos con colores y con una barra interactiva. Puedes leer más sobre las diferencias en [este enlace](https://askubuntu.com/a/446484).

### Funcionamiento

Internamente, `apt` solo descarga de los repositorios archivos que contienen los ejecutables y los archivos necesarios para ejecutar un cierto _software_, así como también sus dependencias (otros paquetes con código que el _software_ a instalar necesita).

Estos archivos descargados por `apt` son archivos que utilizan la extensión `.deb`. Luego de ser descargados, `apt` los redirige a otra herramienta del sistema, `dpkg`, que es la encargada de procesar los archivos `.deb` e instalarlos.

> Este procedimiento está sobresimplificado para no complicar la lectura de este documento. Internamente, `apt` realiza muchos procesos más.

### Comandos

A continuación, se mencionan algunos de los comandos más importantes de `apt`:

> Se ha omitido el comando `sudo` en estos ejemplos, pero siempre debe ser incluido.

- `apt update`: Nos permite actualizar la lista de repositorios que tiene el sistema, así `apt` puede saber si es que hay nuevas actualizaciones para un paquete que está instalado en el sistema.
- `apt upgrade`: Nos permite actualizar todos los paquetes que tengan una actualización disponible.
- `apt install <package>`: Nos permite instalar un nuevo paquete en el sistema.
- `apt remove <package>`: Nos permite eliminar un paquete entero del sistema (sin incluir los archivos que este generó mientras estaba instalado).
- `apt purge <package>`: Similar al comando `apt remove`, pero con la diferencia principal que este **sí elimina** la gran mayoría de archivos generados.
- `apt list`: Muestra una lista de todos los paquetes disponibles para instalar desde los repositorios configurados con `apt` (incluidos los paquetes de repositorios de terceros, si los hay).
- `apt list --installed`: Muestra una lista de todos los paquetes instalados en el sistema.
- `apt list --upgradable`: Muestra una lista de todos los paquetes que tienen una actualización disponible.
- `apt show <package>`: Muestra información detallada sobre un paquete en específico.

### `dpkg`

Esta herramienta del sistema es la encargada de procesar los paquetes de instalación de un _software_, ya sean estos descargados por `apt` o por el usuario de una fuente externa.

Para instalar un paquete que se ha descargado de alguna fuente externa (no a través del comando `apt`), se puede utilizar la siguiente sintaxis:

```bash
sudo dpkg -i package.deb
```

En caso que este paquete tenga dependencias que no se han encontrado en el sistema, la instalación fallará con un mensaje pidiendo que se instalen las dependencias necesarias. Para realizar este proceso y terminar la instalación de este paquete, podemos instalar las dependencias con `apt` con el siguiente comando:

```bash
sudo apt install -f
```

Este comando reconocerá la instalación fallida de `dpkg`, descargará las dependencias necesarias para continuar, las instalará y continuará con la instalación del paquete `.deb`.

### Troubleshooting

Aquí encontrarás algunos de los problemas más comunes con `apt`, así como sus soluciones.

> Estas soluciones usarán comandos más complejos los cuales no han sido explicados. **Procura seguir las indicaciones correctamente para no causar un daño irreversible al sistema**.

#### `Could not get lock /var/lib/dpkg/lock`

```bash
E: Could not get lock /var/lib/dpkg/lock – open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```

La razón de este problema es que `apt` ya está procesando alguna otra instalación, ya sea una actualización automática o una instalación de un paquete que está corriendo al mismo tiempo.

**Solución:**

Primero, ejecuta el siguiente comando en la terminal: `ps aux | grep -i apt`. Deberías tener una salida como la siguiente:

```bash
$ ps aux | grep -i apt
root  1464  0.0  0.0   4624   772 ?        Ss   19:08   0:00 /bin/sh /usr/lib/apt/apt.systemd.daily update
root  1484  0.0  0.0   4624  1676 ?        S    19:08   0:00 /bin/sh /usr/lib/apt/apt.systemd.daily lock_is_held update
_apt  2836  0.8  0.1  96912  9432 ?        S    19:09   0:03 /usr/lib/apt/methods/http
user  6172  0.0  0.0  21532  1152 pts/1    S+   19:16   0:00 grep --color=auto -i apt
```

Si es que en la salida se menciona algo sobre `apt.systemd.daily`, esto significa que se están ejecutando unas actualizaciones automáticas en el fondo. Si ese es el caso, deberás esperar a que termine de procesar estas actualizaciones.

Si es que no hay una mención a `apt.systemd.daily`, y en cambio la salida es más similar a la siguiente:

```bash
$ ps aux | grep -i apt
root  1464  0.0  0.0   4624   772 ?        Ss   19:08   0:00 sudo apt update
user  6172  0.0  0.0  21532  1152 pts/1    S+   19:16   0:00 grep --color=auto -i apt
```

Puedes también esperar a que termine de procesar lo que esté procesándose, o puedes forzar que el proceso termine con el siguiente comando:

> El ID del proceso es el número en la segunda columna de valores (en este caso, sería 1464).

```bash
sudo kill <id_del_proceso>
```

Después de ejecutar este comando, prueba ejecutar `ps aux | grep -i apt` de nuevo y revisa si es que el proceso desapareció (puedes ignorar el que menciona el comando `grep`). Si es que al proceso aun aparece, utiliza este comando con el mismo ID de proceso:

```bash
sudo kill -9 <id_del_proceso>
```

Otra manera de realizar este proceso sin tener que buscar el ID del proceso es con el siguiente comando (pero más seguro es el método anterior):

```bash
sudo killall apt apt-get
```

Si es que después de todos estos comandos `apt` aun sigue emitiendo el mismo error, puedes ejecutar estos comandos y ver cuál de ellos emite un resultado:

```bash
sudo lsof /var/lib/dpkg/lock
sudo lsof /var/lib/apt/lists/lock
sudo lsof /var/cache/apt/archives/lock
```

Algunos de estos comandos no emitirán resultado, y alguno podría emitir un resultado similar a este:

```bash
$ sudo lsof /var/lib/apt/lists/lock
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
apt     3342 root    4uW  REG   8,16        0 3815 /var/lib/apt/lists/lock
apt     3777 root    4u   REG   8,16        0 3815 /var/lib/apt/lists/lock
```

Si obtienes un resultado en alguno de estos comandos, obtén el número que se ve en la columna PID y ejecutalo en este comando:

```bash
sudo kill -9 <id_del_proceso>
```

Luego, puedes eliminar los archivos que estaban causando el problema `apt`:

```bash
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```

Finalmente, deberás reconfigurar los paquetes para que el sistema se pueda recuperar de las actualizaciones sin terminar:

```bash
sudo dpkg --configure -a
```

> En caso ninguna de estas soluciones funcione, puedes encontrar más soluciones en [este enlace](https://itsfoss.com/could-not-get-lock-error/).

## Windows Subsystem for Linux (WSL)

WSL es un _software_ creado por Microsoft para permitir la interoperabilidad entre Windows 10 y alguna distribución de Linux de las que están disponibles en la Microsoft Store.

Con WSL, se puede ejecutar una instancia de Linux completa sin tener que reemplazar el sistema operativo principal (Windows). Además, se podrá utilizar programas de Windows en paralelo a programas de Linux.

### Instalación

Puedes leer más sobre la instalación de WSL en Windows 10 en el [siguiente enlace](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

> Es recomendable realizar la configuración para usar WSL2 debido a que este es más eficiente que WSL1.

### Windows Terminal

Windows Terminal es la nueva versión de Microsoft para el famoso CMD. Esta aplicación incluye un tema más moderno, además de pestañas para agrupar diferentes terminales en una misma ventana. Para instalar esta herramienta, puedes leer la documentación de Microsoft [aquí](https://docs.microsoft.com/en-us/windows/terminal/get-started).

> Es recomendable usar WSL con Windows Terminal, pero este paso es completamente opcional y WSL funcionará sin Windows Terminal también.

### Visual Studio Code

Este es el famoso editor de Microsoft que permite la integración de nuevos lenguajes a través de nuevas extensiones que se pueden instalar desde su _marketplace_. Asimismo permite trabajar directamente en WSL a través de una extensión. Puedes leer más sobre esta configuración en la documentación de Microsoft [aquí](https://code.visualstudio.com/docs/remote/wsl).
