# Buscar y extraer datos de archivos

## Introducción
En este laboratorio nos centraremos en redirigir o transmitir información de una fuente a otra con la ayuda de herramientas específicas. La línea de comandos de Linux redirige la información a través de canales estándar específicos. La entrada estándar (stdin o canal 0) del comando se considera el teclado y la salida estándar (stdout o canal 1) se considera la pantalla. También hay otro canal destinado a redirigir la salida de error (stderr o canal 2) de un comando o los mensajes de error de un programa. La entrada y/o salida se puede redirigir.

Al ejecutar un comando, a veces queremos transmitir cierta información al comando o redirigir la salida a un archivo específico. Cada una de estas funcionalidades se discutirá en las siguientes dos secciones.



## Redirección I/O
La redirección de I/O (Entrada/Salida) permite al usuario redirigir información desde o hacia un comando mediante un archivo de texto. Como se describió anteriormente, la entrada, salida y salida de error estándar se pueden redirigir y la información se puede tomar de los archivos de texto.



### Redirección de salida estándar

Para redirigir la salida estándar a un archivo, en lugar de la pantalla, necesitamos usar el operador > seguido del nombre del archivo. Si el archivo no existe, se creará uno nuevo; de lo contrario, la información sobrescribirá el archivo existente.

Para ver el contenido del archivo que acabamos de crear, podemos usar el comando cat. Por defecto, este comando muestra el contenido de un archivo en la pantalla. Consulte la página del manual para obtener más información sobre sus funcionalidades.

El siguiente ejemplo demuestra la funcionalidad del operador. En primera instancia, se crea un nuevo archivo que contiene el texto "¡Hola Mundo!":

	$ echo "Hello World!" > text
	$ cat text
	Hello World!

En la segunda invocación, el mismo archivo se sobrescribe con el nuevo texto:

	$ echo "Hello!" > text
	$ cat text
	Hello!

Si queremos agregar nueva información al final del archivo, necesitamos usar el operador >>. Este operador también crea un nuevo archivo si no puede encontrar uno existente.

El primer ejemplo muestra la adición del texto. Como se puede ver, el nuevo texto se agregó en la siguiente línea:

	$ echo "Hello to you too!" >> text
	$ cat text
	Hello!
	Hello to you too!

El segundo ejemplo demuestra que se creará un nuevo archivo:

	$ echo "Hello to you too!" >> text2
	$ cat text2
	Hello to you too!
	Error estándar de redireccionamiento

Para redirigir solo los mensajes de error, un administrador del sistema deberá usar el operador 2> seguido del nombre del archivo en el que se escribirán los errores. Si el archivo no existe, se creará uno nuevo; de lo contrario, el archivo se sobrescribirá.

Como se explicó, el canal para redirigir el error estándar es el canal 2. Al redirigir el error estándar, se debe especificar el canal, a diferencia de la otra salida estándar en la que el canal 1 está configurado de manera predeterminada. Por ejemplo, el siguiente comando busca un archivo o directorio llamado juegos y solo escribe el error en el archivo text-error, mientras muestra la salida estándar en la pantalla:

	$ find /usr games 2> text-error
	/usr
	/usr/share
	/usr/share/misc
	---------Omitted output----------
	/usr/lib/libmagic.so.1.0.0
	/usr/lib/libdns.so.81
	/usr/games
	$ cat text-error
	find: `games': No such file or directory

Por ejemplo, el siguiente comando se ejecutará sin errores, por lo tanto, no se escribirá información en el archivo error de texto:

	$ sort /etc/passwd 2> text-error
	$ cat text-error

Además de la salida estándar, el error estándar también se puede agregar a un archivo con el operador 2 >>. Esto agregará el nuevo error al final del archivo. Si el archivo no existe, se creará uno nuevo. El primer ejemplo muestra la adición de la nueva información en el archivo, mientras que el segundo ejemplo muestra que el comando crea un nuevo archivo donde no se puede encontrar uno existente con el mismo nombre:

	$ sort /etc 2>> text-error
	$ cat text-error
	sort: read failed: /etc: Is a directory
	
	$ sort /etc/shadow 2>> text-error2
	$ cat text-error2
	sort: open failed: /etc/shadow: Permission denied

Usando este tipo de redirección, solo los mensajes de error serán redirigidos al archivo, la salida normal se escribirá en la pantalla o pasará por la salida estándar o stdout.

Hay un archivo en particular que técnicamente es un depósito de bits (un archivo que acepta entrada y no hace nada con él): /dev/null. Puede redirigir cualquier información irrelevante que no desee que se muestre o redirija a un archivo importante, como se muestra en el siguiente ejemplo:

$ sort /etc 2> /dev/null
Redireccionamiento de entrada estándar
Este tipo de redirección se utiliza para ingresar datos a un comando, desde un archivo especificado en lugar de un teclado. En este caso, el operador <se usa como se muestra en el ejemplo:

	$ cat < text
	Hello!
	Hello to you too!

La entrada estándar de redireccionamiento generalmente se usa con comandos que no aceptan argumentos de archivo. El comando tr es uno de ellos. Este comando se puede usar para traducir el contenido del archivo modificando las palabras de maneras específicas, como eliminar cualquier carácter particular de un archivo, el siguiente ejemplo muestra la eliminación del carácter l:
$ tr -d "l" < text
Heo!
Heo to you too!
Para obtener más información, consulte la página de manual de tr.

### Aquí documentos

A diferencia de las redirecciones de salida, el operador << actúa de manera diferente en comparación con los otros operadores. Este flujo de entrada también se llama aquí documento. Aquí el documento representa el bloque de código o texto que se puede redirigir al comando o al programa interactivo. Los diferentes tipos de lenguajes de secuencias de comandos, como bash, sh y csh pueden recibir información directamente desde la línea de comandos, sin utilizar ningún archivo de texto.

Como se puede ver en el siguiente ejemplo, el operador se usa para ingresar datos en el comando, mientras que la palabra después no especifica el nombre del archivo. La palabra se interpreta como el delimitador de la entrada y no se tomará en consideración como contenido, por lo tanto, cat no la mostrará:

	$ cat << hello
	> hey
	> ola
	> hello
	hey
	ola

Consulte la página del comando man cat para encontrar más información.



### Combinaciones
La primera combinación que exploraremos combina la redirección de la salida estándar y la salida de error estándar al mismo archivo. Se utilizan los operadores &> y & >>, y representan la combinación del canal 1 y el canal 2. El primer operador sobrescribirá el contenido existente del archivo y el segundo agregará o agregará la nueva información al final del archivo . Ambos operadores permitirán la creación del nuevo archivo si no existe, al igual que en las secciones anteriores:

	$ find /usr admin &> newfile
	$ cat newfile
	/usr
	/usr/share
	/usr/share/misc
	---------Omitted output----------
	/usr/lib/libmagic.so.1.0.0
	/usr/lib/libdns.so.81
	/usr/games
	find: `admin': No such file or directory
	$ find /etc/calendar &>> newfile
	$ cat newfile
	/usr
	/usr/share
	/usr/share/misc
	---------Omitted output----------
	/usr/lib/libmagic.so.1.0.0
	/usr/lib/libdns.so.81
	/usr/games
	find: `admin': No such file or directory
	/etc/calendar
	/etc/calendar/default
	find: `admin': No such file or directory

Otro operador de uso frecuente que combina la redirección de entrada y salida es <>. Esto usa el archivo especificado como entrada y salida del comando, lo que significa que modificará el texto y sobrescribirá el archivo con la nueva salida:

	$ cut -f 3 -d "/" <> newfile
	$ cat newfile
	
	share
	share
	share
	---------Omitted output----------
	lib
	games
	find: `admin': No such file or directory
	calendar
	calendar
	find: `admin': No such file or directory

Tomemos el ejemplo anterior. El comando cut corta los campos especificados del archivo de entrada usando la opción -f, el tercer campo en nuestro caso. Para que el comando encuentre el campo, también se debe especificar un delimitador con la opción -d. En nuestro caso, el delimitador será el carácter /.

Para obtener más información sobre el comando de cut, consulte su página de manual.


## Tuberías de línea de comando
La redirección se usa principalmente para almacenar el resultado de un comando, para ser procesado por un comando diferente. Este tipo de proceso intermedio puede volverse muy tedioso y complicado si desea que los datos pasen por múltiples procesos. Para evitar esto, puede vincular el comando directamente a través de tuberías. En otras palabras, la salida del primer comando se convierte automáticamente en la entrada del segundo comando. Esta conexión se realiza mediante el | Operador (barra vertical):

	$ cat /etc/passwd | less
	root:x:0:0:root:/root:/bin/bash
	daemon:x:1:1:daemon:/usr/sbin:/bin/sh
	bin:x:2:2:bin:/bin:/bin/sh
	:

En el ejemplo anterior, el comando less después del operador de tubería modifica la forma en que se muestra el archivo. El comando less muestra el archivo de texto que permite al usuario desplazarse hacia arriba y hacia abajo en una línea en ese momento. less también se usa por defecto para mostrar las páginas man, como se discutió en las lecciones anteriores.
Es posible usar múltiples tuberías al mismo tiempo. Los comandos intermedios que reciben entrada y luego la cambian y producen salida se denominan filtros. Tomemos el comando ls -l e intentemos contar el número de palabras de las primeras 10 líneas de la salida. Para hacer esto, tendremos que usar el comando head que por defecto muestra las primeras 10 líneas de un archivo y luego contar las palabras usando el comando wc:

	$ ls -l | head | wc -l
	10

Como se mencionó anteriormente, por defecto, head solo muestra las primeras 10 líneas del archivo de texto especificado. Este comportamiento puede modificarse mediante el uso de opciones específicas. Consulte la página del comando man para encontrar más.

Hay otro comando que muestra el final de un archivo: tail. Por defecto, este comando selecciona las últimas 10 líneas y las muestra, pero como encabezado, el número también se puede modificar. Consulte la página de manual de tail para obtener más detalles.

El comando wc (word count) cuenta por defecto las líneas, palabras y bytes de un archivo. Como se muestra en el ejercicio, la opción -w hace que el comando solo cuente las palabras dentro de las líneas seleccionadas. Las opciones más comunes que puede usar con este comando son: -l, que especifica que el comando solo cuenta las líneas, y -b, que se usa para contar solo los bytes. Se pueden encontrar más variaciones y opciones del comando, así como más información sobre wc en la página del comando man.

En esta sección, vamos a ver las herramientas que se utilizan para manipular texto. Estas herramientas son utilizadas frecuentemente por los administradores o programas del sistema para monitorear o identificar automáticamente información recurrente específica.



Buscar dentro de archivos con grep
La primera herramienta que discutiremos en esta lección es el comando grep. grep es la abreviatura de la frase "global regular expression impression (impresión de expresión regular global)" y su funcionalidad principal es buscar dentro de los archivos el patrón especificado. El comando genera la línea que contiene el patrón especificado resaltado en rojo.

	$ grep bash /etc/passwd
	root:x:0:0:root:/root:/bin/bash
	user:x:1001:1001:User,,,,:/home/user:/bin/bash

grep, como la mayoría de los comandos, también se puede ajustar mediante el uso de opciones. Aquí están los más comunes:



- -i

la búsqueda no distingue entre mayúsculas y minúsculas



- -r

la búsqueda es recursiva (busca en todos los archivos dentro del directorio dado y sus subdirectorios)



- -c

la búsqueda cuenta el número de coincidencias



- -E

activa expresiones regulares extendidas (necesarias para algunos de los metacaracteres más avanzados como |, + y?)


grep tiene muchas otras opciones útiles. Consulte la página de manual para obtener más información al respecto.

## Expresiones regulares

La segunda herramienta es muy poderosa. Se utiliza para describir fragmentos de texto dentro de archivos, también llamados expresiones regulares. Las expresiones regulares son extremadamente útiles para extraer datos de archivos de texto mediante la construcción de patrones. Se usan comúnmente en scripts o cuando se programa con lenguajes de alto nivel, como Perl o Python.

Cuando se trabaja con expresiones regulares, es muy importante tener en cuenta que cada carácter cuenta y el patrón se escribe con el propósito de hacer coincidir una secuencia específica de caracteres, conocida como una cadena. La mayoría de los patrones usan los símbolos ASCII normales, como letras, dígitos, signos de puntuación u otros símbolos, pero también pueden usar caracteres Unicode para que coincidan con cualquier otro tipo de texto.

La siguiente lista explica los metacaracteres de expresiones regulares que se utilizan para formar los patrones.

- .

Coincidir con cualquier caracter individual (excepto nueva línea)



- [abcABC]

Empareja cualquier caracter dentro de los corchetes



- [^ abcABC]

Empareja cualquier caracter, excepto los que están entre paréntesis



- [a-z]

Empareja cualquier caracter en el rango



- [^ a-z]

Empareja cualquier caracter excepto los del rango



- sol | luna

Encuentra cualquiera de las cadenas enumeradas



- ^

Inicio de una línea



- PS

Fin de una línea



Todas las funcionalidades de las expresiones regulares se pueden implementar a través de grep también. Puede ver que en el ejemplo anterior, la palabra no está entre comillas dobles. Para evitar que el intérprete interprete el metacaracter en sí, se recomienda que el patrón más complejo esté entre comillas dobles (" "). A los fines de la práctica, utilizaremos comillas dobles al implementar expresiones regulares. Las otras comillas mantienen su funcionalidad normal, como se discutió en lecciones anteriores.

Los siguientes ejemplos enfatizan la funcionalidad de las expresiones regulares. Necesitaremos datos dentro del archivo, por lo tanto, el siguiente conjunto de comandos solo agrega diferentes cadenas al archivo text.txt.

	$ echo "aaabbb1" > text.txt
	$ echo "abab2" >> text.txt
	$ echo "noone2" >> text.txt
	$ echo "class1" >> text.txt
	$ echo "alien2" >> text.txt
	$ cat text.txt
	aaabbb1
	abab2
	noone2
	class1
	alien2

El primer ejemplo es una combinación de búsqueda a través del archivo sin y con expresiones regulares. Para comprender completamente las expresiones regulares, es muy importante mostrar la diferencia. El primer comando busca la cadena exacta, en cualquier lugar de la línea, mientras que el segundo comando busca conjuntos de caracteres que contengan cualquiera de los caracteres entre paréntesis. Por lo tanto, los resultados de los comandos son diferentes.

	$ grep "ab" text.txt
	aaabbb1
	abab2
	$ grep "[ab]" text.txt
	aaabbb1
	abab2
	class1
	alien2

El segundo conjunto de ejemplos muestra la aplicación del principio y el final del meta-caracter de la línea. Es muy importante especificar la necesidad de colocar los 2 caracteres en el lugar correcto de la expresión. Al especificar el comienzo de la línea, el meta-caracter debe estar antes de la expresión, mientras que, al especificar el final de la línea, el meta-caracter debe estar después de la expresión.

	$ grep "^a"
	aaabbb1
	abab2
	$ grep "2$"
	abab2

Además de los metacaracteres explicados anteriormente, las expresiones regulares también tienen metacaracteres que permiten la multiplicación del patrón previamente especificado:

- * 

Cero o más del patrón anterior

- +

Uno o más de los patrones anteriores

- ?

Cero o uno de los patrones anteriores


Para los metacaracteres multiplicadores, el siguiente comando busca una cadena que contenga ab, un solo caracter y uno o más de los caracteres encontrados previamente. El resultado muestra que grep encontró la cadena aaabbb1, que coincide con la parte abbb.

	$ grep "ab.+" text.txt
	aaabbb1


La mayoría de los metacaracteres se explican por sí mismos, pero pueden volverse difíciles cuando se usan por primera vez. Los ejemplos anteriores representan una pequeña parte de la funcionalidad de las expresiones regulares. Pruebe todos los metacaracteres de la tabla anterior para comprender mejor cómo funcionan.