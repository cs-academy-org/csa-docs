# Seguridad e identificación de tipos de usuario

Esta lección se centrará en la terminología básica de las cuentas, los controles de acceso y la seguridad de los sistemas Linux locales, las herramientas de la interfaz de línea de comandos (CLI) en un sistema Linux para controles básicos de acceso de seguridad y los archivos básicos para admitir cuentas de usuarios y grupos, incluidos los utilizados para la escalada de privilegios.



La seguridad básica en los sistemas Linux se basa en los controles de acceso de Unix que, a pesar de tener casi cincuenta años, son bastante efectivos en comparación con algunos sistemas operativos de consumo populares de un linaje mucho más nuevo. Incluso algunos otros sistemas operativos populares basados ​​en Unix tienden a "tomar libertades" que se centran en la "facilidad de acceso", mientras que Linux no.



Los entornos e interfaces de escritorio modernos de Linux simplifican la creación y administración de usuarios y, a menudo, automatizan la asignación de controles de acceso cuando un usuario inicia sesión, por ejemplo, en la pantalla, audio y otros servicios, y no requiere prácticamente intervención manual del administrador del sistema. Sin embargo, es importante comprender los conceptos básicos de un sistema operativo Linux subyacente.



## Cuentas
La seguridad implica muchos conceptos, uno de los más comunes es el concepto general de controles de acceso. Antes de poder abordar los controles de acceso a los archivos, como la propiedad y los permisos, se deben comprender los conceptos básicos de las cuentas de usuario de Linux, que se dividen en varios tipos.



Cada usuario en un sistema Linux tiene una cuenta asociada que además de la información de inicio de sesión (como nombre de usuario y contraseña) también define cómo y dónde puede interactuar el usuario con el sistema. Los privilegios y los controles de acceso definen los "límites" dentro de los cuales puede operar cada usuario.



## Identificadores (UIDs/GIDs)
Los identificadores de usuario y grupo (UIDs/GIDs) son las referencias básicas y enumeradas a las cuentas. Las primeras implementaciones eran enteros limitados de 16 bits (valores de 0 a 65535), pero los sistemas del siglo XXI admiten UID y GID de 64 bits. Los usuarios y grupos se enumeran de forma independiente, por lo que la misma ID puede representar tanto un usuario como un grupo.



Cada usuario tiene no solo un UID, sino también un GID primario. El GID primario para un usuario puede ser exclusivo de ese usuario solo y puede terminar sin ser utilizado por ningún otro usuario. Sin embargo, este grupo también podría ser un grupo compartido por numerosos usuarios. Además de estos grupos principales, cada usuario también puede ser miembro de otros grupos.



Por defecto en los sistemas Linux, cada usuario está asignado a un grupo con el mismo nombre que su nombre de usuario y el mismo GID que su UID. Por ejemplo, cree un nuevo usuario llamado newuser y, de manera predeterminada, su grupo predeterminado también es newuser.



## La cuenta de superusuario
En Linux, la cuenta de superusuario es root, que siempre tiene UID 0. El superusuario a veces se llama administrador del sistema y tiene acceso y control ilimitados sobre el sistema, incluidos otros usuarios.



El grupo predeterminado para el superusuario tiene el GID 0 y también se denomina raíz. El directorio de inicio para el superusuario es un directorio dedicado de nivel superior, /root, al que solo puede acceder el usuario root.



## Cuentas de usuario estándar
Todas las cuentas que no sean root son cuentas de usuario técnicamente regulares, pero en un sistema Linux el término de cuenta de usuario coloquial a menudo significa una cuenta de usuario "regular" (sin privilegios). Por lo general, tienen las siguientes propiedades, con algunas excepciones:



UID que comienzan en 1000 (4 dígitos), aunque algunos sistemas heredados pueden comenzar en 500.


Un directorio de inicio definido, generalmente un subdirectorio de /home, dependiendo de la configuración local del sitio.


Un shell de inicio de sesión definido. En Linux, el shell predeterminado suele ser Bourne Again Shell (/bin/bash), aunque puede haber otros disponibles.


Si una cuenta de usuario no tiene un shell válido en sus atributos, el usuario no podrá abrir un shell interactivo. Por lo general, /sbin/nologin se usa como un shell no válido. Esto puede ser útil, si el usuario solo se autenticará para otros servicios que no sean la consola o el acceso SSH, por ejemplo, solo acceso FTP seguro (sftp).



Nota: Para evitar confusiones, el término cuenta de usuario solo se aplicará a las cuentas de usuario estándar o regulares en adelante. Por ejemplo, la cuenta del sistema se usará para explicar una cuenta de usuario de Linux que sea del tipo de cuenta de usuario del sistema.
Cuentas del sistema
Las cuentas del sistema generalmente se crean previamente en el momento de la instalación del sistema. Estos son para instalaciones, programas y servicios que no se ejecutarán como superusuario. En un mundo ideal, todas estas serían instalaciones del sistema operativo.



Las cuentas del sistema varían, pero sus atributos incluyen:

Los UID suelen ser inferiores a 100 (2 dígitos) o 500-1000 (3 dígitos).
No hay un directorio de inicio dedicado o un directorio que generalmente no está en /home.
Sin shell de inicio de sesión válido (normalmente /sbin/nologin), con raras excepciones.


La mayoría de las cuentas del sistema en Linux nunca iniciarán sesión y no necesitan un shell definido en sus atributos. Muchos procesos de propiedad y ejecutados por las cuentas del sistema se bifurcan en su propio entorno por la administración del sistema, que se ejecuta con la cuenta del sistema especificada. Estas cuentas generalmente tienen privilegios limitados o, la mayoría de las veces, no tienen privilegios.

Nota: Desde el punto de vista de LPI Linux Essentials, las cuentas del sistema son UID <1000, con UID de 2 o 3 dígitos (y GID).
En general, las cuentas del sistema no deben tener un shell de inicio de sesión válido. Lo contrario sería un problema de seguridad en la mayoría de los casos.



## Cuentas de servicio
Las cuentas de servicio generalmente se crean cuando los servicios se instalan y configuran. Al igual que las cuentas del sistema, son para instalaciones, programas y servicios que no se ejecutarán como superusuario.



En mucha documentación, las cuentas de sistema y servicio son similares y se intercambian a menudo. Esto incluye la ubicación de los directorios de inicio que normalmente están fuera de /home, si se define (las cuentas de servicio a menudo tienen más probabilidades de tener uno, en comparación con las cuentas del sistema), y no hay un shell de inicio de sesión válido. Aunque no existe una definición estricta, la diferencia principal entre las cuentas de sistema y servicio se desglosa en UID/GID.



## Cuenta del sistema

UID/GID <100 (2 dígitos) o <500-1000 (3 dígitos)


## Cuenta de servicio

UID / GID> 1000 (4+ dígitos), pero no una cuenta de usuario "estándar" o "regular", una cuenta para servicios, con un UID/GID> 1000 (4+ dígitos)


Algunas distribuciones de Linux todavía tienen cuentas de servicio reservadas previamente con UID <100, y esas también podrían considerarse una cuenta del sistema, a pesar de que no se crean en la instalación del sistema. Por ejemplo, en las distribuciones de Linux basadas en Fedora (incluido Red Hat), el usuario del servidor web Apache tiene UID (y GID) 48, claramente una cuenta del sistema, a pesar de tener un directorio de inicio (generalmente en /usr/share/httpd o /var/www/html/).



Nota: Desde el punto de vista de LPI Linux Essentials, las cuentas del sistema son UID <1000, y las cuentas de usuario normales son UID> 1000. Como las cuentas de usuario normales son> 1000, estos UID también pueden incluir cuentas de servicio.
Shells de inicio de sesión y directorios principales
Algunas cuentas tienen un shell de inicio de sesión, mientras que otras no tienen fines de seguridad, ya que no requieren acceso interactivo. El shell de inicio de sesión predeterminado en la mayoría de las distribuciones de Linux es Bourne Again Shell, o bash, pero puede haber otros shells disponibles, como C Shell (csh), Korn shell (ksh) o Z shell (zsh), por nombrar algunos.



Un usuario puede cambiar su shell de inicio de sesión utilizando el comando chsh. Por defecto, el comando se ejecuta en modo interactivo y muestra un mensaje preguntando qué shell se debe usar. La respuesta debería ser la ruta completa al binario de shell, como a continuación:


	$ chsh

	Changing the login shell for emma
	Enter the new value, or press ENTER for the default
		Login Shell [/bin/bash]: /usr/bin/zsh



También puede ejecutar el comando en modo no interactivo, con el parámetro -s seguido de la ruta al binario, así:

	$ chsh -s /usr/bin/zsh

La mayoría de las cuentas tienen un directorio de inicio definido. En Linux, esta suele ser la única ubicación donde esa cuenta de usuario tiene acceso de escritura garantizado, con algunas excepciones (por ejemplo, áreas del sistema de archivos temporales). Sin embargo, algunas cuentas se configuran a propósito para que no tengan acceso de escritura incluso a su propio directorio de inicio, por razones de seguridad.



## Obteniendo información sobre sus usuarios
Enumerar la información básica del usuario es una práctica común y cotidiana en un sistema Linux. En algunos casos, los usuarios deberán cambiar de usuario y aumentar los privilegios para completar tareas privilegiadas.



Incluso los usuarios tienen la capacidad de enumerar atributos y acceder desde la línea de comandos, utilizando los comandos a continuación. La información básica en un contexto limitado no es una operación privilegiada.



Listar la información actual de un usuario en la línea de comando es tan simple como un comando de dos letras, id. La salida variará según su ID de inicio de sesión:

	$ id
	uid=1024(emma) gid=1024(emma) 1024(emma),20(games),groups=10240(netusers),20480(netadmin) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

En la lista anterior, el usuario (emma) tiene identificadores que se desglosan de la siguiente manera:

- 1024 = ID de usuario (UID), seguido del nombre de usuario (nombre común, también conocido como nombre de inicio de sesión) entre paréntesis.
- 1024 = el ID de grupo primario (GID), seguido del nombre de grupo (nombre común) entre paréntesis.
- Una lista de GID adicionales (nombres de grupo) a los que también pertenece el usuario.


Enumerar la última vez que los usuarios han iniciado sesión en el sistema se realiza con el comando last:

	$ last
	emma     pts/3        ::1              Fri Jun 14 04:28   still logged in
	reboot   system boot  5.0.17-300.fc30. Fri Jun 14 04:03   still running
	reboot   system boot  5.0.17-300.fc30. Wed Jun  5 14:32 - 15:19  (00:46)
	reboot   system boot  5.0.17-300.fc30. Sat May 25 18:27 - 19:11  (00:43)
	reboot   system boot  5.0.16-100.fc28. Sat May 25 16:44 - 17:06  (00:21)
	reboot   system boot  5.0.9-100.fc28.x Sun May 12 14:32 - 14:46  (00:14)
	root     tty2                          Fri May 10 21:55 - 21:55  (00:00)
		...

La información que figura en las columnas puede variar, pero algunas entradas notables en el listado anterior son:

Un usuario (emma) inició sesión a través de la red (pseudo TTY pts/3) y todavía está conectado.
Se enumera el momento del arranque actual, junto con el núcleo. En el ejemplo anterior, unos 25 minutos antes de que el usuario inicie sesión.
El superusuario (root) inició sesión a través de una consola virtual (TTY tty2), brevemente, a mediados de mayo.
Una variante del último comando es el comando lastb, que enumera todos los últimos intentos de inicio de sesión incorrectos.

Los comandos who y w enumeran solo los inicios de sesión activos en el sistema:

	$ who
	emma  pts/3        2019-06-14 04:28 (::1)


	$ w
	 05:43:41 up  1:40,  1 user,  load average: 0.25, 0.53, 0.51
	USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
	emma  pts/3     04:28    1:14m  0.04s  0.04s -bash


Ambos comandos enumeran parte de la misma información. Por ejemplo, un usuario (emma) ha iniciado sesión con un dispositivo pseudo TTY (pts / 3) y la hora de inicio de sesión es 04:28.



El comando w muestra más información, incluida la siguiente:

La hora actual y cuánto tiempo ha estado funcionando el sistema
¿Cuántos usuarios están conectados?
Los promedios de carga de los últimos 1, 5 y 15 minutos.


Y la información adicional para cada sesión de usuario activa.

Seleccionar, tiempos totales de utilización de CPU (IDLE, JCPU y PCPU)
El proceso actual (-bash). El tiempo total de utilización de la CPU de ese proceso es el último elemento (PCPU).


Ambos comandos tienen más opciones para enumerar diversa información adicional.



## Cambio de usuarios y aumento de privilegios
En un mundo ideal, los usuarios nunca necesitarían escalar privilegios para completar sus tareas. El sistema siempre "simplemente funcionaría" y todo estaría configurado para varios accesos.



Afortunadamente para nosotros, Linux - out-of-the-box - funciona así para la mayoría de los usuarios que no son administradores del sistema, a pesar de seguir siempre el modelo de seguridad con menos privilegios.



Sin embargo, hay comandos que permiten la escalada de privilegios cuando es necesario. Dos de los más importantes son su y sudo.



En la mayoría de los sistemas Linux actuales, el comando su solo se usa para escalar privilegios a root, que es el usuario predeterminado si no se especifica un nombre de usuario después del nombre del comando. Si bien se puede usar para cambiar a otro usuario, no es una buena práctica: los usuarios deben iniciar sesión desde otro sistema, a través de la red o consola física o terminal en el sistema.

	emma ~$ su -
	Password:
	root ~#

Después de ingresar la contraseña de superusuario (root), el usuario tiene un shell de superusuario (observe el # al final del símbolo del sistema) y es, para todos los efectos, el superusuario (root).



Compartir contraseñas es una práctica de seguridad muy mala, por lo que debería haber muy pocas, si es que hay alguna, necesidad del comando su en un sistema Linux moderno.



El símbolo de dólar ($) debe terminar el indicador de línea de comando para un shell de usuario no privilegiado, mientras que el símbolo de libra (#) debe finalizar el indicador de línea de comando para el indicador de shell de superusuario (root). Se recomienda encarecidamente que el carácter final de cualquier solicitud nunca se cambie de este estándar "universalmente entendido", ya que esta nomenclatura se utiliza en materiales de aprendizaje, incluidos estos.

Nota: Nunca cambie al superusuario (root) sin pasar el parámetro de inicio de sesión (-). A menos que el proveedor de SO o software indique explícitamente lo contrario cuando se requiere su, ejecute siempre su - con excepciones extremadamente limitadas. Los entornos de usuario pueden causar cambios y problemas de configuración no deseados cuando se usan en modo de privilegio completo como superusuario.
¿Cuál es el mayor problema con el uso de su para cambiar al superusuario (root)? Si la sesión de un usuario normal se ha visto comprometida, se podría capturar la contraseña de superusuario (root). Ahí es donde entra en juego "Switch User Do" (o "Superuser Do"):

	$ cat /sys/devices/virtual/dmi/id/board_serial
	cat: /sys/devices/virtual/dmi/id/board_serial: Permission denied


	$ sudo cat /sys/devices/virtual/dmi/id/board_serial
	[sudo] password for emma:
	/6789ABC/

En la lista anterior, el usuario está intentando buscar el número de serie de su placa del sistema. Sin embargo, el permiso es denegado, ya que esa información está marcada como privilegiada.



Sin embargo, al usar sudo, el usuario ingresa su propia contraseña para autenticar quién es. Si ha sido autorizado en la configuración de sudoers para ejecutar ese comando con privilegios, con las opciones permitidas, funcionará.

Nota: Por defecto, el primer comando sudo autorizado autenticará los siguientes comandos sudo por un período de tiempo (muy corto). Esto es configurable por el administrador del sistema.
Archivos de control de acceso
Casi todos los sistemas operativos tienen un conjunto de lugares utilizados para almacenar controles de acceso. En Linux, estos son típicamente archivos de texto ubicados en el directorio /etc, que es donde deben almacenarse los archivos de configuración del sistema. Por defecto, todos los usuarios del sistema pueden leer este directorio, pero solo el usuario root puede escribirlo.



Los archivos principales relacionados con cuentas de usuario, atributos y control de acceso son:

- /etc/passwd

Este archivo almacena información básica sobre los usuarios en el sistema, incluidos UID y GID, directorio de inicio, shell, etc. A pesar del nombre, aquí no se almacenan contraseñas.


- /etc/group

Este archivo almacena información básica sobre todos los grupos de usuarios en el sistema, como el nombre del grupo y GID y miembros.


- /etc/shadow

Aquí es donde se almacenan las contraseñas de los usuarios. Son hash, por seguridad.


- /etc/gshadow

Este archivo almacena información más detallada sobre los grupos, incluida una contraseña cifrada que permite a los usuarios convertirse temporalmente en un miembro del grupo, una lista de usuarios que pueden convertirse en miembros del grupo en ese momento y una lista de administradores de grupo.




Cuidado: Estos archivos no están diseñados y nunca deben editarse directamente. Esta lección solo cubre la información almacenada en estos archivos y no edita estos archivos.
Por defecto, todos los usuarios pueden ingresar /etc y leer los archivos /etc/passwd y /etc/group. Y también, por defecto, ningún usuario, excepto root, puede leer los archivos /etc/shadow o /etc/gshadow.



También hay archivos relacionados con la escalada de privilegios básicos en sistemas Linux, como en los comandos su y sudo. Por defecto, estos solo son accesibles para el usuario root.



- /etc/sudoers

Este archivo controla quién puede usar el comando sudo y cómo.


- /etc/sudoers.d

Este directorio puede contener archivos que complementan la configuración en el archivo sudoers.


Desde el punto de vista del examen LPI Linux Essentials, solo conozca la ruta y el nombre de archivo del archivo de configuración de sudo predeterminado, /etc/sudoers. Su configuración está más allá del alcance de estos materiales.



Cuidado: Aunque /etc/sudoers es un archivo de texto, nunca debe editarse directamente. Si se necesitan cambios en su contenido, se deben hacer usando la utilidad visudo.


## /etc/passwd
El archivo /etc/passwd se conoce comúnmente como el "archivo de contraseña". Cada línea contiene múltiples campos siempre delimitados por dos puntos (smile. A pesar del nombre, el hash de contraseña unidireccional real actualmente no se almacena en este archivo.



La sintaxis típica para una línea en este archivo es:

	USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL

Donde:
- USERNAME
El nombre de usuario aka login (nombre), como root, nobody, emma.
- PASSWORD
Ubicación heredada del hash de contraseña. Casi siempre x, lo que indica que la contraseña está almacenada en el archivo /etc/shadow.
- UID
ID de usuario (UID), como 0, 99, 1024.
- GID
Default Group ID (GID), like 0, 99, 1024.
- GECOS
Una lista CSV de información del usuario que incluye nombre, ubicación, número de teléfono. Por ejemplo: Emma Smith, 42 Douglas St, 555.555.5555
- HOMEDIR
Ruta al directorio de inicio del usuario, como /root, /home/emma, etc.
- SHELL
El shell predeterminado para este usuario, como /bin/bash, /sbin/nologin, /bin/ksh, etc..

Por ejemplo, la siguiente línea describe el usuario emma:

	emma:x:1000:1000:Emma Smith,42 Douglas St,555.555.5555:/home/emma:/bin/bash


Comprender el campo GECOS
El campo GECOS contiene tres (3) o más campos, delimitados por una coma (,), también conocida como una lista de valores separados por comas (CSV). Aunque no existe un estándar obligatorio, los campos suelen estar en el siguiente orden:

NAME,LOCATION,CONTACT
Dónde:

- NOMBRE

es el "Nombre completo" del usuario, o el "Nombre del software" en el caso de una cuenta de servicio.


- UBICACIÓN

suele ser la ubicación física del usuario dentro de un edificio, número de habitación o el departamento de contacto o persona en el caso de una cuenta de servicio.


- CONTACTO

enumera información de contacto, como el número de teléfono del hogar o del trabajo.


Los campos adicionales pueden incluir información de contacto adicional, como un número de casa o una dirección de correo electrónico. Para cambiar la información en el campo GECOS, use el comando chfn y responda las preguntas, como a continuación. Si no se proporciona un nombre de usuario después del nombre del comando, cambiará la información para el usuario actual:

	$ chfn

	Changing the user information for emma
	Enter the new value, or press ENTER for the default
		Full Name: Emma Smith
		Room Number []: 42
		Work Phone []: 555.555.5555
		Home Phone []: 555.555.6666


## /etc/group
El archivo /etc/group contiene campos siempre delimitados por dos puntos (smile, que almacenan información básica sobre los grupos en el sistema. A veces se le llama el "archivo de grupo". La sintaxis para cada línea es:

	NAME:PASSWORD:GID:MEMBERS

Dónde:

- NAME

es el nombre del grupo, como root, usuarios, emma, etc.


- PASSWORD

ubicación heredada de un hash de contraseña de grupo opcional. Casi siempre x, lo que indica que la contraseña (si está definida) se almacena en el archivo / etc / gshadow.


- GID

ID de grupo (GID), como 0, 99, 1024.


- MEMBERS

una lista de nombres de usuario separados por comas que son miembros del grupo, como jsmith, emma.


El siguiente ejemplo muestra una línea que contiene información sobre el grupo adm (administradores del sistema):

	students:x:1023:jsmith,emma



No es necesario que el usuario aparezca en el campo de miembros cuando el grupo es el grupo primario para un usuario. Si un usuario está en la lista, entonces es redundante, es decir, no hay cambio en la funcionalidad, listada o no.



Nota: El uso de contraseñas para grupos está más allá del alcance de esta sección, sin embargo, si se define, el hash de la contraseña se almacena en el archivo /etc/gshadow. Esto también está más allá del alcance de esta sección.


## /etc/shadow
La siguiente tabla enumera los atributos almacenados en el archivo /etc/shadow, comúnmente conocido como el "archivo shadow". El archivo contiene campos siempre delimitados por dos puntos (smile. Aunque el archivo tiene muchos campos, la mayoría están más allá del alcance de esta lección, aparte de los dos primeros.



La sintaxis básica para una línea en este archivo es:

	USERNAME:PASSWORD:LASTCHANGE:MINAGE:MAXAGE:WARN:INACTIVE:EXPDATE

Donde:

- USERNAME
El nombre de usuario (igual que en /etc/passwd), como root, nobody, emma.

- PASSWORD
Un hash unidireccional de la contraseña, incluido salt anterior. Por ejemplo: !!, !$1$01234567$ABC…​, $6$012345789ABCDEF$012…​.

- LASTCHANGE
Fecha del último cambio de contraseña en días desde la "época", como 17909.

- MINAGE
Edad mínima de contraseña en días.

- MAXAGE
Contraseña máxima en días.

- WARN
Período de advertencia antes de que caduque la contraseña, en días.

- INACTIVE
Antigüedad máxima de la contraseña después del vencimiento, en días.

- EXPDATE
Fecha de caducidad de la contraseña, en días desde la "época".

En el siguiente ejemplo, puede ver una  del archivo /etc/shadow . Tenga en cuenta que algunos valores, como INACTIVE y EXPDATE son indefinidos

	emma:$6$nP532JDDogQYZF8I$bjFNh9eT1xpb9/n6pmjlIwgu7hGjH/eytSdttbmVv0MlyTMFgBIXESFNUmTo9EGxxH1OT1HGQzR0so4n1npbE0:18064:0:99999:7:::

La "época" de un sistema POSIX es la medianoche (0000), hora universal coordinada (UTC), el jueves 1 de enero de 1970. La mayoría de las fechas y horas POSIX están en segundos desde "época", o en el caso del archivo /etc/shadow, días desde "época".



Importante:

El archivo shadow está diseñado para que solo el superusuario pueda leerlo, y seleccione los servicios de autenticación del sistema central que verifican el hash de contraseña unidireccional al iniciar sesión u otro tiempo de autenticación.


Aunque existen diferentes soluciones de autenticación, el método elemental de almacenamiento de contraseñas es la función hash unidireccional. Esto se hace para que la contraseña nunca se almacene en texto sin cifrar en un sistema, ya que la función de hash no es reversible. Puede convertir una contraseña en un hash, pero (idealmente) no es posible volver a convertir un hash en una contraseña.



Como máximo, se requiere un método de fuerza bruta para trocear todas las combinaciones de una contraseña, hasta que coincida. Para mitigar el problema de que un hash de contraseña sea descifrado en un sistema, los sistemas Linux usan una "sal" aleatoria en cada hash de contraseña para un usuario. Por lo tanto, el hash para una contraseña de usuario en un sistema Linux generalmente no será el mismo que en otro sistema Linux, incluso si la contraseña es la misma.



En el archivo /etc/shadow, la contraseña puede tomar varias formas. Estos formularios generalmente incluyen lo siguiente:

- !!
Esto significa una cuenta "deshabilitada" (no es posible la autenticación), sin hash de contraseña almacenado.

- !$1$01234567$ABC…​
Una cuenta "deshabilitada" (debido al signo de exclamación inicial), con una función hash anterior, sal de hash y cadena de hash almacenados.

- $1$0123456789ABC$012…​
Una cuenta "habilitada", con una función de hash, sal de hash y cadena de hash almacenada.

La función hash, la sal hash y la cadena hash están precedidas y delimitadas por un símbolo de dólar ($). La longitud de sal de hash debe ser de entre ocho y dieciséis (8-16) caracteres. Ejemplos de los tres más comunes son los siguientes:

- $1$01234567$ABC…​
Una función hash de MD5 (1), con un ejemplo de longitud hash de ocho.

- $5$01234567ABCD$012…​
Una función hash de SHA256 (5), con un ejemplo de longitud hash de doce.

- $6$01234567ABCD$012…​
Una función hash de SHA512 (6), con un ejemplo de longitud hash de doce.


Importante:

La función hash MD5 se considera criptográficamente insegura con el nivel actual de ASIC (2010s y posteriores) e incluso el rendimiento de SIMD de computación general. Por ejemplo, las Normas federales de procesamiento de información (FIPS) de EE. UU. No permiten que MD5 se use para ninguna función criptográfica, solo aspectos muy limitados de la validación, pero no la integridad de las firmas digitales o propósitos similares.


Desde el punto de vista de los objetivos y el examen de LPI Linux Essentials, solo comprenda que el hash de contraseña para un usuario local solo se almacena en el archivo /etc/shadow, los servicios de autenticación pueden leer, o el superusuario puede modificar a través de otros comandos.