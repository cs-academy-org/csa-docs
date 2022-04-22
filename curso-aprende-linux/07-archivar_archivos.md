# Archivando archivos en la línea de comandos

La compresión se usa para reducir la cantidad de espacio que consume un conjunto específico de datos. La compresión se usa comúnmente para reducir la cantidad de espacio que se necesita para almacenar un archivo. Otro uso común es reducir la cantidad de datos enviados a través de una conexión de red.

La compresión funciona reemplazando patrones repetitivos en los datos. Supongamos que tienes una novela. Algunas palabras son extremadamente comunes pero tienen múltiples caracteres, como la palabra "el". Podrías reducir el tamaño de la novela significativamente si reemplazaras estas palabras y patrones comunes de varios caracteres con reemplazos de un solo carácter. Por ejemplo, reemplace "el" con una letra griega que no se utiliza en ninguna otra parte del texto. Los algoritmos de compresión de datos son similares a esto pero más complejos.

La compresión viene en dos variedades, sin pérdida y con pérdida. Las cosas comprimidas con un algoritmo sin pérdidas pueden descomprimirse nuevamente en su forma original. Los datos comprimidos con un algoritmo con pérdida no se pueden recuperar. Los algoritmos con pérdida se usan a menudo para imágenes, video y audio donde la pérdida de calidad es imperceptible para los humanos, irrelevante para el contexto, o la pérdida vale el espacio ahorrado o el rendimiento de la red.

Las herramientas de archivo se utilizan para agrupar archivos y directorios en un solo archivo. Algunos usos comunes son las copias de seguridad, la agrupación del código fuente del software y la retención de datos.

El archivo y la compresión se usan comúnmente juntos. Algunas herramientas de archivo incluso comprimen su contenido de forma predeterminada. Otros pueden opcionalmente comprimir sus contenidos. Se deben utilizar algunas herramientas de archivo en combinación con herramientas de compresión independientes si desea comprimir el contenido.

La herramienta más común para archivar archivos en sistemas Linux es tar. La mayoría de las distribuciones de Linux se entregan con la versión GNU de tar, por lo que es la que se tratará en esta lección. tar por sí solo gestiona el archivado de archivos pero no los comprime.

Hay muchas herramientas de compresión disponibles en Linux. Algunos sin pérdida comunes son bzip2, gzip y xz. Encontrará los tres en la mayoría de los sistemas. Puede encontrar un sistema antiguo o muy mínimo en el que xz o bzip no esté instalado. Si se convierte en un usuario habitual de Linux, es probable que encuentre archivos comprimidos con estos tres. Los tres usan algoritmos diferentes, por lo que un archivo comprimido con una herramienta no puede ser descomprimido por otra. Las herramientas de compresión tienen un compromiso. Si desea una relación de compresión alta, llevará más tiempo comprimir y descomprimir el archivo. Esto se debe a que una mayor compresión requiere más trabajo para encontrar patrones más complejos. Todas estas herramientas comprimen datos pero no pueden crear archivos que contengan múltiples archivos.

Las herramientas de compresión independientes no suelen estar disponibles en los sistemas Windows. Las herramientas de compresión y archivado de Windows generalmente se agrupan. Tenga esto en cuenta si tiene sistemas Linux y Windows que necesitan compartir archivos.

Los sistemas Linux también tienen herramientas para manejar archivos .zip comúnmente utilizados en el sistema Windows. Se llaman zip y descomprimir. Estas herramientas no están instaladas de manera predeterminada en todos los sistemas, por lo que si necesita usarlas puede que tenga que instalarlas. Afortunadamente, generalmente se encuentran en los repositorios de paquetes de distribuciones.



## Herramientas de compresión
La cantidad de espacio en disco que se ahorra al comprimir archivos depende de algunos factores. La naturaleza de los datos que está comprimiendo, el algoritmo utilizado para comprimir los datos y el nivel de compresión. No todos los algoritmos admiten diferentes niveles de compresión.

Comencemos por configurar algunos archivos de prueba para comprimir:

	$ mkdir ~/linux_essentials-3.1
	$ cd ~/linux_essentials-3.1
	$ mkdir compression archiving
	$ cd compression
	$ cat /etc/* > bigfile 2> /dev/null

Ahora creamos tres copias de este archivo:

	$ cp bigfile bigfile2
	$ cp bigfile bigfile3
	$ cp bigfile bigfile4
	$ ls -lh
	total 2.8M
	-rw-r--r-- 1 emma emma 712K Jun 23 08:08 bigfile
	-rw-r--r-- 1 emma emma 712K Jun 23 08:08 bigfile2
	-rw-r--r-- 1 emma emma 712K Jun 23 08:08 bigfile3
	-rw-r--r-- 1 emma emma 712K Jun 23 08:08 bigfile4

Ahora vamos a comprimir los archivos con cada herramienta de compresión mencionada anteriormente:

	$ bzip2 bigfile2
	$ gzip bigfile3
	$ xz bigfile4
	$ ls -lh
	total 1.2M
	-rw-r--r-- 1 emma emma 712K Jun 23 08:08 bigfile
	-rw-r--r-- 1 emma emma 170K Jun 23 08:08 bigfile2.bz2
	-rw-r--r-- 1 emma emma 179K Jun 23 08:08 bigfile3.gz
	-rw-r--r-- 1 emma emma 144K Jun 23 08:08 bigfile4.xz

Compare los tamaños de los archivos comprimidos con el archivo sin comprimir denominado bigfile. Observe también cómo las herramientas de compresión agregaron extensiones a los nombres de los archivos y eliminaron los archivos sin comprimir.

Use bunzip2, gunzip o unxz para descomprimir los archivos:

	$ bunzip2 bigfile2.bz2
	$ gunzip bigfile3.gz
	$ unxz bigfile4.xz
	$ ls -lh
	total 2.8M
	-rw-r--r-- 1 emma emma 712K Jun 23 08:20 bigfile
	-rw-r--r-- 1 emma emma 712K Jun 23 08:20 bigfile2
	-rw-r--r-- 1 emma emma 712K Jun 23 08:20 bigfile3
	-rw-r--r-- 1 emma emma 712K Jun 23 08:20 bigfile4

Observe nuevamente que ahora el archivo comprimido se elimina una vez que se descomprime.

Algunas herramientas de compresión admiten diferentes niveles de compresión. Un nivel de compresión más alto generalmente requiere más memoria y ciclos de CPU, pero da como resultado un archivo comprimido más pequeño. Lo contrario es cierto para un nivel inferior. A continuación se muestra una demostración con xz y gzip:

	$ cp bigfile bigfile-gz1
	$ cp bigfile bigfile-gz9
	$ gzip -1 bigfile-gz1
	$ gzip -9 bigfile-gz9
	$ cp bigfile bigfile-xz1
	$ cp bigfile bigfile-xz9
	$ xz -1 bigfile bigfile-xz1
	$ xz -9 bigfile bigfile-xz9
	$ ls -lh bigfile bigfile-* *
	total 3.5M
	-rw-r--r-- 1 emma emma 712K Jun 23 08:08 bigfile
	-rw-r--r-- 1 emma emma 205K Jun 23 13:14 bigfile-gz1.gz
	-rw-r--r-- 1 emma emma 178K Jun 23 13:14 bigfile-gz9.gz
	-rw-r--r-- 1 emma emma 156K Jun 23 08:08 bigfile-xz1.xz
	-rw-r--r-- 1 emma emma 143K Jun 23 08:08 bigfile-xz9.xz

No es necesario descomprimir un archivo cada vez que lo usa. Las herramientas de compresión generalmente vienen con versiones especiales de herramientas comunes utilizadas para leer archivos de texto. Por ejemplo, gzip tiene una versión de cat, grep, diff, less, more y algunas otras. Para gzip, las herramientas tienen como prefijo una z, mientras que el prefijo bz existe para bzip2 y xz existe para xz. A continuación se muestra un ejemplo del uso de zcat para leer mostrar un archivo comprimido con gzip:

	$ cp /etc/hosts ./
	$ gzip hosts
	$ zcat hosts.gz
	127.0.0.1	localhost
	
	# The following lines are desirable for IPv6 capable hosts
	::1     localhost ip6-localhost ip6-loopback
	ff02::1 ip6-allnodes
	ff02::2 ip6-allrouters

## Herramientas de archivo
El programa tar es probablemente la herramienta de archivo más utilizada en sistemas Linux. En caso de que se pregunte por qué se llama así, es la abreviatura de "archivo de cinta". Los archivos creados con tar a menudo se llaman bolas de tar (tar balls). Es muy común que las aplicaciones distribuidas como código fuente estén en bolas de tar.

La versión GNU de tar con la que se distribuyen las distribuciones de Linux tiene muchas opciones. Esta lección cubrirá el subconjunto más utilizado.

Comencemos creando un archivo de los archivos utilizados para la compresión:

	$ cd ~/linux_essentials-3.1
	$ tar cf archiving/3.1.tar compression

La opción c le indica a tar que cree un nuevo archivo y la opción f es el nombre del archivo a crear. El argumento que sigue inmediatamente a las opciones siempre será el nombre del archivo para trabajar. El resto de los argumentos son las rutas a cualquier archivo o directorio que desee agregar, enumerar o extraer del archivo. En el ejemplo, estamos agregando la compresión del directorio y todos sus contenidos al archivo.

Para ver el contenido de un tar balls, use la opción t de tar:

	$ tar -tf 3.1.tar
	compression/
	compression/bigfile-xz1.xz
	compression/bigfile-gz9.gz
	compression/hosts.gz
	compression/bigfile2
	compression/bigfile
	compression/bigfile-gz1.gz
	compression/bigfile-xz9.xz
	compression/bigfile3
	compression/bigfile4

Observe cómo las opciones están precedidas por -. A diferencia de la mayoría de los programas, con tar, el - no se requiere cuando se especifican opciones, aunque no causa ningún daño si se usa.

Ahora vamos a extraer el archivo:

	$ cd ~/linux_essentials-3.1/archiving
	$ ls
	3.1.tar
	$ tar xf 3.1.tar
	$ ls
	3.1.tar  compression

Supongamos que solo necesita un archivo del archivo comprimido. Si este es el caso, puede especificarlo después del nombre del archivo. Puede especificar varios archivos si es necesario:

	$ cd ~/linux_essentials-3.1/archiving
	$ rm -rf compression
	$ ls
	3.1.tar
	$ tar xvf 3.1.tar compression/hosts.gz
	compression/
	compression/bigfile-xz1.xz
	compression/bigfile-gz9.gz
	compression/hosts.gz
	compression/bigfile2
	compression/bigfile
	compression/bigfile-gz1.gz
	compression/bigfile-xz9.xz
	compression/bigfile3
	compression/bigfile4
	$ ls
	3.1.tar  compression
	$ ls compression
	hosts.gz

Con la excepción de las rutas absolutas (rutas que comienzan con /), los archivos tar conservan la ruta completa a los archivos cuando se crean. Dado que el archivo 3.1.tar se creó con un solo directorio, ese directorio se creará en relación con su directorio de trabajo actual cuando se extraiga. Otro ejemplo debería aclarar esto:

	$ cd ~/linux_essentials-3.1/archiving
	$ rm -rf compression
	$ cd ../compression
	$ tar cf ../tar/3.1-nodir.tar *
	$ cd ../archiving
	$ mkdir untar
	$ cd untar
	$ tar -xf ../3.1-nodir.tar
	$ ls
	bigfile   bigfile3  bigfile-gz1.gz  bigfile-xz1.xz  hosts.gz
	bigfile2  bigfile4  bigfile-gz9.gz  bigfile-xz9.xz

El programa tar también puede gestionar la compresión y descompresión de archivos sobre la marcha. tar lo hace llamando a una de las herramientas de compresión discutidas anteriormente en esta sección. Es tan simple como agregar la opción adecuada al algoritmo de compresión. Los más utilizados son j, J y z para bzip2, xz y gzip, respectivamente. A continuación hay ejemplos que utilizan los algoritmos mencionados anteriormente:

	$ cd ~/linux_essentials-3.1/compression
	$ ls
	bigfile   bigfile3  bigfile-gz1.gz  bigfile-xz1.xz  hosts.gz
	bigfile2  bigfile4  bigfile-gz9.gz  bigfile-xz9.xz
	$ tar -czf gzip.tar.gz bigfile bigfile2 bigfile3
	$ tar -cjf bzip2.tar.bz2 bigfile bigfile2 bigfile3
	$ tar -cJf xz.tar.xz bigfile bigfile2 bigfile3
	$ ls -l | grep tar
	-rw-r--r-- 1 emma emma  450202 Jun 27 05:56 bzip2.tar.bz2
	-rw-r--r-- 1 emma emma  548656 Jun 27 05:55 gzip.tar.gz
	-rw-r--r-- 1 emma emma  147068 Jun 27 05:56 xz.tar.xz

Observe cómo en el ejemplo los archivos .tar tienen diferentes tamaños. Esto muestra que fueron comprimidos con éxito. Si crea archivos .tar comprimidos, siempre debe agregar una segunda extensión de archivo que denote el algoritmo que utilizó. Son .xz, .bz y .gz para xz, bzip2 y gzip, respectivamente. A veces se usan extensiones acortadas como .tgz.

Es posible agregar archivos a archivos tar sin comprimir ya existentes. Use la opción u para hacer esto. Si intenta agregar a un archivo comprimido, obtendrá un error.

	$ cd ~/linux_essentials-3.1/compression
	$ ls
	bigfile   bigfile3  bigfile-gz1.gz  bigfile-xz1.xz  bzip2.tar.bz2  hosts.gz
	bigfile2  bigfile4  bigfile-gz9.gz  bigfile-xz9.xz  gzip.tar.gz    xz.tar.xz
	$ tar cf plain.tar bigfile bigfile2 bigfile3
	$ tar tf plain.tar
	bigfile
	bigfile2
	bigfile3
	$ tar uf plain.tar bigfile4
	$ tar tf plain.tar
	bigfile
	bigfile2
	bigfile3
	bigfile4
	$ tar uzf gzip.tar.gz bigfile4
	tar: Cannot update compressed archives
	Try 'tar --help' or 'tar --usage' for more information.

## Administrar archivos ZIP
Las máquinas con Windows a menudo no tienen aplicaciones para manejar las "tar balls" o muchas de las herramientas de compresión que se encuentran comúnmente en los sistemas Linux. Si necesita interactuar con sistemas Windows, puede usar archivos ZIP. Un archivo ZIP es un archivo de almacenamiento similar a un archivo tar comprimido.

Los programas zip y unzip se pueden usar para trabajar con archivos ZIP en sistemas Linux. El siguiente ejemplo debería ser todo lo que necesita para comenzar a usarlos. Primero creamos un conjunto de archivos:

	$ cd ~/linux_essentials-3.1
	$ mkdir zip
	$ cd zip/
	$ mkdir dir
	$ touch dir/file1 dir/file2

Ahora usamos zip para empaquetar estos archivos en un archivo ZIP:

	$ zip -r zipfile.zip dir
	  adding: dir/ (stored 0%)
	  adding: dir/file1 (stored 0%)
	  adding: dir/file2 (stored 0%)
	$ rm -rf dir

Finalmente, desempaquetamos el archivo ZIP nuevamente:

	$ ls
	zipfile.zip
	$ unzip zipfile.zip
	Archive:  zipfile.zip
	   creating: dir/
	 extracting: dir/file1
	 extracting: dir/file2
	$ find
	.
	./zipfile.zip
	./dir
	./dir/file1
	./dir/file2

Al agregar directorios a archivos ZIP, la opción -r hace que zip incluya el contenido de un directorio. Sin él, tendría un directorio vacío en el archivo ZIP.