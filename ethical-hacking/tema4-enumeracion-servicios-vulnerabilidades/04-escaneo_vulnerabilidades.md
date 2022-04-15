# Introducción al Escaneo, Manejo de Vulnerabilidades y Búsqueda de Exploits

## Introducción


Ahora llegó el momento de utilizar toda esa información recopilada para lanzar uno o varios escaneos de vulnerabilidades y lograr descubrir malas configuraciones en los servicios de la víctima o posibles vulnerabilidades conocidas, se debe hacer uso de todo lo que se encontró durante las etapas de Reconocimiento, Enumeración de Servicios y Escaneo de Puertos. A muchos les surgen estas preguntas; 

- ¿Qué debo hacer con los resultados encontrados anteriormente? 
- ¿Cómo encuentro servicios vulnerables?
- ¿Cómo gestiono esas vulnerabilidades?
- ¿Dónde encuentro programas, códigos que me permitan saber si un servicio es vulnerable?


En este documento estaremos respondiendo esas y otras interrogantes, así como recomendando servicios online y herramientas que nos permiten completar esta etapa de la prueba de penetración.



### ¿Qué es una vulnerabilidad?

Explicado de forma sencilla; Una vulnerabilidad es un bug o falla presente a nivel de sistema operativo, software o hardware que permite a un tercero modificar el comportamiento de determinado software o hardware permite a un atacante violar la confidencialidad, integridad, disponibilidad, control de acceso del sistema o de sus datos y aplicaciones. Estas vulnerabilidades son producto de fallos producidos por el mal diseño de un software, sin embargo, una vulnerabilidad también puede ser producto de las limitaciones propias de la tecnología para la que fue diseñado.



### Típos de vulnerabilidades


#### Vulnerabilidades de desbordamiento de buffer
Esta condición se cumple cuando una aplicación no es capaz de controlar la cantidad de datos que se copian en buffer, de forma que si esa cantidad es superior a la capacidad del buffer los bytes sobrantes se almacenan en zonas de memoria adyacentes, sobrescribiendo su contenido original. Este problema se puede aprovechar para ejecutar código que le otorga a un atacante privilegios de administrador.



#### Vulnerabilidades de error de formato de cadena (format string bugs)
El motivo fundamental de los llamados errores de cadena de formato es la condición de aceptar sin validar la entrada de datos proporcionada por el usuario. Este es un error de diseño de la aplicación, es decir que proviene de descuidos en su programación. En este sentido el lenguaje de programación más afectado por este tipo de vulnerabilidades es C/C++. Un ataque perpetrado utilizando este método definitivamente conduce a la ejecución de código arbitrario y al robo de información y datos del usuario.



#### Vulnerabilidades de Cross Site Scripting (XSS)
Las vulnerabilidades del tipo Cross Site Scripting (XSS) son utilizadas en ataques en donde las condiciones permitan ejecutar scripts de lenguajes como VBScript o JavaScript. Es posible encontrar este tipo de situaciones en cualquier aplicación que se utilice para mostrar información en un navegador web cualquiera, que no se encuentre debidamente protegido contra estos ataques.



#### Vulnerabilidades de Inyección SQL
Las llamadas “vulnerabilidades de inyección SQL” se producen cuando mediante alguna técnica se inserta o adjunta código SQL que no formaba parte de un código SQL programado. Esta técnica se utiliza con el propósito de alterar el buen funcionamiento de la base de datos de una aplicación, “inyectando” código foráneo que permita el proceso de datos que el atacante desee.


#### Vulnerabilidades de denegación del servicio
La técnica de denegación de servicio se utiliza con el propósito de que los usuarios no puedan utilizar un servicio, aplicación o recurso. Básicamente lo que produce un ataque de denegación de servicio es la pérdida de la conectividad de la red de la víctima del ataque por el excesivo consumo del ancho de banda de la red o de los recursos conectados al sistema informático

#### Error en la gestión de recursos
Suele suceder cuando una aplicación permite que se consuman un exceso de recursos afectando la disponibilidad de los mismos.

#### Error de Configuración
Problema de configuración de un software o de un servicio como puede ser el caso de un servidor web. Este error puede provocar la inutilización de la página web a través de ataques de denegación de servicios (DoS), el acceso a información privada de servidor e incluso la subida no autorizada de archivos, lo que puede llevar en ocasiones a una ejecución de código.

#### Factor humano
Sí, el factor humano también se considera una causa de vulnerabilidades, en ocasiones los administradores de un sistema cometen negligencias causadas principalmente por la falta de formación.

Permisos, privilegios y/o control de acceso
Fallos a la protección y gestión de permisos que permiten controlar el nivel de acceso de los usuarios, estas vulnerabilidades en ocasiones son de tipo local y permiten a un atacante elevar sus privilegios en un sistema previamente comprometido.



### Clasificación de Vulnerabilidades


#### Vector de acceso (Access Vector (AV))
- Local: vulnerabilidades sólo explotables desde el equipo local.
- Red adyacente: Solo explotables desde la misma red (ataques de N2 por ejemplo).
- Remoto: Accesibles desde cualquier acceso remoto de red.

#### Complejidad de acceso (Access Complexity (AC))
- Alta: Complejidad en el ataque, combinación de ataques y de circustancias.
- Media: Complejidad media para un grupo de usuario.
- Baja: Configuración y usuarios por defecto.
- Autenticación: Describe si…
Múltiple: Si se requieren varias autenticaciones para poder explotar la vulnerabilidad.
- Simple: Si sólo se requiere una autenticación para poder explotar la vulnerabilidad.
- Ninguna: Si no se requiere autenticación para poder explotar la vulnerabilidad.

#### Impacto
- Impacto en la confidencialidad: Si se ve afectada la confidencialidad.
- Impacto en la integridad: Si se ve afectada la integridad.
- Impacto en la disponibilidad: Si se ve afectada la disponibilidad.

#### Explotabilidad
- Explotabilidad: Si existen o no exploits disponibles.
- Facilidad de corrección:Si es fácil de solucionar.
- Fiabilidad del informe de vulnerabilidad: En ocasiones se anuncia la existencia de la vulnerabilidad pero no se confirman las fuentes.

### Estandares de vulnerabilidades
- CVE: Common Vulnerability Enumeration
CVE es una lista de vulnerabilidades y exposiciones de seguridad de la información que tiene como objetivo proporcionar nombres comunes para problemas conocidos públicamente. El objetivo de CVE es facilitar el intercambio de datos entre distintas capacidades de vulnerabilidad (herramientas, repositorios y servicios) con esta “enumeración común”.

- CWE: Common Weakness Enumeration
Es un diccionario universal en línea de las debilidades que se han encontrado en los programas informáticos. El diccionario es mantenido por MITRE Corporation y se puede acceder de forma gratuita a nivel mundial.

- CPE: Common Platform Enumeration
Enumeración de plataforma común (CPE) se especifica un método estructurado para describir e identificar clases de aplicaciones, sistemas operativos y equipos que figuran en los activos informáticos una empresa. La CPE se define a través de un conjunto de especificaciones en un modelo de pila, donde las capacidades se basan en elementos más simples y definidos con más precisión que se especifican en la parte inferior de la pila. La pila consiste en un especificación de diccionario y una especificación de lenguaje de aplicabilidad, que dependen de una especificación de correspondencia de nombre que a su vez depende de una especificación de denominación.

- CAPEC: Common Attack Pattern Enumeration and Classification
El esfuerzo de Enumeración y Clasificación de Patrones de Ataque Común (CAPEC ™) proporciona un catálogo disponible públicamente de patrones comunes de ataque que ayuda a los usuarios a comprender cómo los adversarios explotan las debilidades en las aplicaciones y otras capacidades habilitadas para el ciberespacio.

Los “Patrones de ataque” son descripciones de los atributos y enfoques comunes empleados por los adversarios para explotar las debilidades conocidas en las capacidades habilitadas para el ciberespacio. Los patrones de ataque definen los desafíos que un adversario puede enfrentar y cómo resolverlos. Se derivan del concepto de patrones de diseño aplicados en un contexto destructivo más que constructivo y se generan a partir del análisis en profundidad de ejemplos específicos de exploits del mundo real.

Cada patrón de ataque captura el conocimiento sobre cómo se diseñan y ejecutan las partes específicas de un ataque, y brinda orientación sobre cómo mitigar la efectividad del ataque. Los patrones de ataque ayudan a aquellos que desarrollan aplicaciones, o administran capacidades habilitadas por cyber para comprender mejor los elementos específicos de un ataque y cómo evitar que tengan éxito.


- OVAL: Open Vulnerability and Assessment Language
Es una norma internacional de seguridad de la información para la comunidad que promueve contenidos de seguridad abiertos y disponibles al público, y para estandarizar la transferencia de esta información a través de todo el espectro de herramientas y servicios de seguridad.

- CVSS: Common Vulnerability Scoring System
El Sistema de puntuación de vulnerabilidad común (CVSS) proporciona una forma de capturar las características principales de una vulnerabilidad y producir una puntuación numérica que refleje su gravedad. El puntaje numérico puede traducirse en una representación cualitativa (como baja, media, alta y crítica) para ayudar a las organizaciones a evaluar y priorizar adecuadamente sus procesos de gestión de vulnerabilidad.

CVSS es un estándar publicado utilizado por organizaciones de todo el mundo, y la misión de SIG es continuar mejorando.


### Escaner de Vulnerabilidades
Un escáner de vulnerabilidades es un software diseñado para realizar análisis automáticos de cualquier aplicación, sistema o red en busca de cualquier posible vulnerabilidad que exista. Aunque estas aplicaciones no son capaces de detectar la vulnerabilidad con total precisión, si son capaces de detectar ciertos elementos que podrían desencadenar en una vulnerabilidad, facilitando enormemente el trabajo a los investigadores e ingenieros. Hay varios tipos de escáneres de vulnerabilidades, autenticados, en los que se realizan pruebas y ataques potenciales desde la propia red, y no autenticados, en los que el investigador o hacker ético se intenta hacer pasar por un pirata informático simulando un ataque desde fuera para ver hasta dónde es capaz de llegar analizando (y explotando) posibles vulnerabilidades.

### Algunos escáner de vulnerabilidades

- Suite Aircrack-ng
Las redes Wi-Fi son uno de los puntos más débiles de las empresas, y por ello es uno de los aspectos que mas debemos cuidar. En este punto, Aircrack-ng es sin duda la mejor herramienta para poner a prueba la seguridad de cualquier red Wi-Fi en busca de cualquier posible vulnerabilidad que pueda permitir a cualquier usuario no autorizado hacerse con la contraseña de nuestra red.

Estaremos trabajando con esta suite en el capitulo de Pruebas de penetración en redes Inalámbricas


- Nmap y sus Scripts NSE
Ya hemos estado trabajando con Nmap en clases anteriores para enumerar servicios y hacer escaneos de puertos, pero el poder de esta herramienta va mucho más allá de solo escanear puertos y decirnos cuales se encuentran abiertos y cerrados. Nmap tiene una serie de scripts especificos para hacer escaneos avanzados a un puerdo objetivo y detectar malas configuraciones del servicio, vulnerabilidades conocidas, obtener información extra de ese servicio e incluso realizar ataques de fuerza bruta. Estos scripts estan divididos por servicios entre los que podemos mensonar dns, http, smb, ftp, imap, mongodb, msrpc, ms-sql, mysql, ssh, entre otros, a su vez estos scripts se dividen en sub-categorías como pueden ser enum, vulns, brute, malware, form, info, entre otras, los podemos encontrar en el directorio /usr/share/nmap/scripts:

		─[xc0d3@cs-academy]─[~]
		└──╼ [★]$ ls /usr/share/nmap/scripts/
		Display all 599 possibilities? (y or n)
		acarsd-info.nse                         ip-geolocation-geoplugin.nse
		address-info.nse                        ip-geolocation-ipinfodb.nse
		afp-brute.nse                           ip-geolocation-map-bing.nse
		afp-ls.nse                              ip-geolocation-map-google.nse
		afp-path-vuln.nse                       ip-geolocation-map-kml.nse
		afp-serverinfo.nse                      ip-geolocation-maxmind.nse
		afp-showmount.nse                       ip-https-discover.nse
		ajp-auth.nse                            ipidseq.nse
		ajp-brute.nse                           ipmi-brute.nse
		ajp-headers.nse                         ipmi-cipher-zero.nse
		ajp-methods.nse                         ipmi-version.nse
		ajp-request.nse                         ipv6-multicast-mld-list.nse
		allseeingeye-info.nse                   ipv6-node-info.nse
		amqp-info.nse                           ipv6-ra-flood.nse
		asn-query.nse                           irc-botnet-channels.nse
		auth-owners.nse                         irc-brute.nse
		auth-spoof.nse                          irc-info.nse
		backorifice-brute.nse                   irc-sasl-brute.nse
		backorifice-info.nse                    irc-unrealircd-backdoor.nse
		bacnet-info.nse                         iscsi-brute.nse
		banner.nse                              iscsi-info.nse
		bitcoin-getaddr.nse                     isns-info.nse
		bitcoin-info.nse                        jdwp-exec.nse
		bitcoinrpc-info.nse                     jdwp-info.nse
		bittorrent-discovery.nse                jdwp-inject.nse
		bjnp-discover.nse                       jdwp-version.nse
		broadcast-ataoe-discover.nse            knx-gateway-discover.nse
		broadcast-avahi-dos.nse                 knx-gateway-info.nse
		broadcast-bjnp-discover.nse             krb5-enum-users.nse
		broadcast-db2-discover.nse              ldap-brute.nse
		broadcast-dhcp6-discover.nse            ldap-novell-getpass.nse
		broadcast-dhcp-discover.nse             ldap-rootdse.nse
		broadcast-dns-service-discovery.nse     ldap-search.nse
		broadcast-dropbox-listener.nse          lexmark-config.nse
		broadcast-eigrp-discovery.nse           llmnr-resolve.nse
		broadcast-hid-discoveryd.nse            lltd-discovery.nse
		broadcast-igmp-discovery.nse            lu-enum.nse
		broadcast-jenkins-discover.nse          maxdb-info.nse
		broadcast-listener.nse                  mcafee-epo-agent.nse
		broadcast-ms-sql-discover.nse           membase-brute.nse
		broadcast-netbios-master-browser.nse    membase-http-info.nse
		broadcast-networker-discover.nse        memcached-info.nse
		broadcast-novell-locate.nse             metasploit-info.nse
		broadcast-ospf2-discover.nse            metasploit-msgrpc-brute.nse
		broadcast-pc-anywhere.nse               metasploit-xmlrpc-brute.nse
		broadcast-pc-duo.nse                    mikrotik-routeros-brute.nse
		broadcast-pim-discovery.nse             mmouse-brute.nse
		broadcast-ping.nse                      mmouse-exec.nse


A continuación se muestran algunos ejemplos de uso de estos scripts:

- ftp-anon.nse: nos permite verificar si un servidor ftp acepta conexiones anónimas

Ejemplo:

	─[xc0d3@cs-academy]─[~]
	└──╼ [★]$ nmap -p21 --script=ftp-anon.nse 192.168.56.102
	Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 10:18 EST
	Nmap scan report for 192.168.56.102
	Host is up (0.00069s latency).
	PORT   STATE SERVICE
	21/tcp open  ftp
	|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
	Nmap done: 1 IP address (1 host up) scanned in 0.55 seconds
	
ftp-vsftpd-backdoor.nse: nos permite comprobar una vulnerabilidad presente en una versión específica de vsftpd:

Ejemplo:

	─[xc0d3@cs-academy]─[~]
	└──╼ [★]$ nmap -p21 --script=ftp-vsftpd-backdoor.nse 192.168.56.102
	Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 10:20 EST
	Nmap scan report for 192.168.56.102
	Host is up (0.0033s latency).
	PORT   STATE SERVICE
	21/tcp open  ftp
	| ftp-vsftpd-backdoor: 
	|   VULNERABLE:
	|   vsFTPd version 2.3.4 backdoor
	|     State: VULNERABLE (Exploitable)
	|     IDs:  CVE:CVE-2011-2523  BID:48539
	|       vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.
	|     Disclosure date: 2011-07-03
	|     Exploit results:
	|       Shell command: id
	|       Results: uid=0(root) gid=0(root)
	|     References:
	|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2523
	|       http://scarybeastsecurity.blogspot.com/2011/07/alert-vsftpd-download-backdoored.html
	|       https://www.securityfocus.com/bid/48539
	|_      https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/ftp/vsftpd_234_backdoor.rb
	Nmap done: 1 IP address (1 host up) scanned in 1.23 seconds

smb-vuln-ms17-010: nos permite comprobar si el servicio SMB de la víctima es vulnerable a este exploit
Ejemplo:

	─[xc0d3@cs-academy]─[~]
	└──╼ [★]$ nmap -p445 --script=smb-vuln-ms17-010 192.168.56.103
	Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 10:22 EST
	Nmap scan report for 192.168.56.103
	Host is up (0.00073s latency).
	PORT    STATE SERVICE
	445/tcp open  microsoft-ds
	Host script results:
	| smb-vuln-ms17-010: 
	|   VULNERABLE:
	|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
	|     State: VULNERABLE
	|     IDs:  CVE:CVE-2017-0143
	|     Risk factor: HIGH
	|       A critical remote code execution vulnerability exists in Microsoft SMBv1
	|        servers (ms17-010).
	|           
	|     Disclosure date: 2017-03-14
	|     References:
	|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
	|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
	|_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
	Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds

smb-enum-shares.nse: nos enumera los recursos compartidos en un objetivo

Ejemplo:

	Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
	─[xc0d3@cs-academy]─[~]
	└──╼ [★]$ nmap -p445 --script=smb-enum-shares.nse 192.168.56.102
	Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-21 10:25 EST
	Nmap scan report for 192.168.56.102
	Host is up (0.00039s latency).
	PORT    STATE SERVICE
	445/tcp open  microsoft-ds
	Host script results:
	| smb-enum-shares: 
	|   account_used: <blank>
	|   \\192.168.56.102\ADMIN$: 
	|     Type: STYPE_IPC
	|     Comment: IPC Service (metasploitable server (Samba 3.0.20-Debian))
	|     Users: 1
	|     Max Users: <unlimited>
	|     Path: C:\tmp
	|     Anonymous access: <none>
	|   \\192.168.56.102\IPC$: 
	|     Type: STYPE_IPC
	|     Comment: IPC Service (metasploitable server (Samba 3.0.20-Debian))
	|     Users: 1
	|     Max Users: <unlimited>
	|     Path: C:\tmp
	|     Anonymous access: READ/WRITE
	|   \\192.168.56.102\opt: 
	|     Type: STYPE_DISKTREE
	|     Comment: 
	|     Users: 1
	|     Max Users: <unlimited>
	|     Path: C:\tmp
	|     Anonymous access: <none>
	|   \\192.168.56.102\print$: 
	|     Type: STYPE_DISKTREE
	|     Comment: Printer Drivers
	|     Users: 1
	|     Max Users: <unlimited>
	|     Path: C:\var\lib\samba\printers
	|     Anonymous access: <none>
	|   \\192.168.56.102\tmp: 
	|     Type: STYPE_DISKTREE
	|     Comment: oh noes!
	|     Users: 1
	|     Max Users: <unlimited>
	|     Path: C:\tmp
	|_    Anonymous access: READ/WRITE
	Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds

Nota: En cada uno de los scripts ejecutados previamente tenemos referencias de cómo explotar dicha vulnerabilidad, buscar exploit u obtener más información sobre dicha vulnerabilidad o servicio.


### OpenVAS
Entrando ya en la búsqueda de vulnerabilidades en aplicaciones y equipos, una de las aplicaciones mas completas que podemos encontrar en la red es OpenVAS. OpenVAS es un escáner de vulnerabilidades al que podemos introducir una dirección IP y encargarle el análisis de dicho equipo, recogiendo información sobre los servicios en funcionamiento, los puertos abiertos, fallos de configuración, posibles vulnerabilidades conocidas en el software del equipo o servidor, etc.

Este programa se puede ejecutar tanto desde dentro de una red como desde un servidor externo, simulando así un ataque real. Cuando finaliza nos genera un completo informe con todas las posibles debilidades que pueden suponer un peligro para nuestra seguridad. Además, podemos configurarlo en modo monitorización continua, estableciendo alertas que saltarán cuando se detecte el más mínimo fallo.



### Nessus
Nessus es un programa de escaneo de vulnerabilidades para todos los sistemas operativos, consiste en un demonio nessusd que realiza el escaneo del sistema operativo objetivo, y nessus el cliente que muestra el avance e informa de todo lo que va encontrando en los diferentes escaneos. Nessus se puede ejecutar tanto a nivel de consola por comandos, o también con interfaz gráfica de usuario. Nessus primero empieza realizando un escaneo de puertos, ya que es lo primero que se suele hacer en un pentesting, Nessus hace uso de la potencia de Nmap para ello, aunque también tiene su propio escáner de puertos abiertos.

Esta herramienta permite exportar los resultados del escaneo en diferentes formatos, como texto plano, XML, HTML y LaTeX, además, toda la información se guarda en una base de datos de «conocimiento» para posteriores revisiones. Actualmente Nessus tiene una versión gratuita muy limitada, y posteriormente una versión de pago mucho más completa y con soporte de la empresa que tiene detrás.

Algunas características muy importantes de Nessus son que tiene muy pocos falsos positivos, tiene una gran cobertura de vulnerabilidades y es utilizada ampliamente por toda la industria de la seguridad, por lo que se actualiza casi continuamente para incorporar las últimas tecnologías y fallos de seguridad de las aplicaciones.



### OWASP Zen Attack Proxy ZAP
El proyecto OWASP (Open Web Application Security Project) es un proyecto abierto y sin ánimo de lucro pensado para mejorar la seguridad de las redes, los servidores, equipos y las aplicaciones y servicios con el fin de convertir Internet en un lugar más seguro. Zed Attack Proxy, ZAP, es una de las herramientas libres de este proyecto cuya principal finalidad es monitorizar la seguridad de redes y aplicaciones web en busca de cualquier posible fallo de seguridad, mala configuración e incluso vulnerabilidad aún desconocida que pueda suponer un problema para la red.



### Seccubus
Seccubus es una herramienta que utiliza otros escáneres de vulnerabilidades y automatiza la tarea lo máximo posible. Aunque este no es un escáner propiamente dicho como los anteriores, esta aplicación une varios de los escáneres más populares del mercado, como Nessus, OpenVAS, NMap, SSLyze, Medusa, SkipFish, OWASP ZAP y SSLlabs y nos permite automatizar todos los análisis de manera que desde esta única aplicación podamos realizar un análisis lo más profundo posible, además de poder programas análisis a intervalos regulares para asegurarnos de que todos los equipos y las redes están siempre correctamente protegidas y, en caso de que algo vaya mal, recibir avisos en tiempo real.


### Uniscan
Una de las aplicaciones más conocidas dentro de este tipo de aplicaciones para buscar vulnerabilidades es Uniscan. Las principales características de esta herramienta son que es una de las más sencillas para la búsqueda de vulnerabilidades, pero también una de las más potentes, siendo capaz de buscar fallos de seguridad críticos en los sistemas. Es capaz de localizar fallos desde el acceso a los archivos locales, hasta la ejecución de código remoto, e incluso es capaz de cargar archivos de forma remota a los sistemas vulnerables. También permite realizar un seguimiento de huella digital y listar los servicios, archivos y directorios de cualquier servidor.


### Metasploit
Metasploit es una de las mejores herramientas de código abierto que nos permite localizar y explotar vulnerabilidades de seguridad en sistemas y servicios, esta herramienta es fundamental para realizar pentesting. El proyecto más popular es Metasploit Framework, el cual se encuentra instalado de manera predeterminada en distribuciones Linux como ParrotOS Security y Kali Linux. Gracias a la potencia de Metasploit, podremos realizar pruebas de penetración a servicios, aplicaciones y demás ataques.

Es una de las herramientas que debes tener en tu arsenal de herramientas para realizar pentesting, se complementa con el resto de herramientas que hemos hablado anteriormente. Metasploit tiene una gran comunidad detrás, y se han diseñado herramientas basadas en esta para facilitar enormemente todas las tareas automatizándolas.


### Donde encuentro exploits
Vulnerability Details
A menudo se comienza investigando proveedores particulares, productos de software y vulnerabilidades específicas.
Por lo tanto los siguientes sitios son útiles para tener una idea de lo que hay por ahí:
- CVEDetails.com: https://www.cvedetails.com/
- ITSecDB.com: http://www.itsecdb.com/
Estos ofrecen un mejor front-end para sus diversas fuentes, tales como Mitre (https://www.cve.mitre.org/cve/index.html) y la Base de Datos Nacional de Vulnerabilidad de los Estados Unidos (https://web.nvd.nist.gov/view/vuln/search).

CVEDetails permite generar feeds RSS personalizados, listar widgets y consultar su API JSON: https://www.cvedetails.com/vulnerability-feeds-form.php.

Se puede indexar, correlacionar y gestionar vulnerabilidades de software mediante CVE-Search (http://cve-search.org) que permite realizar búsquedas locales rápidas utilizando la interfaz web o la API y reduciendo las consultas potencialmente confidenciales enviadas a través de Internet.

### Explorar los motores de búsqueda de exploits
Si se está apuntando a un producto de software específico, o incluso la versión, la siguiente parada es utilizar el motor de búsqueda ExploitSearch.net http://exploitsearch.net/.

Este explorará varias bases de datos exploit, frameworks y proveedores de exploit-pack para los resultados que encuentre en:
- Exploit-DB.com: https://www.exploit-db.com/
- 0day.today: http://0day.today/
- PacketStormSecurity.com: https://packetstormsecurity.com/
- SecurityFocus.com: http://www.securityfocus.com/

El dashboard (http://exploitsearch.net/dashboard) tiene una visión general completa, así como algunas estadísticas interesantes

### Sitios de exploits
Las bases de datos de recolección y los sitios van y vienen. Algunos desaparecen completamente y otros permanecen en línea como un archivo.
Por el momento ExploitSearch no indexa ni se integra con:
- Brakertech.com: http://brakertech.com/
- CXSecurity.com: https://cxsecurity.com/
- ExploitAlert.com: http://www.exploitalert.com/
- Iranian Exploit Database: http://iedb.ir/
- RouterPwn.com: http://www.routerpwn.com/
- SeeBug.org: https://www.seebug.org/
- SecuriTeam.com: http://www.securiteam.com/
- SecurityPhresh.com: http://securityphresh.com/
- SecurityVulns.com: http://securityvulns.com/
- Vulnerability-Lab.com: https://www.vulnerability-lab.com/
- Vulners.com: https://vulners.com/
- WpVulnDB.com: https://wpvulndb.com/
- Zero Day Initiative: http://zerodayinitiative.com/advisories/published/

### Herramientas de búsquedas de exploits
El actual campeón reinante es Pompem (https://github.com/rfunix/Pompem).
Esta herramienta de línea de comandos busca en algunos de los sitios enumerados anteriormente como CXSecurity, PacketStorm, U.S. National Vulnerability DB, Vulners, WpvulndbB, 0day.today, etc.

También existe findsploit (https://github.com/1N3/Findsploit) que realiza búsquedas en Exploit-DB.com, Nmap NSE scripts y Metasploit Framework, con la opción de continuar su búsqueda en línea.

No podemos hablar de herramientas de búsqueda de exploit sin mencionar searchsploit (exploitdb) la cual es una herramienta de linea de comandos con la base de datos de https://www.exploit-db.com/ y un excelente punto de partida para buscar exploits rápido de forma local teniendo referencias de una vulnerabilidad, código CVE, versión de un servicio, etc.