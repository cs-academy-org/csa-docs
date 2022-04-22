# Bash Scripting

## Introducción

Hasta ahora hemos estado aprendiendo a ejecutar comandos desde el shell, pero también podemos ingresar comandos en un archivo y luego configurar ese archivo para que sea ejecutable. Cuando se ejecuta el archivo, esos comandos se ejecutan uno tras otro. Estos archivos ejecutables se denominan scripts y son una herramienta absolutamente crucial para cualquier administrador de sistemas Linux. Básicamente, podemos considerar que Bash es un lenguaje de programación y también un shell.



## Imprimiendo Salidas
Comencemos demostrando un comando que puede haber visto en lecciones anteriores: echo imprimirá un argumento en la salida estándar.

	$ echo "Hello World!"
	Hello World!

Ahora, utilizaremos la redirección de archivos para enviar este comando a un nuevo archivo llamado new_script.

	$ echo 'echo "Hello World!"' > new_script
	$ cat new_script
	echo "Hello World!"

El archivo new_script ahora contiene el mismo comando que antes.



## Hacer un script ejecutable

Demostremos algunos de los pasos necesarios para que este archivo se ejecute de la manera que esperamos. El primer pensamiento de un usuario podría ser simplemente escribir el nombre del script, la forma en que podría escribir el nombre de cualquier otro comando:

	$ new_script
	/bin/bash: new_script: command not found

Podemos suponer con seguridad que new_script existe en nuestra ubicación actual, pero tenga en cuenta que el mensaje de error no nos dice que el archivo no existe, nos dice que el comando no existe. Sería útil discutir cómo Linux maneja comandos y ejecutables.

## Comandos y PATH
Cuando escribimos el comando ls en el shell, por ejemplo, estamos ejecutando un archivo llamado ls que existe en nuestro sistema de archivos. Puede probar esto usando:

	$ which ls
	/bin/ls

Rápidamente se volvería aburrido escribir la ruta absoluta de ls cada vez que deseamos ver el contenido de un directorio, por lo que Bash tiene una variable de entorno que contiene todos los directorios donde podemos encontrar los comandos que deseamos ejecutar. Puede ver el contenido de esta variable utilizando echo.

	$ echo $PATH
	/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

Cada una de estas ubicaciones es donde el shell espera encontrar un comando, delimitado con dos puntos (smile. Notará que /bin está presente, pero es seguro asumir que nuestra ubicación actual no lo está. El shell buscará new_script en cada uno de estos directorios, pero no lo encontrará y, por lo tanto, arrojará el error que vimos anteriormente.

Hay tres soluciones a este problema: podemos mover new_script a uno de los directorios PATH, podemos agregar nuestro directorio actual a PATH o podemos cambiar la forma en que intentamos llamar al script. La última solución es la más fácil, simplemente requiere que especifiquemos la ubicación actual cuando llamamos al script usando una barra diagonal (./).

	$ ./new_script
	/bin/bash: ./new_script: Permission denied

El mensaje de error ha cambiado, lo que indica que hemos progresado un poco.



## Permisos de ejecución
La primera investigación que un usuario debe hacer en este caso es usar ls -l para mirar el archivo:

	$ ls -l new_script
	-rw-rw-r-- 1 user user 20 Apr 30 12:12 new_script

Podemos ver que los permisos para este archivo están establecidos en 664 de forma predeterminada. Todavía no hemos configurado este archivo para que tenga permisos de ejecución.

	$ chmod +x new_script
	$ ls -l new_script
	-rwxrwxr-x 1 user user 20 Apr 30 12:12 new_script

Este comando ha otorgado permisos de ejecución a todos los usuarios. Tenga en cuenta que esto podría ser un riesgo de seguridad, pero por ahora este es un nivel de permiso aceptable.

	$ ./new_script
	Hello World!

Ahora podemos ejecutar nuestro script.



## Definiendo el intérprete
Como hemos demostrado, pudimos simplemente ingresar texto en un archivo, configurarlo como un ejecutable y ejecutarlo. new_script sigue siendo funcionalmente un archivo de texto normal, pero logramos que Bash lo interprete. Pero, ¿y si está escrito en Perl o Python?

Es una buena práctica especificar el tipo de intérprete que queremos usar en la primera línea de un script. Esta línea se llama línea de explosión o, más comúnmente, shebang. Indica al sistema cómo queremos que se ejecute este archivo. Como estamos aprendiendo Bash, usaremos la ruta absoluta a nuestro ejecutable de Bash, una vez más, utilizando:

	$ which bash
	/bin/bash

Nuestro shebang comienza con un signo hash y un signo de exclamación, seguido del camino absoluto de arriba. Abramos new_script en un editor de texto e insertemos el shebang. Aprovechemos también para insertar un comentario en nuestro script. Los comentarios son ignorados por el intérprete. Están escritos para el beneficio de otros usuarios que desean entender su script.

	#!/bin/bash
	
	# This is our first comment. It is also good practice to document all scripts.
	
	echo "Hello World!"


También haremos un cambio adicional en el nombre del archivo: guardaremos este archivo como new_script.sh. El sufijo de archivo ".sh" no cambia la ejecución del archivo de ninguna manera. Es una convención que los scripts de bash se etiqueten con .sh o .bash para identificarlos más fácilmente, de la misma manera que los scripts de Python se identifican generalmente con el sufijo .py.



## Editores de texto comunes

Los usuarios de Linux a menudo tienen que trabajar en un entorno donde los editores de texto gráficos no están disponibles. Por lo tanto, se recomienda desarrollar al menos cierta familiaridad con la edición de archivos de texto desde la línea de comandos. Dos de los editores de texto más comunes son vi y nano.



- vi

vi es un editor de texto venerable y se instala por defecto en casi todos los sistemas Linux existentes. vi generó un clon llamado vi IMproved o vim que agrega alguna funcionalidad pero mantiene la interfaz de vi. Si bien trabajar con vi es desalentador para un nuevo usuario, el editor es popular y muy querido por los usuarios que aprenden sus muchas características.

La diferencia más importante entre vi y aplicaciones como Notepad es que vi tiene tres modos diferentes. En el inicio, las teclas H, J, K y L se utilizan para navegar, no para escribir. En este modo de navegación, puede presionar I para ingresar al modo de inserción. En este punto, puede escribir normalmente. Para salir del modo de inserción, presiona Esc para volver al modo de navegación. Desde el modo de navegación, puede presionar : para ingresar al modo de comando. Desde este modo, puede guardar, eliminar, salir o cambiar las opciones.

Si bien vi tiene una curva de aprendizaje, los diferentes modos pueden permitir que un usuario inteligente se vuelva más eficiente que con otros editores.

- nano

nano es una herramienta más nueva, creada para ser simple y más fácil de usar que vi. nano no tiene modos diferentes. En cambio, un usuario en el inicio puede comenzar a escribir y usa Ctrl para acceder a las herramientas impresas en la parte inferior de la pantalla.

Los editores de texto son una cuestión de preferencia personal, y el editor que elija utilizar no tendrá relación con esta lección. Pero familiarizarse y sentirse cómodo con uno o más editores de texto dará sus frutos en el futuro.



## Variables
Las variables son una parte importante de cualquier lenguaje de programación, y Bash no es diferente. Cuando inicia una nueva sesión desde la terminal, el shell ya establece algunas variables para usted. La variable PATH es un ejemplo de esto. Llamamos a estas variables de entorno, porque generalmente definen las características de nuestro entorno de shell. Puede modificar y agregar variables de entorno, pero por ahora centrémonos en establecer variables dentro de nuestro script.



Modificaremos nuestro script para que se vea así:

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username=Carol
	
	echo "Hello $username!"


En este caso, hemos creado una variable llamada nombre de usuario y le hemos asignado el valor de Carol. Tenga en cuenta que no hay espacios entre el nombre de la variable, el signo igual o el valor asignado.

En la siguiente línea, hemos usado el comando echo con la variable, pero hay un signo de dólar ($) delante del nombre de la variable. Esto es importante, ya que le indica al shell que deseamos tratar el nombre de usuario como una variable, y no solo como una palabra normal. Al ingresar $username en nuestro comando, indicamos que queremos realizar una sustitución, reemplazando el nombre de una variable con el valor asignado a esa variable.

Al ejecutar el nuevo script, obtenemos este resultado:

	$ ./new_script.sh
	Hello Carol!

Las variables deben contener solo caracteres alfanuméricos o guiones bajos, y distinguen entre mayúsculas y minúsculas. Nombre de usuario y nombre de usuario serán tratados como variables separadas.
La sustitución de variables también puede tener el formato $ {username}, con la adición de {}. Esto también es aceptable.
Las variables en Bash tienen un tipo implícito y se consideran cadenas. Esto significa que realizar funciones matemáticas en Bash es más complicado de lo que sería en otros lenguajes de programación como C/C ++:

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username=Carol
	x=2
	y=4
	z=$x+$y
	echo "Hello $username!"
	echo "$x + $y"
	echo "$z"
	
	$ ./new_script.sh
	Hello Carol!
	2 + 4
	2+4

## Usar comillas con variables
Hagamos el siguiente cambio en el valor de nuestra variable username:

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username=Carol Smith
	
	echo "Hello $username!"


Ejecutar este script nos dará un error:

	$ ./new_script.sh
	./new_script.sh: line 5: Smith: command not found
	Hello !

Tenga en cuenta que Bash es un intérprete y, como tal, interpreta nuestro script línea por línea. En este caso, interpreta correctamente username = Carol para establecer una variable username con el valor Carol. Pero luego interpreta el espacio como indicando el final de esa asignación, y Smith como el nombre de un comando. Para que el espacio y el nombre Smith se incluyan como el nuevo valor de nuestra variable, colocaremos comillas dobles (") alrededor del nombre.

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username="Carol Smith"
	
	echo "Hello $username!"
	
	$ ./new_script.sh
	Hello Carol Smith!

Una cosa importante a tener en cuenta en Bash es que las comillas dobles y las comillas simples (') se comportan de manera muy diferente. Las comillas dobles se consideran "débiles" porque permiten que el intérprete realice la sustitución dentro de las comillas. Las comillas simples se consideran "fuertes", porque evitan que ocurra cualquier sustitución. Considere el siguiente ejemplo:


	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username="Carol Smith"
	
	echo "Hello $username!"
	echo 'Hello $username!'
	
	$ ./new_script.sh
	Hello Carol Smith!
	Hello $username!

En el segundo comando echo, se ha impedido que el intérprete sustituya $username con Carol Smith, por lo que la salida se toma literalmente.



## Argumentos
Ya está familiarizado con el uso de argumentos en las utilidades principales de Linux. Por ejemplo, rm testfile contiene el rm ejecutable y un argumento testfile. Los argumentos se pueden pasar al script después de la ejecución y modificarán el comportamiento del script. Se implementan fácilmente.

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username=$1
	
	echo "Hello $username!"

En lugar de asignar un valor al nombre de usuario directamente dentro del script, le estamos asignando el valor de una nueva variable $ 1. Esto se refiere al valor del primer argumento.

	$ ./new_script.sh Carol
	Hello Carol!

Los primeros nueve argumentos se manejan de esta manera. Hay formas de manejar más de nueve argumentos, pero eso está fuera del alcance de esta lección. Vamos a demostrar un ejemplo usando solo dos argumentos:

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username1=$1
	username2=$2
	echo "Hello $username1 and $username2!"
	
	$ ./new_script.sh Carol Dave
	Hello Carol and Dave!

Hay una consideración importante cuando se usan argumentos: en el ejemplo anterior, hay dos argumentos Carol y Dave, asignados a $1 y $2 respectivamente. Si falta el segundo argumento, por ejemplo, el shell no arrojará un error. El valor de $2 simplemente será nulo, o nada en absoluto.

	$ ./new_script.sh Carol
	Hello Carol and !


En nuestro caso, sería una buena idea introducir algo de lógica en nuestro script para que diferentes condiciones afecten el resultado que deseamos imprimir. Comenzaremos presentando otra variable útil y luego pasaremos a crear sentencias if.



## Devolviendo el número de argumentos
Mientras que variables como $ 1 y $ 2 contienen el valor de los argumentos posicionales, otra variable $# contiene el número de argumentos.

	#!/bin/bash
	
	# This is our first comment. It is also good practice to comment all scripts.
	
	username=$1
	
	echo "Hello $username!"
	echo "Number of arguments: $#."
	
	$ ./new_script.sh Carol Dave
	Hello Carol!
	Number of arguments: 2.


## Lógica condicional

El uso de la lógica condicional en la programación es un tema muy amplio y no se tratará en profundidad en esta lección. Nos centraremos en la sintaxis de condicionales en Bash, que difiere de la mayoría de los otros lenguajes de programación.

Comencemos por revisar lo que esperamos lograr. Tenemos un script simple que debería poder imprimir un saludo a un solo usuario. Si hay algo más que un usuario, debemos imprimir un mensaje de error.

La condición que estamos probando es la cantidad de usuarios, que está contenida en la variable $#. Nos gustaría saber si el valor de $# es 1.
Si la condición es verdadera, la acción que tomaremos es saludar al usuario.
Si la condición es falsa, imprimiremos un mensaje de error.


Ahora que la lógica es clara, nos centraremos en la sintaxis requerida para implementar esta lógica.


	#!/bin/bash
	
	# A simple script to greet a single user.
	
	if [ $# -eq 1 ]
	then
	  username=$1
	
	  echo "Hello $username!"
	else
	  echo "Please enter only one argument."
	fi
	echo "Number of arguments: $#."

La lógica condicional está contenida entre if y fi. La condición a probar se encuentra entre corchetes [], y la acción a tomar si la condición es verdadera se indica después de eso. Tenga en cuenta los espacios entre los corchetes y la lógica contenida. Omitir este espacio causará errores.

Este script generará nuestro saludo o el mensaje de error. Pero siempre imprimirá la línea Número de argumentos.

	$ ./new_script.sh
	Please enter only one argument.
	Number of arguments: 0.
	$ ./new_script.sh Carol
	Hello Carol!
	Number of arguments: 1.

Tome nota de la declaración if. Hemos usado -eq para hacer una comparación numérica. En este caso, estamos probando que el valor de $# es igual a uno. Las otras comparaciones que podemos realizar son:

- -ne

No igual a


- -gt

Mas grande que


- -ge

Mayor qué o igual a

- -lt

Menos que

- -le

Menos que o igual a



En la última sección, utilizamos este ejemplo simple para demostrar las secuencias de comandos Bash:

	#!/bin/bash
	
	# A simple script to greet a single user.
	
	if [ $# -eq 1 ]
	then
	  username=$1
	
	  echo "Hello $username!"
	else
	  echo "Please enter only one argument."
	fi
	echo "Number of arguments: $#."


Todos los scripts deben comenzar con un shebang, que define la ruta al intérprete.
Todos los scripts deben incluir comentarios para describir su uso.
Este script en particular funciona con un argumento, que se pasa al script cuando se llama.
Este script contiene una instrucción if, que prueba las condiciones de una variable incorporada $#. Esta variable se establece en el número de argumentos.
Si el número de variables es igual a 1, el valor del primer argumento se pasa a una nueva variable llamada nombre de usuario y el script hace eco de un saludo al usuario. De lo contrario, se muestra un mensaje de error.
Finalmente, el script hace eco de la cantidad de argumentos. Esto es útil para la depuración.

Este es un ejemplo útil para comenzar a explicar algunas de las otras características de las secuencias de comandos Bash.

## Códigos de salida
Notará que nuestro script tiene dos estados posibles: o imprime "¡Hola <user>!" o imprime un mensaje de error. Esto es bastante normal para muchas de nuestras utilidades principales. Considere el comando cat, con el que sin duda se está familiarizando.

Comparemos un uso exitoso del comando cat con una situación en la que falla. Un recordatorio de que nuestro ejemplo anterior es un script llamado new_script.sh.

	$ cat -n new_script.sh
	
	     1	#!/bin/bash
	     2
	     3	# A simple script to greet a single user.
	     4
	     5	if [ $# -eq 1 ]
	     6	then
	     7	  username=$1
	     8
	     9	  echo "Hello $username!"
	    10	else
	    11	  echo "Please enter only one argument."
	    12	fi
	    13	echo "Number of arguments: $#."

Este comando tiene éxito y notará que la bandera -n también ha impreso números de línea. Estos son muy útiles al depurar scripts, pero tenga en cuenta que no son parte del script.

Ahora vamos a verificar el valor de una nueva variable incorporada $?. Por ahora, solo observe la salida:

	$ echo $?
	0

Ahora consideremos una situación en la que el comando cat fallará. Primero veremos un mensaje de error, y luego verificaremos el valor de $?.

	$ cat -n dummyfile.sh
	cat: dummyfile.sh: No such file or directory
	$ echo $?
	1

La explicación de este comportamiento es esta: cualquier ejecución de la utilidad cat devolverá un código de salida. Un código de salida nos dirá si el comando tuvo éxito o si experimentó un error. Un código de salida de cero indica que el comando se completó correctamente. Esto es cierto para casi todos los comandos de Linux con los que trabaja. Cualquier otro código de salida indicará un error de algún tipo. El código de salida del último comando a ejecutar se almacenará en la variable $?.

Los códigos de salida generalmente no son vistos por usuarios humanos, pero son muy útiles al escribir scripts. Considere un script donde podemos estar copiando archivos a una unidad de red remota. Es posible que la tarea de copia haya fallado de muchas maneras: por ejemplo, nuestra máquina local podría no estar conectada a la red o la unidad remota podría estar llena. Al verificar el código de salida de nuestra utilidad de copia, podemos alertar al usuario de problemas al ejecutar el script.

Es muy buena práctica implementar códigos de salida, por lo que haremos esto ahora. Tenemos dos caminos en nuestro script, un éxito y un fracaso. Usemos cero para indicar éxito, y uno para indicar fracaso.


	     1	#!/bin/bash
	     2
	     3	# A simple script to greet a single user.
	     4
	     5	if [ $# -eq 1 ]
	     6	then
	     7	  username=$1
	     8
	     9	  echo "Hello $username!"
	    10	  exit 0
	    11	else
	    12	  echo "Please enter only one argument."
	    13	  exit 1
	    14	fi
	    15	echo "Number of arguments: $#."
	
	$ ./new_script.sh Carol
	Hello Carol!
	$ echo $?
	0

Observe que el comando echo en la línea 15 se ignoró por completo. El uso de exit finalizará el script de inmediato, por lo que esta línea nunca se encuentra.



## Manejo de muchos argumentos

Hasta ahora, nuestro script solo puede manejar un solo nombre de usuario a la vez. Cualquier número de argumentos además de uno causará un error. Exploremos cómo podemos hacer que este script sea más versátil.

El primer instinto de un usuario podría ser utilizar más variables posicionales como $2, $3, etc. Desafortunadamente, no podemos anticipar la cantidad de argumentos que un usuario podría elegir usar. Para resolver este problema, será útil introducir más variables integradas.

Modificaremos la lógica de nuestro script. Tener cero argumentos debería causar un error, pero cualquier otro número de argumentos debería ser exitoso. Este nuevo script se llamará friendly2.sh.


	     1	#!/bin/bash
	     2
	     3	# a friendly script to greet users
	     4
	     5	if [ $# -eq 0 ]
	     6	then
	     7	  echo "Please enter at least one user to greet."
	     8	  exit 1
	     9	else
	    10	  echo "Hello $@!"
	    11	  exit 0
	    12	fi
	$ ./friendly2.sh Carol Dave Henry
	Hello Carol Dave Henry!

Hay dos variables integradas que contienen todos los argumentos pasados al script: $@ y $*. En su mayor parte, ambos se comportan igual. Bash analizará los argumentos y separará cada argumento cuando encuentre un espacio entre ellos. 



Si está familiarizado con otros lenguajes de programación, puede reconocer este tipo de variable como una matriz. Las matrices en Bash se pueden crear simplemente poniendo espacio entre elementos como la variable FILES en la secuencia de comandos de prueba a continuación:

	FILES="/usr/sbin/accept /usr/sbin/pwck/ usr/sbin/chroot"

Contiene una lista de muchos artículos. Hasta ahora, esto no es muy útil, porque todavía no hemos introducido ninguna forma de manejar estos elementos individualmente.



## Bucle For

Consulte el ejemplo de arraytest.sh que se muestra anteriormente. Si recuerdas, en este ejemplo estamos especificando una matriz propia llamada FILES. Lo que necesitamos es una forma de "desempaquetar" esta variable y acceder a cada valor individual, uno tras otro. Para hacer esto, utilizaremos una estructura llamada bucle for, que está presente en todos los lenguajes de programación. Hay dos variables a las que nos referiremos: una es el rango y la otra es para el valor individual en el que estamos trabajando actualmente. Este es el guión en su totalidad:


	#!/bin/bash
	
	FILES="/usr/sbin/accept /usr/sbin/pwck/ usr/sbin/chroot"
	
	for file in $FILES
	do
	  ls -lh $file
	done
	
	$ ./arraytest.sh
	lrwxrwxrwx 1 root root 10 Apr 24 11:02 /usr/sbin/accept -> cupsaccept
	-rwxr-xr-x 1 root root 54K Mar 22 14:32 /usr/sbin/pwck
	-rwxr-xr-x 1 root root 43K Jan 14 07:17 /usr/sbin/chroot


Si vuelve a consultar el ejemplo anterior de friendly2.sh, puede ver que estamos trabajando con un rango de valores contenidos dentro de una sola variable $@. En aras de la claridad, llamaremos a la última variable username. Nuestro script ahora se ve así:


	     1	#!/bin/bash
	     2
	     3	# a friendly script to greet users
	     4
	     5	if [ $# -eq 0 ]
	     6	then
	     7	  echo "Please enter at least one user to greet."
	     8	  exit 1
	     9	else
	    10	  for username in $@
	    11	  do
	    12	    echo "Hello $username!"
	    13	  done
	    14	  exit 0
	    15	fi

Recuerde que la variable que defina aquí se puede nombrar como desee, y que todas las líneas dentro do ... done se ejecutarán una vez para cada elemento de la matriz. Observemos el resultado de nuestro script:

	$ ./friendly2.sh Carol Dave Henry
	Hello Carol!
	Hello Dave!
	Hello Henry!

Ahora supongamos que queremos que nuestra salida parezca un poco más humana. Queremos que nuestro saludo esté en una línea.

	     1	#!/bin/bash
	     2
	     3	# a friendly script to greet users
	     4
	     5	if [ $# -eq 0 ]
	     6	then
	     7	  echo "Please enter at least one user to greet."
	     8	  exit 1
	     9	else
	    10	  echo -n "Hello $1"
	    11	  shift
	    12	  for username in $@
	    13	  do
	    14	    echo -n ", and $username"
	    15	  done
	    16	  echo "!"
	    17	  exit 0
	    18	fi

Un par de notas:

El uso de -n con eco suprimirá la nueva línea después de la impresión. Esto significa que todos los echos se imprimirán en la misma línea, y la nueva línea se imprimirá solo después de ! en la línea 16.
El comando shift eliminará el primer elemento de nuestra matriz.

Observemos el resultado:

	$ ./friendly2.sh Carol
	Hello Carol!
	$ ./friendly2.sh Carol Dave Henry
	Hello Carol, and Dave, and Henry!

Uso de expresiones regulares para realizar la comprobación de errores
Es posible que queramos verificar todos los argumentos que el usuario está ingresando. Por ejemplo, quizás queremos asegurarnos de que todos los nombres pasados a friendly2.sh contengan solo letras y que cualquier carácter o número especial cause un error. Para realizar esta comprobación de errores, usaremos grep.

Recuerde que podemos usar expresiones regulares con grep.

	$ echo Animal | grep "^[A-Za-z]*$"
	Animal
	$ echo $?
	0
	
	$ echo 4n1ml | grep "^[A-Za-z]*$"
	$ echo $?
	1

^ Y $ indican el principio y el final de la línea, respectivamente. [A-Za-z] indica un rango de letras, mayúsculas o minúsculas. El * es un cuantificador y modifica nuestro rango de letras para que coincidamos cero con muchas letras. En resumen, nuestro grep tendrá éxito si la entrada es solo letras y falla de lo contrario.

Lo siguiente a tener en cuenta es que grep está devolviendo códigos de salida en función de si hubo una coincidencia o no. Una coincidencia positiva devuelve 0, y una no coincidencia devuelve un 1. Podemos usar esto para probar nuestros argumentos dentro de nuestro script.


	     1	#!/bin/bash
	     2
	     3	# a friendly script to greet users
	     4
	     5	if [ $# -eq 0 ]
	     6	then
	     7	  echo "Please enter at least one user to greet."
	     8	  exit 1
	     9	else
	    10	  for username in $@
	    11	  do
	    12	    echo $username | grep "^[A-Za-z]*$" > /dev/null
	    13	    if [ $? -eq 1 ]
	    14	    then
	    15	      echo "ERROR: Names must only contains letters."
	    16	      exit 2
	    17	    else
	    18	      echo "Hello $username!"
	    19	    fi
	    20	  done
	    21	  exit 0
	    22	fi


En la línea 12, estamos redirigiendo la salida estándar a /dev/null, que es una forma sencilla de suprimirla. No queremos ver ningún resultado del comando grep, solo queremos probar su código de salida, que ocurre en la línea 13. Observe también que estamos usando un código de salida de 2 para indicar un argumento no válido. Generalmente es una buena práctica usar diferentes códigos de salida para indicar diferentes errores; de esta manera, un usuario inteligente puede usar estos códigos de salida para solucionar problemas.


	$ ./friendly2.sh Carol Dave Henry
	Hello Carol!
	Hello Dave!
	Hello Henry!
	$ ./friendly2.sh 42 Carol Dave Henry
	ERROR: Names must only contains letters.
	$ echo $?
	2