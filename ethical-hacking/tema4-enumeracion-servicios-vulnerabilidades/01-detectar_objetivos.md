# Como detectar objetivos en la red

## ping

Como programa, ping es una utilidad de diagnóstico en redes de computadoras que comprueba el estado de la comunicación del anfitrión local con uno o varios equipos remotos de una red que ejecuten IP. Se vale del envío de paquetes ICMP de solicitud (ICMP Echo Request) y de respuesta (ICMP Echo Reply). Mediante esta utilidad puede diagnosticarse el estado, velocidad y calidad de una red determinada.

Ejecutando Ping de solicitud, el anfitrión local (en inglés, local host) envía un mensaje ICMP, incrustado en un paquete IP. El mensaje ICMP de solicitud incluye, además del tipo de mensaje y el código del mismo, un número identificador y una secuencia de números, de 32 bits, que deberán coincidir con el mensaje ICMP de respuesta; además de un espacio opcional para datos. Como protocolo ICMP no se basa en un protocolo de capa de transporte como TCP o UDP y no utiliza ningún protocolo de capa de aplicación.

Muchas veces se utiliza para medir la latencia o tiempo que tardan en comunicarse dos puntos remotos, y por ello, se utiliza el término PING para referirse al retardo o latencia (en inglés, lag) de la conexión en los juegos en red.

En nuestro caso vamos a utilizar el comando ping para detectar objetivos en la red, combinando su ejecución básica con un poquito de bash scripting para obtener resultados personalizados.


Veamos algunos ejemplos:
- Haciendo ping a un host:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ping 192.168.56.104
	PING 192.168.56.104 (192.168.56.104) 56(84) bytes of data.
	64 bytes from 192.168.56.104: icmp_seq=1 ttl=64 time=0.349 ms
	64 bytes from 192.168.56.104: icmp_seq=2 ttl=64 time=0.304 ms
	64 bytes from 192.168.56.104: icmp_seq=3 ttl=64 time=0.324 ms
	64 bytes from 192.168.56.104: icmp_seq=4 ttl=64 time=0.392 ms
	^C
	--- 192.168.56.104 ping statistics ---
	4 packets transmitted, 4 received, 0% packet loss, time 3062ms
	rtt min/avg/max/mdev = 0.304/0.342/0.392/0.032 ms


- Haciendo un solo ping a un host:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ping -c 1 192.168.56.104
	PING 192.168.56.104 (192.168.56.104) 56(84) bytes of data.
	64 bytes from 192.168.56.104: icmp_seq=1 ttl=64 time=0.394 ms
	--- 192.168.56.104 ping statistics ---
	1 packets transmitted, 1 received, 0% packet loss, time 0ms
	rtt min/avg/max/mdev = 0.394/0.394/0.394/0.000 ms


- Haciendo ping a un host que no está en linea:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ping -c 1 192.168.56.103
	PING 192.168.56.103 (192.168.56.103) 56(84) bytes of data.
	From 192.168.56.1 icmp_seq=1 Destination Host Unreachable
	--- 192.168.56.103 ping statistics ---
	1 packets transmitted, 0 received, +1 errors, 100% packet loss, time 0ms


Si comparamos ambas salidas, podemos llegar a la conclusión de que son totalmente distintas. Para obtener una salida mas personalizada donde solo veamos resultados positivos podemos utilizar el comando grep para filtrar, utilizando la cadena "64 bytes from", veamos un ejemplo:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ping -c 1 192.168.56.104 | grep "64 bytes"
	64 bytes from 192.168.56.104: icmp_seq=1 ttl=64 time=0.391 ms
Ya tenemos solo la linea donde aparece solamente el host en linea, ahora el objetivo es quedarnos solamente con su dirección IP, para ello vamos a utilizar el comando cut para quedarnos con el 4 campo entre los campos separados por espacios, la opción -d es para establecer un delimitador y la opción -f es para seleccionar uno o varios campos separados por coma (,). Veamos un ejemplo:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ping -c 1 192.168.56.104 | grep "64 bytes" | cut -d " " -f 4
	192.168.56.104:

Por último filtramos nuevamente con el comando cut, esta vez estableciendo como delimitador ":" y quedándonos con el primer campo:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ping -c 1 192.168.56.104 | grep "64 bytes" | cut -d " " -f 4 | cut -d ":" -f 1
	192.168.56.104


Ya tenemos la dirección IP limpia, el objetivo es que este proceso sea automatizado para todo un rango de direcciones IP, para ello vamos a crear un bucle for estableciendo la secuencia deseada. Veamos un ejemplo:


	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ for ip in `seq 1 254`; do ping -c 1 192.168.56.$ip | grep "64 bytes" | cut -d " " -f 4 | cut -d ":" -f 1; done
	192.168.56.1
	192.168.56.100
	192.168.56.102
	192.168.56.103
	192.168.56.104
	192.168.56.108


Ya el script es funcional, pero toma mucho tiempo en detectar las direcciones IP que no están en linea, esto podemos mejorarlo ajustando el tiempo de espera del comando ping con la opción -W o bien creando un script y corriendo todo esto en segundo plano. Veamos un ejemplo:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ cat scripts/pingscan
	#!/bin/bash
	#Banner
	echo " ____  _               ____ "
	echo "|  _ \(_)_ __   __ _  / ___|  ___ __ _ _ __  _ __   ___ _ __ "
	echo "| |_) | | \`_ \ / _\` | \___ \ / __/ _\` | '_ \| '_ \ / _ \ '__|"
	echo "|  __/| | | | | (_| |  ___) | (_| (_| | | | | | | |  __/ |"
	echo "|_|   |_|_| |_|\__, | |____/ \___\__,_|_| |_|_| |_|\___|_| "
	echo "               |___/"
	if [ "$#" -ne "1" ]
	then
	echo "Usage: ./pingscanner.sh [network]"
	echo "Example	./pingscanner.sh 192.168.1"
	else
	for x in `seq 1 254` ; do
	ping -c 1 $1.$x | grep "64 bytes" | cut -d ' ' -f 4 | cut -d: -f 1 | sort -n &
	done
	fi
Al ejecutar el script todas las solicitudes de ping se envían en segundo plano y vemos solamente los resultados positivos, o sea, los hosts que se encuentran en linea.

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ ./scripts/pingscan 192.168.56
	 ____  _               ____ 
	|  _ \(_)_ __   __ _  / ___|  ___ __ _ _ __  _ __   ___ _ __ 
	| |_) | | `_ \ / _` | \___ \ / __/ _` | '_ \| '_ \ / _ \ '__|
	|  __/| | | | | (_| |  ___) | (_| (_| | | | | | | |  __/ |
	|_|   |_|_| |_|\__, | |____/ \___\__,_|_| |_|_| |_|\___|_| 
	               |___/
	192.168.56.1
	192.168.56.100
	192.168.56.102
	192.168.56.103
	192.168.56.104
	192.168.56.108


De esta forma podemos detectar rápidamente todos los objetivos que se encuentren activos en la red utilizando el protocolo ICMP, lista de objetivos que posteriormente podemos utilizar con herramientas como nmap.



## arping


El comando arping funciona muy similar al comando ping, solo que el lugar de utilizar el protocolo ICMP consulta directamente la tabla ARP, suele ser más efectivo porque al utilizar el comando ping si un hosts tiene las respuestas ICMP desactivadas no nos va a dar resultados, ni positivo ni negativos, en cambio el comando arping si nos va a dar resultados, ya que no utiliza el protocolo ICMP. Veamos un ejemplo de uso:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ sudo arping 192.168.56.104 -i vboxnet0
	ARPING 192.168.56.104
	60 bytes from 08:00:27:5e:56:6f (192.168.56.104): index=0 time=301.501 usec
	60 bytes from 08:00:27:5e:56:6f (192.168.56.104): index=1 time=308.140 usec
	60 bytes from 08:00:27:5e:56:6f (192.168.56.104): index=2 time=371.309 usec
	^C
	--- 192.168.56.104 statistics ---
	3 packets transmitted, 3 packets received,   0% unanswered (0 extra)
	rtt min/avg/max/std-dev = 0.302/0.327/0.371/0.031 ms


El comando ping debe ser utilizado como super usuario, ya que necesita acceder a la tabla ARP para realizar su trabajo. También se debe utilizar la opción -i para establecer un adaptador por el cual queremos realizar esta solicitud.


Intenta realizar un script similar al del comando ping, pero utilizando el comando arping.

## arp-scan

Esta herramienta viene muy bien a la hora de detectar objetivos en la red, funciona muy similar al comando arping, utiliza la tabla ARP para buscar objetivos en la red y no solo imprime dirección IP y MAC del adaptador de Host, también nos muestra el fabricante del dispositivo de red del host remoto. Veamos un ejemplo de uso:

	┌─[cs-academy@parrotsec]─[~]
	└──╼ $sudo arp-scan -I eth1 192.168.56.1/24
	[sudo] password for cs-academy: 
	Interface: eth1, type: EN10MB, MAC: 08:00:27:5e:56:6f, IPv4: 192.168.56.104
	WARNING: host part of 192.168.56.1/24 is non-zero
	Starting arp-scan 1.9.7 with 256 hosts (https://github.com/royhills/arp-scan)
	192.168.56.1	0a:00:27:00:00:00	(Unknown: locally administered)
	192.168.56.100	08:00:27:47:a7:81	PCS Systemtechnik GmbH
	3 packets received by filter, 0 packets dropped by kernel
	Ending arp-scan 1.9.7: 256 hosts scanned in 1.983 seconds (129.10 hosts/sec). 2 responded


## hping3

hping es un ensamblador/analizador de paquetes TCP/IP orientado a la línea de comandos. La interfaz está inspirada en el comando ping, pero hping no solo puede enviar solicitudes de eco ICMP. Es compatible con los protocolos TCP, UDP, ICMP y RAW-IP, tiene un modo de trazado de ruta, la capacidad de enviar archivos entre un canal cubierto y muchas otras características.

Si bien hping se usó principalmente como herramienta de seguridad en el pasado, las personas que no se preocupan por la seguridad pueden usarlo de muchas maneras para probar redes y hosts. Un subconjunto de las cosas que puedes hacer usando hping:

-Prueba de firewall

- Escaneo de puertos avanzado

- Pruebas de red, utilizando diferentes protocolos, TOS, fragmentación.

- Descubrimiento manual de MTU de ruta

- Traceroute avanzado, bajo todos los protocolos soportados.

- Huellas digitales remotas del sistema operativo

- Adivinación remota del tiempo de actividad

- Auditoría de pilas TCP/IP

- hping también puede ser útil para los estudiantes que están aprendiendo TCP / IP.



## Netdiscover


Netdiscover es otra herramienta que nos permite detectar objetivos en la red, dentro y fuera de nuestra sub-net, utiliza varios protocolos como ICMP, TCP, UDP y un modo de sniffing que permite capturar tráfico generado por un host remoto. Es otra herramienta que podemos incorporar en nuestro arsenal para encontrar objetivos en una red. Su uso es muy sencillo, vasta con decirle en que interfaz queremos escanear y la herramienta hace su trabajo. Un punto a tener en cuenta es que netdiscover debemos ejecutarla como super usuario.

	┌─[cs-academy@parrotsec]─[~]
	└──╼ $sudo netdiscover -i eth0
	 Currently scanning: Starting.   |   Screen View: Unique Hosts                                                                                   
	                                                                                                                                                 
	 0 Captured ARP Req/Rep packets, from 0 hosts.   Total size: 0                                                                                   
	 _____________________________________________________________________________
	   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
	 -----------------------------------------------------------------------------


Para ver más ejemplos prácticos sobre como detectar objetivos en la red vea la [Video Clase] Detectando objetivos en la red