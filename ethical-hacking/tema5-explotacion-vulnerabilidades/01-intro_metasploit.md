# INTRODUCCIÓN A METASPLOIT
## ¿QUÉ ES METASPLOIT?
El Metasploit Framework (MSF) es mucho más que una simple colección de exploits, sino que también es una base sólida que se puede aprovechar y personalizar fácilmente para satisfacer sus necesidades. Esto le permite concentrarse en su entorno objetivo único y no tener que reinventar la rueda. Consideramos que MSF es una de las herramientas de auditoría de seguridad más útiles disponibles gratuitamente para los profesionales de la seguridad en la actualidad. Desde una amplia gama de exploits de grado comercial y un amplio entorno de desarrollo de exploits, hasta herramientas de recopilación de información de red y complementos de vulnerabilidad web, Metasploit Framework proporciona un entorno de trabajo realmente impresionante.

Este curso se ha escrito de manera que no solo abarque los aspectos del "usuario" de interfaz del marco, sino que además le dará una introducción a las capacidades que proporciona Metasploit. Nuestro objetivo es brindarle una visión en profundidad de las muchas características de Metasploit y brindarle las habilidades y la confianza para aprovechar esta increíble herramienta.


### Prepare su entorno de laboratorio Metasploit
Antes de aprender a utilizar Metasploit Framework, primero debemos asegurarnos de que nuestra configuración cumpla o supere los requisitos del sistema descritos en las siguientes secciones. Tomarse el tiempo para preparar adecuadamente su entorno de laboratorio Metasploit  ayudará a eliminar muchos problemas antes de que surjan más adelante en el curso. Recomendamos encarecidamente utilizar un sistema que sea capaz de ejecutar varias máquinas virtuales para alojar sus laboratorios.

### REQUISITOS DE HARDWARE DE METASPLOIT UNLEASHED
Todos los valores enumerados a continuación son estimados o recomendados. Puede salirse con la suya con menos en algunos casos, pero tenga en cuenta que el rendimiento se verá afectado, lo que lo convierte en una experiencia de aprendizaje menos que ideal.

#### Espacio en disco duro
Deberá tener, como mínimo, 10 gigabytes de espacio de almacenamiento disponible en su host. Dado que estamos usando máquinas virtuales con archivos de gran tamaño, esto significa que no podemos usar una partición FAT32 ya que los archivos grandes no son compatibles con ese sistema de archivos, así que asegúrese de elegir NTFS, ext3 o algún otro formato de sistema de archivos. La cantidad de espacio recomendada necesaria es de 30 gigabytes .

Si decidió crear clones o instantáneas de sus máquinas virtuales a medida que avanza en el curso, estos también ocuparán un espacio valioso en su sistema. Esté atento y no tenga miedo de reclamar espacio según sea necesario.

#### MEMORIA DISPONIBLE
Si no proporciona suficiente memoria a sus sistemas operativos host e invitados, eventualmente se producirá una falla en el sistema y / o no podrá iniciar su (s) máquina (s) virtual (es). Necesitará RAM para su sistema operativo host, así como la cantidad de RAM que está dedicando a cada máquina virtual. Utilice la guía a continuación para ayudarlo a decidir la cantidad de RAM necesaria para su situación.

Requisitos mínimos de memoria "HOST" de Linux
2 GB de memoria del sistema (RAM)
De manera realista, 2 GB o más
Requisitos mínimos de memoria Kali "GUEST"
Al menos 2 GB de RAM (se recomiendan 4 GB) // ¡más nunca está de más!
De manera realista, 2 GB o más con un archivo SWAP de igual valor
Requisitos mínimos de memoria de Metasploitable "GUEST"
Al menos 256 MB de RAM (se recomiendan 512 MB) // ¡más nunca está de más!
(Opcional) Requisitos mínimos de memoria según Windows "GUEST"
Al menos 256 MB de RAM (se recomienda 1 GB) // ¡más nunca está de más!
De manera realista, 1 GB o más con un archivo de página de igual valor
PROCESADOR
Para garantizar la mejor experiencia, recomendamos una CPU de cuatro núcleos de 64 bits o mejor. El requisito mínimo para VMware Player es un procesador de 400MHz o más rápido (se recomienda 500MHz) pero estas velocidades son inadecuadas para los propósitos de este curso. Cuantos más caballos de fuerza pueda lanzar a su laboratorio, mejor.

#### ACCESIBILIDAD A INTERNET
La configuración de su laboratorio requerirá descargar algunas máquinas virtuales grandes, por lo que querrá tener una buena conexión de alta velocidad para hacerlo. Si opta por utilizar una red "en puente" para sus máquinas virtuales y no hay un servidor DHCP en su red, tendrá que asignar direcciones IP estáticas a sus máquinas virtuales invitadas.

#### REQUISITOS DE SOFTWARE DE METASPLOIT UNLEASHED
Antes de saltar al Metasploit Framework, necesitaremos tener una máquina atacante (Parrot OS Security Edition) y una máquina víctima (metasploitable 2), así como un hipervisor para ejecutar tanto en un entorno de red seguro como aislado.

#### METASPLOITABLE
Uno de los problemas que encuentra al aprender a usar un marco de explotación es intentar encontrar y configurar objetivos para escanear y atacar. Afortunadamente, el equipo de Metasploit es consciente de esto y lanzó una máquina virtual VMware vulnerable llamada 'Metasploitable'.

Metasploitable es una máquina virtual Linux intencionalmente vulnerable que se puede usar para realizar capacitación en seguridad, probar herramientas de seguridad y practicar técnicas comunes de prueba de penetración. La máquina virtual se ejecutará en cualquier producto VMware reciente y otras tecnologías de visualización como VirtualBox. Puede descargar el archivo de imagen de Metasploitable 2 desde SourceForge .

¡Nunca exponga Metasploitable a una red que no sea de confianza, use NAT o el modo solo de host!

Una vez que haya descargado la máquina virtual Metasploitable, extraiga el archivo zip, abra el archivo .vmx con el producto VMware de su elección y enciéndalo. Después de un breve tiempo, el sistema se iniciará y estará listo para la acción. El nombre de usuario y la contraseña predeterminados son msfadmin: msfadmin .

Para obtener más información sobre la configuración de la máquina virtual, hay una Guía de explotación de Metasploitable 2 en el sitio web de Rapid7, pero tenga cuidado ... contiene spoilers.

#### Windows
Microsoft ha puesto a disposición una serie de máquinas virtuales que se pueden descargar para probar Microsoft Edge y diferentes versiones de Internet Explorer. Podremos utilizar estas máquinas virtuales cuando trabajemos con algunos de los exploits y herramientas disponibles en Metasploit. Puede descargar las VM desde la siguiente URL:

https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/
Una vez que haya cumplido con los requisitos del sistema anteriores, no debería tener problemas para ejecutar los ejercicios del curso en Metasploit.


### COMPRENDER LA ARQUITECTURA DE METASPLOIT
Uno puede entender más fácilmente la arquitectura de Metasploit echando un vistazo debajo de su capó. Al aprender a usar Metasploit, tómese un tiempo para familiarizarse con su sistema de archivos y bibliotecas. En Parrot OS, Metasploit se proporciona en el paquete metasploit-framework y se instala en el directorio /usr/share/metasploit-framework.



#### Sistema de archivos Metasploit
El sistema de archivos MSF se presenta de manera intuitiva y está organizado por directorio. Algunos de los directorios más importantes se describen brevemente a continuación.

DATOS
El directorio de datos contiene archivos editables utilizados por Metasploit para almacenar binarios necesarios para ciertos exploits, listas de palabras, imágenes y más.

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/data/
	cpuinfo ipwn meterpreter snmp cámara web
	eicar.com isight.bundle mime.yml suena wmap
	eicar.txt john.conf msfcrawler SqlClrPayload listas de palabras
	emailer_config.yaml lab passivex templates
	explota logotipos php vncdll.x64.dll
	flash_detector markdown_doc post vncdll.x86.dll

#### DOCUMENTACIÓN
Como sugiere su nombre, el directorio de documentación contiene la documentación disponible para el marco.

	┌─[root@cs-academy]─[~]
	└──╼ # ls / usr / share / metasploit-framework / documentation /
	changelog.Debian.gz CONTRIBUTING.md.gz developers_guide.pdf.gz README.md
	Módulos de derechos de autor CODE_OF_CONDUCT.md

#### LIB
El directorio lib contiene la "carne" del código base del marco.

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/lib/
	anémona msfenv.rb rbmysql.rb sqlmap
	tareas de anemone.rb net rex
	telefonía enumerable.rb postgres rex.rb
	metasm postgres_msf.rb robots.rb telephony.rb
	metasploit rabal snmp windows_console_color_support.rb
	msf rbmysql snmp.rb

#### MÓDULOS
El directorio de módulos es donde encontrará los módulos MSF reales para exploits, módulos auxiliares y posteriores, cargas útiles, codificadores y generadores nop.

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/modules/
Los codificadores auxiliares explotan las cargas útiles de Nops.

#### COMPLEMENTOS
Como verá más adelante en este curso, Metasploit incluye muchos complementos , que encontrará en este directorio.

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/plugins/
	aggregator.rb ips_filter.rb openvas.rb sounds.rb
	alias.rb komand.rb pcap_log.rb sqlmap.rb
	auto_add_route.rb lab.rb request.rb thread.rb
	beholder.rb libnotify.rb rssfeed.rb token_adduser.rb
	db_credcollect.rb msfd.rb sample.rb token_hunter.rb
	db_tracker.rb msgrpc.rb session_notifier.rb wiki.rb
	event_tester.rb nessus.rb session_tagger.rb wmap.rb
	ffautoregen.rb nexpose.rb socket_logger.rb

#### SCRIPTS
El directorio de scripts contiene Meterpreter y otros scripts.

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/scripts/

#### HERRAMIENTAS
El directorio de herramientas tiene varias utilidades de línea de comandos útiles.

	┌─[root@cs-academy]─[~]
	└──╼ #  ls /usr/share/metasploit-framework/tools/

#### Bibliotecas Metasploit
Hay una serie de bibliotecas de MSF que nos permiten ejecutar nuestros exploits sin tener que escribir código adicional para tareas rudimentarias, como solicitudes HTTP o codificación de cargas útiles. Algunas de las bibliotecas más importantes se describen a continuación.

REX
La biblioteca básica para la mayoría de las tareas.
Maneja sockets, protocolos, transformaciones de texto y otros
SSL, SMB, HTTP, XOR, Base64, Unicode
MSF :: CORE
Proporciona la API 'básica'
Define el marco de Metasploit
MSF :: BASE
Proporciona la API 'amigable'
Proporciona API simplificadas para usar en el marco.
A lo largo de este curso, abordaremos cómo usar otras herramientas directamente dentro de Metasploit. Comprender cómo se almacenan las cosas y cómo se relacionan con el sistema de archivos Metasploit lo ayudará a usar msfconsole  y las otras interfaces de Metasploit.

#### MÓDULOS Y UBICACIONES DE METASPLOIT

Casi toda su interacción con Metasploit será a través de sus muchos módulos , que busca en dos ubicaciones. El primero es la tienda del módulo principal en /usr/share/metasploit-framework/modules/ y el segundo, que es donde almacenará los módulos personalizados, está en su directorio de inicio en ~ /.msf4/modules/ .

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/modules/

Los codificadores auxiliares explotan las cargas útiles de Nops.
Todos los módulos de Metasploit están organizados en directorios separados, según su propósito. A continuación se muestra una descripción general básica de los distintos tipos de módulos de Metasploit.

##### Exploits
En Metasploit Framework, los módulos de explotación se definen como módulos que utilizan cargas útiles.

	┌─[root@cs-academy]─[~]
	└──╼ # ls / usr / share / metasploit-framework / modules / exploits /
	aix bsdi firefox irix multi solaris
	android dialup freebsd linux netware unix
	apple_ios example.rb hpux mainframe osx windows

##### Auxiliar
Los módulos auxiliares incluyen escáneres de puertos, fuzzers, sniffers y más.

	┌─[root@cs-academy]─[~]
	└──╼ # ls / usr / share / metasploit-framework / modules / auxiliary /
	administrador cliente dos recopilar escáner spoof vsploit
	analizar rastreador ejemplo.rb servidor analizador sqli
	bnat docx fuzzers pdf sniffer voip

##### Cargas útiles, codificadores, Nops
Las cargas útiles consisten en un código que se ejecuta de forma remota, mientras que los codificadores garantizan que las cargas útiles lleguen intactas a su destino. Nops mantienen el tamaño de la carga útil constante en todos los intentos de explotación.

	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/modules/payloads/
	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/modules/encoders/
	┌─[root@cs-academy]─[~]
	└──╼ # ls /usr/share/metasploit-framework/modules/nops/

##### Carga de árboles de módulos adicionales
Metasploit le brinda la opción de cargar módulos en tiempo de ejecución o después de que msfconsole ya se haya iniciado. Pase la opción -m al ejecutar msfconsole para cargar módulos adicionales en tiempo de ejecución:

	┌─[root@cs-academy]─[~]
	└──╼ # msfconsole -m ~ /módulos-secretos/

Si necesita cargar módulos adicionales desde msfconsole, use el comando loadpath :

	msf> loadpath
	Uso: loadpath </ruta/a/módulos>

Carga módulos del directorio dado que debe contener subdirectorios para tipos de módulo, por ejemplo, /ruta/a/módulos/exploits

	msf> ruta de carga /usr/share/metasploit-framework/modules/
	399 módulos cargados:
	    399 cargas útiles