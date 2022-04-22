# Crear, mover y eliminar archivos

Esta lección cubre la administración de archivos y directorios en Linux usando herramientas de línea de comandos.

Un archivo es una colección de datos con un nombre y un conjunto de atributos. Si, por ejemplo, transfiriera algunas fotos de su teléfono a una computadora y les diera nombres descriptivos, ahora tendría un montón de archivos de imagen en su computadora. Estos archivos tienen atributos como la hora en que se accedió o modificó por última vez.

Un directorio es un tipo especial de archivo utilizado para organizar archivos. Una buena manera de pensar en los directorios es como las carpetas de archivos utilizadas para organizar documentos en un archivador. A diferencia de las carpetas de archivos de papel, puede colocar fácilmente directorios dentro de otros directorios.

La línea de comando es la forma más efectiva de administrar archivos en un sistema Linux. Las herramientas de shell y línea de comando tienen características que hacen que usar la línea de comando sea más rápido y fácil que un administrador de archivos gráfico.

En esta sección, usará los comandos ls, mv, cp, pwd, find, touch, rm, rmdir, echo, cat y mkdir para administrar y organizar archivos y directorios.



## Case Sensitivity
A diferencia de Microsoft Windows, los nombres de archivos y directorios en sistemas Linux distinguen entre mayúsculas y minúsculas. Esto significa que los nombres /etc/ y /ETC/ son directorios diferentes. Pruebe los siguientes comandos:

	$ cd /
	$ ls
	bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
	boot  etc  lib   media  opt  root  sbin  sys  usr
	$ cd ETC
	bash: cd: ETC: No such file or directory
	$ pwd
	/
	$ cd etc
	$ pwd
	/etc

El pwd le muestra el directorio en el que se encuentra actualmente. Como puede ver, el cambio a /ETC no funcionó ya que no existe dicho directorio. El cambio al directorio /etc que existe, tuvo éxito.


## Crear directorios
El comando mkdir se usa para crear directorios.

Vamos a crear un nuevo directorio dentro de nuestro directorio de inicio:

	$ cd ~
	$ pwd
	/home/user
	$ ls
	Desktop  Documents  Downloads
	$ mkdir linux_essentials-2.4
	$ ls
	Desktop  Documents  Downloads  linux_essentials-2.4
	$ cd linux_essentials-2.4
	$ pwd
	/home/emma/linux_essentials-2.4

En esta lección, todos los comandos se llevarán a cabo dentro de este directorio o en uno de sus subdirectorios.

Para regresar fácilmente al directorio de lecciones desde cualquier otra posición en su sistema de archivos, puede usar el comando:

	$ cd ~/linux_essentials-2.4

El shell interpreta el carácter ~ como su directorio de inicio.

Cuando esté en el directorio de la lección, cree algunos directorios más que usaremos para los ejercicios. Puede agregar todos los nombres de directorio, separados por espacios, a mkdir:

	$ mkdir creating moving copying/files copying/directories deleting/directories deleting/files globs
	mkdir: cannot create directory ‘copying/files’: No such file or directory
	mkdir: cannot create directory ‘copying/directories’: No such file or directory
	mkdir: cannot create directory ‘deleting/directories’: No such file or directory
	mkdir: cannot create directory ‘deleting/files’: No such file or directory
	$ ls
	creating  globs  moving

Observe el mensaje de error y que solo se crearon moving, globs y creating. Los directorios de copia y eliminación aún no existen. mkdir, por defecto, no creará un directorio dentro de un directorio que no existe. La opción -p o --parents le indica a mkdir que cree directorios principales si no existen. Pruebe el mismo comando mkdir con la opción -p:
$ mkdir -p creating moving copying/files copying/directories deleting/directories deleting/files globs

Ahora no recibe ningún mensaje de error. Veamos qué directorios existen ahora:

	$ find
	.
	./creating
	./moving
	./globs
	./copying
	./copying/files
	./copying/directories
	./deleting
	./deleting/directories
	./deleting/files

El programa find se usa generalmente para buscar archivos y directorios, pero sin ninguna opción, le mostrará una lista de todos los archivos, directorios y subdirectorios de su directorio actual.

## Crear archivos

Por lo general, los archivos serán creados por los programas que trabajan con los datos almacenados en los archivos. Se puede crear un archivo vacío con el comando touch. Si ejecuta touch en un archivo existente, el contenido del archivo no se cambiará, pero la marca de tiempo de modificación de archivos se actualizará.

Ejecute el siguiente comando para crear algunos archivos para la lección globs:

	$ touch globs/question1 globs/question2012 globs/question23 globs/question13 globs/question14
	$ touch globs/star10 globs/star1100 globs/star2002 globs/star2013

Ahora verifiquemos que todos los archivos existen en el directorio globs:

	$ cd globs
	$ ls
	question1   question14    question23  star1100  star2013
	question13  question2012  star10      star2002

Observe cómo touch creó los archivos. Puede ver el contenido de un archivo de texto con el comando cat. Pruébelo en uno de los archivos que acaba de crear:

	$ cat question14

Dado que touch crea archivos vacíos, no debería obtener ningún resultado. Puede usar echo con > para crear archivos de texto simples. Inténtalo:

	$ echo hello > question15
	$ cat question15
	hello

echo muestra texto en la línea de comando. El carácter > indica al shell que escriba la salida de un archivo en el archivo especificado en lugar de su terminal. Esto lleva a la salida de echo, hello en este caso, que se escribe en el archivo question15. Esto no es específico de echo, se puede usar con cualquier comando.

## Renombrar archivos
Los archivos se mueven y cambian de nombre con el comando mv.

Establezca su directorio de trabajo en el directorio moving:

	$ cd ~/linux_essentials-2.4/moving

Crea algunos archivos para practicar. Por ahora, ya debería estar familiarizado con estos comandos:

	$ touch file1 file22
	$ echo file3 > file3
	$ echo file4 > file4
	$ ls
	file1  file22  file3  file4

Supongamos que file22 es un error tipográfico y debería ser file2. Arréglalo con el comando mv. Al cambiar el nombre de un archivo, el primer argumento es el nombre actual, el segundo es el nuevo nombre:

	$ mv file22 file2
	$ ls
	file1  file2  file3  file4

Tenga cuidado con el comando mv. Si cambia el nombre de un archivo al nombre de un archivo existente, lo sobrescribirá. Probemos esto con file3 y file4:

	$ cat file3
	file3
	$ cat file4
	file4
	$ mv file4 file3
	$ cat file3
	file4
	$ ls
	file1  file2  file3

Observe como el contenido de file3 ahora es el file4. Use la opción -i para hacer que mv le pregunte si está a punto de sobrescribir un archivo existente. Inténtalo:

	$ touch file4 file5
	$ mv -i file4 file3
	mv: overwrite ‘file3’? y

## Moviendo archivos

Los archivos se mueven de un directorio a otro con el comando mv.

Cree algunos directorios para mover archivos a:

	$ cd ~/linux_essentials-2.4/moving
	$ mkdir dir1 dir2
	$ ls
	dir1  dir2  file1  file2  file3  file5

Mueva el file1 a dir1:

	$ mv file1 dir1
	$ ls
	dir1  dir2  file2  file3  file5
	$ ls dir1
	file1

Observe cómo el último argumento para mv es el directorio de destino. Cada vez que el último argumento para mv es un directorio, los archivos se mueven a él. Se pueden especificar varios archivos en un solo comando mv:

	$ mv file2 file3 dir2
	$ ls
	dir1  dir2  file5
	$ ls dir2
	file2  file3

También es posible usar mv para mover y renombrar directorios. Cambie el nombre de dir1 a dir3:

	$ ls
	dir1  dir2  file5
	$ ls dir1
	file1
	$ mv dir1 dir3
	$ ls
	dir2  dir3  file5
	$ ls dir3
	file1

## Eliminar archivos y directorios

El comando rm puede eliminar archivos y directorios, mientras que el comando rmdir solo puede eliminar directorios. Vamos a limpiar el directorio moving eliminando el archivo file5:

	$ cd ~/linux_essentials-2.4/moving
	$ ls
	dir2  dir3  file5
	$ rmdir file5
	rmdir: failed to remove ‘file5’: Not a directory
	$ rm file5
	$ ls
	dir2  dir3

Por defecto, rmdir solo puede eliminar directorios vacíos, por lo tanto, tuvimos que usar rm para eliminar un archivo normal. Intente eliminar el directorio deleting:

	$ cd ~/linux_essentials-2.4/
	$ ls
	copying  creating  deleting  globs  moving
	$ rmdir deleting
	rmdir: failed to remove ‘deleting’: Directory not empty
	$ ls -l deleting
	total 0
	drwxrwxr-x. 2 emma emma 6 Mar 26 14:58 directories
	drwxrwxr-x. 2 emma emma 6 Mar 26 14:58 files

De forma predeterminada, rmdir se niega a eliminar un directorio que no está vacío. Use rmdir para eliminar uno de los subdirectorios vacíos del directorio deleting:

	$ ls -a deleting/files
	.  ..
	$ rmdir deleting/files
	$ ls -l deleting
	directories

Eliminar grandes cantidades de archivos o estructuras de directorios profundas con muchos subdirectorios puede parecer tedioso, pero en realidad es fácil. Por defecto, rm solo funciona en archivos normales. La opción -r se usa para anular este comportamiento. ¡Cuidado, rm -r es un excelente pistolero! Cuando usa la opción -r, rm no solo eliminará cualquier directorio, sino todo lo que esté dentro de ese directorio, incluidos los subdirectorios y sus contenidos. Vea usted mismo cómo funciona rm -r:

	$ ls
	copying  creating  deleting  globs  moving
	$ rm deleting
	rm: cannot remove ‘deleting’: Is a directory
	$ ls -l deleting
	total 0
	drwxrwxr-x. 2 emma emma 6 Mar 26 14:58 directories
	$ rm -r deleting
	$ ls
	copying  creating  globs  moving

Observe cómo se eliminó deleting, incluso si no estaba vacío. Al igual que mv, rm tiene una opción -i para avisarle antes de hacer cualquier cosa. Use rm -ri para eliminar directorios de la sección moving que ya no son necesarios:

	$ find
	.
	./creating
	./moving
	./moving/dir2
	./moving/dir2/file2
	./moving/dir2/file3
	./moving/dir3
	./moving/dir3/file1
	./globs
	./globs/question1
	./globs/question2012
	./globs/question23
	./globs/question13
	./globs/question14
	./globs/star10
	./globs/star1100
	./globs/star2002
	./globs/star2013
	./globs/question15
	./copying
	./copying/files
	./copying/directories
	$ rm -ri moving
	rm: descend into directory ‘moving’? y
	rm: descend into directory ‘moving/dir2’? y
	rm: remove regular empty file ‘moving/dir2/file2’? y
	rm: remove regular empty file ‘moving/dir2/file3’? y
	rm: remove directory ‘moving/dir2’? y
	rm: descend into directory ‘moving/dir3’? y
	rm: remove regular empty file ‘moving/dir3/file1’? y
	rm: remove directory ‘moving/dir3’? y
	rm: remove directory ‘moving’? y
	Copiar archivos y directorios

El comando cp se usa para copiar archivos y directorios. Copie algunos archivos en el directorio de copia:

	$ cd ~/linux_essentials-2.4/copying
	$ ls
	directories  files
	$ cp /etc/nsswitch.conf files/nsswitch.conf
	$ cp /etc/issue /etc/hostname files

Si el último argumento es un directorio, cp creará una copia de los argumentos anteriores dentro del directorio. Al igual que mv, se pueden especificar varios archivos a la vez, siempre que el destino sea un directorio.



Cuando ambos operandos de cp son archivos y ambos archivos existen, cp sobrescribe el segundo archivo con una copia del primer archivo. Practiquemos esto sobrescribiendo el archivo issue con el archivo hostname:

	$ cd ~/linux_essentials-2.4/copying/files
	$ ls
	hostname  issue  nsswitch.conf
	$ cat hostname
	mycomputer
	$ cat issue
	Debian GNU/Linux 9 \n \l
	
	$ cp hostname issue
	$ cat issue
	mycomputer

Ahora intentemos crear una copia de los archivos del directorio files dentro del directorio directory:

	$ cd ~/linux_essentials-2.4/copying
	$ cp files directories
	cp: omitting directory ‘files’

Como puede ver, cp por defecto solo funciona en archivos individuales. Para copiar un directorio, usa la opción -r. Tenga en cuenta que la opción -r hará que cp copie también el contenido del directorio que está copiando:

	$ cp -r files directories
	$ find
	.
	./files
	./files/nsswitch.conf
	./files/fstab
	./files/hostname
	./directories
	./directories/files
	./directories/files/nsswitch.conf
	./directories/files/fstab
	./directories/files/hostname

¿Observa cómo cuando se utilizó un directorio existente como destino, cp crea una copia del directorio fuente dentro de él? Si el destino no existe, lo creará y lo llenará con el contenido del directorio de origen:

	$ cp -r files files2
	$ find
	.
	./files
	./files/nsswitch.conf
	./files/fstab
	./files/hostname
	./directories
	./directories/files
	./directories/files/nsswitch.conf
	./directories/files/fstab
	./directories/files/hostname
	./files2
	./files2/nsswitch.conf
	./files2/fstab
	./files2/hostname

## Globbing
Lo que comúnmente se conoce como globbing es un lenguaje simple de coincidencia de patrones. Los shells de línea de comandos en sistemas Linux usan este lenguaje para referirse a grupos de archivos cuyos nombres coinciden con un patrón específico. POSIX.1-2017 especifica los siguientes caracteres de coincidencia de patrones:

- * 

Coincide con cualquier número de cualquier carácter, incluido ningún carácter

- ?

Coincide con cualquier caracter

- []

Coincide con una clase de caracteres
En español, esto significa que puede indicarle a su shell que coincida con un patrón en lugar de una cadena literal de texto. Por lo general, los usuarios de Linux especifican varios archivos con un globo en lugar de escribir cada nombre de archivo. Ejecute los siguientes comandos:

	$ cd ~/linux_essentials-2.4/globs
	$ ls
	question1   question14  question2012  star10    star2002
	question13  question15  question23    star1100  star2013
	$ ls star1*
	star10  star1100
	$ ls star*
	star10  star1100  star2002  star2013
	$ ls star2*
	star2002  star2013
	$ ls star2*2
	star2002
	$ ls star2013*
	star2013

El shell se expande * a cualquier número de cualquier cosa, por lo que su shell interpreta star * como cualquier cosa en el contexto relevante que comienza con star. Cuando ejecuta el comando ls star *, su shell no ejecuta el programa ls con un argumento de star *, busca archivos en el directorio actual que coincidan con el patrón star * (incluyendo solo star), y hace que cada archivo coincida con el patrón sea un argumento para ls:

	$ ls star*

en lo que respecta a ls es el equivalente de:

	$ ls star10 star1100 star2002 star2013

El carácter * no significa nada para ls. Para probar esto, ejecute el siguiente comando:

	$ ls star\* ls: cannot access star*: No such file or directory

Cuando precede a un carácter con un \, le indica a su shell que no lo interprete. En este caso, desea que ls tenga un argumento de star* en lugar de a qué se expande la star* a glob.

Los ? se expande a cualquier caracter individual. Prueba los siguientes comandos para ver por ti mismo:

	$ ls
	question1   question14  question2012  star10    star2002
	question13  question15  question23    star1100  star2013
	$ ls question?
	question1
	$ ls question1?
	question13  question14  question15
	$ ls question?3
	question13  question23
	$ ls question13?
	ls: cannot access question13?: No such file or directory

Los corchetes [] se usan para unir rangos o clases de caracteres. Los corchetes [] funcionan como lo hacen en las expresiones regulares POSIX, excepto con los globos que se usan ^ en lugar de !

Crea algunos archivos para experimentar con:

	$ mkdir brackets
	$ cd brackets
	$ touch file1 file2 file3 file4 filea fileb filec file5 file6 file7

Los rangos entre corchetes [] se expresan usando un -:

	$ ls
	file1  file2  file3  file4  file5  file6  file7  filea  fileb  filec
	$ ls file[1-2]
	file1  file2
	$ ls file[1-3]
	file1  file2  file3

Se pueden especificar múltiples rangos:

	$ ls file[1-25-7]
	file1  file2  file5  file6  file7
	$ ls file[1-35-6a-c]
	file1  file2  file3  file5  file6  filea  fileb  filec

Los corchetes también se pueden usar para unir un conjunto específico de caracteres.

	$ ls file[1a5]
	file1  file5  filea

También puede usar el carácter ^ como primer carácter para hacer coincidir todo excepto ciertos caracteres.

	$ ls file[^a]
	file1  file2  file3  file4  file5  file6  file7  fileb  filec

Lo último que cubriremos en esta lección son las clases de caracteres. Para hacer coincidir una clase de caracteres, usa [: classname:]. Por ejemplo, para usar la clase de dígitos, que coincide con los números, haría algo como esto:

	$ ls file[[:digit:]]
	file1  file2  file3  file4  file5  file6  file7
	$ touch file1a file11
	$ ls file[[:digit:]a]
	file1  file2  file3  file4  file5  file6  file7  filea
	$ ls file[[:digit:]]a
	file1a

El archivo global [[: digit:] a], coincide con el archivo seguido de un dígito o a.

Las clases de caracteres admitidas dependen de su entorno local actual. POSIX requiere las siguientes clases de caracteres para todas las configuraciones regionales:

	[:alnum:]

Letras y numeros.

	[:alpha:]

Letras mayúsculas o minúsculas.

	[:blank:]

Espacios y pestañas.

	[:cntrl:]

Personajes de control, ejemplo. retroceso, campana, NAK, escape.

	[:digit:]

Numerales (0123456789).

	[:graph:]

Caracteres gráficos (todos los caracteres excepto ctrl y el espacio)

	[:lower:]

Letras minusculas (a-z).

	[:print:]

Caracteres imprimibles (alnum, punct, y el caracter espacio).

	[:punct:]

Caracteres de puntuación, i.e. !, &, ".

	[:space:]

Caracteres de espacios en blanco, ejemplo: pestañas, espacios, líneas nuevas.

	[:upper:]

Letras mayúsculas (A-Z).

	[:xdigit:]

Números hexadecimales (usualmente 0123456789abcdefABCDEF).