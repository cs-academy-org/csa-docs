# Usar la línea de comando para obtener ayuda

## Introducción

La línea de comando es una herramienta muy compleja. Cada comando tiene sus propias opciones únicas, por lo tanto, la documentación es clave cuando se trabaja con un sistema Linux. Además del directorio /usr/share/doc/, que almacena la mayor parte de la documentación, varias otras herramientas proporcionan información sobre el uso de comandos de Linux. Este capítulo se centra en los métodos para acceder a esa documentación, con el fin de obtener ayuda.

Hay una multitud de métodos para obtener ayuda dentro de la línea de comandos de Linux. man, --help e info son solo algunos de ellos. Para Linux Essentials, nos centraremos en man e info pages, ya que son las herramientas mas utilizadas para obtener ayuda.

Otro tema de este capítulo será localizar archivos. Trabajará principalmente con el comando de locate.

## Obteniendo ayuda en la línea de comando

###Ayuda incorporada (Built-in Help)

Cuando se inicia con el parámetro --help, la mayoría de los comandos muestran algunas breves instrucciones sobre su uso. Aunque no todos los comandos proporcionan este interruptor, todavía es un buen primer intento para aprender más sobre los parámetros de un comando. Tenga en cuenta que las instrucciones de --help a menudo son bastante breves en comparación con las otras fuentes de documentación que discutiremos en el resto de esta lección.

### Man Pages

La mayoría de los comandos proporcionan una página de manual o  "man page". Esta documentación generalmente se instala con el software y se puede acceder con el comando man. El comando cuya página de manual debe mostrarse se agrega a man como argumento:

	$ man mkdir

Este comando abre la página de manual para mkdir. Puede usar las teclas de flecha arriba y abajo o la barra espaciadora para navegar por la página del manual. Para salir de la página del manual, presione kbd: [Q].



Cada página de manual pertenece exactamente a una sección. Sin embargo, varias secciones pueden contener páginas de manual con el mismo nombre. Tomemos el comando passwd como ejemplo. Este comando se puede usar para cambiar la contraseña de un usuario. Como passwd es un comando de usuario, su página de manual reside en la sección 1. Además del comando passwd, el archivo de base de datos de contraseñas /etc/passwd también tiene una página de manual que también se llama passwd. Como este archivo es un archivo de configuración, pertenece a la sección 5. Cuando se hace referencia a una página de manual, la categoría a menudo se agrega al nombre de la página de manual, como en passwd (1) o passwd (5) para identificar el respectivo hombre página.

Por defecto, man passwd muestra la primera página man disponible, en este caso passwd (1). La categoría de la página man deseada se puede especificar en un comando como man 1 passwd o man 5 passwd.

Ya hemos discutido cómo navegar a través de una página de manual y cómo volver a la línea de comando. Internamente, man usa el comando less para mostrar el contenido de la página man. less le permite buscar texto dentro de una página de manual. Para buscar la palabra linux, puede usar /linux para la búsqueda hacia adelante desde el punto en que se encuentra en la página, o ?Linux para iniciar una búsqueda hacia atrás. Esta acción resalta todos los resultados coincidentes y mueve la página a la primera coincidencia resaltada. En ambos casos, puede escribir kbd: [N] para saltar a la siguiente coincidencia. Para encontrar más información sobre estas características adicionales, presione kbd: [H] y se mostrará un menú con toda la información.

### Info Pages

Otra herramienta que lo ayudará mientras trabaja con el sistema Linux son las páginas de información. Las páginas de información suelen ser más detalladas que las páginas de manual y están formateadas en hipertexto, similar a las páginas web en Internet.

Las páginas de información se pueden mostrar así:

	$ info mkdir

Para cada página de información, la información lee un archivo de información que está estructurado en nodos individuales dentro de un árbol. Cada nodo contiene un tema simple y el comando info contiene hipervínculos que pueden ayudarlo a moverse de uno a otro. Puede acceder al enlace presionando enter mientras coloca el cursor en uno de los asteriscos principales.

Similar a man, la herramienta de información (info) también tiene comandos de navegación de página. Puede obtener más información sobre estos comandos presionando kbd: [?] Mientras se encuentra en la página de información. Estas herramientas lo ayudarán a navegar por la página más fácilmente, así como a comprender cómo acceder a los nodos y moverse dentro del árbol de nodos.

El directorio /usr/share/doc/  
Como se mencionó anteriormente, el directorio /usr/share/doc/ almacena la mayoría de la documentación de los comandos que está utilizando el sistema. Este directorio contiene un directorio para la mayoría de los paquetes instalados en el sistema. El nombre del directorio suele ser el nombre del paquete y, en ocasiones, su versión. Estos directorios incluyen un archivo README o readme.txt que contiene la documentación básica del paquete. Junto con el archivo README, la carpeta también puede contener otros archivos de documentación, como el registro de cambios que incluye el historial del programa en detalle o ejemplos de archivos de configuración para el paquete específico.

La información dentro del archivo README varía de un paquete a otro. Todos los archivos están escritos en texto plano, por lo tanto, se pueden leer con cualquier editor de texto preferido. El número exacto y los tipos de archivos dependen del paquete. Consulte algunos de los directorios para obtener una descripción general de sus contenidos.

## Localizar archivos

### El comando locate
Un sistema Linux está construido a partir de numerosos directorios y archivos. Linux tiene muchas herramientas para ubicar un archivo en particular dentro de un sistema. El más rápido es el comando locate.

locate búsquedas dentro de una base de datos y luego generar cada nombre que coincida con la cadena dada:

	$ locate note
	/lib/udev/keymaps/zepto-znote
	/usr/bin/zipnote
	/usr/share/doc/initramfs-tools/maintainer-notes.html
	/usr/share/man/man1/zipnote.1.gz

El comando locate también admite el uso de comodines y expresiones regulares, por lo tanto, la cadena de búsqueda no tiene que coincidir con el nombre completo del archivo deseado. Aprenderá más sobre las expresiones regulares en un capítulo posterior.

Por defecto, locate se comporta como si el patrón estuviera rodeado de asteriscos, por lo que locate PATTERN es lo mismo que locate *PATTERN*. Esto le permite simplemente proporcionar subcadenas en lugar del nombre de archivo exacto. Puede modificar este comportamiento con las diferentes opciones que puede encontrar explicadas en la página del manual de locate.

Como localizar está leyendo desde una base de datos, es posible que no encuentre un archivo que haya creado recientemente. La base de datos es administrada por un programa llamado updatedb. Por lo general, se ejecuta periódicamente, pero si tiene privilegios de root y necesita que la base de datos se actualice de inmediato, puede ejecutar el comando updatedb usted mismo en cualquier momento.

### El comando find

find es otra herramienta muy popular que se utiliza para buscar archivos. Este comando tiene un enfoque diferente, en comparación con el comando locate. El comando find busca un árbol de directorios de forma recursiva, incluidos sus subdirectorios. find realiza dicha búsqueda en cada invocación, no mantiene una base de datos como locate. Similar a locate, find también admite comodines y expresiones regulares.

find requiere al menos la ruta en la que debe buscar. Además, se pueden agregar las llamadas expresiones para proporcionar criterios de filtro para que archivos mostrar. Un ejemplo es la expresión -name, que busca archivos con un nombre específico:

	~$ cd Downloads
	~/Downloads
	$ find . -name thesis.pdf
	./thesis.pdf
	~/Downloads
	$ find ~ -name thesis.pdf
	/home/carol/Downloads/thesis.pdf

El primer comando de find busca el archivo dentro del directorio de descargas actual (Downloads), mientras que el segundo busca el archivo en el directorio de inicio del usuario (~).

El comando find es muy complejo, por lo tanto, no se cubrirá en el examen de Linux Essentials. Sin embargo, es una herramienta poderosa que es particularmente útil en la práctica.