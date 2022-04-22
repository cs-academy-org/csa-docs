Crear usuarios y grupos
Introducción

Administrar usuarios y grupos en una máquina Linux es uno de los aspectos clave de la administración del sistema. De hecho, Linux es un sistema operativo multiusuario en el que múltiples usuarios pueden usar la misma máquina al mismo tiempo.



La información sobre usuarios y grupos se almacena en cuatro archivos dentro del árbol de directorio / etc /:



/etc/passwd

un archivo de siete campos delimitados por dos puntos que contiene información básica sobre los usuarios



/etc/group

un archivo de cuatro campos delimitados por dos puntos que contiene información básica sobre grupos



/etc/shadow

un archivo de nueve campos delimitados por dos puntos que contienen contraseñas de usuario cifradas



/etc/gshadow

Un archivo de cuatro campos delimitados por dos puntos que contiene contraseñas de grupo cifradas



Todos estos archivos se actualizan mediante un conjunto de herramientas de línea de comandos para la administración de usuarios y grupos, que discutiremos más adelante en esta lección. También se pueden administrar mediante aplicaciones gráficas, específicas de cada distribución de Linux, que proporcionan interfaces de administración más simples e inmediatas.



Advertencia: Aunque los archivos son texto sin formato, no los edite directamente. Utilice siempre las herramientas proporcionadas con su distribución para este propósito.



El Archivo /etc/passwd
/etc/passwd es un archivo legible por todo el mundo que contiene una lista de usuarios, cada uno en una línea separada:


frank:x:1001:1001::/home/frank:/bin/bash

Cada línea consta de siete campos delimitados por dos puntos:



Nombre de usuario

El nombre utilizado cuando el usuario inicia sesión en el sistema.


Contraseña

La contraseña cifrada (o una x si se usan contraseñas ocultas).


ID de usuario (UID)

El número de identificación asignado al usuario en el sistema.


ID de grupo (GID)

El número de grupo primario del usuario en el sistema.


Gecos

Un campo de comentario opcional, que se utiliza para agregar información adicional sobre el usuario (como el nombre completo). El campo puede contener múltiples entradas separadas por comas.


Directorio de inicio

La ruta absoluta del directorio de inicio del usuario.


Shell

La ruta absoluta del programa que se inicia automáticamente cuando el usuario inicia sesión en el sistema (generalmente un shell interactivo como / bin / bash).


El Archivo /etc/group
/etc/group es un archivo de lectura para todos que contiene una lista de grupos, cada uno en una línea separada:

developer:x:1002:


Cada línea consta de cuatro campos delimitados por dos puntos:

Nombre del grupo
El nombre del grupo.

Contraseña de grupo
La contraseña cifrada del grupo (o una x si se usan contraseñas ocultas).

ID de grupo (GID)
El número de identificación asignado al grupo en el sistema.

Lista de miembros
Una lista delimitada por comas de usuarios que pertenecen al grupo, excepto aquellos para quienes este es el grupo primario.


El Archivo /etc/shadow
/etc/shadow es un archivo legible solo por el usuario root y usuarios con privilegios de root y contiene las contraseñas cifradas de los usuarios, cada una en una línea separada:

frank:$6$i9gjM4Md4MuelZCd$7jJa8Cd2bbADFH4dwtfvTvJLOYCCCBf/.jYbK1IMYx7Wh4fErXcc2xQVU2N1gb97yIYaiqH.jjJammzof2Jfr/:18029:0:99999:7:::

Cada línea consta de nueve campos delimitados por dos puntos:



Nombre de usuario

El nombre utilizado cuando el usuario inicia sesión en el sistema.


Contraseña cifrada
La contraseña cifrada del usuario (si el valor es!, La cuenta está bloqueada).


Fecha del último cambio de contraseña

La fecha del último cambio de contraseña, como número de días desde 01/01/1970. Un valor de 0 significa que el usuario debe cambiar la contraseña en el siguiente acceso.


Edad mínima de contraseña

El número mínimo de días, después de un cambio de contraseña, que debe pasar antes de que el usuario pueda cambiar la contraseña nuevamente.


Edad máxima de contraseña

El número máximo de días que deben transcurrir antes de que se requiera un cambio de contraseña.


Período de advertencia de contraseña

El número de días, antes de que caduque la contraseña, durante los cuales se advierte al usuario que se debe cambiar la contraseña.


Periodo de inactividad de contraseña

El número de días después de que caduca una contraseña durante el cual el usuario debe actualizar la contraseña. Después de este período, si el usuario no cambia la contraseña, la cuenta se deshabilitará.


Fecha de vencimiento de la cuenta

La fecha, como número de días desde el 01/01/1970, en que se deshabilitará la cuenta de usuario. Un campo vacío significa que la cuenta de usuario nunca caducará.


Un campo reservado

Un campo reservado para uso futuro.


El Archivo /etc/gshadow
/etc/gshadow es un archivo legible solo por root y por usuarios con privilegios de root que contiene contraseñas cifradas para grupos, cada una en una línea separada:

developer:$6$7QUIhUX1WdO6$H7kOYgsboLkDseFHpk04lwAtweSUQHipoxIgo83QNDxYtYwgmZTCU0qSCuCkErmyR263rvHiLctZVDR7Ya9Ai1::

Cada línea consta de cuatro campos delimitados por dos puntos:



Nombre del grupo

El nombre del grupo.


Contraseña cifrada

La contraseña cifrada para el grupo (se usa cuando un usuario, que no es miembro del grupo, quiere unirse al grupo usando el comando newgrp; si la contraseña comienza con!, Nadie puede acceder al grupo con newgrp )


Administradores de grupo

Una lista delimitada por comas de los administradores del grupo (pueden cambiar la contraseña del grupo y pueden agregar o eliminar miembros del grupo con el comando gpasswd).


Miembros del grupo

Una lista delimitada por comas de los miembros del grupo.


Ahora que hemos visto dónde se almacena la información de usuarios y grupos, hablemos de las herramientas de línea de comandos más importantes para actualizar estos archivos.



Agregar y eliminar cuentas de usuario
En Linux, agrega una nueva cuenta de usuario con el comando useradd y elimina una cuenta de usuario con el comando userdel.



Si desea crear una nueva cuenta de usuario llamada frank con una configuración predeterminada, puede ejecutar lo siguiente:

# useradd frank
Después de crear el nuevo usuario, puede establecer una contraseña con passwd:
# passwd frank
Changing password for user frank.
New UNIX password:
Retype new UNIX password:
passwd: all authentication tokens updated successfully.

Ambos comandos requieren autorización de root. Cuando ejecuta el comando useradd, la información del usuario y del grupo almacenada en las bases de datos de contraseñas y grupos se actualiza para la cuenta de usuario recién creada y, si se especifica, se crea el directorio de inicio del nuevo usuario, así como un grupo con el mismo nombre que la cuenta de usuario



Las opciones más importantes que se aplican al comando useradd son:



-C

Cree una nueva cuenta de usuario con comentarios personalizados (por ejemplo, nombre completo).


-re

Cree una nueva cuenta de usuario con un directorio de inicio personalizado.


-mi

Cree una nueva cuenta de usuario estableciendo una fecha específica en la que se deshabilitará.


-F

Cree una nueva cuenta de usuario estableciendo el número de días después de que caduque la contraseña durante los cuales el usuario debe actualizar la contraseña.


-sol

Crear una nueva cuenta de usuario con un GID específico


-SOL

Cree una nueva cuenta de usuario agregándola a múltiples grupos secundarios.


-metro

Cree una nueva cuenta de usuario con su directorio de inicio.


-METRO

Cree una nueva cuenta de usuario sin su directorio de inicio.


-s

Cree una nueva cuenta de usuario con un shell de inicio de sesión específico.


-u

Cree una nueva cuenta de usuario con un UID específico.


Una vez que se crea la nueva cuenta de usuario, puede usar los comandos id y grupos para averiguar su UID, GID y los grupos a los que pertenece.

# id frank
uid=1000(frank) gid=1000(frank) groups=1000(frank)
# groups frank
frank : frank

Si desea eliminar una cuenta de usuario, puede usar el comando userdel. En particular, este comando actualiza la información almacenada en las bases de datos de la cuenta, eliminando todas las entradas que se refieren al usuario especificado. La opción -r también elimina el directorio de inicio del usuario y todos sus contenidos, junto con la cola de correo del usuario. Otros archivos, ubicados en otro lugar, deben buscarse y eliminarse manualmente.

# userdel -r frank
Como antes, necesita autorización de root para eliminar cuentas de usuario.



El Directorio de esqueletos
Cuando agrega una nueva cuenta de usuario, incluso al crear su directorio de inicio, el directorio de inicio recién creado se llena con archivos y carpetas que se copian del directorio de esqueleto (por defecto /etc/skel). La idea detrás de esto es simple: un administrador del sistema quiere agregar nuevos usuarios que tengan los mismos archivos y directorios en su hogar. Por lo tanto, si desea personalizar los archivos y carpetas que se crean automáticamente en el directorio de inicio de las nuevas cuentas de usuario, debe agregar estos nuevos archivos y carpetas al directorio de esqueleto.



Agregar y eliminar grupos
En cuanto a la administración de grupos, puede agregar o eliminar grupos usando los comandos groupadd y groupdel.



Si desea crear un nuevo grupo llamado developer, puede ejecutar el siguiente comando como root:

# groupadd -g 1090 developer
La opción -g de este comando crea un grupo con un GID específico.



Si desea eliminar el grupo de developer, puede ejecutar lo siguiente:

# groupdel developer

Advertencia: Recuerde que cuando agrega una nueva cuenta de usuario, el grupo primario y los grupos secundarios a los que pertenece deben existir antes de iniciar el comando useradd. Además, no puede eliminar un grupo si es el grupo principal de una cuenta de usuario.

El comando passwd
Este comando se usa principalmente para cambiar la contraseña de un usuario. Cualquier usuario puede cambiar su contraseña, pero solo el usuario root puede cambiar la contraseña de cualquier usuario.



Dependiendo de la opción passwd utilizada, puede controlar aspectos específicos del envejecimiento de la contraseña:



-re

Elimine la contraseña de una cuenta de usuario (deshabilitando así al usuario).


-mi

Forzar la cuenta de usuario para cambiar la contraseña.


-l

Bloquee la cuenta de usuario (la contraseña cifrada tiene el prefijo con un signo de exclamación).


-u

Desbloquee la cuenta de usuario (elimina el signo de exclamación).


-S

Salida de información sobre el estado de la contraseña para una cuenta específica.


Estas opciones están disponibles solo para root. Para ver la lista completa de opciones, consulte las páginas del manual.