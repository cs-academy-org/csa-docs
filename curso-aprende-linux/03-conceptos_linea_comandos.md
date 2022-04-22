# Conceptos básicos de la línea de comando

## Introducción
Las distribuciones modernas de Linux tienen una amplia gama de interfaces gráficas de usuario, pero un administrador siempre necesitará saber cómo trabajar con la línea de comando o shell como se le llama. El shell es un programa que permite la comunicación basada en texto entre el sistema operativo y el usuario. Por lo general, es un programa en modo de texto que lee la entrada del usuario y la interpreta como comandos para el sistema.

Hay varios shells diferentes en Linux, estos son solo algunos:
- Bourne-again shell (Bash)
- C shell (csh or tcsh, the enhanced csh)
- Korn shell (ksh)
- Z shell (zsh)

En Linux, el mas común es el Bash Shell. Este también es el que se utilizará en ejemplos o ejercicios aquí.

Cuando se utiliza un shell interactivo, el usuario ingresa comandos en un llamado indicador. Para cada distribución de Linux, el indicador predeterminado puede verse un poco diferente, pero generalmente sigue esta estructura:

		username@hostname directorio_actual tipo_shell

En Ubuntu o Debian GNU / Linux, el prompt para un usuario normal probablemente se verá así:

		username@localhost:~$

El prompt del super usuario se verá así:

		root@localhost:~#

En CentOS o Red Hat Linux, el prompt para un usuario normal se verá así:

		[username@localhost ~$]

El prompt del super usuario se verá así:

		[root@localhost ~#]

Expliquemos cada componente de la estructura:

- username:
Nombre del usuario que ejecuta el shell

- localhost
Nombre del host en el que se ejecuta el shell

- directorio_actual
El directorio en el que se encuentra actualmente el shell. A ~ significa que el shell se encuentra en el directorio de inicio del usuario actual.

- tipo_shell
$ indica que el shell lo ejecuta un usuario normal.
# indica que el shell es ejecutado por el superusuario root.
Como no necesitamos ningún privilegio especial, utilizaremos una solicitud sin privilegios en los siguientes ejemplos. Por brevedad, solo usaremos $ como indicador.

## Estructura de línea de comando
La mayoría de los comandos en la línea de comando siguen la misma estructura básica:

		command  [opcione(s)/parametro(s)...]  [algumento(s)...]

Tome el siguiente comando como ejemplo:

		$ ls -l /home

Expliquemos el propósito de cada componente:

- Comando
Programa que ejecutará el usuario - ls en el ejemplo anterior.

- Opción (es) / Parámetro (s)
Un "interruptor" que modifica el comportamiento del comando de alguna manera, como -l en el ejemplo anterior. Se puede acceder a las opciones en forma corta y larga. Por ejemplo, -l es idéntico a --format = long.

También se pueden combinar varias opciones y, para la forma abreviada, las letras generalmente se pueden escribir juntas. Por ejemplo, los siguientes comandos hacen lo mismo:

		$ ls -al
		$ ls -a -l
		$ ls --all --long

- Argumento (s)
Datos adicionales que requiere el programa, como un nombre de archivo o ruta, como /home en el ejemplo anterior.

La única parte obligatoria de esta estructura es el comando en sí. En general, todos los demás elementos son opcionales, pero un programa puede requerir que se especifiquen ciertas opciones, parámetros o argumentos.

## Tipos de comportamiento del comando
El shell admite dos tipos de comandos:

- Interno:
Estos comandos son parte del propio shell y no son programas separados. Hay alrededor de 30 de estos comandos. Su objetivo principal es ejecutar tareas dentro del shell (por ejemplo, cd, set, export).

- Externo:
Estos comandos residen en archivos individuales. Estos archivos suelen ser programas binarios o scripts. Cuando se ejecuta un comando que no es un shell incorporado, el shell utiliza la variable PATH para buscar un archivo ejecutable con el mismo nombre que el comando. Además de los programas que se instalan con el administrador de paquetes de la distribución, los usuarios también pueden crear sus propios comandos externos.

El tipo de comando muestra de qué tipo es un comando específico:

	$ type echo
	echo is a shell builtin
	$ type man
	man is /usr/bin/man

Uso de Globbing para manipular archivos y directorios
El shell tiene muchas características que facilitan el trabajo con archivos para el usuario. Uno de ellos es engorroso. Globbing utiliza comodines, que son caracteres o grupos de caracteres que facilitan patrones o grupos de nombres de archivos. Los comodines más utilizados son:

- *

Cero, una o más ocurrencias de un caracter arbitrario



- ?

Una sola ocurrencia de un caracter arbitrario

- [a-z]

Una sola aparición de un caracter incluido en el rango especificado



- [!a-z]

Una sola aparición de un caracter no incluido en el rango especificado



- [sol | luna]

Al ocurrir una de las dos opciones



Veamos algunos ejemplos de archivos que coinciden con comodines:

- * 

Coincide con todos los archivos y directorios en el directorio actual.

Ejemplos: ssh-b, paranormal



- a*

Coincide con todos los archivos y directorios cuyo nombre comienza con a.

Ejemplos: alien, anna



- f * .txt

Coincide con todos los archivos y directorios cuyo nombre comienza con f y termina en .txt.

Ejemplos: file.txt, freddy.txt



- p??

Coincide con todos los archivos y directorios cuyo nombre comienza con p seguido de exactamente dos caracteres.

Ejemplos: pastel, pi4



- f [a-z] [0-9]

Coincide con cualquier archivo cuyo nombre comience con f, seguido de una letra minúscula, seguido de un número.

Ejemplos: fa0, fb8



- g [! a-z]

Coincide con cualquier archivo cuyo nombre comience con g, seguido de un carácter que no sea una letra minúscula.

Ejemplos: gF, g6



- a [lien | mmy] .txt

Coincide con cualquier archivo cuyo nombre comience con a, seguido de lien o mmy, seguido de .txt.

Ejemplos: alien.txt, ammy.txt



## Citando (Quoting)
Como usuario de Linux, deberá crear o manipular archivos o variables de varias maneras. Esto es fácil cuando se trabaja con nombres de archivo cortos y valores únicos, pero se vuelve más complicado cuando, por ejemplo, están involucrados espacios, caracteres especiales y variables. Los shells proporcionan una característica llamada comillas que encapsula dichos datos usando varios tipos de comillas ("", '', ``). En Bash, hay tres tipos de citas:

- Doble comillas
- Comillas simples
- Comillas posteriores


Por ejemplo, los siguientes comandos no actúan de la misma manera debido a las citas:

		$ touch $USER
		$ touch "$USER"
		$ touch '$USER'
		$ touch `$USER`

### Doble comillas
Las comillas dobles le dicen al shell que tome el texto entre comillas ("...") como caracteres regulares. Todos los caracteres especiales pierden su significado, excepto $ (signo de dólar), \ (barra invertida) y `(comilla invertida). Esto significa que las variables, la sustitución de comandos y las funciones aritméticas todavía se pueden usar.



Por ejemplo, la sustitución de la variable $USER no se ve afectada por las comillas dobles:

		$ echo I am $USER
		I am tom
		$ echo "I am $USER"
		I am tom

Un carácter espacial, por otro lado, pierde su significado como separador de argumentos:

		$ touch new file
		$ ls -l
		-rw-rw-r-- 1 tom students 0 Oct 8 15:18 file
		-rw-rw-r-- 1 tom students 0 Oct 8 15:18 new
		$ touch "new file"
		$ ls -l
		-rw-rw-r-- 1 tom students 0 Oct 8 15:19 new file

Como puede ver, en el primer ejemplo, el comando touch crea dos archivos individuales, el comando interpreta las dos cadenas como argumentos individuales. En el segundo ejemplo, el comando interpreta ambas cadenas como un argumento, por lo tanto, solo crea un archivo. Sin embargo, es una buena práctica evitar el carácter de espacio en los nombres de archivo. En cambio, se podría usar un guión bajo (_) o un punto (.).



### Comillas simples
Las comillas simples no tienen la excepción de las comillas dobles. Revocan cualquier significado especial de cada personaje. Tomemos uno de los primeros ejemplos de arriba:

		$ echo I am $USER
		I am tom

Al aplicar las comillas simples, verá un resultado diferente:

		$ echo 'I am $USER'
		I am $USER

El comando ahora muestra la cadena exacta sin sustituir la variable.

### Comillas posteriores
Las comillas con más poder son las comillas inversas. Estas citas incluyen un comando completo. Mientras procesa las comillas inversas, el shell ejecuta el comando y reemplaza las comillas con la salida de los comandos:

		$ hostname
		localhost
		$ touch hostname
		$ ls -l
		-rw-rw-r-- 1 tom students    0 Oct 8 16:08 hostname
		$ touch `hostname`
		$ ls -l*
		-rw-rw-r-- 1 tom students    0 Oct  8 15:45 localhost

Como puede ver, los dos comandos muestran dos resultados diferentes. En el primer ejemplo, el comando táctil crea un archivo con el nombre hostname, por lo tanto, trata la palabra literalmente. En el segundo ejemplo, las comillas inversas ejecutan el nombre de host antes del toque, reemplace las comillas inversas con la salida del nombre de host y luego ejecute el comando táctil resultante.

Otra expresión de Bash que tiene la misma funcionalidad que las comillas inversas es $ ():

		$ touch $(hostname)
		$ ls -l
		-rw-rw-r-- 1 tom students    0 Oct  8 15:45 localhost

Sin embargo, esta notación está más allá del alcance del examen de Linux Essentials.



Todos los shells administran un conjunto de información de estado a lo largo de las sesiones de shell. Esta información de tiempo de ejecución puede cambiar durante la sesión e influye en el comportamiento del shell. Estos datos también son utilizados por los programas para determinar aspectos de la configuración del sistema. La mayoría de estos datos se almacenan en las llamadas variables, que cubriremos en esta lección.

### Variables
Las variables son piezas de almacenamiento de datos, como texto o números. Una vez establecido, se accede al valor de una variable más adelante. Las variables tienen un nombre que permite acceder a una variable específica, incluso cuando cambia el contenido de la variable. Son una herramienta muy común en la mayoría de los lenguajes de programación.

En la mayoría de los shells de Linux, hay dos tipos de variables:

### Variables locales
Estas variables están disponibles solo para el proceso de shell actual. Si crea una variable local y luego inicia otro programa desde este shell, la variable ya no estará accesible para ese programa. Debido a que no son heredados por subprocesos, estas variables se llaman variables locales.
Variables de entorno

Estas variables están disponibles tanto en una sesión de shell específica como en subprocesos generados a partir de esa sesión de shell. Estas variables se pueden usar para pasar datos de configuración a comandos que se ejecutan. Debido a que estos programas pueden acceder a estas variables, se denominan variables de entorno. La mayoría de las variables de entorno están en mayúsculas (por ejemplo, RUTA, FECHA, USUARIO). Un conjunto de variables de entorno predeterminadas proporciona, por ejemplo, información sobre el directorio de inicio del usuario o el tipo de terminal. A veces, el conjunto completo de todas las variables de entorno se denomina entorno.
Estos tipos de variables también se conocen como alcance variable.

### Manipulando Variables
Como administrador del sistema, deberá crear, modificar o eliminar variables locales y de entorno.

Trabajando con variables locales
Puede configurar una variable local utilizando el operador = (igual). Una asignación simple creará una variable local:

		$ greeting=hello

Puede mostrar cualquier variable con el comando echo. El comando generalmente muestra el texto en la sección de argumentos:

		$ echo greeting
		greeting

Para acceder al valor de la variable, deberá usar $ (signo de dólar) delante del nombre de la variable.

		$ echo $greeting
		hello

Como se puede ver, la variable ha sido creada. Ahora abra otro shell e intente mostrar el contenido de la variable creada.

		$ echo $greeting

No se muestra nada. Esto ilustra que las variables siempre existen solo en un shell específico.

Para verificar que la variable es en realidad una variable local, intente generar un nuevo proceso y verifique si este proceso puede acceder a la variable. Podemos hacerlo iniciando otro shell y dejando que este ejecute el comando echo. Como el nuevo shell se ejecuta en un nuevo proceso, no heredará variables locales de su proceso padre:

		$ echo $greeting world
		hello world
		$ bash -c 'echo $greeting world'
		world

Para eliminar una variable, deberá usar el comando unset:

		$ echo $greeting
		hey
		$ unset greeting
		$ echo $greeting

Trabajando con variables globales
Para hacer que una variable esté disponible para los subprocesos, conviértala de una variable local en una variable de entorno. Esto se hace mediante el comando exportar. Cuando se invoca con el nombre de la variable, esta variable se agrega al entorno del shell:

		$ greeting=hello
		$ export greeting

Una forma más fácil de crear la variable de entorno es combinar ambos métodos anteriores, asignando el valor de la variable en la parte del argumento del comando.

		$ export greeting=hey

Verifiquemos nuevamente si la variable es accesible para los subprocesos:

		$ export greeting=hey
		$ echo $greeting world
		hey world
		$ bash -c 'echo $greeting world'
		hey world

Otra forma de usar variables de entorno es usarlas frente a los comandos. Podemos probar esto con la variable de entorno TZ que contiene la zona horaria. El comando date usa esta variable para determinar 
la hora de la zona horaria que se mostrará:

		$ TZ=EST date
		Thu 31 Jan 10:07:35 EST 2019
		$ TZ=GMT date
		Thu 31 Jan 15:07:35 GMT 2019

Puede mostrar todas las variables de entorno utilizando el comando env.

### La variable PATH
La variable PATH es una de las variables de entorno más importantes en un sistema Linux. Almacena una lista de directorios, separados por dos puntos, que contienen programas ejecutables elegibles como comandos del shell de Linux.

		$ echo $PATH
		/home/user/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

Para agregar un nuevo directorio a la variable, deberá usar el signo de dos puntos (smile.

		$ PATH=$PATH:new_directory

Aquí un ejemplo:

		$ echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
		$ PATH=$PATH:/home/user/bin
		$ echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/user/bin

Como puede ver, $PATH se usa en el nuevo valor asignado a PATH. Esta variable se resuelve durante la ejecución del comando y se asegura de que se conserva el contenido original de la variable. Por supuesto, también puede usar otras variables en la asignación:

		$ mybin=/opt/bin
		$ PATH=$PATH:$mybin
		$ echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/user/bin:/opt/bin

La variable PATH debe manejarse con precaución, ya que es crucial para trabajar en la línea de comando. Consideremos la siguiente variable PATH:

		$ echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

Para averiguar cómo el shell invoca un comando específico, which se puede ejecutar con el nombre del comando como argumento. Podemos, por ejemplo, tratar de averiguar dónde se almacena nano:

		$ which nano
		/usr/bin/nano

Como se puede ver, el ejecutable de nano se encuentra dentro del directorio /usr/bin. Eliminemos el directorio de la variable y verifiquemos si el comando aún funciona:

		$ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/sbin:/bin:/usr/games
		$ echo $PATH
		/usr/local/sbin:/usr/local/bin:/usr/sbin:/sbin:/bin:/usr/games

Busquemos nuevamente el comando nano:

		$ which nano

which: no nano in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/sbin:/bin:/usr/games)
Como se puede ver, el comando no se encuentra, por lo tanto, no se ejecuta. El mensaje de error también explica la razón por la cual no se encontró el comando y en qué ubicaciones se buscó.

Volvamos a agregar los directorios e intentemos ejecutar el comando nuevamente.

		$ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
		$ which nano
		/usr/bin/nano

Ahora nuestro comando funciona de nuevo.