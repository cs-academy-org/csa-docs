Administración de permisos y propiedad de archivos
Introducción
Al ser un sistema multiusuario, Linux necesita alguna forma de rastrear quién posee cada archivo y si un usuario puede o no realizar acciones en ese archivo. Esto es para garantizar la privacidad de los usuarios que deseen mantener la confidencialidad del contenido de sus archivos, así como para garantizar la colaboración al hacer que ciertos archivos sean accesibles para múltiples usuarios.



Esto se realiza a través de un sistema de permisos de tres niveles: cada archivo en el disco es propiedad de un usuario y un grupo de usuarios y tiene tres conjuntos de permisos: uno para su propietario, otro para el grupo propietario del archivo y otro para todos los demás. En esta lección, aprenderá cómo consultar los permisos de un archivo y cómo manipularlos.



Consulta de información sobre archivos y directorios
El comando ls se usa para obtener una lista de los contenidos de cualquier directorio. En esta forma básica, todo lo que obtienes son los nombres de archivo:

$ ls
Another_Directory  picture.jpg  text.txt
Pero hay mucha más información disponible para cada archivo, incluyendo tipo, tamaño, propiedad y más. Para ver esta información, debe solicitar a ls una lista de "formato largo", utilizando el parámetro -l:
$ ls -l
total 536
drwxrwxr-x 2 carol carol   4096 Dec 10 15:57 Another_Directory
-rw------- 1 carol carol 539663 Dec 10 10:43 picture.jpg
-rw-rw-r-- 1 carol carol   1881 Dec 10 15:57 text.txt

Cada columna en la salida anterior tiene un significado:

La primera columna de la lista muestra el tipo de archivo y los permisos.
Por ejemplo, en drwxrwxr-x:
El primer carácter, d, indica el tipo de archivo.
Los siguientes tres caracteres, rwx, indican los permisos para el propietario del archivo, también conocido como usuario o u.
Los siguientes tres caracteres, rwx, indican los permisos del grupo propietario del archivo, también conocido como g.
Los últimos tres caracteres, r-x, indican los permisos para cualquier otra persona, también conocidos como otros u o.
La segunda columna indica el número de enlaces duros que apuntan a ese archivo. Para un directorio, esto significa el número de subdirectorios, más un enlace a sí mismo (.) Y al directorio padre (..).
Las columnas tercera y cuarta muestran información de propiedad: respectivamente, el usuario y el grupo que posee el archivo.
La quinta columna muestra el tamaño del archivo, en bytes.
La sexta columna muestra la fecha y hora precisas, o la marca de tiempo, cuando se modificó el archivo por última vez.
La séptima y última columna muestra el nombre del archivo.


Si desea ver los tamaños de archivo en formato "legible para humanos", agregue el parámetro -h a ls. Los archivos de menos de un kilobyte tendrán el tamaño que se muestra en bytes. Los archivos con más de un kilobyte y menos de un megabyte tendrán una K agregada después del tamaño, lo que indica que el tamaño está en kilobytes. Lo mismo sigue para los tamaños de archivo en los rangos de megabyte (M) y gigabyte (G):

$ ls -lh
total 1,2G
drwxrwxr-x 2 carol carol 4,0K Dec 10 17:59 Another_Directory
----r--r-- 1 carol carol    0 Dec 11 10:55 foo.bar
-rw-rw-r-- 1 carol carol 1,2G Dec 20 18:22 HugeFile.zip
-rw------- 1 carol carol 528K Dec 10 10:43 picture.jpg
---xr-xr-x 1 carol carol   33 Dec 11 10:36 test.sh
-rwxr--r-- 1 carol carol 1,9K Dec 20 18:13 text.txt
-rw-rw-r-- 1 carol carol 2,6M Dec 11 22:14 Zipped.zip
Para mostrar solo información sobre un conjunto específico de archivos, agregue los nombres de estos archivos a ls:
$ ls -lh HugeFile.zip test.sh
total 1,2G
-rw-rw-r-- 1 carol carol 1,2G Dec 20 18:22 HugeFile.zip
---xr-xr-x 1 carol carol   33 Dec 11 10:36 test.sh
¿Qué pasa con los directorios?
Si intenta consultar información sobre un directorio usando ls -l, en su lugar le mostrará una lista del contenido del directorio:

$ ls -l Another_Directory/
total 0
-rw-r--r-- 1 carol carol 0 Dec 10 17:59 another_file.txt
Para evitar esto y consultar información sobre el directorio en sí, agregue el parámetro -d a ls:
$ ls -l -d Another_Directory/
drwxrwxr-x 2 carol carol 4096 Dec 10 17:59 Another_Directory/

Ver archivos ocultos
El listado del directorio que hemos recuperado usando ls -l antes está incompleto:

$ ls -l
total 544
drwxrwxr-x 2 carol carol   4096 Dec 10 17:59 Another_Directory
-rw------- 1 carol carol 539663 Dec 10 10:43 picture.jpg
-rw-rw-r-- 1 carol carol   1881 Dec 10 15:57 text.txt
Hay otros tres archivos en ese directorio, pero están ocultos. En Linux, los archivos cuyo nombre comienza con un punto (.) Se ocultan automáticamente. Para verlos necesitamos agregar el parámetro -a a ls:
$ ls -l -a
total 544
drwxrwxr-x 3 carol carol   4096 Dec 10 16:01 .
drwxrwxr-x 4 carol carol   4096 Dec 10 15:56 ..
drwxrwxr-x 2 carol carol   4096 Dec 10 17:59 Another_Directory
-rw------- 1 carol carol 539663 Dec 10 10:43 picture.jpg
-rw-rw-r-- 1 carol carol   1881 Dec 10 15:57 text.txt
-rw-r--r-- 1 carol carol      0 Dec 10 16:01 .thisIsHidden
El archivo .thisIsHidden simplemente está oculto porque su nombre comienza con (.).



Los directorios y ... sin embargo son especiales. (.) es un puntero al directorio actual, mientras que (..) es un puntero al directorio principal (el directorio que contiene el directorio actual). En Linux, cada directorio contiene al menos estos dos directorios especiales.

Comprender los tipos de archivo
Ya hemos mencionado que la primera letra en cada salida de ls -l describe el tipo de archivo. Los tres tipos de archivos más comunes son:



- (archivo normal)

Un archivo puede contener datos de cualquier tipo. Los archivos se pueden modificar, mover, copiar y eliminar.


d (directorio)

Un directorio contiene otros archivos o directorios y ayuda a organizar el sistema de archivos. Técnicamente, los directorios son un tipo especial de archivo.


l (enlace suave)

Este "archivo" es un puntero a otro archivo o directorio en otra parte del sistema de archivos.


Además de esos, hay otros tres tipos de archivos que al menos debería conocer, pero que están fuera del alcance de esta lección:



b (dispositivo de bloque)

Este archivo representa un dispositivo virtual o físico, generalmente discos u otros tipos de dispositivos de almacenamiento. Por ejemplo, el primer disco duro del sistema podría estar representado por /dev/sda.


c (dispositivo de caracteres)

Este archivo representa un dispositivo virtual o físico. Los terminales (como el terminal principal en /dev/ttyS0) y los puertos seriales son ejemplos comunes de dispositivos de caracteres.


s (sockets)

Los sockets sirven como "conductos" para pasar información entre dos programas.


Advertencia: No modifique ninguno de los permisos en dispositivos de bloque, dispositivos de caracteres o sockets, a menos que sepa lo que está haciendo. ¡Esto puede evitar que su sistema funcione!



Entendiendo los permisos
En la salida de ls -l, los permisos de archivo se muestran justo después del tipo de archivo, como tres grupos de tres caracteres cada uno, en el orden r, w y x. Esto es lo que quieren decir. Tenga en cuenta que un guión representa la falta de un permiso particular.



Permisos en archivos

r

Significa lectura y tiene un valor octal de 4 (no se preocupe, hablaremos de los octales en breve). Esto significa permiso para abrir un archivo y leer su contenido.


w

Significa escritura y tiene un valor octal de 2. Esto significa permiso para editar o eliminar un archivo.


x

Significa ejecutar y tiene un valor octal de 1. Esto significa que el archivo puede ejecutarse como un ejecutable o script.


Entonces, por ejemplo, un archivo con permisos rw- se puede leer y escribir, pero no se puede ejecutar.



Permisos en directorios
r

Significa lectura y tiene un valor octal de 4. Esto significa permiso para leer el contenido del directorio, como los nombres de archivo. Pero no implica permiso para leer los archivos ellos mismos.


w

Significa escritura y tiene un valor octal de 2. Esto significa permiso para crear o eliminar archivos en un directorio, o cambiar sus nombres, permisos y propietarios. Si un usuario tiene permiso de escritura en un directorio, el usuario puede cambiar los permisos de cualquier archivo en el directorio, incluso si el usuario no tiene permisos en el archivo o si el archivo es propiedad de otro usuario.


x

Significa ejecutar y tiene un valor octal de 1. Esto significa permiso para ingresar a un directorio, pero no para enumerar sus archivos (para eso, se necesita el permiso r).


El último bit sobre directorios puede sonar un poco confuso. Imaginemos, por ejemplo, que tiene un directorio llamado Another_Directory, con los siguientes permisos:

$ ls -ld Another_Directory/
d--xr-xr-x 2 carol carol 4,0K Dec 20 18:46 Another_Directory
También imagine que dentro de este directorio tiene un script de shell llamado hello.sh con los siguientes permisos:
-rwxr-xr-x 1 carol carol 33 Dec 20 18:46 hello.sh
Si usted es el villancico e intenta enumerar el contenido de Another_Directory, recibirá un mensaje de error, ya que su usuario carece de permiso de lectura para ese directorio:
$ ls -l Another_Directory/
ls: cannot open directory 'Another_Directory/': Permission denied
Sin embargo, el usuario Carol tiene permisos de ejecución, lo que significa que puede ingresar al directorio. Por lo tanto, el usuario villancico puede acceder a los archivos dentro del directorio, siempre que tenga los permisos correctos para el archivo respectivo. En este ejemplo, el usuario tiene permisos completos para el script hello.sh, por lo que puede ejecutar el script, incluso si no puede leer el contenido del directorio que lo contiene. Todo lo que se necesita es el nombre de archivo completo.
$ sh Another_Directory/hello.sh
Hello LPI World!

Como dijimos antes, los permisos se especifican en secuencia: primero para el propietario del archivo, luego para el grupo propietario y luego para otros usuarios. Cada vez que alguien intenta realizar una acción en el archivo, los permisos se verifican de la misma manera. Primero, el sistema verifica si el usuario actual posee el archivo, y si esto es cierto, aplica solo el primer conjunto de permisos. De lo contrario, verifica si el usuario actual pertenece al grupo propietario del archivo. En ese caso, solo aplica el segundo conjunto de permisos. En cualquier otro caso, el sistema aplicará el tercer conjunto de permisos. Esto significa que si el usuario actual es el propietario del archivo, solo los permisos del propietario son efectivos, incluso si el grupo u otros permisos son más permisivos que los permisos del propietario.



Modificar permisos de archivo

El comando chmod se usa para modificar los permisos de un archivo y toma al menos dos parámetros: el primero describe qué permisos cambiar y el segundo apunta al archivo o directorio donde se realizará el cambio. Sin embargo, los permisos para cambiar se pueden describir de dos maneras diferentes, o "modos".
El primero, llamado modo simbólico, ofrece un control detallado, lo que le permite agregar o revocar un solo permiso sin modificar otros en el conjunto. El otro modo, llamado modo numérico, es más fácil de recordar y más rápido de usar si desea establecer todos los valores de permiso a la vez.
Ambos modos conducirán al mismo resultado final. Entonces, por ejemplo, los comandos:

$ chmod ug+rw-x,o-rwx text.txt
o
$ chmod 660 text.txt

producirá exactamente la misma salida, un archivo con los permisos establecidos:

-rw-rw---- 1 carol carol  765 Dec 20 21:25 text.txt

Ahora, comprendamos cómo funciona cada modo.



Modo simbólico
Al describir qué permisos cambiar en modo simbólico, los primeros caracteres indican los permisos que alterará: los del usuario (u), del grupo (g), de otros (o) y / o para los tres juntos (a).



Luego debe decirle al comando qué hacer: puede otorgar un permiso (+), revocar un permiso (-) o establecerlo en un valor específico (=).



Por último, especifique a qué permiso desea afectar: leer (r), escribir (w) o ejecutar (x).



Por ejemplo, imagine que tenemos un archivo llamado text.txt con el siguiente conjunto de permisos:

$ ls -l text.txt
-rw-r--r-- 1 carol carol 765 Dec 20 21:25 text.txt
Si desea otorgar permisos de escritura a los miembros del grupo que posee el archivo, usaría el parámetro g + w. Es más fácil si lo piensa de esta manera: "Para el grupo (g), otorgue (+) permisos de escritura (w)". Entonces, el comando sería:
$ chmod g+w text.txt
Verifiquemos el resultado con ls:
$ ls -l text.txt

-rw-rw-r-- 1 carol carol 765 Dec 20 21:25 text.txt

Si desea eliminar los permisos de lectura para el propietario del mismo archivo, piense en ello como: "Para el usuario (u) revocar (-), permisos de lectura (r)". Entonces el parámetro es u-r, así:

$ chmod u-r text.txt
$ ls -l text.txt
--w-rw-r-- 1 carol carol 765 Dec 20 21:25 text.txt
¿Qué pasa si queremos establecer los permisos exactamente como rw- para todos? Luego piense en ello como: "Para todos (a), establezca exactamente (=), lea (r), escriba (w) y no ejecute (-)". Entonces:
$ chmod a=rw- text.txt
$ ls -l text.txt
-rw-rw-rw- 1 carol carol 765 Dec 20 21:25 text.txt
Por supuesto, es posible modificar múltiples permisos al mismo tiempo. En este caso, sepárelos con una coma (,):
$ chmod u+rwx,g-x text.txt
$ ls -lh text.txt
-rwxrw-rw- 1 carol carol 765 Dec 20 21:25 text.txt
El ejemplo anterior se puede leer como: “Para el usuario (u), otorgue (+) permisos de lectura, escritura y ejecución (rwx), para el grupo (g) revoque (-), ejecute permisos (x)”.



Cuando se ejecuta en un directorio, chmod modifica solo los permisos del directorio. chmod tiene un modo recursivo, útil cuando desea cambiar los permisos para "todos los archivos dentro de un directorio y sus subdirectorios". Para usar esto, agregue el parámetro -R después del nombre del comando y antes de que cambien los permisos, así:

$ chmod -R u+rwx Another_Directory/
Este comando se puede leer como: "Recursivamente (-R), para el usuario (u), otorgar (+) permisos de lectura, escritura y ejecución (rwx)".


Advertencia: Tenga cuidado y piense dos veces antes de usar el modificador -R, ya que es fácil cambiar los permisos en archivos y directorios que no desea cambiar, especialmente en directorios con una gran cantidad de archivos y subdirectorios.



Modo numérico
En el modo numérico, los permisos se especifican de manera diferente: como un valor numérico de tres dígitos en notación octal, un sistema numérico de base 8.



Cada permiso tiene un valor correspondiente, y se especifican en el siguiente orden: primero viene read (r), que es 4, luego write (w), que es 2 y el último es execute (x), representado por 1. Si existe no hay permiso, use el valor cero (0). Entonces, un permiso de rwx sería 7 (4 + 2 + 1) y r-x sería 5 (4 + 0 + 1).



El primero de los tres dígitos en el conjunto de permisos representa los permisos para el usuario (u), el segundo para el grupo (g) y el tercero para el propietario (o). Si quisiéramos establecer los permisos para un archivo en rw-rw ----, el valor octal sería 660:

$ chmod 660 text.txt
$ ls -l text.txt
-rw-rw---- 1 carol carol 765 Dec 20 21:25 text.txt
Además de esto, la sintaxis en modo numérico es la misma que en modo simbólico, el primer parámetro representa los permisos que desea cambiar, y el segundo apunta al archivo o directorio donde se realizará el cambio.



Pista: Si un valor de permiso es impar, ¡el archivo seguramente es ejecutable!



¿Qué sintaxis usar? Se recomienda el modo numérico si desea cambiar los permisos a un valor específico, por ejemplo 640 (rw- r-- ---).



El modo simbólico es más útil si desea voltear solo un valor específico, independientemente de los permisos actuales para el archivo. Por ejemplo, puedo agregar permisos de ejecución para el usuario usando solo chmod u + x script.sh sin tener en cuenta, o incluso tocar, los permisos actuales para el grupo y otros.

Modificación de la propiedad del archivo
El comando chown se usa para modificar la propiedad de un archivo o directorio. La sintaxis es bastante simple:

chown username:groupname filename
Por ejemplo, verifiquemos un archivo llamado text.txt:

$ ls -l text.txt
-rw-rw---- 1 carol carol 1881 Dec 10 15:57 text.txt
El usuario que posee el archivo es Carol, y el grupo también es Carol. Ahora, modifiquemos el grupo que posee el archivo a otro grupo, como los estudiantes:
$ chown carol:students text.txt
$ ls -l text.txt
-rw-rw---- 1 carol students 1881 Dec 10 15:57 text.txt

Tenga en cuenta que el usuario que posee un archivo no necesita pertenecer al grupo que posee un archivo. En el ejemplo anterior, el usuario villancico no necesita ser miembro del grupo de estudiantes. Sin embargo, ella tiene que ser miembro del grupo para transferir la propiedad del grupo del archivo a ese grupo.



El usuario o grupo puede omitirse si no desea cambiarlos. Entonces, para cambiar solo el grupo que posee un archivo, usaría chown: students text.txt. Para cambiar solo el usuario, el comando sería chown carol: text.txt o simplemente chown carol text.txt. Alternativamente, puede usar el comando chgrp students text.txt para cambiar solo el grupo.



A menos que sea el administrador del sistema (root), no puede cambiar la propiedad de un archivo propiedad de otro usuario o grupo al que no pertenece. Si intenta hacer esto, recibirá el mensaje de error Operación no permitida.



Consultar grupos
Antes de cambiar la propiedad de un archivo, puede ser útil saber qué grupos existen en el sistema, qué usuarios son miembros de un grupo y a qué grupos pertenece un usuario. Esas tareas se pueden lograr con dos comandos, grupos y groupmems.



Para ver qué grupos existen en su sistema, simplemente escriba grupos:

$ groups
carol students cdrom sudo dip plugdev lpadmin sambashare
Y si desea saber a qué grupos pertenece un usuario, agregue el nombre de usuario como parámetro:
$ groups carol
carol : carol students cdrom sudo dip plugdev lpadmin sambashare
Para hacer lo contrario, mostrar qué usuarios pertenecen a un grupo, use groupmems. El parámetro -g especifica el grupo, y -l enumerará todos sus miembros:
$ groupmems -g cdrom -l
carol

Permisos especiales
Además de los permisos de lectura, escritura y ejecución para usuarios, grupos y otros, cada archivo puede tener otros tres permisos especiales que pueden alterar la forma en que funciona un directorio o cómo se ejecuta un programa. Se pueden especificar en modo simbólico o numérico, y son los siguientes:

Stiky Bit


Stiky Bit, también llamado indicador de eliminación restringida, tiene el valor octal 1 y en modo simbólico está representado por una t dentro de los permisos del otro. Esto se aplica solo a los directorios, y en Linux evita que los usuarios eliminen o cambien el nombre de un archivo en un directorio a menos que sean propietarios de ese archivo o directorio.
Los directorios con el conjunto de bits fijos muestran una t que reemplaza la x en los permisos para otros en la salida de ls -l:

$ ls -ld Sample_Directory/
drwxr-xr-t 2 carol carol 4096 Dec 20 18:46 Sample_Directory/
En el modo numérico, los permisos especiales se especifican utilizando una "notación de 4 dígitos", con el primer dígito que representa el permiso especial para actuar. Por ejemplo, para establecer el bit fijo (valor 1) para el directorio Another_Directory en modo numérico, con permisos 755, el comando sería:

$ chmod 1755 Another_Directory
$ ls -ld Another_Directory
drwxr-xr-t 2 carol carol 4,0K Dec 20 18:46 Another_Directory
Establecer GID
Set GID, también conocido como SGID o Set Group ID bit, tiene el valor octal 2 y en modo simbólico está representado por una s en los permisos del grupo. Esto se puede aplicar a archivos ejecutables o directorios. En los archivos ejecutables, otorgará al proceso resultante de la ejecución del acceso al archivo los privilegios del grupo que posee el archivo. Cuando se aplica a los directorios, hará que cada archivo o directorio creado debajo herede el grupo del directorio principal.



Los archivos y directorios con bit SGID muestran una s que reemplaza la x en los permisos para el grupo en la salida de ls -l:

$ ls -l test.sh
-rwxr-sr-x 1 carol carol 33 Dec 11 10:36 test.sh
Para agregar permisos SGID a un archivo en modo simbólico, el comando sería:
$ chmod g+s test.sh
$ ls -l test.sh
-rwxr-sr-x 1 carol root     33 Dec 11 10:36 test.sh
El siguiente ejemplo lo hará comprender mejor los efectos de SGID en un directorio. Supongamos que tenemos un directorio llamado Sample_Directory, propiedad del usuario carol y los usuarios del grupo, con la siguiente estructura de permisos:
$ ls -ldh Sample_Directory/
drwxr-xr-x 2 carol users 4,0K Jan 18 17:06 Sample_Directory/
Ahora, cambiemos a este directorio y, usando el comando touch, creemos un archivo vacío dentro de él. El resultado sería:
$ cd Sample_Directory/
$ touch newfile
$ ls -lh newfile
-rw-r--r-- 1 carol carol 0 Jan 18 17:11 newfile
Como podemos ver, el archivo es propiedad del usuario Carol y Group Carol. Pero, si el directorio tuviera el conjunto de permisos SGID, el resultado sería diferente. Primero, agreguemos el bit SGID al Sample_Directory y verifiquemos los resultados:
$ sudo chmod g+s Sample_Directory/
$ ls -ldh Sample_Directory/
drwxr-sr-x 2 carol users 4,0K Jan 18 17:17 Sample_Directory/
Los s en los permisos de grupo indican que el bit SGID está establecido. Ahora, cambiemos a este directorio y, nuevamente, creemos un archivo vacío con el comando touch
$ cd Sample_Directory/
$ touch emptyfile
$ ls -lh emptyfile
 -rw-r--r-- 1 carol users 0 Jan 18 17:20 emptyfile
Como podemos ver, el grupo que posee el archivo son los usuarios. Esto se debe a que el bit SGID hizo que el archivo heredara el propietario del grupo de su directorio principal, que son los usuarios.



Establecer UID
SUID, también conocido como Set User ID, tiene el valor octal 4 y está representado por una s en los permisos del usuario en modo simbólico. Solo se aplica a los archivos y su comportamiento es similar al bit SGID, pero el proceso se ejecutará con los privilegios del usuario propietario del archivo. Los archivos con el bit SUID muestran una s reemplazando la x en los permisos para el usuario en la salida de ls -l:

$ ls -ld test.sh
-rwsr-xr-x 1 carol carol 33 Dec 11 10:36 test.sh
Puede combinar múltiples permisos especiales en un parámetro agregándolos juntos. Entonces, para establecer SGID (valor 2) y SUID (valor 4) en modo numérico para el script test.sh con permisos 755, debe escribir:
$ chmod 6755 test.sh
Y el resultado sería:

$ ls -lh test.sh
-rwsr-sr-x 1 carol carol 66 Jan 18 17:29 test.sh
Nota: Si su terminal admite el color, y en la actualidad la mayoría de ellos lo hacen, puede ver rápidamente si estos permisos especiales se configuran al observar la salida de ls -l. Para la parte adhesiva, el nombre del directorio puede mostrarse en una fuente negra con fondo azul. Lo mismo se aplica a los archivos con los bits SGID (fondo amarillo) y SUID (fondo rojo). Los colores pueden ser diferentes según la distribución de Linux y la configuración de terminal que use.
