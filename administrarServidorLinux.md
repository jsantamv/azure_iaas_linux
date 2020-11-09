# ADMINISTRACION DE SERVIDORES EN BASADOS EN LINUX
Toda esta informacion se recopilo del curso que lleve en Platzi de administracion de servidores. 


## DIFERENCIAS ENTRE LESS, CAT, HEAD Y TAIL PARA LECTURA DE ARCHIVOS

cat: nos permite leer archivos en su totalidad.
	less: 	nos ayuda a leer el contenido de nuestros archivos por páginas. 
			Nos movemos con las flechas del teclado o la tecla de espacio. 
			Salimos de la lectura del archivo con la letra q. Buscamos palabras específicas escribiendo /palabra.
	tail: 	nos muestra las últimas 10 líneas de nuestros archivos.
	head: 	nos muestra las primeras 10 líneas de nuestros archivos
	
	CREAR ARCHIVOS
	touch name.txt


## INTERACCIÓN CON ARCHIVOS Y PERMISOS

Con el comando ls -l podemos observar la lista de archivos de nuestro directorio actual con información un poco más detallada. 
El primer campo nos indica los diferentes permisos para cada archivo o directorio. 

Por ejemplo: -rwxrw-r--.

El primer carácter nos indica si tenemos un archivo (-), enlace simbólico (l) o directorio (d).

Los siguientes caracteres se dividen en grupos de 3: 
	lectura (r), 
	escritura (w) 
	y ejecución (x). 
El primer grupo son los permisos del usuario que creó ese archivo, 
el segundo para el grupo al que pertenece este usuario y el tercero 
para cualquier otro usuario de tu sistema operativo.

Los grupos nos ayudan a darle los mismos permisos a diferentes usuarios 
sin necesidad de asignarlos a cada uno individualmente. Todos los usuarios 
que pertenezcan al grupo tendrán los mismos permisos.

Si en vez de estas letras encuentras un guion significa 
que ese usuario o grupo de usuarios no tiene permiso 
para esa acción en particular.

Por ejemplo: -rwxrw-r-- nos indica que trabajamos con un archivo. 
Todos los usuarios del sistema tienen permisos de lectura. 
El usuario creador y su grupo tienen permiso de escritura. 
Y solo el usuario creador puede ejecutar el archivo.

También podemos encontrar estos permisos como 3 números del 1 al 7. 
Estos números son la suma de los 3 caracteres de permisos para cada usuario o grupo.

	- = 0
	x = 1
	w = 2
	r = 4
Por lo tanto, los permisos de nuestro archivo de ejemplo serían: 7 (1+2+4) 6 (0+2+4) 4 (0+0+4).

Para cambiar los permisos de un archivo o directorio podemos usar el comando chmod + a quién queremos añadir o quitar los permisos:

El usuario propietario: u.
El grupo, 				g.
El resto de usuarios, 	o.
Para todos, 			a.
Por ejemplo, para añadir permisos de ejecución a nuestro usuario propietario usamos:
```b
chmod u+x nombre-del-archivo
```
También podemos cambiar al usuario propietario del archivo con el comando chown.
```b
sudo chown nuevo-usuario:grupo-usuarios nombre-del-archivo
```

## CONOCIENDO LAS TERMINALES EN LINUX

Las distribuciones de Linux para servidores no incluyen interfaz gráfica, 
ya que consumen muchos recursos. Esto significa que siempre vamos a trabajar desde la terminal.

Tendremos disponibles 6 terminales virtuales a las que podemos entrar o 
salir con las teclas Ctrl + Alt primer + Fx. También podemos usar el comando chvt. 
La séptima terminal es la interfaz gráfica, así que en este caso no disponemos de ella.

Cada usuario activo en nuestro sistema operativo crea una nueva conexión. 
Podemos ver todas estas conexiones con los comandos who y w (este último nos da un poco más de información).

Para ver todos los procesos que corren en el sistema podemos 
usar el comando ps. Para filtrar los procesos y ver únicamente 
las conexiones de los usuarios usamos ps -ft tty.

Este comando nos muestra el identificador de cada proceso. 
Para terminarlo podemos usar el comando kill -9 PID.

## MANEJO Y MONITOREO DE PROCESOS Y RECURSOS DEL SISTEMA
Para ver todos los procesos que corren en el sistema podemos usar el comando ps. Recuerda que puedes ver la documentación de este comando con el comando man ps.

El comando grep nos ayuda a filtrar el resultado de un comando o archivo dependiendo de las palabras de cada línea. Para esto también vamos a usar el pipe (|), 
un símbolo que nos ayuda a enviar el resultado de un comando a un segundo comando.

Por ejemplo, el comando *ps aux | grep platzi* imprime los procesos activos del sistema y, con ayuda del pipe, envía la lista al comando grep para filtrar el resultado, 
mostrando únicamente las líneas con la palabra platzi.

## MONITOREO DE RECURSOS DEL SISTEMA
El comando top nos permite interactuar con una interfaz gráfica 
que nos muestra información específica del sistema operativo: 
	cantidad de usuarios, 
	tareas corriendo o “durmiendo”, 
	identificadores de los procesos, 
	entre otras.
COMANDOS

- free: 	Me muestra información sobre la memoria de mi sistema. Con el modificador -h la información es más legible para un humano
- du: 	Muestra información sobre el disco duro. Con el modificador -hsc y un directorio especificado muestra el tamaño de ese directorio
- htop: 	Funciona como top pero funciona de forma más intuitiva

Para ver la información de la CPU podemos usar el comando 
```b
	cat /proc/cpuinfo | grep "processor". 
```

Recuerda que Linux hace diferencia entra mayúsculas y minúsculas, 
pero puedes usar el comando:
```b
	grep -i para filtrar sin estas diferencias de mayusculas o minisculas.
```

Para ver la información de la memoria podemos usar el comando:
	free o, para que la información sea más fácil de leer, free -h. 
Y para ver el uso del disco duro está el comando 
```b
	du o du -hsc.
```

### Comandos útiles
```b
cat /proc/cpuinfo | grep "processor": 	Muestra información sobre el CPU
sudo ps auxf | sort -nr -k 3 | head -5: Muestra los 5 procesos que más uso hacen del CPU
sudo ps auxf | sort -nr -k 4 | head -5: Muestra los 5 procesos que más uso hacen de la memoria RAM
```

## ANÁLISIS DE LOS PARÁMETROS DE RED
Una IP es un identificador único para los equipos que están conectados a una red.

Las IPs Privadas se utilizan para identificar los dispositivos dentro de una red 
local. Por ejemplo: los dispositivos conectados en tu casa u oficina.

Las IPs Públicas son la que se asignan a cualquier dispositivo conectado a Internet. 
Por ejemplo: los servidores que alojan tus sitios web, el router que te da acceso 
a internet, entre otros.

Si tu dispositivo tiene una IP pública significa que puede conectarse a otro que 
también tenga una. Por esto mismo, no puede haber dos dispositivos con la misma IP pública.

Para encontrar la dirección IP de nuestros dispositivos podemos usar los comandos 
ifconfig en Linux y Mac o ipconfig en Windows. También podemos usar el comando ip a.

Para ver el nombre/identificador de nuestro equipo en todas las redes podemos usar 
el comando hostname. También podemos ver qué dispositivo nos permite acceso a 
Internet con el comando route -n.

Para identificar las IPs de diferentes dominios podemos usar el comando nslookup 
nombredominio.ext. También podemos usar el comando curl para realizar consultas 
a algún servidor.

	ifconfig: 	Interface Configuration, muestra las tarjetas de red que tenemos y su direccionamiento específico
	ip a: 		IP Address Show, muestra las direcciones IP
	hostname: 	Como se identifica este equipo en la red
	route -n: 	Muestra cual es el dispositivo que me permite conectarme a internet
	nslookup: 	Muestra la dirección IP de un dominio determinado
	curl: 		Realiza consultas a un servidor
	wget: 		Permite descargar contenido de un servidor

Comandos útiles

	ip -4 a: Muestra las direcciones IPv4

## 	ADMINISTRACIÓN DE PAQUETES ACORDE A LA DISTRIBUCIÓN

Cada distribución de Linux maneja su software de maneras diferentes.

Red Hat / CentOS / Fedora
Su gestor de paquetes es .rpm (Red hat Package Manager). 
La base de datos de este gestor está localizada en /var/lib/rpm.

El comando rpm -qa nos permite listar todos los rpms instalados en la máquina. 
Con rpm -i nombre-del-paquete.rpm instalamos los paquetes y con rpm -e nombre-del-paquete.rpm los removemos el sistema.

Los paquetes se pueden instalar desde un repositorio sin tener que conocer 
la ruta del archivo o las dependencias con el comando yum install nombre-del-paquete.

También podemos buscar paquetes más específicos con el comando yum search posible-nombre-del-paquete .

Debian / Ubuntu
Su administración de paquetes es .deb. Podemos realizar las instalaciones 
con dpkg -i nombre-del-paquete.deb o repositorios apt.

Su base de datos está localizada en /var/lib/dpkg. 
Con el comando dpkg -l listamos todos los debs instalados en la máquina. Instalamos los paquetes con dpkg -i nombre-del-paquete y los removemos del sistema con dpkg -r nombre-del-paquete.

Si ya tenemos software configurado podemos usar el comando 
dpkg-reconfigure nombre-paquete para volver a ejecutar el asistente de configuración (si está disponible).

	También podemos realizar las instalaciones con el comando 
		apt install nombre-paquete y búsquedas de paquetes con apt search posible-nombre-paquete.
RED HAT / CENTOS / FEDORA
	.rpm Red Hat Package Manager.
	Base de datos RPM, localizada en var/lib/rpm
	rpm -qa Listar todos los rpms instalados en la máquina. (query all)
	rpm -i paquete.rpm Realizar la instalación de un paquete. (install)
	rpm -e paquete.rpm Remover un paquete del sistema. (erase)
	Repositorios yum Permite instalar un paquete desde un repositorio sin tener que conocer la ruta del archivo o las dependencias.
	yum install paquete
DEBIAN / UBUNTU
	.deb Debian package management.
	Base de datos DPKG, localizada en /var/lib/dpkg
	dpkg -l Listar todos los debs instalados en la máquina.
	dpkg -i paquete.deb Realizar la instalación de un paquete.
	dpkg -r paquete.deb Remover un paquete del sistema.
	dpkg-reconfigure
	dpkg-reconfigure paquete Volver a ejecutar el asistente de configuración si está disponible.
	repositorios apt otra forma de instalar.
	apt install paquete

## MANEJO DE PAQUETES EN SISTEMAS BASADOS EN DEBIAN
Antes de actualizar el software de nuestro sistema debemos 
ejecutar el comando sudo apt update para saber qué paquetes 
pueden actualizarse y desde dónde se realizará la descarga.
 Luego de esto podremos actualizar todas las herramientas 
 del sistema usando el comando sudo apt upgrade.

Recuerda que todo lo que tenga que ver con actualizaciones 
o modificaciones del sistema operativo necesitará permisos 
con sudo. También necesitarás conexión a Internet.

Mis apuntes de esta clase:

2.3 Manejo de paquetes en sistemas basados en Debian
	apt-get update ó apt update
	Actualización de los índices del SO.

Cualquier update del SO se debe ejecutar con sudo.

	apt-get upgrade ó apt upgrade: Para descargar e instalar los paquetes de actualización.

En entornos productivos siempre se debe verificar que la información mostrada sea acorde a lo esperado.

	apt dist-upgrade: Realiza actualizaciones a escala de kernel. Estas actualizaciones siempre requieren reinicio, a no de ser de tener Live Patch permite estas actualizaciones sin tener que hacer reinicio, pide registrar hasta 3 máquinas para este proceso.

apt seach paquete ó apt-cache search
Para realizar busqueda de paquetes. Para refinar la búsqueda agregar al final del nombre de paquete $, y encerrar todo el nombre del paquete entre comillas. “mysql-server$”.

tzdata es el paquete que configura la hora del servidor. 
Para reconfigurarlo utilizamos dpkg-reconfigure tzdata.

snap
Otra (nueva) manera de buscar paquetes.
snap search nombre_paquete ==> buscar un paquete.
snap refresh --list ==> Para ver toda la lista de paquetes.
snap info nombre_paquete ==> verificar la información de un paquete especifico.

## ADMINISTRACIÓN DE SOFTWARE CON YUM Y RPM PARA CENTOS

2.4 Administración de software con YUM y RPM para CentOS

rpm -qa:Enlista los paquetes instalados en el SO.

rpm -qi nombre_paquete:Mostrar la información sobre un paquete especifico.

Con Bash podemos hacer scripting en SO Linux.

rpm -qc nombre_paquete: Muestra todos los archivos involucrados sobre el paquete.

También podemos usar yum. Pero lo primero es dar yum update. 
Pero para poder ejecutarlo necesitamos un usuario con todos 
los permisos, por ejemplo el usuario root.

Si se muestra un # al final del nombre del usuario, eso indica
que estamos trabajando con un usuario root. Por ejemplo:
[root@server ~]#

Lo ideal es nunca trabajar con un usuario root. Lo ideal es
crear usuarios que tengan ciertos permisos específicos, 
por medidas de seguridad y evitar errores.

yum install net-tools: Para habilitar el ifconfig.

rpm -e nombre_paquete: Para eliminar un paquete del SO.

INSTALAR HTOP
	yum -y install epel-release
	yum -y update
	yum -y install htop

## NAGIOS: DESEMPAQUETADO, DESCOMPRESIÓN, COMPILACIÓN E INSTALACIÓN DE PAQUETES

No todo el software que necesitamos se encuentra en los repositorios. 
Debido a esto, algunas veces debemos descargar el software, realizar 
un proceso de descompresión y desempaquetado para finalmente instalar 
la herramienta.

Instalación de algunas herramientas para manejar una base de datos 

MySQL (recuerda que instalaremos y trabaremos con MySQL en una próxima clase):
	sudo apt install build-essential libgd-dev openssl libssl-dev unzip apache2 php gcc libdbi-perl libdbd-mysql-perl

Instalación de Nagios:
	wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.4.tar.gz -O nagioscore.tar.gz

Descomprimir y desempaquetar archivos con tar:
	tar xvzf nagioscore.tar.gz
	
Este comando creará una carpeta nagios-4.4.4. El nombre de la carpeta puede variar 
dependiendo de la versión que descargaste. Entrando a esta carpeta podemos ejecutar diferentes archivos y comandos para configurar el software y realizar la instalación.
```b
1. sudo ./configure --with-https-conf=/etc/apache2/sites-enabled
2. sudo make all
2. sudo make install
```
Si les da error en el paso de ejecutar “sudo make install”, primero tienen que hacer lo siguiente:
```b
sudo make install-groups-users
sudo usermod -a -G nagios www-data

4. sudo make install-init
5. sudo make install-commandmode
6. sudo make install-config
7. sudo make install-webconf
```
Por último, para administrar el servicio de nagios podemos usar el comando 
	sudo systemctl (status, start, restart, reload, stop, enable, disable, list-dependencies) nagios.
	
el resto de la configuracion 
https://support.nagios.com/kb/article/nagios-core-installing-nagios-core-from-source-96.html#Ubuntu

http://172.18.26.138/nagios/ ==> la url

					LOS USUARIOS, UNA TAREA VITAL EN EL PROCESO DE ADMINISTRACIÓN DEL SISTEMA OPERATIVO

El comando id nos muestra el identificador único (uid) de cada usuario en nuestro 
sistema operativo. El ID 0 está reservado para el usuario root.

Con el comando whoami podemos ver con qué usuario estamos trabajando en este momento. 
Podemos ver todos los usuarios del sistema leyendo el archivo /etc/passwd.

Las contraseñas de los usuarios están almacenadas en el archivo etc/shadow, 
pero están cifradas. Y solo el usuario root tiene permisos de lectura/escritura.

Para cambiar la contraseña de nuestros usuarios usamos el comando passwd.

RESUMEN DE LA CLASE
|
Los usuarios, una tarea vital en el proceso de administración del sistema operativo
|
|
Comandos
|
	id: Muestra el identificador único de mi usuario, del grupo al que pertenezco y los grupos de los cuales formo parte
	whoami: Muestra que usuario soy
	passwd: Cambia la contraseña del usuario actual
|
|
Comandos útiles
|
	cat /etc/passwd: Muestra todos los usuarios del sistema operativo
	cat /etc/shadow: Muestra las contraseñas del sistema operativo
Por lo general no tenien acceso
	sudo cat /etc/shadow
Cambiar un usuario el passwd
	sudo passwd root
	

							COMANDOS PARA ADMINISTRAR CUENTAS DE USUARIOS:

sudo useradd nombre-usuario: crea un usuario sin asignarle inmediatamente alguna contraseña 
							 ni consultar más información. Debemos terminar de configurar esta cuenta a mano posteriormente.
							 
sudo adduser nombre-usuario: crea un nuevo usuario con contraseña y algo más de información. 
							 También creará una nueva carpeta en la carpeta /home/.
							 
userdel nombre-usuario: eliminar cuentas de usuarios.

usermod: modificar la información de alguna cuenta.

Nunca modifiques a mano el archivo /etc/passwd. Para administrar los usuarios debemos usar los comandos que estudiamos en clase.

TRUCO

comando llamado:
	history
Este comando nos ayuda ver el historial de comando ejejcutados 
y con
	!No de comando : !12

se vuelve a ejecutar. 

RESUMEN DE LA CLASE
|
Entendiendo la membresía de los grupos
|
|
Comandos
|
su - usuario: Switch User, cambia de usuario
groups usuario: Muestra a que grupos pertenece cierto usuario
sudo gpasswd -a usuario grupo: Agrega un usuario a un grupo
sudo gpasswd -d usuario grupo: Quita a un usuario de un grupo
usermod -aG grupo usuario: Agrega un usuario a un grupo
sudo -l: Muestra que permisos tiene el usuario actual

						USANDO PAM PARA EL CONTROL DE ACCESO DE USUARIOS
PAM es un mecanismo para administrar a los usuarios de nuestro sistema operativo. 
Nos permite autenticar usuarios, controlar la cantidad de procesos que ejecutan 
cada uno, verificar la fortaleza de sus contraseñas, ver la hora a la que se 
conectan por SSH, entre otras.

Con el comando "pwscore" podemos probar qué tan fuertes son nuestras contraseñas. 
Recuerda que para usar este comando en sistemas basados en Ubuntu debemos 
instalar el paquete libpwquality-tools. >>> sudo apt install libpwquality-tools

El comando ulimit nos ayuda a listar los permisos de nuestros usuarios. 
Para limitar el número de procesos que nuestros usuarios pueden realizar 
ejecutamos ulimit -u max-numero-procesos.

RESUMEN DE LA CLASE
|
Usando PAM para el control de acceso de usuarios
|
|
Comandos
|
pwscore: Evalúa si una contraseña es buena o mala del 0 al 100
ulimit: Muestra los permisos que tiene el usuario actual. Modificadores:

-u numero: Cambia la cantidad de procesos que mi usuario puede ejecutar
|
|
Comandos útiles
|
sudo vi /etc/security/time.conf: Modifica el archivo que indica en que horarios pueden conectarse ciertos usuarios

							AUTENTICACIÓN DE CLIENTES Y SERVIDORES SOBRE SSH

SSH es un protocolo que nos ayuda a conectarnos a nuestros 
servidores desde nuestras máquinas para administrarlos de 
forma remota. No es muy recomendado usar otros protocolos 
como Telnet, ya que son inseguros y están deprecados.

Con el comando ssh-keygen podemos generar llaves públicas y 
privadas en nuestros sistemas, de esta forma podremos 
conectarnos a servidores remotos o, si es el caso, 
permitir que otras personas se conecten a nuestra máquina.

Para conectarnos desde nuestra máquina a un servidor remoto debemos:

Ejecutar el comando ssh-copy-id -i ubicación-llave-pública nombre-usuario@dirección-IP-servidor-remoto 
y escribir nuestra contraseña para enviar nuestra llave pública al servidor.
Usar el comando ssh nombre-usuario@dirección-IP-servidor-remoto 
para conectarnos al servidor sin necesidad de escribir contraseñas.
También podemos usar el comando ssh -v ... para ver la información 
o los errores de nuestra conexión con el servidor. Puedes usar la 
letra v hasta 4 veces (-vvvv) para leer más información.

Las configuraciones de SSH se encuentran en el archivo /etc/ssh/sshd_config.

RESUMEN DE LA CLASE
|
Autenticación de clientes y servidores sobre SSH
|
|
SSH: Secure Shell, es un protocolo que permite conectar dos computadoras de forma remota sin necesidad de un password, únicamente con la interacción de una llave pública y una llave privada (aunque podemos colocar una contraseña sobre las llaves)
|
|
Configuración
|

En el servidor, abrir el archivo /etc/ssh/sshd_config con algún editor. Leer el archivo y configurar a gusto.
En la consola de la máquina cliente abrir ssh-keygen para generar las llaves
Elegir ubicación para guardar la llave privada
Ejecutar ssh-copy-id -i directorio_de_llave/id_rsa.pub nombre_usuario@direccion_ip_del_servidor para copiar la llave pública al servidor
Ejecutar ssh nombre_usuario@direccion_ip_del_servidor en la máquina cliente para conectarnos exitosamente de forma remota
|
|
Tips
|

En lugar de descargar Putty en Windows podemos utilizar el emulador de consola Unix llamado Cmder para ejecutar los comandos vistos en clase. Incluso si esto falla, lo que personalmente recomiendo es instalar un subsistema de linux si tenemos Windows 10. Platzi tiene incluso un artículo sobre como hacer eso: https://platzi.com/clases/1378-python-practico/19200-importante-instalando-ubuntu-bash-en-windows-para-/
Si la conexión falla, podemos usar el modificador -v (verbose) en el comando ssh para poder ver la información que envían las máquinas que intentan conectarse. La “v” puede repetirse hasta cuatro veces, quedando el comando, por ejemplo, como: ssh -vvvv nombre_usuario@direccion_ip_del_servidor. A mas “v” pongamos, más información se mostrará
|
|
BONUS
|
Reto: Restringir el acceso al usuario root por ssh, y permitir solo un usuario determinado conectado
|
Solución:
|
Colocar en el archivo /etc/ssh/sshd_config del servidor las siguientes líneas:
PermitRootLogin no
AllowUsers nombre_usuario
Ejecutar el siguiente comando para reiniciar el servicio de ssh:
sudo service sshd restart
-

## ARRANQUE, DETENCIÓN Y RECARGA DE SERVICIOS
El comando systemctl nos permite manejar los procesos de nuestro sistema 
operativo. Nuestros servicios pueden estar activos (es decir, encendidos) 
o inactivos (apagados). También podemos configurar si están habilitados 
o deshabilitados para correr automáticamente con el arranque del sistema.

sudo systemctl status nombre-servicio: ver el estado de nuestros servicios.
sudo systemctl (enable, disable) nombre-servicio: activar o desactivar el
arranque automático de nuestros servicios.
sudo systemctl (start, stop, restart) nombre-servicio: encender, apagar o 
reiniciar los servicios.
sudo systemctl list-units -t service --all: ver todos los servicios del sistema.
El comando journalctl nos permite ver los logs de los procesos de nuestro 
sistema operativo. Recuerda que todos ellos están almacenados en la carpeta /var/log/.

sudo journalctl -fu nombre-servicio: ver los logs de nuestros servicios y hacer un seguimiento.
sudo journalctl --disk-usage: ver la cantidad de espacio que ocupan nuestros logs.
sudo journalctl --list-boots: ver los logs desde el último arranque del sistema.
sudo journalctl -p (critic, info, warning, error): filtrar los logs por el tipo de mensaje.
sudo journalctl -o json: ver los logs en formato JSON.


RESUMEN DE LA CLASE
|
Arranque, detención y recarga de servicios
|
|
Comandos
|

sudo systemctl status servicio: Estado de un servicio
sudo systemctl enable servicio: Habilita un servicio
sudo systemctl disable servicio: Deshabilita un servicio
sudo systemctl start servicio: Enciende un servicio
sudo systemctl stop servicio: Apaga un servicio
sudo systemctl restart servicio: Reinicia un servicio
sudo systemctl list-units -t service --all: Lista los servicios del sistema
sudo journalctl -fu servicio: Muestra el log de un servicio
sudo journalctl --disk-usage: Muestra cuanto pesan los logs en el sistema operativo
sudo journalctl --list-boots: Muestra los booteos de la computadora
sudo journalctl -p critic|notice|info|warning|error: Muestra mensajes de determinada categoría de nuestros logs
sudo journalctl -o json: Muestra los logs en formato json

## INSTALACIÓN Y CONFIGURACIÓN DE NGINX
1. Veriricar que apache2 no este arriba
	sudo netstat -tulpn
2. en caso afirmativo bajamos el apache2
3. Debemos de validar donde esta la configuracion de NGINX
	en /etc/nginx$ ls
	vi nginx.conf
	sites-enabled/		>
	sites-available/ 	> "less default"  cuales son los servicios activos cuales paginas esta leyendo y el puerto
	luego validamos la ruta del 

RESUMEN
	sudo apt search nginx
	sudo apt search "nginx$"
	sudo apt update && sudo apt install nginx
	sudo systemctl status apache2
	sudo systemctl status nginx
	netstat -tulpn
	sudo systemctl stop apache2
	sudo systemctl status apache2
	netstat -tulpn
	cd /etc/nginx
	ls
	vi ngnix.conf
	cd sites-available
	ls
	vi default
	cd /var/www/html
	curl localhost
	curl -I localhost
	cd /etx/nginx/sites-enabled/
	ll
-
## ¿QUÉ ES NGINX AMPLIFY?

NGINX Amplify es una herramienta SaaS que permite realizar el monitoreo de NGINX y NGINX Plus. 
Los factores que permite monitorear son el rendimiento, configuraciones con análisis estático.
 parámetros del sistema operativo, así como PHP-FPM, bases de datos y otros componentes. 
 Nginx Amplify es de fácil configuración y llevar control de nuestros servidores es agradable 
 por los tableros de administración que posee.

Con NGINX Amplify podrás recolectar más de 100 métricas de NGINX y el sistema operativo. 
Amplify analiza los archivos de configuración propios del servidor, detecta configuraciones 
incorrectas y da recomendaciones de seguridad, también permite crear notificaciones que pueden 
ser enviadas por correo o a un canal de Slack con un simple clic.

Los tableros de mando de Amplify sirven para verificar la disponibilidad del sitio e 
identificar situaciones anómalas en diferentes periodos de tiempo. Otra característica 
a destacar es que NGINX Amplify te permite administrar varios sitios, direcciones 
IP y un nombre para identificarlo.

Si necesitas conocer un poco más de esta herramienta visita el sitio:
https://www.nginx.com/products/nginx-amplify/

NGINX Amplify: Instalación y configuración de un servidor para producción
NGINX Amplify es una herramienta que permite monitorear aplicaciones y servicios de NGNIX. 
Nos ayuda a detectar problemas en nuestros proyectos, trackear las peticiones, 
conexiones, tiempos de respuesta, tráfico, uso de CPU, entre otras.

Please install the Amplify Agent on a new host system to start monitoring:
1. Use SSH to connect and log in to the system that you want to monitor.
2. Download install script using curl (1) or wget (1).
curl -L -O https://github.com/nginxinc/nginx-amplify-agent/raw/master/packages/install.sh
or

wget https://github.com/nginxinc/nginx-amplify-agent/raw/master/packages/install.sh
3. Run the following command as root to install the Amplify Agent package.
API_KEY='10da25bf4c6d5a5513a6d6f1395dcbf7' sh ./install.sh
Continue
4. After a successful installation, the new system appears in the list on the left in about 1 min or so.

Para los que les interese. se puede instalar en Ubuntu 20 de la siguiente manera:

INSTALAR EN UBUNTU 20
	Después de darle los permisos de ejecución al archivo install.sh de abre con vi o nano
	se edita la linea que dice “packages_url=” se reemplaza la url de las comillas por esta:
	https://packages.amplify.nginx.com/py3/
	Se ejecuta como dice en Nginx Amplify API_KEY=’#####…’ sh ./install.sh
	Y listo ya con eso!
	
 TO START AND STOP THE AMPLIFY AGENT TYPE:

     service amplify-agent { start | stop }

-
## MONITOREO DE MYSQL CON NAGIOS
	Instalación de MySQL:

		sudo apt install mysql-server
		Conexión a una base de datos MySQL:	mysql -u nombre-usuario -p
		Escribe tu contraseña en una nueva línea, de otra forma quedará guardada 
		en tu historial de comandos. En caso de que el host de la base de datos 
		se encuentre en alguna otra máquina debes especificarlo con -h ip-host.

		Recuerda que puedes encontrar la información de tu base de datos 
		(incluyendo la contraseña) en el archivo 
			/etc/mysql/debian.cnf.
			sudo vim /etc/mysql/debian.cnf

	SEGURIDAD
		sudo mysql_secure_installation
	
	INSTALACIÓN DE NAGIOS
		netstat -tulpn
		sudo systemctl start apache2
		sudo systemctl status apache2
		sudo a2enmod rewrite cgi: apache2 activeme un modulo
		sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
		sudo systemctl restart apache2
		sudo systemctl status nagios
		sudo systemctl start nagios
			http://172.18.26.140/nagios/
	CAMBIAR PUERTO A APACHE
		Abre el fichero etc/apache2/ports.conf, puedes hacerlo con el editor VI o vía FTP.
			vi etc/apache2/ports.conf
		Busca la línea con el texto listen
			Listen 80
	
	CONFIGURACION DE NAGIOS
	Instalar las siguientes dependencias:
		sudo apt install -y libmcrypt-dev make libssl-dev bc gawk dc build-essential snmp libnet-snmp-perl gettext
	
	Si no instalaste los plugins en las clases anteriores, debes hacer lo siguiente: 
	en primer lugar, posicionado en tu home, descargarlos
		wget https://nagios-plugins.org/download/nagios-plugins-2.2.1.tar.gz -0 plugins.tar.gz -O plugins.tar.gz
		tar xzvf plugins.tar.gz
		sudo ./configure
	
	Verificar que no existan errores ni warnings
		sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
		sudo systemctl restart nagios
	
	En nuestro home, descargar el plugin de MySQL
		wget https://labs.consol.de/assets/downloads/nagios/check_mysql_health-2.2.2.tar.gz -O mysqlplugin.tar.gz
	Desempaquetar y descomprimir el archivo del plugin
		tar xzvf mysqlplugin.tar.gz
	
	CONFIGURACION DE NAGIOS MYSQL
		sudo mysql
	Creamos un usuario para todas las DB y todas las tablas		
		CREATE USER 'nagios'@'localhost' IDENTIFIED BY '123';
		GRANT ALL PRIVILEGES ON * . * TO 'nagios'@'localhost';
		FLUSH PRIVILEGES;  /*Para que se apliquen todos los privilegios en MySQL*/
	
	Configurar Nagios
		sudo vim /usr/local/nagios/etc/nagios.cfg

		#Ya dentro del archivo, agregar la siguiente linea:
			cfg_file=/usr/local/nagios/etc/objects/mysqlmonitoring.cfg
		
	Luego Crear comandos para hacer uso de Nagios
		sudo vim /usr/local/nagios/etc/objects/commands.cfg
	
		y agregamos las siguientes lineas:
			define command {
				command_name check_mysql_health
				command_line $USER1$/check_mysql_health -H $ARG4$ --username $ARG1$ --password $ARG2$ --port $ARG5$  --mode $ARG3$
			}
		
		Crear el archivo que nombrarmos en la configuración en el archivo nagios.cfg
			sudo vim /usr/local/nagios/etc/objects/mysqlmonitoring.cfg

			#Ya en el archivo, agregar las siguientes líneas

		define service {
			use local-service
			host_name localhost
			service_description MySQL connection-time
			check_command check_mysql_health!nagios!nagiosplatziS14*!connection-time!127.0.0.1!3306!
		}

## 		LOS LOGS, NUESTROS MEJORES AMIGOS

FIND
Nos ayuda a buscar archivos y/o carpetas en el sistema operativo. 
Podemos filtrar por tipo de archivo con -type, por nombre con -name, 
sin hacer diferencia entre mayúsculas y minúsculas con -i, por 
fecha de modificación con -mtime, entre otros.

Por ejemplo:
```b
find /etc -type f
sudo find /etc -mtime 10
find /var/log -name "*.log" -type f
find /var/log -iname "*.LOG" -type f
```

GREP
Nos ayuda a filtrar el resultado de un comando o archivo dependiendo de las palabras de cada línea.

Por ejemplo:

grep "server" /etc/nginx/sites-available/default
ps aux | grep platzi
AWK
Es un lenguaje de scripting que nos ayuda a procesar información usando patrones para filtrar, reorganizar y darle formato a nuestros datos.

Por ejemplo:

awk '{print $1}' /var/log/nginx/access.log
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr

--errores del servidor con el 9
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr


RESUMEN DE LA CLASE
|
Los logs, nuestros mejores amigos
|
|
Comandos
|
find [ruta]: Buscar algo en el sistema operativo.
Modificadores:

-type: Indica que tipo estamos buscando; archivos, directorios y enlaces
-name: Indica el nombre de lo que estamos buscando
-iname: Indica el nombre de lo que estamos buscando, pero sin tener en cuenta mayúsculas y minúsculas
!: Niega la expresión que buscamos (es decir, busca lo contrario)
-mtime: Muestra archivos con cambios en los últimos n minutos
grep [string] [archivo]: Busca una cadena de caracteres o expresión regular en un archivo determinado. Si ejecutamos por ejemplo algo como comando | grep [string] vamos a filtrar el resultado de un comando por la cadena o regex que especifiquemos
awk: Es un lenguaje que nos ayuda a filtrar patrones en un archivo, organizarlos y darles formato
|
|
Comandos útiles
|
find /var/log/ -iname "*.log" -type f: Muestra los archivos de log que tenemos en el sistema
sudo find /etc/ -mtime 10 2: Muestra los archivos de configuración que tuvieron salidas de error en los últimos diez minutos
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr: Muestra las IP’s que se conectaron con nuestro servidor nginx
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr: Muestra los errores que surgieron en nuestro servidor nginx	


## 	OTROS SERVICIOS DE LOGS--

Un servidor puede llegar a registrar millones de líneas de datos en un log. Para facilitar el monitoreo 
y mantenimiento podemos usar herramientas o tecnologías que nos permitan tomar esta información 
sin procesar y convertirla en visualizaciones fáciles de consumir y entender.

El primer paso para seleccionar las herramientas que usaremos. Lo primero es tener una base de 
conocimiento que nos identifique el servidor en circunstancias normales y de esta forma con la 
ayuda de estas herramientas detectar preocupaciones o incluso tendencias con una sola mirada.

Algunas herramientas que podemos tener en distribuciones Linux son:

Collectd
Es un demonio que recopila datos de rendimiento, y junto con la herramienta collectd web, es 
capaz de generar reportes que se pueden visualizar en un navegador WEB. Se puede establecer 
un servidor y a él conectarle un número ilimitado de clientes remotos. Podemos agregar más 
plugins si los necesitamos, para ello podemos visitar la página https://collectd.org/wiki/index.php/Table_of_Plugins

Nmon
Obtener visualizaciones rápidas de mi sistema. Se instala con apt install nmon. Tiene una 
característica especial que me permite guardar en archivos de formato nmon que se pueden convertir
 en información que puede ser presentada en html con la herramienta nmonchart. Nmon -f -s 15 -c 20, 
 se recolectará información por cinco minutos mostrados en incrementos de 15 segundos 20 veces.

Munin
Es una herramienta para analizar el rendimiento del servidor que contiene gráficos históricos 
para facilitar la identificación de problemas en el tiempo.

Grafana
Permite consultar, visualizar, alertar y ante todo entender las métricas de negocio sin importar 
dónde están almacenadas. Se puede crear, explorar y compartir tableros de mando con el equipo 
basados en el principio de la cultura orientada a los datos.

También podemos instalar agentes de monitoreo en los servidores, algunas opciones son https://newrelic.com/ 
y https://www.datadoghq.com/, podemos tener una prueba del servicio y analizar el rendimiento de nuestro servidor.

Cabe aclarar que también necesitará algún sistema de alarma automatizado que nos envíe alertas de forma 
proactiva cuando las cosas no estén funcionando bien.		
		--
## 		LAS BASES DE BASH /* LENGUAJE DE PROGRAMACION PARA TAREAS */--		
RESUMEN DE LA CLASE
|
Las bases de bash
|
¿Qué es Bash? Es una shell de UNIX y el intérprete de comandos por defecto en la mayoría de distribuciónes GNU/Linux. Se pueden crear scripts, los cuales por convención terminan con la extensión .sh
|

Definición de un intérprete para que lo que sigue se ejecute con Bash
	#!/bin/bash
Definición de una variable
	VARIABLE = "Hola mundo"
Impresión en pantalla
	echo $VARIABLE
Creación de un comentario
	# Comentario cualquiera		
		

--
## 		LAS VARIABLES Y SU ENTORNO DE EJECUCIÓN--
Las variables de entorno son un conjunto de variables globales en nuestros sistemas que
 nos permiten acceder de forma más fácil a una ruta o un conjunto de comandos difíciles 
 de recordar. Podemos usarlas en la terminal y en los archivos de bash.

El comando env nos permite ver todas las variables de entorno de nuestro sistema.
RESUMEN DE LA CLASE
|
Las variables y su entorno de ejecución
|
|
Comandos
|
env: Muestra las variables del sistema operativo
|
|
Variables de entorno
|
$PATH: Guarda las rutas donde se ubican los archivos binarios que pueden ejecutarse directamente en la consola
|
|
Scripts útiles
|

Verificar la cantidad de espacio en el S.O
#!/bin/bash
# Verificar la cantidad de espacio en el S.O
# Desarrollado por Jhon Edison

CWD=$(pwd)
FECHA=$(date +"%F%T")
echo $FECHA

df -h | grep /dev > uso_disco_"$FECHA".txt
df -h | grep /dev/sda2 >> uso_disco_"$FECHA".txt

echo "Se ha generado un archivo en la ubicación $CWD"
Entendido y funcionando.

env
pwd
echo $PATH

#!/bin/bash
# Verificar la cantidad de espacio
CWD=$(pwd)
FECHA=$(date +""%F%T"")
echo $FECHA

df -h | grep /dev > uso_disco_""$FECHA"".txt
df -h | grep /dev/sda2 >> uso_disco_""$FECHA"".txt

echo "Se ha generado un archivo con nombre uso_disco$FECHA.txt en la ubicacion $CWD"

							AUTOMATIZANDO TAREAS DESDE LA TERMINAL

#Nuestro principal trabajo como administradores de sistemas es automatizar las tareas y procesos de nuestro servidor. 
#En esta clase vamos a realizar un script que nos permita realizar una copia de seguridad de una base de datos MYSQL.

vi copia_mysql.sh

#!/bin/bash
# Shell script para obtener una copia desde MySQL
#desarrollador Juan Carlos

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

set -e

readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly SCRIPT_NAME="$(basename "$0")"

run
make_backup

function assert_is_installed {
	local readonly name="$1"
	if [[ ! $(command -v ${name})]]; then
		log_error "The binary '$name' is required but it isn't in 		our system"
		exit 1
	fi
}

function log_error {
	local readonly message "$1"
	log "ERROR" "$message"
}

function  log {
	local readonly level="$1"
	local readonly message="$2"
	local readonly timestamp=$(date +"%Y-%m-%d %H:%M:%S") >&2 echo -e "${timestamp} [${level}] [$SCRIPT_NAME] ${message}"
		
}

function run {
	assert_is_installed "mysql"
	assert_is_installed "mysqldump"
	assert_is_installed "gzip"
	assert_is_installed "aws"
}

function make_backup {
	local BAK="$(echo $HOME/mysql)"
	local MYSQL="$(which mysql)"
	local MYSQLDUMP="$(which mysqldump)"
	local GZIP="$(which gzip)"
	local NOW="$(date +"+%d-%m-%Y")"
	local BUCKET="xxxxx"
	
	USER="xxxxxx"
	PASS="xxxxxx"
	HOST="xxxxxx"
	DATABASE="xxxxxx"

	[! -d "$BAK" ] && mkdir -p "$BAK"

	FILE=$BAK/$DATABASE.$NOW-$(date +"%T").gz
	local SECONDS=0
	
	$MYSQLDUMP --single-transaction --set-gtid-purged=OFF -u $USER -p $PASS -h $HOST $DATABASE | $GZIP -9 > $FILE

	duration=$SECONDS
	echo "$($duration / 60) minutes"
	aws s3 cp $BAK "s3://$BUCKET" --recursive
}

## 					CRONTAB /* Para las tareas programadas */
#Para ejecutar nuestra tarea de copia de seguridad debemos hacer uso de cron, el 
#cual es un administrador regular de procesos en segundo plano que comprueba si 
#existen tareas para ejecutar, teniendo en cuenta la hora del sistema.

#Las configuraciones de las tareas a ejecutar se almacenan en el archivo crontab 
#que puede ser editado con el comando crontab -e, si requerimos listar las tareas 
#que tenemos configuradas ejecutamos crontab -l.

#A continuación te muestro lo que se imprime en la pantalla al correr el comando crontab -e

#Para establecer una tarea automatizada con cron se debe seguir un formato específico 
#para definir una tarea como se muestra a continuación:

.--------------- minuto (0-59) 
|  .------------ hora (0-23)
|  |  .--------- día del mes (1-31)
|  |  |  .------ mes (1-12) o jan,feb,mar,apr,may,jun,jul... (meses en inglés)
|  |  |  |  .--- día de la semana (0-6) (domingo=0 ó 7) o sun,mon,tue,wed,thu,fri,sat (días en inglés) 
|  |  |  |  |
*  *  *  *  *  comando a ejecutar

#Lo siguiente sería definir la periodicidad de nuestro cron, para ello podemos hacer pruebas en el sitio https://crontab.guru. 
#Nosotros queremos que nuestra copia se ejecute todos los días a las 03:15 de la mañana, 
#pues es el momento donde menos tráfico tenemos en nuestra base de datos.

#RESUMEN DE LA CLASE
#Crontab

#¿Qué es cron?: Es un administrador regular de procesos en segundo plano que comprueba 
#si existen tareas para ejecutar, teniendo en cuenta la hora del sistema

#¿Qué es crontab?: Es el archivo de configuraciones de las tareas a ejecutar. 
#@Con el comando crontab -e se edita, con crontab -l se listan las tareas configuradas

#Formato de cron

minute(0-59) hour(0-23) day_of_month(1-31) month(1-12|jan,feb,mar...) day_of_week(0-6|sun,mon,tue...) interpreter(ej:"/usr/bin/bash") command(ej:"pwd > /home/plazi/pwd.txt")

## 	EL FIREWALL Y SUS REGLAS
#Los Firewalls son herramientas que monitorean el tráfico de nuestras redes para 
#identificar amenazas e impedir que afecten nuestro sistema.

#Recuerda que la seguridad informática es un proceso constante, así que ninguna 
#herramienta (incluyendo el firewall) puede garantizarnos seguridad absoluta.

#En Ubuntu Server podemos usar ufw (Uncomplicated Firewall) para crear algunas reglas, 
#verificar los puertos que tenemos abiertos y realizar una protección básica de nuestro sistema:

sudo ufw (enable, reset, status): activar, desactivar o ver el estado y reglas de nuestro firewall.
sudo ufw allow numero-puerto: permitir el acceso por medio de un puerto específico. Recuerda que el puerto 22 es por donde trabajamos con SSH.
sudo ufw status numbered: ver el número de nuestras reglas.
sudo ufw delete numero-regla: borrar alguna de nuestras reglas.
sudo ufw allow from numero-ip proto tcp to any port numero-puerto: restringir el acceso de un servicio por alguno de sus puertos a solo un número limitado de IPs específicas.

Recomendación
Abrir al público únicamente el puerto 80 (http), 443 (https). Para un conjunto de IP’s específicas, habilitar el puerto 22 (ssh)


## 	ESCANEO DE PUERTOS CON NMAP Y NIKTO DESDE KALI LINUX
RESUMEN DE LA CLASE
|
Escaneo de puertos con NMAP y NIKTO desde Kali Linux
|
|
Comandos
|

nmap -sV -sC -0 -oA nombre_de_archivo dirección_ip_del_servidor: Realiza un mapeo de la red
nikto -h ip_del_host -o nombre_de_archivo: Escanea vulnerabilidades en un servidor

							LYNIS IT PERFORMS AN EXTENSIVE HEALTH SCAN
sudo lynis audit system

#Lynis is a battle-tested security tool for systems running Linux, macOS, or Unix-based operating system. 
#It performs an extensive health scan of your systems to support system hardening and compliance testing. 
#The project is open source software with the GPL license and available since 2007.

							CONFIGURACIÓN DE NODE.JS EN UN AMBIENTE PRODUCTIVO
#Descarga del repositorio con el proyecto de Node.js:

#Instalación de Node.js:
sudo apt install nodejs npm

#Descarga e instalación de la versión 10 de Node.js:
curl -sL https://deb.nodesource.com/setup_10.x -o node_setup.sh
sudo bash node_setup.sh
sudo apt-get install gcc g++ make
sudo apt-get install -y nodejs

#Creación de un usuario para manejar los procesos de Node.js:
sudo adduser nodejs
# si da error de permisos de usuario https://alexariza.net/tutorial/otorgar-permisos-de-root-a-un-usuario-nuevo-en-linux/

sudo su - nodejs
git clone https://github.com/edisoncast/linux-platzi

#Creación del script /lib/systemd/system/platzi@.service para que el servicio de Node.js arranque con el sistema operativo:
ls -l /lib/systemd/system/ # para validar lo que tenemos servicios que tenemos
sudo vim /lib/systemd/system/platzi@.service

# Una vez creado el archivo, llenarlo con la siguiente información

[Unit]
Description=Balanceo de carga para Platzi
Documentation=https://github.com/edisoncast/linux-platzi
After=network.target

[Service]
Environment=PORT=%i
Type=simple
User=nodejs
WorkingDirectory=/home/nodejs/linux-platzi
ExecStart=/usr/bin/node /home/nodejs/linux-platzi/server.js
Restart=on-abort

[Install]
WantedBy=multi-user.target


#Cambiar el nombre a la carpeta de linux-platzi a server
#Corregir los errores en el archivo de configuración del servicio en 
#/lib/systemd/system/platzi@.service

#Iniciar el servicio (debemos estar en la carpeta /server/configuracion_servidor/bash)
./enable.sh
./start.sh

#Comando para ver los logs
sudo journalctl -fu platzi@.service

#Iniciar el servicio de Nginx (Apagar antes Apache si es necesario)
sudo systemctl start nginx

#Una vez en la carpeta /etc/nginx/sites-available/ 
#eliminar el contenido de la configuración de Nginx
sudo truncate -s0 default

#Editar el archivo de configuración
sudo vim default

# Una vez en el archivo, escribir lo siguiente

server  {
	listen 80 default_server;
	listen [::]:80 default_server;
	
	server_name _;
	
	location / {
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $host;
		proxy_http_version 1.1;
		proxy_pass http://backend;
	}
}

upstream backend {
	server 127.0.0.1:3000;
	server 127.0.0.1:3001;
	server 127.0.0.1:3002;
	server 127.0.0.1:3003;
}

#Validamos que la configuración establecida fue correcta
sudo nginx -t

#Reiniciamos nginx
sudo systemctl restart nginx

#Probamos todo haciendo un curl a localhost
curl localhost
