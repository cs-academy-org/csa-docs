# Metodología de un Pentest
## Introducción

Una prueba de penetración es la unión de varias etapas de investigación y ataques informáticos que se realizan a un objetivo con el fin de vulnerar su seguridad. Estos ataques deben ser calculados correctamente y llevarse a cabo siguiendo una metodología que aumenta las posibilidades de éxito en este tipo de pruebas. Estas son las etapas de una prueba de penetración:


- Reconocimiento
- Escaneo de puertos y Enumeración
- Explotación
- Escalada de Privilegios
- Post-explotación
- Cubriendo pistas y colocando puertas traseras
- Informe o Reporte


Cuanta más información reunamos del objetivo, mayores son las probabilidades de tener éxito en la Prueba de Penetración, de ahí que la etapa de Footprinting (reconocimiento, escaneo de puertos y enumeración de servicios) es la etapa más importante y se puede decir que no termina hasta que la prueba de penetración no termine, porque una vez que logramos vulnerar un servidor de una empresa, comenzamos este ciclo nuevamente, recopilando información sobre la red interna, sus servicios y estructura, con el fin de vulnerar más ese objetivo y hacernos con el control total de todos los dispositivos y/o servicios.



Cada Hacker o Profesional de la Seguridad Informática tiene su propia metodología, que por lo general se basa en sus principales habilidades. La metodología que se sugiere en este artículo, simplemente es eso, una sugerencia del autor, no es algo oficial u obligatorio a la hora de realizar una Prueba de Penetración. Vamos a pasar a explicar cada una de estas etapas.



## Reconocimiento


El reconocimiento se considera la primera fase, en mi opinión no corresponde a la fase de ataque, sino a la fase de preataque. Se trata de recopilar, identificar y registrar información sobre el objetivo de forma pasiva. En esta fase debemos averiguar la mayor información posible sobre la víctima, se trata de conocer a nuestra víctima para formular un plan de ataque. Se puede obtener información pasiva de muchas maneras. Internet la mayor fuente de información que tienen los hackers, es el sitio perfecto para buscar información pública sobre la víctima. Todas las empresas tienen un sitio web que nos puede responder a preguntas como: ¿Qué servicios prestan? ¿Cómo interactuan con sus clientes? ¿Cómo está diseñado su sitio web? ¿Qué tecnologías utilizan? ¿Cantidad de empleados?, podemos obtener datos de contacto, correos electrónicos, números de teléfono, direcciones. Toda esta información podemos utilizarla para obtener más información realizando posibles ataques de Ingeniería Social con el fin de que un empleado revele información confidencial, pidiéndole a un funcionario via telefónica que necesita restablecer una contraseña o información sobre algún otro empleado, haciendo uso de ingeniería social se puede recopilar mucha información jugosa.



Luego están los motores de búsqueda y las redes sociales donde cada día existe menos la privacidad y esto le hace mucho mas fácil el trabajo a los hackers. Existen buenas opciones como Google Hacking, Google Dorks, Shodan.io, estos servicios tienen amplias bases de datos con información pública que puede ser de mucha ayuda en esta primera fase. No se debe pasar por alto el "Dumpster diving", es el acto de buscar en la basura de la víctima. Muchas empresas no tiene buenas prácticas en la política de seguridad, en la basura, en esos documentos impresos que desechan, también se puede encontrar información jugosa. Realizar una prueba de penetración se puede comparar con una investigación policial, toda información es importante, todo dato aporta algo útil a la investigación y aunque al principio no lo parezca, es un progreso.



El reconocimiento se considera la primera fase de preataque y es un intento sistemático de localizar, recopilar, identificar y registrar información sobre el objetivo. El hacker busca averiguar la mayor información posible sobre la víctima. Este primer paso se considera una recopilación de información pasiva. Como ejemplo, muchos de ustedes probablemente hayan visto una película de detectives en la que el policía espera fuera de la casa de un sospechoso toda la noche y luego lo sigue desde la distancia cuando se va en el automóvil. Eso es reconocimiento; es de naturaleza pasiva y, si se hace correctamente, la víctima ni siquiera sabe que está ocurriendo.



## Escaneo y Enumeración


En esta etapa, el atacante escanea y enumera los servicios de la víctima, con el fin de mapear puertos abiertos, aplicaciones y versión de estas aplicaciones. Con una buena recopilación de información utilizando técnicas pasivas y activas, el atacante busca encontrar posibles vulnerabilidades o malas configuraciones en los servicios. Esta es una etapa muy delicada en la que hay que tomarse el tiempo necesario, por dos motivos, el primero es para no hacer mucho ruido en el sistema ya que muchas compañías tienen sistemas de detección de intrusos (IDS) y esto puede dificultar mucho más la prueba de penetración o peor, terminar con ella o enviar respuestas falsas. Y el segundo es que cuanto más tiempo se le dedica a esta etapa, mayores son las posibilidades de encontrar fallas de seguridad y de no pasar nada por alto.



En esta etapa el atacante suele utilizar herramientas como nmap, Openvas, Nessus y otros softwares desarrollados para servicios más específicos como pueden ser el caso de SMB, SMTP, IMAP, POP, Apache, FTP o frameworks específicos para plataformas como Wordpress, Joomla, Drupal, etc. Pero hay que tener mucho cuidado a la hora de utilizar escáneres de vulnerabilidades automatizados como Openvas o Nessus ya que estos están diseñados para detectar vulnerabilidades y generan gran cantidad de tráfico hacia la víctima algo que se vería como una ráfaga de tráfico inusual. En este caso viene bien el uso de herramientas como TOR Proxychains que nos permite utilizar gran cantidad de proxyies, no solo para ocultar el origen del tráfico sino también para que se vea una variedad en este origen, aun así el atacante debe ser muy cauteloso ante el uso de este tipo de herramientas. Voy a repetir algo que ya he mencionado anteriormente, cuanto más tiempo pase el atacante en esta fase, mayores son las posibilidades de éxito para el y menores son las posibilidades de que la víctima detecte esta actividad inusual.

A diferencia del hacker blackhat de élite que intenta permanecer sigilosamente, los kiddies de script incluso pueden usar escáneres de vulnerabilidades como Nessus para escanear la red de la víctima. Aunque las actividades del pirata informático blackhat se pueden ver como un solo disparo en la noche, el escaneo del script kiddies aparecerá como una serie de ráfagas de escopeta, ya que su actividad será alta y detectable. Los programas como Nessus están diseñados para detectar vulnerabilidades, pero no están diseñados para ser una herramienta de pirateo; como tales, generan una gran cantidad de tráfico de red detectable.



Una vez que el atacante conoce a la víctima y tiene suficiente información útil sobre sus servicios, puede pasar a la siguiente fase.



## Ganando Acceso (Explotación)

Esta es la fase preferida de todo hacker, es el momento en el que el felino hambriento deja de estudiar a su presa y lanza el ataque. Llegó el momento de utilizar toda esa información recopilada para lanzar el ataque y lograr comprometer la seguridad de la víctima. El acceso puede llegar a obtenerse al explotar una vulnerabilidad en alguno de los servicios de la víctima, utilizando ingeniería social para colarse en la empresa y vulnerar la red desde dentro, haciendo ataques de spam, phishing o simplemente hackeando la contraseña del Wifi de la empresa, la cual le proporciona acceso a toda la red interna de la misma. Realmente el éxito de esta fase depende mucho de las habilidades del hacker, de cuanta información jugosa fue capaz de recopilar y de como va a aprovechar dicha información. Por más difícil que parezca la tarea, siempre hay un punto débil, si la fortaleza no se puede derrumbar, hay que atacarla desde dentro o buscar el eslabón mas débil de la cadena que es el factor humano. 



En cuanto al daño potencial, este podría considerarse uno de los pasos más importantes de un ataque. Esta fase del ataque ocurre cuando el hacker pasa de simplemente sondear la red a atacarla. Después de que el hacker haya accedido, puede comenzar a moverse de sistema en sistema, propagando su daño a medida que progresa. Los factores que determinan el método que utiliza un hacker para acceder a la red en última instancia se reducen a su nivel de habilidad, la cantidad de acceso que logra, la arquitectura de red y la configuración de la red de la víctima.



## Escalada de privilegios


En esta etapa ya el atacante está celebrando parte la victoria, pero aun queda mucho por hacer. La mayoría de las veces cuando se logra explotar una vulnerabilidad el atacante tiene acceso a la red, al servidor, pero sin privilegios administrativos lo cual dificulta mucho el avance del la prueba de penetración. Es el momento de escalar privilegios para hacerse con el control total del sistema que es el objetivo final.



Nuevamente el atacante debe buscar información, esta vez dentro del sistema operativo, enumerando procesos, aplicaciones instaladas, usuarios, grupos y permisos de estos, en busca de una vulnerabilidad o mala configuración a nivel local que permita al individuo escalar privilegios a administrador (en sistemas Windows), super administrador o root (en Linux & Unix). Esta etapa puede ser muy fácil o muy difícil en dependencia de la configuración del sistema víctima, las habilidades del atacante y los recursos que este tenga a su disposición.



## Post Explotación
En este punto, el atacante ha conseguido penetrar el sistema y alcanzar un nivel de acceso máximo, no hay nada que lo detenga de hacerse con toda la información confidencial del sistema y propagarse por toda la red interna de la empresa u organización, utilizando accesos autorizados de administradores legítimos, o creados por el mismo atacante.



Es el momento de hacerse con toda la información sobre el sistema comprometido, archivos del sistema, hashes de contraseñas, credenciales de usuarios, configuración de la red, bases de datos, tareas programadas y cualquier dato que sea de interés.



Toda esta nueva información una vez recopilada y analizada, nos da la oportunidad como atacantes, de propagarnos por toda la red interna de la organización, ganando acceso a otros ordenadores, otros servicios y otros sistemas. El objetivo final es hacerse con el control total de todos los dispositivos de la red.



En esta fase, hay un sin fin de posibilidades, se puede utilizar ingeniería social para engañar a administradores haciéndonos pasar por usuarios legítimos e incluso por personal directivo de la empresa, se pueden plantar sniffers para capturar todo el tráfico de la red y posteriormente analizarlo en busca de más información jugosa, se pueden crackear los hashes encontrados y posteriormente utilizar esas credenciales, se puede comenzar un segundo ataque de penetración esta vez escaneando, enumerando y explotando servicios internos de la organización. El atacante intenta propagar su infección, comprometer todos los dispositivos y servicios posibles de la organización. Esta fase puede durar, horas, días o semanas, en dependencia de qué tan grande sea esa empresa u organización y de que tan profundo se quiere hacer la prueba de penetración.



## Mantenimiento de acceso o Backdooring 


Es la etapa donde el atacante se asegura de poder entrar nuevamente en el sistema comprometido de forma fácil, donde asegura cada acceso no autorizado que a logrado conseguir utilizando puertas traseras (backdoors), RATs, rootkits, y todo tipo de malware que le pueda garantizar el acceso o controlar los sistemas comprometidos de forma remota.



Existen gran variedad de software que si bien no fueron creados con fines maliciosos, los atacante los utilizan con estos fines, como son el caso de RATs, rootkits y softwares como ansible, que fueron diseñados para facilitar la administración remota de multiples servicios a los administradores legítimos. Esto es un punto a favor de los atacantes, ya que al ser aplicaciones firmadas, reconocidas por las Empresas de Antivirus e IDSs como aplicaciones confiables y legales para realizar este tipo de función, muchas de estas no son detectadas como malware o aplicaciones sospechosas. También está la opción de inyectar código malicioso en aplicaciones legítimas haciendo uso de crypters que se encargan de ofuscar este código malicioso igualmente haciendo indetectable por los Antivirus.



Haciendo uso de este tipo de herramientas y técnicas, los atacantes consiguen mantener el acceso a sus víctimas, de forma indetectable y fácil de administrar, sin necesidad de explotar vulnerabilidades en servicios la próxima vez que quieran acceder a los sistemas comprometidos.



¿Creerías que los hackers son personas paranoicas? Bueno, muchos lo son, y les preocupa que sus malas acciones puedan ser descubiertas. Son diligentes trabajando en formas de mantener el acceso a los sistemas que han atacado y comprometido. Podrían intentar desplegar el archivo /etc/passwd o robar otras contraseñas para que puedan acceder a las cuentas de otros usuarios.



## Cubriendo pistas



Todo buen hacker, se preocupa porque sus acciones sean detectadas, incluso que otros hackers puedan ganar acceso a estos sistemas vulnerados y arriesgarse a perder estos sistemas. De ahí que muchos hackers, una vez que se hacen con el control total del sistema o la red y garantizan su acceso mediante el uso de Backdoors, RATs y Rootkits, eliminan o parchan las vulnerabilidades que le permitieron vulnerar el servicio para que otros hackers no puedan seguir sus pasos. 



Todo delincuente se preocupa por eliminar las pruebas que puede dejar en la escena del crimen, los ciberdelincuentes son iguales. 



Primeramente, todos estos ataques deben ser realizados utilizando cierta capa de anonimato, como es el uso de VPNs, TOR y el cambio de direcciones MACs en los adaptadores utilizados para realizar el ataque.



Lo segundo es que en las modificaciones que se realizan en el sistema de archivos de las víctimas, se deben alterar las fechas modificación (en el caso de ficheros modificados) y las fechas de creación (en el caso de nuevos ficheros), esto minimiza en gran cantidad la detección de estas modificaciones.



Lo tercero, los logs, todo sistema, todo servicio, por lo general tiene un archivo donde se guardan los accesos a estos servicios. Estos archivos de registro o logs del sistema, deben ser alterados o eliminados, igualmente para reducir las posibilidades de detección y análisis de actividades sospechosas.



Como Hackers de Sombrero negro, tienen la tarea de saber cómo cubrir sus huellas, y como Hackers de Sombrero Blanco tienen la tarea de conocer las herramientas y técnicas utilizadas por los Hackers para poder hacer una contramedida adecuada.



## Informe o Reporte
En ocaciones esta es la parte más difícil para un Pentester, "El Informe Final", pero también es la parte más importante, para obtener este informe has pasado un par de días o un par de semanas trabajando duro y no durmiendo o suficiente para tratar de reportar y demostrar más de lo que dice el pronóstico, seria muy chocante que cuando le presentes un Informe Técnico al cliente este diga "¿Qué hace esa ventana negra? ¿Qué son esas letras verdes? ¡Por favor, explícame que hiciste exactamente porque no entiendo nada!"



El Informe final, es el único documento que el cliente recibe y que demuestra todo lo que se ha realizado, debe ser un Informe bien preparado, escrito correctamente y explícito, dirigido a la audiencia correcta. El cliente debe comprender el contenido del informe. Para que un informe cumpla con todos los requisitos y pueda ser comprendido por todo tipo de audiencia, debe incluir una descripción general ejecutiva y un resumen técnico. El resumen ejecutivo, resume los ataques e indica su posible impacto y a la vez sugiere remedios. El resumen técnico debe incluir una metodología de los aspectos técnicos de la prueba de penetración, ya que generalmente es leído por personal del campo de TI.