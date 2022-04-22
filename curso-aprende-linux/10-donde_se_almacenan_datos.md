# Donde se almacenan los datos

## Introducción

Para un sistema operativo, todo se considera datos. Para Linux, todo se considera un archivo: programas, archivos normales, directorios, dispositivos de bloque (discos duros, etc.), dispositivos de caracteres (consolas, etc.), procesos del núcleo, sockets, particiones, enlaces, etc. La estructura del directorio de Linux , comenzando desde la raíz /, es una colección de archivos que contienen datos. El hecho de que todo sea un archivo es una característica poderosa de Linux, ya que permite ajustar prácticamente todos los aspectos del sistema.



En esta lección discutiremos las diferentes ubicaciones en las que se almacenan datos importantes según lo establecido por el Estándar de jerarquía del sistema de archivos de Linux (FHS). Algunas de estas ubicaciones son directorios reales que almacenan datos de forma persistente en discos, mientras que otros son pseudo sistemas de archivos cargados en la memoria que nos dan acceso a datos del subsistema del núcleo, como procesos en ejecución, uso de memoria, configuración de hardware, etc. Los datos almacenados en estos directorios virtuales son utilizados por una serie de comandos que nos permiten monitorearlos y manejarlos.



## Programas y su configuración

Los datos importantes en un sistema Linux son, sin duda, sus programas y sus archivos de configuración. Los primeros son archivos ejecutables que contienen conjuntos de instrucciones que debe ejecutar el procesador de la computadora, mientras que los segundos suelen ser documentos de texto que controlan el funcionamiento de un programa. Los archivos ejecutables pueden ser archivos binarios o archivos de texto. Los archivos de texto ejecutables se denominan scripts. Los datos de configuración en Linux también se almacenan tradicionalmente en archivos de texto, aunque existen varios estilos de representación de datos de configuración.



## Dónde se almacenan los archivos binarios

Como cualquier otro archivo, los archivos ejecutables viven en directorios que cuelgan en última instancia de /. Más específicamente, los programas se distribuyen en una estructura de tres niveles: el primer nivel (/) incluye programas que pueden ser necesarios en modo de usuario único, el segundo nivel (/usr) contiene la mayoría de los programas multiusuario y el tercer nivel (/usr/local) se usa para almacenar software que no es proporcionado por la distribución y que se ha compilado localmente.



Las ubicaciones típicas para los programas incluyen:

- /sbin

Contiene archivos binarios esenciales para la administración del sistema, como parted o ip.

- /bin

Contiene archivos binarios esenciales para todos los usuarios, como ls, mv o mkdir.

- /usr/sbin

Almacena binarios para la administración del sistema, como deluser, o groupadd.

- /usr/bin

Incluye la mayoría de los archivos ejecutables, como free, pstree, sudo o man — que pueden ser utilizado por todos los usuarios.

- /usr/local/sbin

Se utiliza para almacenar programas instalados localmente para la administración del sistema que no son administrados por el administrador de paquetes del sistema.

- /usr/local/bin

Sirve para el mismo propósito que /usr/local/sbin pero para programas de usuario regulares.


Recientemente, algunas distribuciones comenzaron a reemplazar /bin y /sbin con enlaces simbólicos a /usr/bin and /usr/sbin.

Además de estos directorios, los usuarios habituales pueden tener sus propios programas en:

	/home/$USER/bin
	
	/home/$USER/.local/bin



Podemos encontrar la ubicación de los programas con el comando which:

	$ which git
	/usr/bin/git


## Dónde se almacenan los archivos de configuración
### El directorio /etc

En los primeros días de Unix había una carpeta para cada tipo de datos, como /bin para binarios y /boot para los núcleos. Sin embargo, /etc (es decir, etc.) se creó como un directorio general para almacenar los archivos que no pertenecían a las otras categorías. La mayoría de estos archivos eran archivos de configuración. Con el paso del tiempo, se agregaron más y más archivos de configuración, por lo que / etc se convirtió en la carpeta principal para los archivos de configuración de los programas. Como se dijo anteriormente, un archivo de configuración generalmente es un archivo de texto plano local (en oposición al binario) que controla la operación de un programa.



En /etc podemos encontrar diferentes patrones para los nombres de los archivos de configuración:

Archivos con una extensión ad hoc o sin extensión, por ejemplo

- group

Base de datos del grupo del sistema.

- hostname

Nombre de la computadora host.

- hosts

Lista de direcciones IP y sus traducciones de nombres de host.

- passwd

Base de datos del usuario del sistema: compuesta por siete campos separados por dos puntos que proporcionan información sobre el usuario.

- profile

Archivo de configuración de todo el sistema para Bash.

- shadow

Archivo encriptado para contraseñas de usuario.


Archivos de inicialización que terminan en rc:

- bash.bashrc

Archivo .bashrc en todo el sistema para shells bash interactivos.

- nanorc

Ejemplo de archivo de inicialización para GNU nano (un editor de texto simple que normalmente se envía con cualquier distribución).

Archivos que terminan en .conf:

- resolv.conf

Archivo de configuración para el solucionador, que proporciona acceso al Sistema de nombres de dominio de Internet (DNS).

- sysctl.conf
- Archivo de configuración para establecer variables del sistema para el núcleo.

### Directorios con el sufijo .d:

Algunos programas con un archivo de configuración único (* .conf o no) han evolucionado para tener un directorio dedicado * .d que ayuda a construir configuraciones modulares y más robustas. Por ejemplo, para configurar logrotate, encontrará logrotate.conf, pero también los directorios logrotate.d.

Este enfoque es útil en aquellos casos en que diferentes aplicaciones necesitan configuraciones para el mismo servicio específico. Si, por ejemplo, un paquete de servidor web contiene una configuración logrotate, esta configuración ahora se puede colocar en un archivo dedicado en el directorio logrotate.d. Este archivo puede ser actualizado por el paquete del servidor web sin interferir con la configuración de logrotate restante. Del mismo modo, los paquetes pueden agregar tareas específicas colocando archivos en el directorio /etc/cron.d en lugar de modificar / etc / crontab.

En Debian - y derivados de Debian - este enfoque se ha aplicado a la lista de fuentes confiables leídas por la herramienta de gestión de paquetes apt: aparte del clásico /etc/apt/sources.list, ahora encontramos el directorio /etc/apt/sources.list.d:

	$ ls /etc/apt/sources*
	/etc/apt/sources.list
	
	/etc/apt/sources.list.d:


### Archivos de configuración en HOME (archivos de puntos)

A nivel de usuario, los programas almacenan sus configuraciones y configuraciones en archivos ocultos en el directorio de inicio del usuario (también representado ~). Recuerde, los archivos ocultos comienzan con un punto (.), De ahí su nombre: archivos de puntos.



Algunos de estos archivos de puntos son scripts de Bash que personalizan la sesión de shell del usuario y se obtienen tan pronto como el usuario inicia sesión en el sistema:

	.bash_history

Almacena el historial de la línea de comando.

	.bash_logout

Incluye comandos para ejecutar al salir del shell de inicio de sesión.

	.bashrc

Script de inicialización de Bash para shells sin inicio de sesión.

	.profile

Script de inicialización de Bash para shells de inicio de sesión.



Los archivos de configuración de otros programas específicos del usuario se obtienen cuando se inician sus respectivos programas: .gitconfig, .emacs.d, .ssh, etc.



## El kernel de Linux

Antes de que se pueda ejecutar cualquier proceso, el núcleo debe cargarse en un área protegida de memoria. Después de eso, el proceso con PID 1 (más a menudo que systemd hoy en día) desencadena la cadena de procesos, es decir, un proceso inicia otro (s) y así sucesivamente. Una vez que los procesos están activos, el kernel de Linux se encarga de asignarles recursos (teclado, mouse, discos, memoria, interfaces de red, etc.).



### Dónde se almacenan los núcleos (Kernels): /boot
El núcleo reside en / boot, junto con otros archivos relacionados con el arranque. La mayoría de estos archivos incluyen los componentes del número de versión del núcleo en sus nombres (versión del núcleo, revisión mayor, revisión menor y número de parche).



El directorio /boot incluye los siguientes tipos de archivos, con nombres correspondientes a la versión del kernel correspondiente:

	config-4.9.0-9-amd64

Los ajustes de configuración para el núcleo, como las opciones y los módulos que se compilaron junto con el núcleo.

	initrd.img-4.9.0-9-amd64

Imagen de disco RAM inicial que ayuda en el proceso de inicio al cargar un sistema de archivos raíz temporal en la memoria.

	System-map-4.9.0-9-amd64

Mapas de dispositivos de los discos duros. Administra la memoria antes de cargar el kernel.

	vmlinuz-4.9.0-9-amd64

El núcleo adecuado en un formato comprimido autoextraíble que ahorra espacio (de ahí la z en vmlinuz; vm significa memoria virtual y comenzó a usarse cuando el núcleo obtuvo soporte para memoria virtual por primera vez).

	grub

Directorio de configuración para el gestor de arranque grub2.



## El directorio /proc 

El directorio / proc es uno de los llamados sistemas de archivos virtuales o pseudo ya que su contenido no se escribe en el disco, sino que se carga en la memoria. Se llena dinámicamente cada vez que la computadora se inicia y refleja constantemente el estado actual del sistema. / proc incluye información sobre:

- Procesos corriendo

- Configuración del núcleo

- Hardware del sistema

Además de todos los datos relacionados con los procesos que veremos en la próxima lección, este directorio también almacena archivos con información sobre el hardware del sistema y los ajustes de configuración del núcleo. Algunos de estos archivos incluyen:


	/proc/cpuinfo

Almacena información sobre la CPU del sistema:

	$ cat /proc/cpuinfo
	processor	: 0
	vendor_id	: GenuineIntel
	cpu family	: 6
	model		: 158
	model name	: Intel(R) Core(TM) i7-8700K CPU @ 3.70GHz
	stepping	: 10
	cpu MHz		: 3696.000
	cache size	: 12288 KB
	(...)
	


	/proc/cmdline

Almacena las cadenas pasadas al núcleo en el arranque:

	$ cat /proc/cmdline
	BOOT_IMAGE=/boot/vmlinuz-4.9.0-9-amd64 root=UUID=5216e1e4-ae0e-441f-b8f5-8061c0034c74 ro quiet



	/proc/modules

Muestra la lista de módulos cargados en el kernel:


	$ cat /proc/modules
	nls_utf8 16384 1 - Live 0xffffffffc0644000
	isofs 40960 1 - Live 0xffffffffc0635000
	udf 90112 0 - Live 0xffffffffc061e000
	crc_itu_t 16384 1 udf, Live 0xffffffffc04be000
	fuse 98304 3 - Live 0xffffffffc0605000
	vboxsf 45056 0 - Live 0xffffffffc05f9000 (O)
	joydev 20480 0 - Live 0xffffffffc056e000
	vboxguest 327680 5 vboxsf, Live 0xffffffffc05a8000 (O)
	hid_generic 16384 0 - Live 0xffffffffc0569000
	(...)


## El directorio /proc/sys
Este directorio incluye ajustes de configuración del núcleo en archivos clasificados en categorías por subdirectorio:

	$ ls /proc/sys
	abi  debug  dev  fs  kernel  net  user  vm

La mayoría de estos archivos actúan como un interruptor y, por lo tanto, solo contienen uno de los dos valores posibles: 0 o 1 ("activado" o "desactivado"). Por ejemplo:

	/proc/sys/net/ipv4/ip_forward

El valor que habilita o deshabilita nuestra máquina para que actúe como un enrutador (poder reenviar paquetes):

	$ cat /proc/sys/net/ipv4/ip_forward
	0


Sin embargo, hay algunas excepciones:

	/proc/sys/kernel/pid_max

El PID máximo permitido:

	$ cat /proc/sys/kernel/pid_max
	32768


## Dispositivos de hardware

Recuerde, en Linux "todo es un archivo". Esto implica que la información del dispositivo de hardware, así como los ajustes de configuración propios del núcleo, se almacenan en archivos especiales que residen en directorios virtuales.



### El directorio /dev

El directorio /dev del dispositivo contiene archivos de dispositivo (o nodos) para todos los dispositivos de hardware conectados. Estos archivos de dispositivo se utilizan como una interfaz entre los dispositivos y los procesos que los utilizan. Cada archivo de dispositivo se divide en una de dos categorías:

### Block devices
Son aquellos en los que los datos se leen y escriben en bloques que pueden abordarse individualmente. Los ejemplos incluyen discos duros (y sus particiones, como /dev/sda1), unidades flash USB, CD, DVD, etc.

### Character devices
Son aquellos en los que los datos se leen y escriben secuencialmente un carácter a la vez. Los ejemplos incluyen teclados, la consola de texto (/dev/console), puertos serie (como /dev/ttyS0, etc.), etc.

Al enumerar los archivos del dispositivo, asegúrese de usar ls con el modificador -l para diferenciar entre los dos. Podemos, por ejemplo, buscar discos duros y particiones:

	# ls -l /dev/sd*
	brw-rw---- 1 root disk 8, 0 may 25 17:02 /dev/sda
	brw-rw---- 1 root disk 8, 1 may 25 17:02 /dev/sda1
	brw-rw---- 1 root disk 8, 2 may 25 17:02 /dev/sda2
	(...)


O para terminales en serie (TeleTYpewriter):

	# ls -l /dev/tty*
	crw-rw-rw- 1 root tty     5,  0 may 25 17:26 /dev/tty
	crw--w---- 1 root tty     4,  0 may 25 17:26 /dev/tty0
	crw--w---- 1 root tty     4,  1 may 25 17:26 /dev/tty1
	(...)

Observe cómo el primer carácter es b para dispositivos de bloque y c para dispositivos de caracteres.
Además, /dev incluye algunos archivos especiales que son bastante útiles para diferentes propósitos de programación:

	/dev/zero

Proporciona tantos caracteres nulos como se solicite.

	/dev/null

Descarta toda la información que se le envía.

	/dev/urandom

Genera números pseudoaleatorios.



## El directorio /sys

El sistema de archivos sys (sysfs) está montado en /sys. Se introdujo con la llegada del kernel 2.6 y significó una gran mejora en /proc/sys.



Los procesos deben interactuar con los dispositivos en /dev y, por lo tanto, el núcleo necesita un directorio que contenga información sobre estos dispositivos de hardware. Este directorio es / sys y sus datos están ordenados en categorías. Por ejemplo, para verificar la dirección MAC de su tarjeta de red (enp0s3), debe buscar el siguiente archivo:

	$ cat /sys/class/net/enp0s3/address
	08:00:27:02:b2:74

### Memoria y tipos de memoria

Básicamente, para que un programa comience a ejecutarse, debe cargarse en la memoria. En general, cuando hablamos de memoria nos referimos a la memoria de acceso aleatorio (RAM) y, en comparación con los discos duros mecánicos, tiene la ventaja de ser mucho más rápida. En el lado negativo, es volátil (es decir, una vez que la computadora se apaga, los datos se van).



No obstante lo anterior, cuando se trata de memoria, podemos diferenciar dos tipos principales en un sistema Linux:



#### Physical memory
También conocido como RAM, viene en forma de chips formados por circuitos integrados que contienen millones de transistores y condensadores. Estos, a su vez, forman celdas de memoria (el componente básico de la memoria de la computadora). Cada una de estas celdas tiene un código hexadecimal asociado, una dirección de memoria, para que pueda ser referenciada cuando sea necesario.

#### Swap
También conocido como espacio de intercambio, es la porción de memoria virtual que vive en el disco duro y se usa cuando no hay más RAM disponible.

Por otro lado, existe el concepto de memoria virtual, que es una abstracción de la cantidad total de memoria de direccionamiento utilizable (RAM, pero también espacio en disco) tal como la ven las aplicaciones.

- free analiza /proc/meminfo y muestra la cantidad de memoria libre y usada en el sistema de una manera muy clara:


	$ free
	              total        used        free      shared  buff/cache   available
	Mem:        4050960     1474960     1482260       96900     1093740     2246372
	Swap:       4192252           0     4192252
	Expliquemos las diferentes columnas:

- total

Cantidad total de memoria física y de intercambio instalada.

- used

Cantidad de memoria física y de intercambio actualmente en uso.

- free

Cantidad de memoria física e intercambio actualmente no en uso.

- shared

Cantidad de memoria física utilizada, principalmente, por tmpfs.

- buff/cache

Cantidad de memoria física actualmente en uso por las memorias intermedias del kernel y la caché de página y  slabs.

- available

Estimación de cuánta memoria física está disponible para nuevos procesos.

Por defecto, free muestra valores en kibibytes, pero permite que una variedad de interruptores muestren sus resultados en diferentes unidades de medida. Algunas de estas opciones incluyen:

-b
Bytes.

-m
Mebibytes.

-g
Gibibytes.

-h
Formato legible por humanos.

-h siempre es cómodo de leer:


	$ free -h
	              total        used        free      shared  buff/cache   available
	Mem:           3,9G        1,4G        1,5G         75M        1,0G        2,2G
	Swap:          4,0G          0B        4,0G



Después de explorar los programas y sus archivos de configuración, en esta lección aprenderemos cómo se ejecutan los comandos como procesos. Del mismo modo, comentaremos sobre la mensajería del sistema, el uso del búfer de anillo del núcleo y cómo la llegada de systemd y su demonio de diario (journal daemon), journald, cambió la forma en que se habían hecho las cosas hasta ahora con respecto al registro del sistema.

## Procesos
Cada vez que un usuario emite un comando, se ejecuta un programa y se generan uno o más procesos.

Los procesos existen en una jerarquía. Después de que el núcleo se carga en la memoria durante el arranque, se inicia el primer proceso que, a su vez, inicia otros procesos que, nuevamente, pueden iniciar otros procesos. Cada proceso tiene un identificador único (PID) y un identificador de proceso primario (PPID). Estos son enteros positivos que se asignan en orden secuencial.

Explorando procesos dinámicamente: top
Puede obtener una lista dinámica de todos los procesos en ejecución con el comando superior:

	$ top
	
	top - 11:10:29 up  2:21,  1 user,  load average: 0,11, 0,20, 0,14
	Tasks:  73 total,   1 running,  72 sleeping,   0 stopped,   0 zombie
	%Cpu(s):  0,0 us,  0,3 sy,  0,0 ni, 99,7 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st
	KiB Mem :  1020332 total,   909492 free,    38796 used,    72044 buff/cache
	KiB Swap:  1046524 total,  1046524 free,        0 used.   873264 avail Mem
	
	   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
	   436 carol     20   0   42696   3624   3060 R  0,7  0,4   0:00.30 top
	     4 root      20   0       0      0      0 S  0,3  0,0   0:00.12 kworker/0:0
	   399 root      20   0   95204   6748   5780 S  0,3  0,7   0:00.22 sshd
	     1 root      20   0   56872   6596   5208 S  0,0  0,6   0:01.29 systemd
	     2 root      20   0       0      0      0 S  0,0  0,0   0:00.00 kthreadd
	     3 root      20   0       0      0      0 S  0,0  0,0   0:00.02 ksoftirqd/0
	     5 root       0 -20       0      0      0 S  0,0  0,0   0:00.00 kworker/0:0H
	     6 root      20   0       0      0      0 S  0,0  0,0   0:00.00 kworker/u2:0
	     7 root      20   0       0      0      0 S  0,0  0,0   0:00.08 rcu_sched
	     8 root      20   0       0      0      0 S  0,0  0,0   0:00.00 rcu_bh
	     9 root      rt   0       0      0      0 S  0,0  0,0   0:00.00 migration/0
	    10 root       0 -20       0      0      0 S  0,0  0,0   0:00.00 lru-add-drain


Como vimos anteriormente, top también puede brindarnos información sobre el consumo de memoria y CPU del sistema en general, así como para cada proceso.

top permite al usuario cierta interacción.

Por defecto, la salida se ordena por el porcentaje de tiempo de CPU utilizado por cada proceso en orden descendente. Este comportamiento puede modificarse presionando las siguientes teclas desde arriba:

- M

Ordenar por uso de memoria.

- N

Ordenar por número de identificación del proceso.

- T

Ordenar por tiempo de ejecución.

- P

Ordenar por porcentaje de uso de CPU.

Para cambiar entre orden descendente/ascendente solo presione R.


Una instantánea de los procesos: ps

Otro comando muy útil para obtener información sobre los procesos es ps. Mientras que top proporciona información dinámica, la de ps es estática.

Si se invoca sin opciones, la salida de ps es bastante discreta y se relaciona solo con los procesos adjuntos al shell actual:

	$ ps
	  PID TTY          TIME CMD
	 2318 pts/0    00:00:00 bash
	 2443 pts/0    00:00:00 ps


La información que se muestra tiene que ver con el identificador de proceso (PID), el terminal en el que se ejecuta el proceso (TTY), el tiempo de CPU tomado por el proceso (TIME) y el comando que inició el proceso (CMD).

Un modificador útil para ps es -f, que muestra la lista de formato completo:

	$ ps -f
	UID        PID  PPID  C STIME TTY          TIME CMD
	carol     2318  1682  0 08:38 pts/1    00:00:00 bash
	carol     2443  2318  0 08:46 pts/1    00:00:00 ps -f

En combinación con otros modificadores, -f muestra la relación entre los procesos padre e hijo:

	$ ps -uf
	USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
	carol       2318  0.0  0.1  21336  5140 pts/1    Ss   08:38   0:00 bash
	carol       2492  0.0  0.0  38304  3332 pts/1    R+   08:51   0:00  \_ ps -uf
	carol       1780  0.0  0.1  21440  5412 pts/0    Ss   08:28   0:00 bash
	carol       2291  0.0  0.7 305352 28736 pts/0    Sl+  08:35   0:00  \_ emacs index.en.adoc -nw
	(...)


Del mismo modo, ps puede mostrar el porcentaje de memoria utilizada cuando se invoca con el modificador -v:

	$ ps -v
	  PID TTY      STAT   TIME  MAJFL   TRS   DRS   RSS %MEM COMMAND
	 1163 tty2     Ssl+   0:00      1    67 201224 5576  0.1 /usr/lib/gdm3/gdm-x-session (...)
	 (...)


### Información del proceso en el directorio /proc

Ya hemos visto el sistema de archivos /proc. /proc incluye un subdirectorio numerado para cada proceso en ejecución en el sistema (el número es el PID del proceso):


	carol@debian:~# ls /proc
	1    108  13   17   21   27   354  41   665  8    9
	10   109  14   173  22   28   355  42   7    804  915
	103  11   140  18   23   29   356  428  749  810  918
	104  111  148  181  24   3    367  432  75   811
	105  112  149  19   244  349  370  433  768  83
	106  115  15   195  25   350  371  5    797  838
	107  12   16   2    26   353  404  507  798  899
	(...)


Por lo tanto, toda la información sobre un proceso particular se incluye dentro de su directorio. Hagamos una lista de los contenidos del primer proceso: aquel cuyo PID es 1 (el resultado se ha truncado para facilitar la lectura):


	# ls /proc/1/
	attr        cmdline          environ  io        mem	      ns
	autogroup   comm             exe      limits    mountinfo numa_maps
	auxv        coredump_filter  fd       loginuid  mounts    oom_adj
	...

Puede verificar, por ejemplo, el ejecutable del proceso:

	# cat /proc/1/cmdline; echo
	/sbin/init

Como puede ver, el binario que inició la jerarquía de procesos fue /sbin/init.

### La carga del sistema

Cada proceso en un sistema puede potencialmente consumir recursos del sistema. La llamada carga del sistema intenta agregar la carga general del sistema en un solo indicador numérico. Puede ver la carga actual de actividad con el  comando uptime:

	$ uptime
	 22:12:54 up 13 days, 20:26,  1 user,  load average: 2.91, 1.59, 0.39

Los tres últimos dígitos indican el promedio de carga del sistema para el último minuto (2.91), los últimos cinco minutos (1.59) y los últimos quince minutos (0.39), respectivamente.

Cada uno de estos números indica cuántos procesos estaban esperando los recursos de la CPU o las operaciones de entrada/salida para completar. Esto significa que estos procesos estaban listos para ejecutarse si hubieran recibido los recursos respectivos.

### Sistema de registro y mensajería del sistema

Tan pronto como el núcleo y los procesos comienzan a ejecutarse y comunicarse entre sí, se produce una gran cantidad de información. La mayor parte se envía a archivos: los llamados archivos de registro o, simplemente, los registros.

Sin iniciar sesión, la búsqueda de un evento que sucedió en un servidor daría mucho dolor de cabeza a los administradores de sistemas, de ahí la importancia de tener una forma estandarizada y centralizada de realizar un seguimiento de los eventos del sistema. Además, los registros son determinantes y reveladores cuando se trata de resolución de problemas y seguridad, así como fuentes de datos confiables para comprender las estadísticas del sistema y hacer predicciones de tendencias.

Iniciar sesión con el syslog Daemon

Tradicionalmente, los mensajes del sistema han sido gestionados por la instalación de registro estándar - syslog - o cualquiera de sus derivados - syslog-ng o rsyslog. El demonio de registro recopila mensajes de otros servicios y programas y los almacena en archivos de registro, generalmente en /var/log. Sin embargo, algunos servicios se encargan de sus propios registros (tome, por ejemplo, el servidor web Apache HTTPD). Del mismo modo, el kernel de Linux utiliza un búfer de anillo en memoria para almacenar sus mensajes de registro.

### Archivos de registro en /var/log
Debido a que los registros son datos que varían con el tiempo, normalmente se encuentran en /var/log.

Si exploras /var/log, te darás cuenta de que los nombres de los registros son, hasta cierto punto, bastante explicativos. Algunos ejemplos incluyen:

	/var/log/auth.log

Almacena información sobre la autenticación.

	/var/log/kern.log

Almacena información del núcleo.

	/var/log/syslog

Almacena información del sistema.

	/var/log/messages

Almacena datos del sistema y de aplicaciones.


### Acceso a archivos de registro

Cuando explore archivos de registro, recuerde ser root (si no tiene permisos de lectura) y use un localizador como less;

	# less /var/log/messages
	Jun  4 18:22:48 debian liblogging-stdlog:  [origin software="rsyslogd" swVersion="8.24.0" x-pid="285" x-info="http://www.rsyslog.com"] rsyslogd was HUPed
	Jun 29 16:57:10 debian kernel: [    0.000000] Linux version 4.9.0-8-amd64 (debian-kernel@lists.debian.org) (gcc version 6.3.0 20170516 (Debian 6.3.0-18+deb9u1) ) #1 SMP Debian 4.9.130-2 (2018-10-27)
	Jun 29 16:57:10 debian kernel: [    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-4.9.0-8-amd64 root=/dev/sda1 ro quiet


Alternativamente, puede usar tail con el modificador -f para leer los mensajes más recientes del archivo y mostrar dinámicamente nuevas líneas a medida que se agregan:

	# tail -f /var/log/messages
	Jul  9 18:39:37 debian kernel: [    2.350572] RAPL PMU: hw unit of domain psys 2^-0 Joules
	Jul  9 18:39:37 debian kernel: [    2.512802] input: VirtualBox USB Tablet as /devices/pci0000:00/0000:00:06.0/usb1/1-1/1-1:1.0/0003:80EE:0021.0001/input/input7
	Jul  9 18:39:37 debian kernel: [    2.513861] Adding 1046524k swap on /dev/sda5.  Priority:-1 extents:1 across:1046524k FS
	Jul  9 18:39:37 debian kernel: [    2.519301] hid-generic 0003:80EE:0021.0001: input,hidraw0: USB HID v1.10 Mouse [VirtualBox USB Tablet] on usb-0000:00:06.0-1/input0
	Jul  9 18:39:37 debian kernel: [    2.623947] snd_intel8x0 0000:00:05.0: white list rate for 1028:0177 is 48000
	Jul  9 18:39:37 debian kernel: [    2.914805] IPv6: ADDRCONF(NETDEV_UP): enp0s3: link is not ready
	Jul  9 18:39:39 debian kernel: [    4.937283] e1000: enp0s3 NIC Link is Up 1000 Mbps Full Duplex, Flow Control: RX
	Jul  9 18:39:39 debian kernel: [    4.938493] IPv6: ADDRCONF(NETDEV_CHANGE): enp0s3: link becomes ready
	Jul  9 18:39:40 debian kernel: [    5.315603] random: crng init done
	Jul  9 18:39:40 debian kernel: [    5.315608] random: 7 urandom warning(s) missed due to ratelimiting

Encontrará la salida en el siguiente formato:

- Marca de tiempo (Timestamp)
- Nombre de host del que proviene el mensaje
- Nombre del programa/servicio que generó el mensaje.
- El PID del programa que generó el mensaje.
- Descripción de la acción que tuvo lugar.

La mayoría de los archivos de registro están escritos en texto plano; sin embargo, algunos pueden contener datos binarios, como es el caso de / var / log / wtmp, que almacena datos relevantes para inicios de sesión exitosos. Puede usar el comando de archivo para determinar cuál es el caso:


	$ file /var/log/wtmp
	/var/log/wtmp: dBase III DBT, version number 0, next free block index 8

Estos archivos se leen normalmente mediante comandos especiales. last se usa para interpretar los datos en /var/log/wtmp:


	$ last
	carol    tty2         :0               Thu May 30 10:53   still logged in
	reboot   system boot  4.9.0-9-amd64    Thu May 30 10:52   still running
	carol    tty2         :0               Thu May 30 10:47 - crash  (00:05)
	reboot   system boot  4.9.0-9-amd64    Thu May 30 09:11   still running
	carol    tty2         :0               Tue May 28 08:28 - 14:11  (05:42)
	reboot   system boot  4.9.0-9-amd64    Tue May 28 08:27 - 14:11  (05:43)
	carol    tty2         :0               Mon May 27 19:40 - 19:52  (00:11)
	reboot   system boot  4.9.0-9-amd64    Mon May 27 19:38 - 19:52  (00:13)
	carol    tty2         :0               Mon May 27 19:35 - down   (00:03)
	reboot   system boot  4.9.0-9-amd64    Mon May 27 19:34 - 19:38  (00:04)

### Rotación de registro
Los archivos de registro pueden crecer mucho en unas pocas semanas o meses y ocupar todo el espacio libre en disco. Para abordar esto, se utiliza la utilidad logrotate. Implementa la rotación de registros o ciclos que implica acciones como mover archivos de registro a un nuevo nombre, archivarlos y/o comprimirlos, a veces enviándolos por correo electrónico al administrador del sistema y eventualmente eliminándolos a medida que envejecen. Las convenciones utilizadas para nombrar estos archivos de registro rotados son diversas (agregando un sufijo con la fecha, por ejemplo); sin embargo, simplemente agregar un sufijo con un número entero es común:

	# ls /var/log/apache2/
	access.log  error.log  error.log.1  error.log.2.gz  other_vhosts_access.log

Tenga en cuenta que error.log.2.gz ya se ha comprimido con gunzip (de ahí el sufijo .gz).

### El búfer de anillo del núcleo

El búfer de anillo del núcleo es una estructura de datos de tamaño fijo que registra los mensajes de arranque del núcleo. La función de este búfer, muy importante, es registrar todos los mensajes del núcleo producidos en el arranque, cuando syslog aún no está disponible. El comando dmesg imprime el búfer de anillo del núcleo (que también solía almacenarse en /var/log/dmesg). Debido a la extensión del búfer en anillo, este comando normalmente se usa en combinación con la herramienta de filtrado de texto grep (recuerde, también debe ser root). Por ejemplo, para buscar mensajes de arranque:

	$ dmesg | grep boot
	[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-4.9.0-9-amd64 root=UUID=5216e1e4-ae0e-441f-b8f5-8061c0034c74 ro quiet
	[    0.000000] smpboot: Allowing 1 CPUs, 0 hotplug CPUs
	[    0.000000] Kernel command line: BOOT_IMAGE=/boot/vmlinuz-4.9.0-9-amd64 root=UUID=5216e1e4-ae0e-441f-b8f5-8061c0034c74 ro quiet
	[    0.144986] AppArmor: AppArmor disabled by boot time parameter
	(...)


### El diario del sistema: systemd-journald

A partir de 2015, systemd reemplazó a SysV Init como administrador de sistemas y servicios de facto en la mayoría de las principales distribuciones de Linux. Como consecuencia, el daemon journal - journald - se ha convertido en el componente de registro estándar, reemplazando a syslog en la mayoría de los aspectos. Los datos ya no se almacenan en texto plano sino en forma binaria. Por lo tanto, la utilidad journalctl es necesaria para leer los registros. Además de eso, journald es compatible con syslog y puede integrarse con syslog.

journalctl es la utilidad que debe usar para consultar el diario del sistema. Tienes que ser root o usar sudo para invocarlo. Si se invoca sin opciones, imprime el diario completo:

	# journalctl
	-- Logs begin at Tue 2019-06-04 17:49:40 CEST, end at Tue 2019-06-04 18:13:10 CEST. --
	jun 04 17:49:40 debian systemd-journald[339]: Runtime journal (/run/log/journal/) is 8.0M, max 159.6M, 151.6M free.
	jun 04 17:49:40 debian kernel: microcode: microcode updated early to revision 0xcc, date = 2019-04-01
	Jun 04 17:49:40 debian kernel: Linux version 4.9.0-8-amd64 (debian-kernel@lists.debian.org) (gcc version 6.3.0 20170516 (Debian 6.3.0-18+deb9u1) )
	Jun 04 17:49:40 debian kernel: Command line: BOOT_IMAGE=/boot/vmlinuz-4.9.0-8-amd64 root=/dev/sda1 ro quiet
	(...)

Sin embargo, si se invoca con los modificadores -k o --dmesg, será equivalente a usar el comando dmesg:

	# journalctl -k
	[    0.000000] Linux version 4.9.0-9-amd64 (debian-kernel@lists.debian.org) (gcc version 6.3.0 20170516 (Debian 6.3.0-18+deb9u1) ) #1 SMP Debian 4.9.168-1+deb9u2 (2019-05-13)
	[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-4.9.0-9-amd64 root=UUID=5216e1e4-ae0e-441f-b8f5-8061c0034c74 ro quiet
	(...)

Otras opciones interesantes para journalctl:

	-b, --boot

Muestra información de arranque.

	-u

Muestra mensajes sobre una unidad especificada. Aproximadamente, una unidad se puede definir como cualquier recurso manejado por systemd. Por ejemplo journalctl -u apache2.service se usa para leer mensajes sobre el servidor web  apache2.

	-f

Muestra los mensajes de diario (journal) más recientes y sigue imprimiendo nuevas entradas a medida que se agregan al diario (journal), al igual que tail -f.

