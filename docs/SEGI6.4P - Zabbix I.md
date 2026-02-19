![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)

# Instalación y uso de *Zabbix* I

**Zabbix** es una herramienta de monitorización de [código abierto](https://es.wikipedia.org/wiki/C%C3%B3digo_abierto) que permite obtener información continua sobre el rendimiento de muy variados servicios de red, servidores y dispositivos de todo tipo. Zabbix puede recolectar una gran cantidad de datos de los tipos más diversos y mostrar gráficas que den una idea inmediata del estado de los dispositivos conectados a la red y de su evolución.

En esta guía vamos a instalar Zabbix sobre un servidor Debian.

## Máquina virtual 

Vamos a instalar Zabbix en una máquina virtual, y después configuraremos y usaremos la aplicación desde otra máquina, accediendo a través del interfaz web proporcionado por la aplicación en la primera máquina.

Para poder hacer esto, la máquina virtual donde instalaremos Zabbix necesitará una dirección IP fija; crea un servidor Debian usando [este tutorial](https://raw.githubusercontent.com/juliogaray/comunes/main/Tutorial%20de%20instalaci%C3%B3n%20Servidor%20Debian.md) y configúralo en la *red NAT* **SEGI**, con la dirección IP fija ```10.0.2.X```, donde 'X' es tu n.º de PC en clase. ten en cuenta que su **puerta de enlace** es la dirección de tu máquina anfitriona dentro de la red SEGI, es decir, 10.0.2.1.

## Proceso de instalación de Zabbix

Para poder usar Zabbix, necesitaremos instalar el [servidor de páginas web](https://es.wikipedia.org/wiki/Servidor_web) [**Apache**](https://es.wikipedia.org/wiki/Servidor_HTTP_Apache), el [SGBD](https://es.wikipedia.org/wiki/Sistema_de_gesti%C3%B3n_de_bases_de_datos) [**MariaDB**](https://es.wikipedia.org/wiki/MariaDB), y los paquetes del lenguaje de programación [**PHP**](https://es.wikipedia.org/wiki/PHP) (incluyendo varios módulos adicionales específicos). Vamos a ver todo el proceso paso a paso.

> **NOTA sobre seguridad**: en un caso real deberíamos configurar **SSL/TLS** para usar Zabbix con HTTPS como medida elemental de seguridad. En esta práctica vamos a obviar este paso para simplificarla. Puedes seguir [este enlace](https://www.server-world.info/en/note?os=Debian_12&p=httpd&f=3) para ver cómo obtener y configurar certificados digitales de *Let's Encrypt* para tu servidor web.

### Actualización del servidor

Vamos a instalar la aplicación Zabbix en un servidor GNU/Linux montado sobre una máquina virtual.

Si no acabas de crear el servidor, es siempre recomendable actualizar el sistema con los siguientes comandos:

```bash
sudo apt -y update
...
sudo apt -y upgrade
```

> **NOTA**: es probable que a lo largo de los próximos pasos desees copiar el texto de este documento a la máquina virtual. Normalmente esto no es posible, ya que la máquina virtual dispone sólo de un entorno de texto, y no maneja ningún tipo de portapapeles (la herramienta con la que hacemos normalmente las operaciones de copiar/cortar/pegar).
>
> Una forma de solucionar esto es conectarse a la máquina virtual con **SSH** desde una ventana de comandos de la máquina anfitriona; pero como la máquina virtual está en la red NAT, necesitamos hacer [redirección de puertos](https://es.wikipedia.org/wiki/Redirecci%C3%B3n_de_puertos). En el [tutorial de instalación de servidores Debian](https://raw.githubusercontent.com/juliogaray/comunes/main/Tutorial%20de%20instalaci%C3%B3n%20Servidor%20Debian.md) se explica cómo hacerlo: sección 6.4.

### Instalación de Apache

La instalación de Apache se efectúa con el siguiente sencillo comando:

```bash
# Antes de hacer la instalación, vamos a elevar privilegios para trabajar como 'root'
sudo su
# Ahora ya podemos instalar Apache y continuar con todos los demás pasos en modo privilegiado:
apt -y install apache2
```

#### Configuración de Apache

Vamos a seguir ahora una serie de instrucciones de seguridad básicas para la configuración de Apache.

1. Edita el archivo ```/etc/apache2/conf-enabled/security.conf``` y cambia las líneas que comienzan con los identificadores *ServerSignature* y *ServerTokens* para que queden así (por la razón explicada en [este enlace](https://medium.com/guayoyo/hardening-asegurando-apache-abc52f87d750)):

    ```properties
    ServerSignature Off
    ServerTokens Prod

    ```
2. Reinicia el servidor Apache:
    ```bash
    systemctl restart apache2
    ```

Para poder acceder al servidor Apache de tu máquina Zabbix, de nuevo tenemos que hacer redirección de puertos. Siguiendo la misma técnica que has visto para usar SSH con tu máquina Zabbix, asigna en VirtualBox (en la configuración de la Red NAT SEGI) un puerto de tu máquina anfitriona a la dirección IP del servidor Zabbix, puerto 80.

A continuación deberías poder ver, desde tu navegador (de la máquina anfitriona), la página por defecto de Apache para Debian, visitando la web ```http://D.I.M.A:PuertoAsociado/```, donde «D.I.M.A» es la dirección IP de tu máquina anfitriona, y «PuertoAsociado» el número de puerto que has usado para hacer la redirección en el párrafo anterior. Deberías ver algo parecido a esto:

![Página por defecto de Apache2 en Debian](https://i.stack.imgur.com/1NOHl.jpg)

### Instalación de PHP y PHP-FPM

**PHP** es un lenguaje de programación muy utilizado para generar páginas web dinámicas. **FPM** *(FastCGI Process Manager)* es un módulo para PHP que se utiliza para gestionar y mejorar el rendimiento de aplicaciones web desarrolladas en PHP. Se integra con servidores web, como Nginx y Apache, y proporciona una forma más eficiente de procesar las solicitudes PHP.

1. Para instalar PHP, sólo necesitamos proceder así:

    ```bash
    apt -y install php php-mbstring php-pear
    ```

2. Ahora podemos comprobar la versión instalada:

    ```bash
    php -v
    ```
3. A continuación procedemos a instalar FPM:

    ```bash
    apt -y install php-fpm
    ```

4. El siguiente paso es activar FPM en nuestro host virtual. Edita el archivo ```/etc/apache2/sites-available/000-default.conf``` y añade, al final de la sección ***\<VirtualHost\>*** (justo antes de la línea ***\</VirtualHost\>**)* el siguiente texto:

    ```apacheconf
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/php-fpm.sock|fcgi://localhost/"
    </FilesMatch>
    ```
5. Por último activa dos módulos necesarios, activa la configuración para usar FPM, y reinicia el módulo y el servidor Apache:

    ```bash
    a2enmod proxy_fcgi setenvif 
    a2enconf `ls /etc/apache2/conf-available|grep fpm`
    systemctl restart `ls /lib/systemd/system|grep fpm` apache2
    ```  
    **NOTA:** estos comandos se han diseñado para funcionar independientemente de la versión de PHP instalada. Si sabes la versión (p. ej. 8.2) puedes simplificarlos. El segundo comando sería, por ejemplo, ```a2enconf php8.2-fpm```. Y el último, ```systemctl restart php8.2-fpm apache2```.

6. Si lo deseas, puedes generar un archivo web en tu servidor Apache para acceder desde el navegador a la información de PHP:

    ```bash
    echo '<?php phpinfo(); ?>' > /var/www/html/info.php 
    ```

Ahora puedes acceder, desde tu navegador de la máquina anfitriona, a la web ```http://D.I.M.A:PuertoAsociado/info.php```, donde «D.I.M.A» es la dirección IP de tu máquina anfitriona, y «PuertoAsociado» el número de puerto que has usado para hacer la redirección a la web de Zabbix; deberías ver algo parecido a esto:

![Página web de información PHP](https://www.server-world.info/en/Debian_12/httpd/img/7.png)


### Instalación de MariaDB

Ten en cuenta que **MariaDB** es una especie de versión avanzada del conocidísimo SGBD **mysql**. Para mantener una completa compatibilidad, la gestión y acceso a bases de datos de *MariaDB* se hace exactamente igual (con los mismos comandos e instrucciones) que en el caso de *mysql.* Es decir, en la práctica sólo se observa la diferencia (entre *MariaDB* y *mysql)* durante la instalación de los paquetes de MariaDB.

1. El primer paso es bastante obvio:

    ```bash
    apt -y install mariadb-server
    ```

2. Ahora vamos a editar un archivo de configuración de MariaDB para especificar qué tipos de caracteres va a admitir y cómo debe hacer las ordenaciones alfabéticas. Edita el archivo ```/etc/mysql/mariadb.conf.d/50-server.cnf``` y, a la altura de la línea 95, asegúrate de dejar activas dos líneas así (en Debian suelen estar configuradas ya de esta manera pero, si no están, las puedes crear tú mismo):

    ```properties
    character-set-server  = utf8mb4
    collation-server      = utf8mb4_general_ci
    ```

3. Reinicia el SGBD (si has tenido que modificar el archivo del punto anterior):

    ```bash
    systemctl restart mariadb
    ```

4. La instalación por defecto de MariaDB, al igual que la de MySQL, es bastante insegura, y requiere de una serie de pasos para dotarla de una seguridad elemental. Por suerte existe un *script* que se encarga de ello; una vez concluida la instalación de los paquetes, procedemos a ejecutar:
    ```bash
    sudo mariadb_secure_installation
    ```
5. Este *script* nos planteará una serie de preguntas que debemos contestar para que pueda hacer su trabajo. La primera de ellas es ***«Enter current password for root»***. Se refiere al root de MySQL, es decir, a la cuenta de administración del SGBD (y **no del sistema operativo**).  
Puesto que aún no hemos puesto ninguna contraseña, **pulsaremos INTRO** para continuar.

6. En este momento se nos plantea la pregunta ***«Switch to unix_socket authentication? [Y/n]»**.* Podemos **pulsar 'N'** e \<INTRO\> (de esta manera impedimos las conexiones a *mysql* usando cuentas de usuario local de la máquina, lo que plantearía problemas serios de seguridad; eso sí, el usuario *root* del servidor siempre tendrá acceso *root* al SGBD).

7. A continuación nos preguntará si deseamos establecer una contraseña de root *(**«Set root password? [Y/n]»**).*  
 **Pulsa «N»** y pulsa \<INTRO\>. De esta manera no admitimos que nadie tenga acceso *root* al SGBD con una simple contraseña; sólo el usuario *root* del sistema podrá conectarse a la BD como *root* (en una situación real es posible que esta no sea la opción más deseable).

9. ***«Remove anonymous users? [Y/n]»**.* De nuevo pulsamos INTRO para deshacernos de los usuarios anónimos del SGBD.

10. ***«Disallow root login remotely? [Y/n]»**.* Otra vez pulsamos INTRO, para prohibir el acceso remoto de la cuenta root al SGBD.
11. ***«Remove test database and access to it? [Y/n]»**.* Por supuesto, de nuevo pulsamos INTRO, para eliminar la base de datos de pruebas y el acceso a la misma.

12. ***«Reload privilege tables now? [Y/n]»**.* Una vez más pulsamos INTRO para recargar las tablas de privilegios del SGBD.

13. Si todo ha ido bien, nuestro SGBD debería estar ya activo. Lo comprobamos con el siguiente comando, que nos debería dar como resultado, entre otras líneas, una que dice **«Active: <span style="color:green">active (running)</span>…»**:
    ```bash
    systemctl status mysql
    ```  
    Sal de la notificación de estatus pulsando 'q'.

14. Puedes comprobar si tienes acceso de root al SGBD con este comando:
    ```bash
    mysql -u root -p
    ```
15. Sólo podrás acceder desde el usuario *root* del sistema. Te pedirá la contraseña, pero puedes introducir cualquier cosa, porque se ignorará. Ahora tendrás acceso a la consola de MySQL. Para salir de ella y volver al sistema, teclea ```quit``` y pulsa \<INTRO\>.

### Instalación de Zabbix desde su propio repositorio

En el momento de crear este tutorial, la versión de Zabbix disponible en los repositorios Debian deja bastante que desear a la hora de instalarla, así que vamos a configurar los repositorios de Zabbix para bajarnos una versión actualizada del software desde allí. Usaremos la versión 7.0, que es la última versión [LTS](https://es.wikipedia.org/wiki/Soporte_de_largo_plazo) disponible:

```bash
cd
mkdir Descargas && cd Descargas
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_7.0+debian13_all.deb
dpkg -i zabbix-release_7.0+debian13_all.deb
apt update
apt -y install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent2 php-mysql php-gd php-bcmath php-net-socket
```

### Creación de una base de datos para Zabbix

Los pasos de creación de la base de datos para Zabbix son los mismos que para cualquier [CMS](https://es.wikipedia.org/wiki/Sistema_de_gesti%C3%B3n_de_contenidos). No obstante merece la pena que eches una ojeada a la subsección de ***Notas sobre la creación de la base de datos**,* más abajo. Entra en el SGBD de la forma habitual (recuerda que debes estar en modo superusuario):

```bash
mysql
```
Y, a continuación, con el prompt ```MariaDB [(none)]>```, ejecuta los siguientes comandos:

```sql
CREATE DATABASE bdzabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
GRANT ALL PRIVILEGES ON bdzabbix.* to usrzabbix@'localhost' IDENTIFIED BY 'psswrdzabbix';
SET GLOBAL log_bin_trust_function_creators = 1;
FLUSH PRIVILEGES;
QUIT
```
>**NOTA:** observa que todos los comandos deben acabar en «;». Excepto ```QUIT``` para salir del SGBD. No es necesario poner los comandos en mayúsculas (aunque es lo tradicional en [SQL](https://es.wikipedia.org/wiki/SQL)).

#### Notas sobre la creación de la base de datos

1. Hemos creado una BD llamada **bdzabbix**.
2. Hemos creado un usuario llamado **usrzabbix** (en la máquina local) que será el que use Zabbix para acceder a la BD. Puedes elegir cualquier otro nombre para el usuario, con tal de que **no lo olvides** (más tarde te hará falta). La parte «**@'localhost'**» no debe ser modificada.
3. Hemos definido la contraseña **psswrdzabbix** para el usuario del punto anterior. Igual que en el caso del nombre, puedes usar la contraseña que quieras; **¡pero no la olvides!**

#### Incorporación de los *scripts* SQL de Zabbix a la BD

Zabbix incluye una serie de *scripts* de SQL necesarios para su funcionamiento. Vamos a incorporarlos a su propia base de datos:

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -u usrzabbix -p bdzabbix
```

MariaDB te pedirá la contraseña del usuario usrzabbix que creamos en la sección anterior, e incorporará a continuación los *scripts* en cuestión a la base de datos zabbix (que definimos igualmente en la sección anterior).

**Este paso puede tardar varios minutos en completarse. Ten paciencia.**

### Configuración básica y arranque del servidor Zabbix

Edita el archivo ```/etc/zabbix/zabbix_server.conf```. Busca las siguientes propiedades, que deberás editar. La primera es el nombre de la BD (**zabbix**); la segunda es el usuario que accederá a ella (**usrzabbix**); y la última, la contraseña de dicho usuario (en nuestro ejemplo, **psswrdzabbix**). Si alguna línea está comentada con una almohadilla («#») al inicio, elimina dicho carácter:

```properties
# línea 105: confirma el nombre de la BD
DBName=bdzabbix

# línea 121: identifica el nombre del usuario
DBUser=usrzabbix

# línea 130: declara la contraseña de acceso de dicho usuario
DBPassword=psswrdzabbix
```

Guarda los cambios, sal del editor, y ahora edita el archivo ```/etc/apache2/conf-available/zabbix.conf```. Debajo de la linea en la que pone ```<Directory "/usr/share/zabbix">``` modifica la línea que comienza por «Options» para que quede así:

```apacheconf
Options Indexes FollowSymLinks Includes ExecCGI
```

Después de guardar los cambios y salir del editor, rearranca el servicio y prepáralo para que arranque automáticamente cada vez que reinicies la máquina:

```bash
systemctl restart zabbix-server
systemctl enable zabbix-server
```

### Configuración básica y arranque del agente Zabbix

El agente de Zabbix es una pieza de software que se puede instalar en cualquier máquina para permitir que el servidor Zabbix la monitorice. En este caso vamos a instalarlo en el propio servidor, para que Zabbix pueda monitorizar a su propia máquina.  
Edita el archivo ```/etc/zabbix/zabbix_agent2.conf``` y busca las siguientes propiedades, para dejarlas como en el ejemplo:

```properties
# línea 80: especifica la dirección IP del servidor Zabbix
Server=127.0.0.1

# línea 133: más o menos lo mismo
ServerActive=127.0.0.1

# línea 144: pon aquí el nombre de la máquina monitorizada (en este caso, el propio servidor Zabbix)
Hostname=mi-servidor-zabbix.iespacomolla.es
```

Después de guardar los cambios y salir del editor, arranca el servicio agente:

```bash
systemctl restart zabbix-agent2
```

### Configuración de PHP para Zabbix

Para acabar la instalación de Zabbix, configuraremos algunos parámetros de PHP que requiere nuestro software de monitorización para funcionar. Abre el archivo de configuración necesario como se indica:

```bash
nano /etc/php/`ls /etc/php/|tail -1`/fpm/pool.d/www.conf
```

Al final del archivo, añade las siguientes líneas (no modifiques las líneas parecidas que ya están presentes en el archivo):

```properties
php_value[max_execution_time] = 300
php_value[memory_limit] = 128M
php_value[post_max_size] = 16M
php_value[upload_max_filesize] = 2M
php_value[max_input_time] = 300
php_value[max_input_vars] = 10000
php_value[always_populate_raw_post_data] = -1
php_value[date.timezone] = Europe/Madrid
```

> NOTA: En una instalación real editaríamos también el archivo ```/etc/apache2/conf-enabled/zabbix.conf``` para modificar la línea 10, de manera que pusiese algo así como ```Allow from 192.168.2.0/24```, limitando las direcciones desde las que se puede acceder a Zabbix. En esta práctica no lo vamos a hacer.

#### Idioma

Zabbix por defecto no incluye el castellano entre sus idiomas disponibles. Para corregir este problema, tienes que editar el archivo ```/usr/share/zabbix/include/locales.inc.php``` y buscar la línea siguiente:

```properties
                'es_ES' => ['name' => _('Spanish (es_ES)'),     'display' => false],
```

Cambia el valor «false» a «true» (puedes hacer lo mismo con cualquier otro idioma que no aparezca en el menú de bienvenida). Guarda el archivo y sal del editor.

Para acabar esta fase, reinicia tanto Apache2 como el módulo PHP-FPM:

```bash
systemctl restart apache2 `ls /lib/systemd/system|grep fpm`
```

### Prueba tu instalación

Si has seguido todos los pasos correctamente, deberías poder acceder a la página de configuración web inicial de Zabbix. Abre tu navegador en otra máquina (por ejemplo, en tu máquina anfitriona), y en la barra de direcciones pon ```http://D.I.M.A:PuertoAsociado/zabbix/``` (recuerda que «D.I.M.A» es la dirección IP de tu máquina anfitriona, y «PuertoAsociado» el número de puerto que has usado para hacer la redirección). Si todo ha ido bien, verás la primera página de Zabbix para comenzar su configuración a través del interfaz web. Algo así:

![Página de bienvenida y selección de idioma de Zabbix](https://kifarunix.com/wp-content/uploads/2021/09/zabbix-welcome-page-1068x674.png?v=1630820413)

Comprueba que el idioma español está disponible (también lo estará el inglés, pero los demás idiomas estarán marcados en gris como «no seleccionables», simplemente porque no están instalados como [*locales*](https://es.wikipedia.org/wiki/Configuraci%C3%B3n_regional) en el sistema operativo). Si deseas instalar otros idiomas, asegúrate de que están instalados como locales del sistema y, si es necesario, sigue los pasos vistos para el castellano.

Si todo está bien, apaga el servidor y toma una instantánea. Después puedes seguir con la [segunda parte de este tutorial](https://julio.iespacomolla.es/SMR2.SEGI/SEGI6.4P%20-%20Zabbix%20II.md).

&nbsp;

&nbsp;

&nbsp;

## Referencias

[Página de instrucciones de ***Server World*** para instalar Zabbix en Debian 12](https://www.server-world.info/en/note?os=Debian_12&p=zabbix60&f=1)

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2025 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)


