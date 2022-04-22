# Uso de directorios y listado de archivos

## Archivos y directorios

El sistema de archivos de Linux es similar a los sistemas de archivos de otros sistemas operativos en el sentido de que contiene archivos y directorios. Los archivos contienen datos como texto legible por humanos, programas ejecutables o datos binarios que utiliza la computadora. Los directorios se utilizan para crear organizaciones dentro del sistema de archivos. Los directorios pueden contener archivos y otros directorios.

	$ tree
	
	Documents
	├── Mission-Statement.txt
	└── Reports
	    └── report2018.txt
	
	1 directory, 2 files

En este ejemplo, Documents es un directorio que contiene un archivo (Mission-Statement.txt) y un subdirectorio (Reports). El directorio Reports a su vez contiene un archivo llamado report2018.txt. Se dice que el directorio Documents es el padre del directorio Reports.
Nombres de archivo y directorio
Los nombres de archivos y directorios en Linux pueden contener letras minúsculas y mayúsculas, números, espacios y caracteres especiales. Sin embargo, dado que muchos caracteres especiales tienen un significado especial en el shell de Linux, es una buena práctica no usar espacios o caracteres especiales al nombrar archivos o directorios. Los espacios, por ejemplo, necesitan que el carácter de escape \ se ingrese correctamente:

	$ cd Mission\ Statements

Además, consulte el nombre del archivo report2018.txt. Los nombres de archivo pueden contener un sufijo que viene después del punto (.). A diferencia de Windows, este sufijo no tiene un significado especial en Linux; está ahí para la comprensión humana. En nuestro ejemplo, .txt nos indica que este es un archivo de texto sin formato, aunque técnicamente podría contener cualquier tipo de datos.

## Navegando por el sistema de archivos

Obtener ubicación actual
Dado que los shells de Linux como Bash están basados en texto, es importante recordar su ubicación actual cuando navega por el sistema de archivos. El símbolo del sistema proporciona esta información:

	user@hostname ~/Documents/Reports $

Tenga en cuenta que la información como el nombre de usuario y host se cubrirá en secciones futuras. Desde el indicador, ahora sabemos que nuestra ubicación actual está en el directorio Reports. Del mismo modo, el comando pwd imprimirá el directorio de trabajo:

	user@hostname ~/Documents/Reports $ pwd
	/home/user/Documents/Reports

La relación de directorios se representa con una barra diagonal (/). Sabemos que Reports es un subdirectorio de Documents, que es un subdirectorio de usuario, que se encuentra en un directorio llamado home. home no parece tener un directorio padre, pero eso no es cierto en absoluto. El padre de casa se llama raíz, y está representado por la primera barra inclinada (/). Discutiremos el directorio raíz en una sección posterior.

Observe que la salida del comando pwd difiere ligeramente de la ruta dada en el símbolo del sistema. En lugar de /home/user, el símbolo del sistema contiene una tilde (~). La tilde es un carácter especial que representa el directorio de inicio del usuario. Esto se tratará con más detalle en la próxima lección.

## Listado de contenidos del directorio

El contenido del directorio actual se enumera con el comando ls:

	user@hostname ~/Documents/Reports $ ls
	report2018.txt

Tenga en cuenta que ls no proporciona información sobre el directorio principal. De manera similar, por defecto ls no muestra ninguna información sobre el contenido de los subdirectorios. Solo puede "ver" lo que está en el directorio actual.

## Cambiar el directorio actual
La navegación en Linux se realiza principalmente con el comando cd. Esto cambia el directorio. Usando el comando pwd de antes, sabemos que nuestro directorio actual es /home/user/Documents/Reports. Podemos cambiar nuestro directorio actual ingresando una nueva ruta:

	user@hostname ~ $ cd /home/user/Documents
	user@hostname ~/Documents $ pwd
	/home/user/Documents
	user@hostname ~/Documents $ ls
	Mission-Statement.txt Reports

Desde nuestra nueva ubicación, podemos "ver" Mission-Statement.txt y nuestro subdirectorio Reports, pero no los contenidos de nuestro subdirectorio. Podemos volver a navegar en Reports como este:

	user@hostname ~/Documents $ cd Reports
	user@hostname ~/Documents/Reports $ pwd
	/home/user/Documents/Reports
	user@hostname ~/Documents/Reports $ ls
	report2018.txt

Ahora estamos de vuelta donde empezamos.

## Caminos absolutos y relativos

El comando pwd siempre imprime una ruta absoluta. Esto significa que la ruta contiene cada paso de la ruta, desde la parte superior del sistema de archivos (/) hasta la parte inferior (Reports). Los caminos absolutos siempre comienzan con un /.

	/
	└── home
	    └── user
	        └── Documents
	            └── Reports

La ruta absoluta contiene toda la información requerida para llegar a Reports desde cualquier lugar del sistema de archivos. El inconveniente es que es tedioso escribir.

El segundo ejemplo (cd Reports) fue mucho más fácil de escribir. Este es un ejemplo de una ruta relativa. Las rutas relativas son más cortas pero solo tienen significado en relación con su ubicación actual. Considera esta analogía: te estoy visitando en tu casa. Me dices que tu amigo vive al lado. Entenderé esa ubicación porque es relativa a mi ubicación actual. Pero si me dices esto por teléfono, no podré encontrar la casa de tu amigo. Deberá darme la dirección completa.

## Caminos Relativos Especiales

El shell de Linux nos brinda formas de acortar nuestros caminos al navegar. Para revelar las primeras rutas especiales, ingresamos el comando ls con la bandera -a. Este indicador modifica el comando ls para que se enumeren todos los archivos y directorios, incluidos los archivos y directorios ocultos:

	user@hostname ~/Documents/Reports $ ls -a
	.
	..
	report2018.txt

Este comando ha revelado dos resultados adicionales: Estas son rutas especiales. No representan nuevos archivos o directorios, sino que representan directorios que ya conoce:

- .

Indica la ubicación actual (en este caso, Reports).

- ..

Indica el directorio principal (en este caso, Documents).
Por lo general, no es necesario usar la ruta relativa especial para la ubicación actual. Es más fácil y más comprensible escribir report2018.txt que escribir ./report2018.txt. Pero el . tiene usos que aprenderás en futuras secciones. Por ahora, nos centraremos en la ruta relativa para el directorio principal:

	user@hostname ~/Documents/Reports $ cd ..
	user@hostname ~/Documents $ pwd
	/home/user/Documents

El ejemplo de cd es mucho más fácil cuando se usa .. en lugar de la ruta absoluta. Además, podemos combinar este patrón para navegar por el árbol de archivos muy rápidamente.

	user@hostname ~/Documents $ cd ../..
	$ pwd
	/home

El sistema operativo Unix se diseñó originalmente para computadoras mainframe a mediados de la década de 1960. Estas computadoras fueron compartidas entre muchos usuarios, quienes accedieron a los recursos del sistema a través de terminales. Estas ideas fundamentales se transmiten hoy a los sistemas Linux. Todavía hablamos sobre el uso de "terminales" para ingresar comandos en el shell, y cada sistema Linux está organizado de tal manera que es fácil crear muchos usuarios en un solo sistema.

## Directorios de inicio
Este es un ejemplo de un sistema de archivos normal en Linux:

	$ tree -L 1 /
	/
	├── bin
	├── boot
	├── cdrom
	├── dev
	├── etc
	├── home
	├── lib
	├── mnt
	├── opt
	├── proc
	├── root
	├── run
	├── sbin
	├── srv
	├── sys
	├── tmp
	├── usr
	└── var

La mayoría de estos directorios son consistentes en todos los sistemas Linux. Desde servidores hasta supercomputadoras y pequeños sistemas integrados, un usuario experimentado de Linux puede estar seguro de que puede encontrar el comando ls dentro de /bin, puede cambiar la configuración del sistema modificando archivos en /etc, y leer los registros del sistema en /var. La ubicación estándar de estos archivos y directorios está definida por el Estándar de jerarquía del sistema de archivos (FHS), que se discutirá en una lección posterior. Aprenderá más sobre el contenido de estos directorios a medida que continúe aprendiendo sobre Linux, pero por el momento, sepa que:

los cambios que realice en el sistema de archivos raíz afectarán a todos los usuarios, y
cambiar archivos en el sistema de archivos raíz requerirá permisos de super usuario.
Esto significa que los usuarios normales tendrán prohibido modificar estos archivos, y también se les puede prohibir incluso leer estos archivos. Cubriremos el tema de los permisos en una sección posterior.

Ahora, nos centraremos en el directorio /home, que debería ser algo familiar en este punto:

	$ tree -L 1 /home
	/home
	├── user
	├── michael
	└── lara

Nuestro sistema de ejemplo tiene tres usuarios normales, y cada uno de nuestros usuarios tiene su propia ubicación dedicada, donde pueden crear y modificar archivos y directorios sin afectar a su vecino. Por ejemplo, en la lección anterior estábamos trabajando con la siguiente estructura de archivos:

	$ tree /home/user
	user
	└── Documents
	    ├── Mission-Statement
	    └── Reports
	        └── report2018.txt

En realidad, el sistema de archivos real puede verse así:

	$ tree /home
	/home
	├── user
	│   └── Documents
	│       ├── Mission-Statement
	│       └── Reports
	│           └── report2018.txt
	├── michael
	│   ├── Documents
	│   │   └── presentation-for-clients.odp
	│   └── Music

... y así por lara.

En Linux, /home es similar a un edificio de apartamentos. Muchos usuarios pueden tener su espacio aquí, separados en apartamentos dedicados. Las utilidades y el mantenimiento del edificio en sí son responsabilidad del usuario root del superintendente.


El camino relativo especial para Home
Cuando inicia una nueva sesión de terminal en Linux, ve un símbolo del sistema similar a este:

	user@hostname ~ $

La tilde (~) aquí representa nuestro directorio de inicio. Si ejecuta el comando ls, verá algunos resultados familiares:

	$ cd ~
	$ ls
	Documents

Compare eso con el sistema de archivos anterior para verificar su comprensión.

Considere ahora lo que sabemos sobre Linux: es similar a un edificio de apartamentos, con muchos usuarios que residen en /home. Entonces la casa del usuario user será diferente de la casa del usuario michael. Para demostrar esto, utilizaremos el comando su para cambiar de usuario.

	user@hostname ~ $ pwd
	/home/user
	user@hostname ~ $ su michael
	Password:
	michael@hostname ~ $ pwd
	/home/michael

El significado de ~ cambia dependiendo de quién sea el usuario. Para michael, la ruta absoluta de ~ es /home/michael. Para lara, el camino absoluto de ~ es /home/lara, y así sucesivamente.

### Rutas de archivo relativas al inicio

Usar ~ para comandos es muy conveniente, siempre que no cambie de usuario. Consideraremos el siguiente ejemplo para el usuario, que ha comenzado una nueva sesión:

	$ ls
	Documents
	$ cd Documents
	$ ls
	Mission-Statement
	Reports
	$ cd Reports
	$ ls
	report2018.txt
	$ cd ~
	$ ls
	Documents

Tenga en cuenta que los usuarios siempre comenzarán una nueva sesión en su directorio de inicio. En este ejemplo, el usuario ha viajado a su subdirectorio Documents/Reports, y con el comando cd ~ ha regresado a donde comenzó. Puede realizar la misma acción utilizando el comando cd sin argumentos:

	$ cd Documents/Reports
	$ pwd
	/home/user/Documents/Reports
	$ cd
	$ pwd
	/home/user

Una última cosa a tener en cuenta: podemos especificar los directorios de inicio de otros usuarios especificando el nombre de usuario después de la tilde. Por ejemplo:

	$ ls ~michael
	Documents
	Music

Tenga en cuenta que esto solo funcionará si michael nos ha dado permiso para ver el contenido de su directorio de inicio.

Consideremos una situación en la que a michael le gustaría ver el archivo report2018.txt en el directorio de inicio del usuario User. Suponiendo que michael tiene permiso para hacerlo, puede usar el comando less.

	$ less ~user/Documents/Reports/report2018.txt

Cualquier ruta de archivo que contenga el carácter ~ se denomina ruta relativa al inicio.

### Archivos y directorios ocultos

En la lección anterior, presentamos la opción -a para el comando ls. Utilizamos ls -a para presentar las dos rutas relativas especiales: (. y ..) . La opción -a enumerará todos los archivos y directorios, incluidos los archivos y directorios ocultos.

	$ ls -a ~
	.
	..
	.bash_history
	.bash_logout
	.bash-profile
	.bashrc
	Documents

Los archivos y directorios ocultos siempre comenzarán con un punto (.). De manera predeterminada, el directorio de inicio de un usuario incluirá muchos archivos ocultos. A menudo se usan para establecer configuraciones de configuración específicas del usuario, y solo deben ser modificadas por un usuario experimentado.

### La opción de lista larga

El comando ls tiene muchas opciones para cambiar su comportamiento. Veamos una de las opciones más comunes:

	$ ls -l
	-rw-r--r-- 1 user staff      3606 Jan 13  2017 report2018.txt

- -l crea una larga lista. Los archivos y directorios ocuparán cada uno una línea, pero se mostrará información adicional sobre cada archivo y directorio.

- -rw-r— r--

Tipo de archivo y permisos del archivo. Tenga en cuenta que un archivo normal comenzará con un guión y un directorio comenzará con d.

- 1

Número de enlaces al archivo.

- user staff

Especifica la propiedad del archivo. user es el propietario del archivo, y el archivo también está asociado con el grupo staff.

- 3606

Tamaño del archivo en bytes.

- 13 de ene de 2017

Marca de tiempo de la última modificación del archivo.

- report2018.txt

Nombre del archivo.


Temas como la propiedad, los permisos y los enlaces se tratarán en secciones futuras. Como puede ver, la versión de lista larga de ls a menudo es preferible a la predeterminada.


### Recursión en Bash
La recursión se refiere a una situación en la que "algo se define en términos de sí mismo". La recursión es un concepto muy importante en informática, pero aquí su significado es mucho más simple. Consideremos nuestro ejemplo anterior:

	$ ls ~
	Documents

Sabemos desde antes User tenia un directorio de inicio, y en este directorio hay un subdirectorio. Hasta ahora, ls solo nos muestra los archivos y subdirectorios de una ubicación, pero no puede decirnos el contenido de estos subdirectorios. En estas lecciones, hemos estado usando el comando tree cuando queremos mostrar el contenido de muchos directorios. Desafortunadamente, tree no es una de las principales utilidades de Linux y, por lo tanto, no siempre está disponible. Compare la salida de tree con la salida de ls -R en los siguientes ejemplos:

	$ tree /home/user
	user
	└── Documents
	    ├── Mission-Statement
	    └── Reports
	        └── report2018.txt
	
	$ ls -R ~
	/home/user/:
	Documents
	
	/home/user/Documents:
	Mission-Statement
	Reports
	
	/home/user/Documents/Reports:
	report2018.txt

Como puede ver, con la opción recursiva, obtenemos una lista mucho más larga de archivos. De hecho, es como si ejecutamos el comando ls en el directorio de inicio del usuario y encontramos un subdirectorio. Luego, ingresamos a ese subdirectorio y volvimos a ejecutar el comando ls. Encontramos el archivo Mission-Statement y otro subdirectorio llamado Reports. Y nuevamente, ingresamos al subdirectorio y ejecutamos el comando ls nuevamente. Esencialmente, ejecutar ls -R es como decirle a Bash: "Ejecute ls aquí y repita el comando en cada subdirectorio que encuentre".

La recursión es particularmente importante en los comandos de modificación de archivos, como copiar o eliminar directorios. Por ejemplo, si desea copiar el subdirectorio Documents, necesitaría especificar una copia recursiva para extender este comando a todos los subdirectorios.