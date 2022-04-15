# Técnicas y herramientas de reconocimiento pasivo

## Introducción
El Reconocimiento se considera la primera etapa de una prueba de penetración y por ende la más importante. Es la etapa que nos va a permitir recopilar información sobre nuestro objetivo y en base a esa información recopilada podemos desarrollar un plan de ataque. El reconocimiento se divide en dos etapas, Reconocimiento Pasivo y Reconocimiento Activo.



## Etapas de reconocimiento:

Pasivo: Es cuando se recopila información del objetivo sin interactuar directamente con él, utilizando información pública, motores de búsqueda y servicios o herramientas OSINT (OpenSource Intelligence).

Activo: Es cuando se recopila información del objetivo interactuando directamente con él, utilizando herramientas para identificar objetivos en linea, escaneadores de puertos, enumerando servicios y utilizando escaneadores de vulnerabilidades.



En este módulo vamos a ver cómo recopilar información sobre un objetivo de forma pasiva.



## Sitio Web (Website)
Cuando hemos sido contratados por una empresas u organización para realizar una prueba de penetración en sus servicios, el primero lugar dónde tenemos que buscar información es en el sitio web de dicha organización. Un sitio web nos puede responder a preguntas tales como: ¿Qué hacen? ¿Cuántos empleados tienen? ¿Qué tecnología utilizan? ¿Cuan grande es la empresa u organización? incluso ¿Quienes son su clientes y socios? entre otros datos como Información de contacto, correos electrónicos, números de teléfonos, dirección, sucursales, redes sociales, etc. Toda esta información nos permite crear un plan de ataque para pasar a la siguiente etapa de la prueba de penetración.

En gran parte de nuestro curso de hacking ético vamos a utilizar el dominio megacorpone.com que pertenece a OffensiveSecurity. MegaCorpOne es una empresa ficticia creada por el equipo de OffSec (Offensive Security) para realizar pruebas de reconocimiento en los entrenamientos para OSCP (Offensive Security Certified Professional), por lo cual es totalmente legal hacer todo tipo de reconocimiento en esta supuesta organización.

## Motores de búsqueda

Todos los días utilizamos motores de búsqueda como Google o DuckDuckGo (entre otros) para buscar información en internet. Como mismo buscamos información sobre "como instalar una distribución GNU/Linux basada en Debian", "como utilizar el comando apt", etc, también podemos buscar información pública sobre un objetivo, esta técnica se conoce como OSINT (Open Source INTelligence), Información de Fuente Abierta o Información de Dominio Público.

Entre algunas de las técnicas OSINT que podemos utilizar se encuentran:

- Whois
- Google Hacking
- Shodan.io
- Reconocimiento DNS
- Maltego
Entre otras fuentes de información de fuente abierta, para no cargar el contenido del curso, vamos a enfocarnos en las mencionadas anteriormente.


Whois
Whois es un servicio en linea que contiene información pública sobre nombres de dominio, información tal como fecha de registro, última actualización, fecha de caducidad, información de contacto, correos electrónicos y números de teléfonos asociados al propietario, administrador y/o técnico encargado del dominio.

Whois ademas de tener su servicio online https://whois.net/ también tiene su versión cli (cliente de terminal) dicha herramienta viene instalada por defecto en ParrotOS Security Edition. Su uso es muy sencillo:

	┌─[xc0d3@cs-academy]─[~]
	└──╼ $ whois megacorpone.com
	   Domain Name: MEGACORPONE.COM
	   Registry Domain ID: 1775445745_DOMAIN_COM-VRSN
	   Registrar WHOIS Server: whois.gandi.net
	   Registrar URL: http://www.gandi.net
	   Updated Date: 2020-06-16T17:05:41Z
	   Creation Date: 2013-01-22T23:01:00Z
	   Registry Expiry Date: 2024-01-22T23:01:00Z
	   Registrar: Gandi SAS
	   Registrar IANA ID: 81
	   Registrar Abuse Contact Email: abuse@support.gandi.net
	   Registrar Abuse Contact Phone: +33.170377661
	   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
	   Name Server: NS1.MEGACORPONE.COM
	   Name Server: NS2.MEGACORPONE.COM
	   Name Server: NS3.MEGACORPONE.COM
	   DNSSEC: unsigned
	   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
	>>> Last update of whois database: 2020-07-12T14:35:03Z <<<
	For more information on Whois status codes, please visit https://icann.org/epp
	NOTICE: The expiration date displayed in this record is the date the
	registrar's sponsorship of the domain name registration in the registry is
	currently set to expire. This date does not necessarily reflect the expiration
	date of the domain name registrant's agreement with the sponsoring
	registrar.  Users may consult the sponsoring registrar's Whois database to
	view the registrar's reported date of expiration for this registration.
	TERMS OF USE: You are not authorized to access or query our Whois
	database through the use of electronic processes that are high-volume and
	automated except as reasonably necessary to register domain names or
	modify existing registrations; the Data in VeriSign Global Registry
	Services' ("VeriSign") Whois database is provided by VeriSign for
	information purposes only, and to assist persons in obtaining information
	about or related to a domain name registration record. VeriSign does not
	guarantee its accuracy. By submitting a Whois query, you agree to abide
	by the following terms of use: You agree that you may use this Data only
	for lawful purposes and that under no circumstances will you use this Data
	to: (1) allow, enable, or otherwise support the transmission of mass
	unsolicited, commercial advertising or solicitations via e-mail, telephone,
	or facsimile; or (2) enable high volume, automated, electronic processes
	that apply to VeriSign (or its computer systems). The compilation,
	repackaging, dissemination or other use of this Data is expressly
	prohibited without the prior written consent of VeriSign. You agree not to
	use electronic processes that are automated and high-volume to access or
	query the Whois database except as reasonably necessary to register
	domain names or modify existing registrations. VeriSign reserves the right
	to restrict your access to the Whois database in its sole discretion to ensure
	operational stability.  VeriSign may restrict or terminate your access to the
	Whois database for failure to abide by these terms of use. VeriSign
	reserves the right to modify these terms at any time.
	The Registry database contains ONLY .COM, .NET, .EDU domains and
	Registrars.
	Domain Name: megacorpone.com
	Registry Domain ID: 1775445745_DOMAIN_COM-VRSN
	Registrar WHOIS Server: whois.gandi.net
	Registrar URL: http://www.gandi.net
	Updated Date: 2020-06-16T19:05:41Z
	Creation Date: 2013-01-22T23:01:00Z
	Registrar Registration Expiration Date: 2024-01-22T23:01:00Z
	Registrar: GANDI SAS
	Registrar IANA ID: 81
	Registrar Abuse Contact Email: abuse@support.gandi.net
	Registrar Abuse Contact Phone: +33.170377661
	Reseller: 
	Domain Status: clientTransferProhibited http://www.icann.org/epp#clientTransferProhibited
	Domain Status: 
	Domain Status: 
	Domain Status: 
	Domain Status: 
	Registry Registrant ID: 
	Registrant Name: Alan Grofield
	Registrant Organization: MegaCorpOne
	Registrant Street: 2 Old Mill St
	Registrant City: Rachel
	Registrant State/Province: Nevada
	Registrant Postal Code: 89001
	Registrant Country: US
	Registrant Phone: +1.9038836342
	Registrant Phone Ext:
	Registrant Fax: 
	Registrant Fax Ext:
	Registrant Email: 3310f82fb4a8f79ee9a6bfe8d672d87e-1696395@contact.gandi.net
	Registry Admin ID: 
	Admin Name: Alan Grofield
	Admin Organization: MegaCorpOne
	Admin Street: 2 Old Mill St
	Admin City: Rachel
	Admin State/Province: Nevada
	Admin Postal Code: 89001
	Admin Country: US
	Admin Phone: +1.9038836342
	Admin Phone Ext:
	Admin Fax: 
	Admin Fax Ext:
	Admin Email: 3310f82fb4a8f79ee9a6bfe8d672d87e-1696395@contact.gandi.net
	Registry Tech ID: 
	Tech Name: Alan Grofield
	Tech Organization: MegaCorpOne
	Tech Street: 2 Old Mill St
	Tech City: Rachel
	Tech State/Province: Nevada
	Tech Postal Code: 89001
	Tech Country: US
	Tech Phone: +1.9038836342
	Tech Phone Ext:
	Tech Fax: 
	Tech Fax Ext:
	Tech Email: 3310f82fb4a8f79ee9a6bfe8d672d87e-1696395@contact.gandi.net
	Name Server: NS1.MEGACORPONE.COM
	Name Server: NS2.MEGACORPONE.COM
	Name Server: NS3.MEGACORPONE.COM
	Name Server: 
	Name Server: 
	Name Server: 
	Name Server: 
	Name Server: 
	Name Server: 
	Name Server: 
	DNSSEC: Unsigned
	URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
	>>> Last update of WHOIS database: 2020-07-12T14:35:23Z <<<
	For more information on Whois status codes, please visit
	https://www.icann.org/epp
	Reseller Email: 
	Reseller URL: 
	Personal data access and use are governed by French law, any use for the purpose of unsolicited mass commercial advertising as well as any mass or automated inquiries (for any intent other than 	the registration or modification of a domain name) are strictly forbidden. Copy of whole or part of our database without Gandi's endorsement is strictly forbidden.
	A dispute over the ownership of a domain name may be subject to the alternate procedure established by the Registry in question or brought before the courts.
	For additional information, please contact us via the following form:
	 https://www.gandi.net/support/contacter/mail/


En la salida del comando whois tenemos toda la información relacionada con el registro del dominio megacopone.com

Veamos un ejemplo práctico:

AGREGAR VIDEO DE YOUTUBE

## Google Hacking

Google Hacking es la técnica que se utiliza para buscar información personalizada sobre un objetivo en el motor de búsqueda de Google, utilizando operadores especiales de búsqueda para obtener resultados más precisos o con un patron deseado.

Entre los operadores más utilizados se encuentran:


- cache: este operador le mostrará la versión en caché de cualquier sitio web.

ejemplo:

	cache:megacopone.com

- allintext: busca texto específico contenido en cualquier página web.

ejemplo:

	allintext:herramientas de hackeo
	allintitle: exactamente igual que allintext, pero mostrará páginas que contienen títulos con caracteres X. 

ejemplo:

	allintitle:"CS-Academy"
	allinurl: se puede usar para obtener resultados cuya URL contiene todos los caracteres especificados, 
ejemplo: 

	allinurl:wp-admin.php

- filetype: se utiliza para buscar cualquier tipo de extensión de archivo, por ejemplo, si desea buscar archivos pdf puede usar: 

ejemplo:

	filetype:pdf

- inurl: es exactamente lo mismo que allinurl, pero solo es útil para una sola palabra clave, 

ejemplo:

	inurl:admin

- intitle: se usa para buscar varias palabras clave dentro del título, por ejemplo, "intitle:las herramientas de seguridad", buscará títulos que comiencen con “seguridad” pero las “herramientas” pueden estar en otro lugar de la página.

- inanchor: esto es útil cuando necesita buscar un texto de anclaje exacto utilizado en cualquier enlace, 

ejemplo:

	inanchor:"seguridad informática"

- intext: útil para localizar páginas que contienen ciertos caracteres o cadenas dentro de su texto, 

ejemplo:

	intext:"internet seguro"

- link: mostrará la lista de páginas web que tienen enlaces a la URL especificada. 

ejemplo:

	link:microsoft.com

- site: le mostrará la lista completa de todas las URL indexadas para el dominio y subdominio especificados, 

ejemplo:

	site:megacorpone.com

- *: comodín utilizado para buscar páginas que contienen "cualquier cosa" antes de su palabra

ejemplo:

cómo * un sitio web 
devolverá "cómo ..." diseñar / crear / piratear, etc ... "un sitio web".
|: este es un operador lógico, ejemplo: |:Los "consejos" de "seguridad" mostrarán todos los sitios que contienen "seguridad" o "consejos", o ambas palabras.

- - +: se utiliza para concatenar palabras, útil para detectar páginas que usan más de una clave específica.

ejemplo:

	seguridad + senderos

- -: el operador menos se usa para evitar mostrar resultados que contienen ciertas palabras, ejemplo: security -trails mostrará páginas que usan "security" en su texto, pero no las que tienen la palabra "trails".



## Shodan.io

Shodan es un motor de búsqueda que le permite al usuario encontrar iguales o diferentes tipos específicos de equipos (routers, servidores, etc.) conectados a Internet a través de una variedad de filtros. Algunos también lo han descrito como un motor de búsqueda de banners de servicios, que son metadatos que el servidor envía de vuelta al cliente.​ Esta información puede ser sobre el software de servidor, qué opciones admite el servicio, un mensaje de bienvenida o cualquier otra cosa que el cliente pueda saber antes de interactuar con el servidor.

Shodan recoge datos de todos los servicios, incluyendo HTTP (puerto 80, 8080), HTTPS (puerto 443, 8443), FTP (21), SSH (22) Telnet (23), SNMP (161) y SIP (5060).

Para utilizar shodan deben darse de alta en su sitio oficial https://shodan.io/, luego del registro pueden utilizar el motor de búsqueda de la plataforma para buscar información sobre un objetivo o bien pueden utilizar la API de shodan con su cliente de terminal, para más detalles sobre el uso de Shodan ver la Video Clase - Reconocimiento (Parte I).


## Consultas a los DNS o Reconocimiento DNS

Los DNS de una empresa pueden brindarnos mucha información interesante sobre un objetivo, desde direcciones IPs, dominios, sub-dominios e incluso ofrecernos todo un mapa de la estructura de los servicios de una organización en dependencia de cuan bien configurados esten sus NS (Name Server).

Para hacer reconocimiento DNS podemos utilizar muchas herramientas, durante el inicio de este curso vamos a utilizar las siguientes herramientas y/o comandos:

- dig
- nslookup
- host
- dnsenum
- dnsrecon
- Uso del comando host:

AGREGAR VIDEO DE YOUTUBE

Existen varias técnicas que podemos usar para obtener cierta información como pueden ser:


- fordward dns lookup: técnica en la que le hacemos consultas a un nombre de dominio para ver a que dirección o direcciones IPs responde.
- reverse dns lookup: técnica en la que le preguntamos a una dirección IP a que dominio o dominios responde.
- dns zone transfer: técnica en la que intentamos acceder a la configuración de la zona dns aprovechándonos de una mala configuración por parte del administrador.

## Maltego

Maltego es un software1​ utilizado para la Inteligencia de fuentes abiertas (OSINT) y forense, desarrollado por Paterva.​ Maltego se enfoca en proporcionar una biblioteca de transformaciones para el descubrimiento de datos de fuentes abiertas y visualizar esa información en un formato gráfico, adecuado para análisis de enlaces y minería de datos. A partir de 2018, el equipo de Maltego Technologies​, ha asumido la responsabilidad de todas las operaciones globales orientadas al cliente.

Maltego permite crear entidades personalizadas, lo que le permite representar cualquier tipo de información además de los tipos básicos de entidades que forman parte del software. El enfoque básico de la aplicación es analizar las relaciones del mundo real, red social y red de computadoras entre personas, grupos, páginas web, dominios, redes, infraestructura de Internet y afiliaciones con servicios de linea nodos de redes informáticas como Twitter y Facebook. Entre sus fuentes de datos se encuentran el sistema de nombres de dominio, entre otros.

Para ver ejemplos prácticos sobre el uso de Maltego puede acceder a la Video Clase - Reconocimiento (Parte II)Lección