# Principales aplicaciones de código abierto

Una aplicación es un programa de computadora cuyo propósito no está atado al correcto funcionamiento de una computadora, sino a tareas realizadas por el usuario. Las distribuciones Linux ofrecen gran variedad de aplicaciones para realizar determinadas tareas, tales como aplicaciones de ofimática, navegadores web, reproductores de multimedia y editores, etc. Por lo general contamos con más de una aplicación para realizar un trabajo en particular, depende del usuario escoger la aplicación que más cumpla con sus necesidades.

## Paquetes de Software

Casi todas las distribuciones ofrecen una serie de aplicaciones instaladas por defecto, además de esas aplicaciones instaladas por defecto, cada distribución cuenta con un repositorio de paquetes con una amplia colección de aplicaciones disponibles para instalar utilizando el gestor de paquetes. Muchas distribuciones ofrecen las mismas aplicaciones, con la diferencia de que no todas usan el mismo gestor de paquetes. Por ejemplo Debian, Ubuntu, Linux Mint y demás distribuciones basadas en Debian o Ubuntu usan las herramientas dpkg, apt-get y apt para instalar paquetes. Las distribuciones de la familia RedHat (RedHat, CentOS, Fedora) utilizan las herramientas rpm, yum y dnf para instalar paquetes RPM. Ya que el empaquetamiento de estas aplicaciones es diferente para cada una de las distribuciones, es importante utilizar el repositorio de paquetes correcto para cada distribución. Por lo general el usuario no debe preocuparse por esto ya que los repositorios suelen estar configurados por defecto en distribuciones modernas y el gestor de paquetes se encarga de seleccionar la versión correcta del paquete a instalar, sus dependencias y futuras actualizaciones.  

## Instalación y Eliminación de Paquetes

Supongamos que necesitas usar el comando figlet, pero al intentar ejecutar el comando te dice que no se encuentra (command not found). Probablemente el paquete no esté instalado en el sistema. Si tu distribución trabaja con paquetes DEB puedes utilizar los comandos apt-cache search figlet o apt search figlet para realizar una búsqueda del paquete en el repositorio, luego de encontrar el paquete puedes proceder a instalarlo con apt-get install figlet o apt install figlet.

Los mismos comandos que se utilizan para instalar paquetes son utilizados para removerlos, todos los comandos aceptan remove como parámetro para eliminar paquetes instalados, por ejemplo: apt-get remove figlet, apt remove figlet. En el caso de las distribuciones que utilizan paquetes rpm seria: yum remove figlet, dnf remove figlet. 

Es importante mencionar que el comando sudo es necesario para realizar estas operaciones ya que la instalación y eliminación de paquetes requiere privilegios de super usuario (root).

## Aplicaciones de Ofimática

Las aplicaciones de ofimática son utilizadas para crear o editar archivos, tales como textos, presentaciones, hojas de cálculo y otros formatos comúnmente utilizados en entornos de oficina. Usualmente estas aplicaciones están organizadas en una colección llamada Office Suite.

Por mucho tiempo la suite ofimática más utilizada fue OpenOffice, la cual al cambiar de licencia fue renombrada a Apache OpenOffice por Oracle quién transfirió el proyecto a Apache Fundation. Mientras tanto otra suite ofimática basada en el mismo código fuente fue liberada por The Document Fundation quien la llamó LibreOffice

Ambos proyectos tienen las mismas características y son compatibles con la suite ofimática de Microsoft, sin embargo el formato de documentos preferido es Open Document Format, un formato estandarizado completamente abierto. El uso de los archivos ODF aseguran que los documentos pueden ser transferidos a otros sistemas operativos y aplicaciones ofimática de diferentes vendedores.

## Principales aplicaciones que ofrece la Suite OpenOffice/LibreOffice

- Writer
Editor de texto
- Calc
Hojas de cálculo
- Impress
Presentaciones
- Draw
Vector drawing
- Math
Formulas matemáticas
- Base
Base de datos

## Navegadores Web

Para muchos usuarios, el mayor propósito de una computadora es tener acceso a internet. Actualmente, las aplicaciones web funcionan como una aplicación, con la ventaja de que son accesibles desde cualquier lugar sin necesidad de instalar software adicional en el ordenador. Esto hace que los navegadores sean una de las aplicaciones más importantes en un sistema operativo, al menos para el usuario común.

Los principales navegadores en entornos Linux son Google Chrome y Mozilla Firefox. Chrome es un navegador Web mantenido por Google basado en el navegador de código abierto Chromium, el cual es completamente compatible con Chrome. Mantenido por Mozilla (una organización sin ánimo de lucro), Firefox es un navegador cuyo origen esta conectado a Netscape, el primer navegador web que adoptó el modelo Open Source.

Mozilla desarrolla otras aplicaciones como el popular cliente de e-mail Thunderbird. Muchos usuarios optan por usar webmail en lugar de aplicaciones dedicadas a gestionar el correo electrónico, pero un cliente como Thunderbird ofrece funcionalidades extras y una mejor integración con las aplicaciones de escritorio.

## Multimedia

En comparación con las aplicaciones web disponibles, las aplicaciones de escritorio son la mejor opción para la creación contenido multimedia. Las actividades relacionadas con contenido multimedia como la renderización de videos, necesitan una mayor cantidad de recursos del sistema, por lo que son mejor gestionadas de manera local por aplicaciones de escritorio. A continuación compartimos algunas de las aplicaciones de multimedia más populares para Linux y sus usos.

### Aplicaciones de Multimedia más Populares

- Blender: 
Renderizador 3D para crear animaciones. Blender también se puede utilizar para exportar objetos 3D para ser impresos por una impresora 3D.

- GIMP: 
Un editor de imágenes equipado con muchas funcionalidades, el cual puede ser comparado con Adobe Photoshop, pero tiene sus propios conceptos y herramientas para trabajar con imágenes. GIMP puede ser usado para crear, editar y guardar la mayoría de archivos bitmap, por ejemplo: JPEG, PNG, GIF, TIFF, entre muchos otros formatos.

- Inkscape: 
Un editor de gráficos vectoriales, similar a Corel Draw o Adobe Illustrator. Su formato por defecto es SVG, el cual es un estándar abierto para gráficos vectoriales. Los archivos SVG pueden ser abiertos por cualquier navegador web y debido a su naturaleza como vector graphic, puede ser usado en capas flexibles de páginas web.

- Audacity: 
Un editor de audio, puede ser usado para filtrar, aplicar efectos y convertir entre distintos formatos de audio, como: MP3, WAV, OGG, FLAC, etc.

- Imagemagick: 
Es una herramienta de linea de comandos para convertir y editar la mayoría de los tipos de archivos de imagen. También puede ser usado para crear documentos PDF desde imágenes y vice versa.

Existen muchas aplicaciones dedicadas a la reproducción de archivos multimedia, La aplicación más popular para reproducción de archivos de video es VLC, pero algunos usuarios prefieren otras alternativas, como smplayer.

Para la reproducción de música y/o archivos de audio también existen varias opciones, como Audacious, Banshee, Rhythmbox y Amarok, los cuales también pueden gestionar colecciones locales de archivos de audio.

## Programas para Servidores

Cuando un navegador carga una página desde un sitio web, realmente se está conectando a una computadora remota y solicita una parte de información. En ese escenario, la computadora que corre el navegador web hace el papel de cliente y la computadora remota es llamada servidor.

La computadora servidor necesita un programa determinado para cada tipo de información que va a brindar. Para servir una página web, la mayoría de los servers en todo el mundo utilizan programas de código abierto. Este servidor en particular es llamado HTTP Server y los más populares son Apache, Nginx y lighttpd.

Una página web simple puede requerir muchas solicitudes, que pueden ser archivos comunes (llamado contenido estático o páginas estáticas) o contenido dinámico renderizado por varios códigos. El papel del servidor HTTP es recolectar y enviar todos los datos solicitados de regreso navegador, el cual se encarga de acomodar y mostrar el contenido tal como está en el documento HTML recibido. Por lo tanto, el renderizado de una página web es una mezcla de operaciones realizadas del lado del servidor y operaciones realizadas del lado del cliente. Ambos lados pueden utilizar scripts personalizados para llevar a cabo estas tareas.

En el lado del servidor es muy común utilizar el lenguaje de programación PHP. Javascript es el lenguaje utilizado del lado del cliente (en el navegador).

Los programas de servidores pueden proporcionar cualquier tipo de información. No es raro que un programa de servidor solicite información de otros programas de servidores. Tal es el caso de cuando un servidor HTTP necesita información ofrecida por un servidor de base de datos.

Por ejemplo, cuando una página dinámica es solicitada, el servidor HTTP suele hacer peticiones a una base de datos para recolectar toda la información solicitada y envía todo el contenido dinámico solicitado al cliente. Un caso similar, es cuando un usuario se registra en un sitio web, el servidor HTTP obtiene la información enviada por el cliente y la guarda en una base de datos.

Una base de datos no es más que una cantidad determinada de información organizada. Las bases de datos almacenan la información de manera formateada. Haciendo posible la lectura, escritura y enlazar grandes cantidades de datos de manera muy rápida y con un alto rendimiento. Los servidores de bases de datos de código abierto son utilizados en muchas aplicaciones, no solo en internet. Incluso, aplicaciones locales pueden almacenar datos conectandose a un servidor de base de datos local. Las bases de datos más comunes son las bases de datos relacionales, donde los datos son almacenados en tablas predefinidas, las más populares de este tipo y de código abierto son MariaDB (nace de MySQL) y PostgreSQL.

## Programas para compartir datos

En una red local, es común que los ordenadores no solo sean capaces de conectarse a internet, sino también puedan comunicarse entre ellos y compartir archivos. Algunas veces una computadora actúa como servidor y otras veces esa misma computadora actúa como cliente. Esto es necesario cuando uno quiere acceder a archivos de otra computadora dentro de la red, por ejemplo: acceder a archivos que se encuentran en un ordenador de escritorio desde un ordenador portátil sin necesidad de copiar los archivos a una USB o algo por el estilo.

- NFS (solo Unix/Linux): 
Nos sirve para compartir archivos e incluso todo un sistema operativo

- Samba: 
Si los ordenadores dentro de la red corren diferentes sistemas operativos se debe implementar un protocolo que sea soportado por todos.

- Domain Controller (Controlador de Dominio): 
En algunas redes empresariales, el nivel de autorización para acceder a una estación de trabajo es controlada por un servidor principal, llamado Domain Controller, quien maneja el nivel de acceso los recursos locales y remotos. Las estaciones de trabajo Linux se pueden asociar a un un Controlador de Dominio usando el protocolo Samba o un subsistema de autenticación llamado SSSD. Samba también puede funcionar como un controlador de dominio en redes heterogéneas.

Si el objetivo es implementar un sistema de computación en la nube para brindar varios métodos de compartir de archivos de la web podemos usar ownCloud y NextCloud.

## Administración de Redes

La comunicación entre las computadoras solo es posible si la red esta funcionando correctamente. Por lo general la configuración de la red es realizada por algunos programas que corren en el router, el encargado de configurar y chequear la disponibilidad de la red. Para poder realizar esta tarea, son utilizados dos servicios de red: DHCP (Dynamic Host Configuration Protocol) y DNS (Domain Name System).

La configuración de ambos (DHCP y DNS) pueden ser modificadas en la interfaz web de administración del router. Por ejemplo, es posible restringir la asignación de IP solo a dispositivos conocidos, o asociar una dirección IP fija a ordenadores específicos. También es posible cambiar la configuración de DNS que nos brinda el ISP. Algunos servidores DNS de terceros, como es el caso de los de Google o OpenDNS, pueden brindar respuestas más rápidas y funcionalidades extras.

## Lenguajes de Programación

Todos los programas de una computadora están hechos por uno o varios lenguajes de programación. Un programa puede ser un solo archivo o un complejo sistema de cientos de archivos. Las cuales el sistema operativo ve como secuencias de instrucciones que son interpretadas y realizadas por el procesador y otros dispositivos.

Hay muchos lenguajes de programación para diferentes propósitos y los sistemas Linux nos ofrecen muchos de ellos. Ya que el software de código abierto también incluye el código del programa, los sistemas Linux ofrecen a los desarrolladores las condiciones perfectas para entender, modificar o crear programas de acuerdo a sus necesidades.

Todo programa comienza como un archivo de texto, llamado código fuente. Este código fuente se escribe en un lenguaje que describe lo que el programa hace. El procesador de una computadora no puede ejecutar directamente este código. En lenguajes compilados, el código fuente es convertido a un archivo binario que posteriormente puede ser ejecutado por la computadora. Un programa llamado compilador es el encargado de esta conversión, de código fuente a ejecutable.

En lenguajes interpretados, el programa no necesita ser previamente compilado, en lugar de esto, un interpreter lee el código fuente y ejecuta sus instrucciones cada vez que el programa es ejecutado. Esto hace que el proceso de desarrollo sea más fácil y cómodo, pero a su vez los programas interpretados suelen ser más lentos que los programas compilados.

## Lenguajes más populares

- JavaScript
- C
- Java
- Perl
- Shell
- Python
- PHP

C y Java son lenguajes compilados. Para poder ser ejecutado por una computadora, el código fuente escrito en C es convertido a binario o código de máquina, mientras que el código escrito en Java es convertido a bytecode el cual se ejecuta en un entorno de software especial llamado Java Virtual Machine. JavaScript, Perl, Shell script (bash), Python y PHP son lenguajes interpretados, también son llamados lenguajes de scripting.