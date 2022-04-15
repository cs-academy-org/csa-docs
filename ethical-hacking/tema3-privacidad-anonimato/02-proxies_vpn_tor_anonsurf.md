# Privacidad y Anonimato
## Introducción

A la hora de realizar una prueba de penetración es importante garantizar cierta capa de privacidad y anonimato, utilizando proxies, VPNs, TOR, etc, podemos ocultar nuestra dirección IP y minimizar la detección de actividad sospechosa, evitar censuras, bloqueos e incluso falsos positivos. Quizás te preguntes: ¿Por qué ocultar nuestra dirección IP si hemos sido contratados por una empresa para realizar una prueba de penetración legal? la respuesta es sencilla, muchas empresas u organizaciones tienen Sistemas de Detección de Intrusos (IDS) que analizan todo el tráfico generado desde y hacia sus servicios, así como los hackers de sombrero negro utilizan estas técnicas para ocultar sus actividades criminales, también un pentester debe conoserlas, por dos motivos, el primero es evitar ser detectado (por IDS o un administrador de sistemas competente) a la hora de realizar una prueba de penetración legal y el segundo es saber hacer una contramedida a estas técnicas porque un día nos puede tocar trabajar en un RedTeam, simular ser los atacantes de la vida real y otro día nos puede tocar trabajar en un BlueTeam, defender la organización para la que trabajamos y debemos estar un paso adelante de los atacantes, por ellos debemos conocer estas técnicas y herramientas para poder hacer una contramedida. 



## Proxies
   
  Un proxy es un servidor o servicio que hace de intermediario entre la comunicación de un cliente y un servidor. Los servidores proxies son comúnmente utilizados en redes empresariales para controlar accesos, ancho de banda, guardar un registro del tráfico generado por los usuarios, restringir determinados servicios y/o protocolos, mejorar el rendimiento de la conectividad implementando cache web, entre otros ejemplos. Ya que esto es un curso de Hacking Ético o Ciberseguridad Ofensiva, vamos a tratar de enfocar todos estos ejemplos a escenarios de la vida real en la que un atacante necesita ocultar su dirección IP para poder acceder a un servicio determinado, ya sea porque no tiene acceso desde su dirección IP o porque quiere ocultar el origen de la solicitud que va a enviar a un ordenador remoto, por ejemplo, si un atacante quiere acceder a lab.cs-academy.org pero no quiere dejar registros de que accedió desde su dirección IP pública, puede utilizar un servidor proxy como intermediario, de esta forma en los registros de lab.cs-academy.org no queda la dirección IP del atacante. Los proxies también nos permiten evadir censura y/o restricciones. 

    

## VPN

  VPN es el acrónimo Virtual Private Network (Red Privada Virtual). Las VPNs se suelen utilizar para extender de manera segura el acceso a una red de area local desde fuera, o sea desde una red pública como internet. Permiten que un ordenador que se encuentra fuera de las instalaciones de una empresa, acceda a la red privada de dicha empresa como si fuera un ordenador más dentro de esa red, estableciendo una conexión cifrada punto a punto. Algunos de los ejemplos más comunes son: conectar varias sucursales de una empresa utilizando como vínculo internet, permitir a los miembros del equipo de TI conectarse a la red de la empresa desde sus hogares o desde otros sitios remotos como un hotel y poder desarrollar actividades de administración, monitoreo y soporte a usuarios de la empresa sin necesidad de estar físicamente en el lugar, todo esto utilizando internet como infraestructura.

        

Los cuatro tipos de VPN o arquitecturas más comunes de conexiones VPN son:

- VPN de Acceso Remoto
- VPN Punto a Punto
- Tunneling
- VPN over LAN
        
Algunas de estas vamos a utilizarlas en esta y en futuras clases.

Ventajas:
- Integridad, Confidencialidad y Seguridad de datos.
- Protegen información de nuestro ordenador, por ejemplo: nuestra dirección IP.
- Evitar censura, nos da acceso a contenido que podría no estar disponible en nuestra región.


Ejemplo práctico sobre el uso de OpenVPN.
 


Reproducir Vídeo
 
Para más ejemplos sobre el uso de VPN vea el contenido de la video clase de esta unidad.



## Tor
  The Onion Router, más conocido como Tor es un proyecto cuyo objetivo es el desarrollo de una red de comunicaciones distribuida de baja latencia y superpuesta a internet, en la que el encaminamiento de los datos intercambiados entre sus usuarios no revela su identidad, o sea, su dirección IP, además de esto, mantiene la integridad de la información que viaja por ella cifrando toda la comunicación. Es por ello que esta tecnología pertenece a la llamada darknet o red oscura, que no se debe confundir con la Deep Web.

Ejemplo práctico sobre el uso de Tor y TorBrowser:

 


Reproducir Vídeo
     
## Anonsurf


¿Que es Anonsurf y como funciona?
Es un script desarrollado por algunos miembros del equipo de desarrollo de ParrotOS. Se dice que es el modo anónimo de Parrot Security ya que en parte es una de las herramientas que diferencia a la distribución del resto, a pesar de esto el equipo de desarrollo la considera una herramienta que puede ser utilizada por cualquier usuario en cualquier distribución GNU/Linux, o sea, un producto de The Parrot Project. Anonsurf es una combinación de Bash, TOR o i2p e iptables el cual es utilizado para direccionar todo el tráfico del sistema a través de la red TOR o i2p (este último ha quedado descontinuado en las últimas versiones). El cliente cuenta con una pequeña interfaz gráfica y también puede ser ejecutado desde el terminal. Su uso es muy sencillo.

### Opciones de Anonsurf:
	┌─[✗]─[xc0d3@cs-academy]─[~]
	└──╼ $ anonsurf
	Parrot AnonSurf Module (v 2.13)
		Developed by Lorenzo "Palinuro" Faletra <palinuro@parrotsec.org>
			     Lisetta "Sheireen" Ferrero <sheireen@parrotsec.org>
			     Francesco "Mibofra" Bonanno <mibofra@parrotsec.org>
		Maintained by Nong Hoang "DmKnght" Tu <dmknght@parrotsec.org>
			and a huge amount of Caffeine, Mountain Dew + some GNU/GPL v3 stuff
		Obfs4 Bridges: M. "meu" Emrah Ünsür
		Extended by Daniel "Sawyer" Garcia <dagaba13@gmail.com>
		Usage:
		┌──[xc0d3@cs-academy]─[/home/xc0d3]
		└──╼ $ anonsurf {start|stop|restart|enable-boot|disable-boot|change|status}
		 start - Start system-wide TOR tunnel
		 start-bridge - Start system-wide TOR tunnel with Obfs4 Bridge support
		 stop - Stop anonsurf and return to clearnet
		 restart - Combines "stop" and "start" options
		 enable-boot - Enable AnonSurf at boot
		 disable-boot - Disable AnonSurf at boot
		 changeid - Restart TOR to change identity
		 status - Check if AnonSurf is working properly
		 myip - Check your ip and verify your tor connection
		 dns - Fast set / fix DNS. Please use /usr/bin/dnstool.
	Dance like no one's watching. Encrypt like everyone is.


Ejemplo práctico sobre el uso de Anonsurf:
 


Reproducir Vídeo
 

## Proxychains
 Proxychain-ng o Proxychains como se le suele llamar en sus últimas versiones, es una herramienta desarrollada en C y distribuida bajo licencia GPL, funciona como servidor proxy local, soporta los protocolos HTTP(S), SOCKS4 y SOCKS5, puede ser utilizado en cualquier distribución GNU/Linux, en sistemas BSD y macOS. Esta herramienta permite que la conexión TCP de un programa determinado sea proxificada siguiendo una cadena de proxies hasta su destino, esta lista de proxies debe ser configurada previamente por el usuario en el archivo /etc/proxychains.conf.

Ejemplo práctico sobre el uso de Proxychains:

 


Reproducir Vídeo
 
Para más información sobre este módulo vea la video clase.