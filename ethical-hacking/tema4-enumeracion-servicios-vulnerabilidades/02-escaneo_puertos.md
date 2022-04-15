# Escaneo de Puertos y Enumeración de Servicios

## Introducción

Una ves que encontremos el/los rangos de IP que usa esta red a la que ganamos acceso, el siguiente paso es hacer un mapeo de la misma. Para ello podemos usar el comando ping como vimos en la clase anterior y con la ayuda de bash obtener mejores resultados. Nmap, está de más mencionar que es "The Most powerfull Port-Scanner Tool" (lo escribo en ingles porque suena mas poderosa), también tenemos herramientas más rápidas como masscan pero a su vez generan más tráfico. 

Esta parte es muy importante (me van a ver repetir esto muchas veces), llegar a conocer bien la red a la que le estamos haciendo una prueba de penetración, es más importante que descubrir una Vulnerabilidad y escribir un exploit para explotarla. En fin me he ido un poco del tema pero lo que quiero dejar claro es que el éxito de la siguiente etapa de la prueba de penetración depende mucho de estas primeras etapas de reconocimiento, escaneo de puertos y enumeración de servicios.



Bueno, luego del Mapeo de la Red, a estas alturas vamos a saber IP, Gateway, DNS, cantidad aproximada de dispositivos en linea, y es muy posible que tengamos incluso algunos Host Identificados, me refiero a posibles servicios como HTTP, FTP, ROUTERS, EMAIL-SERVER, SMB/SAMBA, etc... porque le hemos dedicado tiempo a la enumeración de servicios y de seguro encontramos información interesante.

Ahora es tiempo de ensuciarnos las manos e interactuar un poquito con los objetivos.



## Masscan
Este es el escáner de puertos de Internet más rápido. Puede escanear todo Internet en menos de 6 minutos, transmitiendo 10 millones de paquetes por segundo.

Produce resultados similares a nmap, el escáner de puertos más famoso. Internamente, funciona más como scanrand, unicornscan y ZMap, utilizando transmisión asincrónica. La principal diferencia es que es más rápido que estos otros escáneres. Además, es más flexible, permitiendo rangos de direcciones arbitrarias y rangos de puertos.

NOTA: masscan usa una pila TCP / IP personalizada. Cualquier otra cosa que no sea escaneos de puertos simples causará conflictos con la pila TCP / IP local. Esto significa que debe usar la opción -S para usar una dirección IP separada o configurar su sistema operativo para cortafuegos de los puertos que utiliza masscan.

### Acceder a la configuración de masscan

	┌─[cs-academy@parrotsec]─[~]
	└──╼ $masscan -p1234 --echo
	rate =     100.00
	randomize-hosts = true
	seed = 3278426277001722401
	shard = 1/1
	# ADAPTER SETTINGS
	adapter = 
	adapter-ip = 0.0.0.0
	adapter-mac = 00:00:00:00:00:00
	router-mac = 00:00:00:00:00:00
	# OUTPUT/REPORTING SETTINGS
	output-format = unknown(0)
	show = open,,
	output-filename = 
	rotate = 0
	rotate-dir = .
	rotate-offset = 0
	rotate-filesize = 0
	pcap = 
	# TARGET SELECTION (IP, PORTS, EXCLUDES)
	retries = 0
	ports = 1234
	capture = cert
	nocapture = html
	nocapture = heartbleed
	nocapture = ticketbleed
	min-packet = 60
	
Usar un archivo de configuración personalizado

	┌─[root@parrot]─[~]
	└──╼ # masscan -c <filename>

### Ejemplo de uso de masscan

Escanear puertos seleccionados (-p22,80,445) en una subred determinada (192.168.1.0/24):


### Detección de Sistema Operativo y Servicios(Nmap).

Primeramente determinar qué sistemas operativos están usando los dispositivos de esta red, en este punto la lista que teníamos de dispositivos va a quedar dividida en 2, 3, 4 o mas, separando routers, sistemas linux, windows, apple... etc. El siguiente paso es Enumerar Servicios a cada uno de los objetivos, el caballo de atila para esto es Nmap (existen muchos otros pero vamos a usar Nmap). No vamos a cubrir todos los tipos de escaneo con nmap, pero sí vamos a mencionar algunos.

#### Deteccion de Servicios con Nmap:

Escaneo simple a un objetivo

Al ejecutar Nmap sin opciones de línea de comando, se realizará una exploración básica en el objetivo especificado

	┌─[root@parrot]─[~]
	└──╼ # nmap [target]

Escaneo simple a multiples objetivos

Nmap puede usarse para escanear múltiples hosts al mismo tiempo. La forma más fácil de hacer esto es encadenar las direcciones IP de destino o los nombres de host en la línea de comando

	┌─[root@parrot]─[~]
	└──╼ # nmap [target1] [target2]

Escaneo simple a un rango de objetivos

	┌─[root@parrot]─[~]
	└──╼ # nmap [target-range]

Escaneo simple de una subnet

	┌─[root@parrot]─[~]
	└──╼ # nmap [subnet]

Escaneo a una lista de objetivos

	┌─[root@parrot]─[~]
	└──╼ # nmap -iL [patch_targets]

Escaneo de una subnet excluyendo objetivos

	┌─[root@parrot]─[~]
	└──╼ # nmap [subnet] --exclude [exclude_host]

Nota:
  También podemos excluir múltiples objetivos o un rango de objetivos.
Escaneo de una subnet excluyendo objetivos desde una lista

	┌─[root@parrot]─[~]
	└──╼ # nmap [subnet] --exludefile [patch_file]

Escaneo Agresivo de un objetivo/objetivos/subnet/lista de objetivos

El parámetro -A indica a Nmap que realice una exploración agresiva.

	┌─[root@parrot]─[~]
	└──╼ # nmap -A [target|subnet]


#### Opciones de descubrimiento
No hagas ping:

La opción -Pn indica a Nmap que omita la comprobación de descubrimiento predeterminada y realice un escaneo completo de puertos en el objetivo. Esto es útil al escanear hosts que están protegidos por un firewall que bloquea las sondas de ping.

	┌─[root@parrot]─[~]
	└──╼ # nmap -Pn [target]

Ping Scan: 

Es básicamente un escaneo de descubrimiento

	┌─[root@parrot]─[~]
	└──╼ # nmap -sP [target|subnet]

TCP SYN Ping:

	┌─[root@parrot]─[~]
	└──╼ # nmap -PS target

TCP ACK Ping:

-PA realiza un ping TCP ACK en el objetivo especificado.

	┌─[root@parrot]─[~]
	└──╼ # nmap -PA [target]

UDP Ping:

	┌─[root@parrot]─[~]
	└──╼ # nmap -PU [target]



SCTP INIT Ping:

Este método de descubrimiento intenta localizar hosts utilizando el Stream Control Transmission Protocol (SCTP). SCTP se usa típicamente en sistemas basados en telefonía.

	┌─[root@parrot]─[~]
	└──╼ # nmap -PY [target]

ICMP Timestamp Ping:

	┌─[root@parrot]─[~]
	└──╼ # nmap -PP [target]



Ping de máscara de dirección ICMP:

Esta consulta ICMP no convencional (similar a la opción -PP) intenta hacer ping al host especificado utilizando registros ICMP alternativos. Este tipo de ping puede ocasionalmente pasar a escondidas un firewall que está configurado para bloquear las solicitudes de eco estándar.

	┌─[root@parrot]─[~]
	└──╼ # nmap -PM [target]



IP Protocol Ping:

	┌─[root@parrot]─[~]
	└──╼ # nmap -PO [target]



Ping ARP:

La opción -PR está implícita automáticamente al escanear la red local. Este tipo de descubrimiento es mucho más rápido que los otros métodos de ping descritos en esta guía. Eso también tiene el beneficio adicional de ser más preciso porque los hosts LAN no pueden bloquear Solicitudes ARP (incluso si están detrás de un firewall).

	┌─[root@parrot]─[~]
	└──╼ # nmap -PR [target]



Traceroute:

La información que se muestra es similar a los comandos traceroute o tracepath encontrado en sistemas Unix y Linux, con la ventaja adicional de que el rastreo de Nmap

es muy superior a estos comandos.

	┌─[root@parrot]─[~]
	└──╼ # nmap --traceroute [target]



Forzar resolución inversa de DNS:

De forma predeterminada, Nmap solo realizará DNS inverso para los hosts que parecen estar en línea. La opción -R es útil cuando se realiza un reconocimiento en un bloque de direcciones IP, Nmap intentará resolver la información inversa de DNS de cada dirección IP. La información inversa de DNS puede revelar información interesante sobre la dirección IP de destino (incluso si está fuera de línea o está bloqueando las sondas de Nmap).

	┌─[root@parrot]─[~]
	└──╼ # nmap -R [target]



Deshabilitar la resolución inversa de DNS:

El DNS inverso puede reducir drásticamente el escaneo de Nmap. Usando la opción -n reduce en gran medida los tiempos de escaneo, especialmente cuando se escanea una gran cantidad de hosts. Esta opción es útil si no le importa la información de DNS para el sistema objetivo y prefieren realizar un escaneo que produce resultados más rápidos.

	┌─[root@parrot]─[~]
	└──╼ # nmap -n [target]

Método alternativo de búsqueda de DNS:

La opción --system-dns le indica a Nmap que use la resolución DNS del sistema host en lugar de su propio método interno. Esta opción rara vez se usa, ya que es mucho más lenta que el método predeterminado, sin embargo, puede que sea útil al solucionar problemas de DNS con Nmap.

	┌─[root@parrot]─[~]
	└──╼ # nmap --system-dns [target]



Especifique manualmente los servidores DNS:

La opción --dns-servers se usa para especificar manualmente los servidores DNS que se consultarán al escanear.

	┌─[root@parrot]─[~]
	└──╼ # nmap --dns-servers [server1, server2, server3] [target]



#### Advanced Scanning Options


TCP SYN Scan:

La opción -sS realiza una exploración TCP SYN. El escaneo TCP SYN predeterminado intenta identificar los 1000 puertos TCP más utilizados. Esto evita que muchos sistemas registren un intento de conexión desde su escaneo.

	┌─[root@parrot]─[~]
	└──╼ # nmap -sS [target]

Escaneo de conexión TCP:

La opción -sT realiza una exploración de conexión TCP.

	┌─[root@parrot]─[~]
	└──╼ # nmap-sT [target]



Escaneo UDP

La opción -sU realiza una exploración UDP (User Datagram Protocol). Al realizar una auditoría de red, siempre es una buena idea verificar servicios TCP y UDP para obtener una imagen más completa del objetivo

	┌─[root@parrot]─[~]
	└──╼ # nmap -sU [target]

Escaneo TCP NULL

La opción -sN realiza un escaneo TCP NULL. El envío de paquetes NULL a un objetivo es un método para engañar a un sistema con firewall para generar una respuesta.

	┌─[root@parrot]─[~]
	└──╼ # nmap -sN [target]

TCP FIN Scan

La opción -sF realiza un escaneo TCP FIN. Este es otro método de enviar paquetes inesperados a un objetivo en un intento de producir resultados desde un sistema protegido por un firewall.

Escaneo TCP ACK

La opción -sA se puede usar para determinar si el sistema de destino está protegido por un firewall.

	┌─[root@parrot]─[~]
	└──╼ # nmap -sA [target]



#### Port Scanning Options


Realizar un escaneo rápido:

La opción -F indica a Nmap que realice un escaneo de solo los 100 puertos más utilizados.

	┌─[root@parrot]─[~]
	└──╼ # nmap -F [target]

Escanear puertos específicos:

La opción -p se usa para indicar a Nmap que escanee los puertos especificados.

	┌─[root@parrot]─[~]
	└──╼ # nmap -p 445 [target]

o

	┌─[root@parrot]─[~]
	└──╼ # nmap -p 135,139,445,3389 [target]



Escanear puertos por nombre:

La opción -p se puede usar para escanear puertos por nombre.

	┌─[root@parrot]─[~]
	└──╼ # nmap -p smbp,smb,http [target]

Escanear puertos por protocolo:

Especificar un prefijo T: o U: con la opción -p le permite buscar una combinación específica de puerto y protocolo.

	┌─[root@parrot]─[~]
	└──╼ # nmap -p U:[UDP ports],T:[TCP ports] [target]

Escanear todos los puertos:

La opción -p "*" es un comodín utilizado para escanear todos los 65.535 puertos TCP/IP en el destino especificado.

	┌─[root@parrot]─[~]
	└──╼ # nmap -p "*" [target]

Escanear puertos más usados:

La opción --top-ports se usa para escanear el número especificado de puertos mejor clasificados.

	┌─[root@parrot]─[~]
	└──╼ # nmap --top-ports 100 [target]

Sistema operativo y detección de servicio

El parámetro -O habilita la función de detección del sistema operativo de Nmap.

	┌─[root@parrot]─[~]
	└──╼ # nmap -O [target]

Intentar adivinar un sistema operativo desconocido

Si Nmap no puede identificar con precisión el sistema operativo, puede forzarlo a adivinar utilizando la opción --osscan-guess.

	┌─[root@parrot]─[~]
	└──╼ # nmap -O --osscan-guess [target]

Detección de versión de servicio:

El parámetro -sV habilita la función de detección de versión de servicio de Nmap.

	┌─[root@parrot]─[~]
	└──╼ # nmap -sV 192.168.11.131



#### Opciones de salida


Guardar salida en un archivo de texto:

El parámetro -oN guarda los resultados de un escaneo en un archivo de texto sin formato.

	┌─[root@parrot]─[~]
	└──╼ # nmap -oN [scan.txt] [target]

Guardar salida en un archivo XML:

El parámetro -oX guarda los resultados de una exploración en un archivo XML.

	┌─[root@parrot]─[~]
	└──╼ # nmap -oX [scan.xml] [target]

Salida de todos los tipos de archivos admitidos:

El parámetro -oA guarda la salida de un escaneo en formatos de texto, grepable y XML.

	┌─[root@parrot]─[~]
	└──╼ # nmap -oA [filename] [target]

Mostrar estadísticas de escaneo:

La opción --stats-every se puede usar para mostrar periódicamente el estado del actual escanear.

	┌─[root@parrot]─[~]
	└──╼ # nmap -sV --stats-every 5 192.168.11.130

Salida detallada

La opción -v se usa para habilitar la salida detallada.

	┌─[root@parrot]─[~]
	└──╼ # nmap -sT -sV -A -v [target]

Depuración

La opción -d habilita la salida de depuración.

	┌─[root@parrot]─[~]
	└──╼ # nmap -d[1-9] [target]

Mostrar solo los puertos abiertos:

El parámetro --open indica a Nmap que solo muestre puertos abiertos.

	┌─[root@parrot]─[~]
	└──╼ # nmap --open [target]

Especifique qué interfaz de red usar:

La opción -e se usa para especificar manualmente qué interfaz de red debe usar Nmap.

	┌─[root@parrot]─[~]
	└──╼ # nmap -e eth0 [target]


#### Evadir cortafuegos

Paquetes de fragmentos:

La opción -f se usa para fragmentar las sondas en paquetes de 8 bytes.

	┌─[root@parrot]─[~]
	└──╼ # nmap -f [target]

Especificar una MTU específica

La opción --mtu se usa para especificar una MTU personalizada (Unidad de transmisión máxima). El MTU debe ser un múltiplo de 8

	┌─[root@parrot]─[~]
	└──╼ # nmap --mtu [8|16|24|32] [target]

Usa un señuelo:

La opción -D se usa para enmascarar un escaneo de Nmap utilizando uno o más señuelos. Al realizar un escaneo señuelo, Nmap falsificará paquetes adicionales del número especificado de direcciones señuelo. Esto efectivamente hace parecer que el objetivo está siendo escaneado por múltiples sistemas simultáneamente. El uso de señuelos hace a la fuente real del escaneo  "mezclarse con la multitud", lo que hace que sea más difícil de rastrear de dónde viene el escaneo.

	┌─[root@parrot]─[~]
	└──╼ # nmap -D [decoy1,decoy2,etc|RND:number] [target]

Especifique manualmente un número de puerto de origen:

La opción --source-port se usa para especificar manualmente el número de puerto de origen de una Investigacion o escaneo.

	┌─[root@parrot]─[~]
	└──╼ # nmap --source-port [port] [target]

Dirección MAC falsa:

--spoof-mac se utiliza para falsificar la dirección MAC de un dispositivo ethernet.

	┌─[root@parrot]─[~]
	└──╼ # nmap --spoof [vendor|MAC|0] [target]


#### Nmap NSE Scripts


Ejecutar scripts individuales:

	┌─[root@parrot]─[~]
	└──╼ # nmap --script [script] [target]

Ejecutar múltiples scripts:

	┌─[root@parrot]─[~]
	└──╼ # nmap --script [script1,script2,script3] [target]

Categorías de scripts:

	┌─[root@parrot]─[~]
	└──╼ # nmap --script [all|auth|default|vuln|discovery|external|intrusive|malware|safe] [target]

Actualice la base de datos de script:

	┌─[root@parrot]─[~]
	└──╼ # nmap --script-updatedb



Nota:
Puede ver los scripts disponibles en /usr/share/nmap/scripts/ *


Combina múltiples opciones

Si aún no lo ha notado, Nmap le permite combinar múltiples opciones para producir un escaneo personalizado exclusivo para sus necesidades.

	┌─[root@parrot]─[~]
	└──╼ # nmap [options] [target]