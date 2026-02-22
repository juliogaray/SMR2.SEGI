![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)

# Instalación y configuración de un cortafuegos dedicado


En esta práctica vamos a instalar y configurar un cortafuegos dedicado, es decir, un ordenador con dos interfaces de red que podemos usar como cortafuegos para proteger nuestra red. Para ello montaremos el siguiente esquema con VirtualBox:

![Esquema práctica](http://julio.iespacomolla.es/SMR2.SEGI/img/SEGI-P5.3-esquema.png)

Como puedes ver, necesitaremos **tres** máquinas virtuales:
- Un **servidor web** que será la máquina de pruebas, protegida por nuestro cortafuegos;
- Una máquina virtual que será el **cortafuegos** en sí;
- Un ordenador ligero para usar su navegador para configurar el cortafuegos.

## Las máquinas virtuales
### El cortafuegos

![Ilustración de un cortafuegos](http://julio.iespacomolla.es/SMR2.SEGI/img/cortafuegos_mini.jpeg)

Usaremos una máquina sencilla en la que instalaremos IPFire.
Características requeridas:
- Versión: Linux
- Tipo: **Linux 2.6 / 3.x / 4.x (64-bit)**
- Memoria RAM: 2048 MB 
- Disco duro: VDI, 8 GB, tamaño fijo
- Configuración de red: dos interfaces, configurados de la siguiente manera:
    - Adaptador 1: Modo **red NAT** (será la conexión exterior, a tu máquina anfitriona y a Internet. **Debe estar conectada a la red SEGI**)
    - Adaptador 2: Modo **red interna** (será la conexión a la red protegida)

   > NOTA: La «red interna» es una **red virtual aislada**. Las máquinas conectadas a esta red pueden comunicarse entre ellas (si hay más de una) pero, en general, no pueden comunicarse con el exterior (tampoco con la máquina anfitriona).

Iniciaremos la máquina con el disco virtual de IPFire montado (puedes descargarlo del servidor del centro, en la dirección <http://172.30.12.180/?dir=ISOs/Linux>, o de la página web de IPFire: <https://www.ipfire.org/download>).

#### Instalación inicial del cortafuegos, desde la línea de comandos

1. Selecciona la opción por defecto, para instalar IPFire.
2. Elige el idioma que desees (probablemente «Español»).
3. Pulsa en [Comenzar la instalación] y, en la pantalla siguiente, acepta la licencia GPL.
4. En el disco duro, selecciona «Borrar todos los datos».
5. En sistema de archivos, deja la primera opción: [ext4](https://access.redhat.com/documentation/es-es/red_hat_enterprise_linux/8/html/managing_file_systems/comparison-of-xfs-and-ext4_overview-of-available-file-systems).
6. Reinicia la máquina para acabar la instalación primaria.
7. Elige el teclado para trabajar con la máquina. Lo normal es que sea «es», para el teclado español.
8. Selecciona la zona horaria: «Europe/Madrid».
9. Ponle nombre a tu máquina (llámala cf-XXX, donde XXX son tus iniciales).
10. Deja el nombre de dominio local como está.
11. Introduce la contraseña del 'root' de la máquina.
12. Introduce la contraseña de 'admin', también dos veces. **'admin' es el usuario que gestionará el cortafuegos a través de su interfaz web.**
13. **Configuración de red:**
    1. Usaremos la configuración por defecto, *GREEN + RED (GREEN* identifica el interfaz de red interno, protegido por el cortafuegos; *RED* identifica el interfaz de red externo, considerado *inseguro).*
    2. Selecciona la «Asignación de controladores y tarjetas», para identificar qué adaptador de red es cada interfaz *(GREEN* y *RED).*
    3. Selecciona primero el adaptador *GREEN.*
    4. Selecciona la tarjeta de red conectada como «Red interna» (comprueba la dirección MAC en VirtualBox, yendo a la sección del adaptador de red correspondiente y pulsando en «Avanzadas»; en un servidor de verdad podrías seleccionar la opción de «Identificar» para saber cuál es el interfaz físico correspondiente).
    5. Selecciona el adaptador *RED* y elige el único adaptador de red que queda.
    6. Selecciona «Hecho» cuando ya estén los dos interfaces asignados.
    7. A continuación selecciona «Configuración de dirección».
    8. Comenzaremos con el interfaz ***RED***, con estos pasos:
        1. Direccionamiento «Estático»
        2. Dirección IP: 10.0.2.**X**, donde **X** es tu número de PC en el aula
        3. Máscara: 255.255.255.0
        4. Gateway: 10.0.2.1
        5. Selecciona «Ok» para acabar.
    9. Tras acabar, seguimos con el interfaz ***GREEN**:*
        1. Nos puede aparecer un aviso sobre una posible sesión iniciada que se perdería, pero no nos afecta: selecciona «Ok».
        2. Dirección IP: 10.**X**.0.1, donde **X** esm como antes, tu número de PC en el aula
        3. Máscara de red: 255.255.255.0
        4. Selecciona «Ok» para acabar.
    10. Con esto concluye la configuración de interfaces. Selecciona «Hecho» para terminar.
    11. Y con esto acabamos la configuración de red.  Selecciona «Hecho» para finalizar.
14. **Configuración del servidor DHCP.** Nos va a hacer falta para proporcionar una dirección IP a la máquina desde la que haremos la configuración del cortafuegos a través del navegador.
    1. Marca la opción «[*] Habilitado», y pulsa «Ok».
    2. Dirección inicial: 10.**X**.0.200, donde **X** es tu número de PC.
    3. Dirección final: 10.**X**.0.254, donde **X** es tu número de PC.
    4. El resto de entradas las dejamos como están. Acaba pulsando en «Ok».
15. El sistema nos informará de que la instalación está completa. Selecciona «Ok».

Deja que la máquina se reinicie, hasta que te salga el indicador de *login* (le llevará algún tiempo).
A partir de aquí, la gestión del cortafuegos se hace usando el interfaz web. Es **importante** comprender que **sólo podemos conectarnos al interfaz web de gestión a través del interfaz *GREEN*, que es el único considerado seguro.**  
Por tanto, para gestionar el cortafuegos, **necesitaremos una máquina con interfaz gráfico conectada a la red interna**. En el siguiente apartado cubriremos esta tarea.

La dirección web para gestionar el cortafuegos es: `https://DIR_IP_GREEN:444`, donde ```DIR_IP_GREEN``` es la dirección IP del interfaz *GREEN* (en nuestra práctica, `https://10.X.0.1:444`, donde **X** es tu número de PC).

#### Acceso a la web de configuración del cortafuegos desde la otra máquina

Crea en VirtualBox una nueva máquina. Va a ser una máquina **muy ligera**, porque sólo necesitamos usar el navegador.

Características requeridas:
- Nombre: Configureitor (o lo que tú prefieras)
- Memoria RAM: 1024 MB 
- CPU: 1 (sólo una)
- Disco duro: VDI, 6 GB, tamaño fijo
- Sistema: Debian, última versión
- Configuración de red: un único interfaz, **conectado a red interna**

[Instálalo](https://raw.githubusercontent.com/juliogaray/comunes/main/Tutorial%20de%20instalaci%C3%B3n%20Servidor%20Debian.md) como una máquina normal Debian, **pero**:

Cuando te dé la opción de elegir qué paquetes instalar, marca estos:

![Paquetes que se deben instalar: (1) Entorno de escritorio, (2) LXDE y (3) Utilidades estándar del sistema](http://julio.iespacomolla.es/SMR2.SEGI/img/Seleccionar_paquetes_LXDE.png)

> **NOTA:** [LXDE](https://www.lxde.org/) es uno de los sistemas de escritorio gráfico más ligeros que existen. Por eso nos podemos permitir crear una máquina con tan solo 1 GB de memoria.

Cuando hayas acabado de instalarlo, reinicia la máquina, y sigue estos pasos:

1. Entra en tu cuenta de usuario (la que hayas creado durante la instalación).
2. Si lo deseas, para que la máquina sea más ligera y rápida, desactiva paquetes y servicios innecesarios. Desde la consola, ejecuta los siguientes comandos:

   ```bash
   sudo su
   systemctl disable --now cups.service
   systemctl disable --now avahi-daemon.service
   systemctl disable --now bluetooth.service
   ```
   Cierra la consola.
3. Inicia el navegador Firefox.
4. Introduce en la barra de direcciones de Firefox la dirección `https://10.X.0.1:444`, donde **X** es tu número de PC).
5. Te debería aparecer un aviso de riesgo de seguridad, porque la conexión HTTPS está usando un certificado autofirmado por IPFire. **Esto es normal.** Haz clic en **"Advanced…"** y, seguidamente, en **"Accept the Risk and Continue".**
6. A continuación te aparecerá un mensaje solicitando que te identifiques. Introduce ***admin*** como usuario ("User Name") y la contraseña que especificaste en el paso 12 de la instalación ("Password").
7. Haz clic en "Save" para permitir que el navegador recuerde estos datos (si más adelante necesitas cambiar o corregir algo, será más cómodo).

En este punto deberías estár viendo la página web de configuración del cortafuegos, parecida a esta (aunque con tus propias direcciones IP y demás información definida por ti):
![Página web de configuración de IPFire](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/IPFire_2.21_-_Web_interface.png/800px-IPFire_2.21_-_Web_interface.png)

#### Configuración del cortafuegos
Vamos a definir las reglas básicas para permitir el acceso a nuestro servidor web, y para permitir que desde éste se pueda acceder al servidor de caché de paquetes del instituto, para que la instalación sea rápida y para permitir las actualizaciones.
1. En el menú superior, ve a «Cortafuegos» y selecciona **«Reglas del Cortafuegos».**
2. Define las siguientes reglas (usando el botón `Nueva regla`):
    1. Para el **acceso de nuestro servidor interno al servidor de caché de paquetes:**
        - Dirección de origen: 10.X.0.10 (es la dirección que daremos al servidor. Ya sabes lo que es la *X**)
        - NAT: NAT de origen (SNAT); Nueva dirección IP de origen: RED *(con su IP)*
        - Dirección de destino: 172.30.12.178
        - Protocolo: TCP; Puerto de origen: (vacío); Puerto de destino: 3142
        - El resto puede dejarse como está. Haz clic en `Agregar`.
    2. Para el acceso **del exterior a nuestro servidor web**, con HTTP. Haz clic en `Nueva regla` y usa esta configuración:
        - Origen: Redes estándar - RED
        - NAT: NAT de destino (DNAT);
        - Dirección de destino: 10.X.0.10 (ya sabes, la dirección que daremos al servidor. Recuerda lo  que es la **X**).
        - Protocolo: -Preestablecido-; Servicios: HTTP
        - El resto puede dejarse como está. Haz clic en `Agregar`.
3. Para agregar HTTPS tenemos dos opciones: añadir otra regla como la anterior, pero con «Servicios: HTTPS» o, más elegante, definir un grupo de protocolos que englobe HTTP y HTTPS. Esto se haría así:
    1. Menú «Cortafuegos» → «Grupos Cortafuegos».
    2. Clic en `Grupos de servicios`.
    3. Agregamos un nuevo grupo. Nombre: Web; Remarcar: «Acceso a servicios web». Clic en `Agregar`.
    4. En la sección «Agregar» seleccionamos HTTP y hacemos clic en el botón `Agregar`.
    5. Repetimos el paso anterior con HTTPS.
    6. Para acabar, clic en `Volver`, para salir de aquí. El grupo de servicios ha sido definido.
    7. Vuelve a «Cortafuegos» → «Reglas del Cortafuegos».
    8. Edita la regla 2, que permite el acceso HTTP (haz clic en el icono con un lápiz amarillo de esa regla).
    9. En protocolo, selecciona «Grupos de servicios» y selecciona el grupo «Web» que hemos definido.
    10. Haz clic en `Actualizar`.
4. **IMPORTANTE:** cuando hayas definido todas las reglas a tu gusto, en la página de reglas (menú «Cortafuegos» → «Reglas del Cortafuegos») haz clic en `Aplicar los cambios` para que las reglas se activen.

A partir de aquí tenemos permitido el acceso al servidor desde la red del aula.
La configuración del cortafuegos ha concluido. Puedes apagar la máquina en la que has usado el navegador, para economizar RAM.

### El servidor web

![Ilustración de un web server](http://julio.iespacomolla.es/SMR2.SEGI/img/webserver_mini.jpeg)


Necesitamos una máquina sencilla, un servidor Debian sin entorno gráfico, con el servidor Apache instalado. 

Características requeridas:
- Versión: Linux
- Tipo: **Linux 2.6 / 3.x / 4.x (64-bit)**
- Memoria RAM: 1024 MB 
- Disco duro: VDI, 6 GB, tamaño fijo
- Configuración de red: un interfaz, configurado en modo **red interna** (será la conexión a la red protegida)

Llama a tu maquina `sw-XXX`, donde XXX son tus iniciales.
Sigue los pasos de instalación del tutorial de Debian, pero con la siguiente particularidad: cuando se te dé la opción de elegir los paquetes que se van a instalar, marca los siguientes (y nada más):

![Paquetes que se deben instalar: (1) web server, (2) SSH server y (3) Utilidades estándar del sistema](http://julio.iespacomolla.es/SMR2.SEGI/img/Seleccionar_paquetes_servidor_web.png)

Sigue cuidadosamente las [instrucciones de instalación de Debian](https://raw.githubusercontent.com/juliogaray/comunes/main/Tutorial%20de%20instalaci%C3%B3n%20Servidor%20Debian.md) que ya conoces.

Una vez instalado y reiniciado el sistema, lo actualizaremos e instalaremos el paquete resolvconf:
```shell
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install resolvconf
```

Por último, lo configuraremos con la dirección IP correcta: 10.X.0.10 **(Recuerda lo que es la X**). Edita el archivo /etc/network/interfaces, para aplicar una configuración estática al interfaz de red. El contenido quedará aproximadamente así:
```shell
...
auto enp0s3
iface enp0s3 inet static
    address 10.X.0.10
    netmask 255.255.255.0
    gateway 10.X.0.1
    dns-nameservers 10.239.3.7 10.239.3.8
```
**En este punto deberías darte cuenta de algo que no cuadra en todo esto. Algo que no debería haber funcionado, lo ha hecho. ¿De qué se trata? Habrá premio para el primero que lo explique en clase ;-)**

A continuación vamos a configurar el servidor de manera que atienda peticiones en el puerto 80 (HTTP) y en el puerto 443 (HTTPS). No hace falta que muestre ningún contenido concreto; de hecho, basta con que aparezca la página por defecto de Apache.
Para preparar el servidor para atender peticiones HTTP y HTTPS, instala los módulos requeridos (aunque por defecto ya deberían estar instalados) y actívalos:
```shell
sudo apt install openssl ssl-cert
a2enmod ssl
a2ensite default-ssl
systemctl reload apache2
```
Si no has recibido mensajes de error, reinicia la máquina para activar la nueva dirección IP (o, si lo prefieres, desactiva y reactiva el interfaz de red y recarga el servicio apache2).

## Redirección de puertos

Si ves el esquema de la práctica, verás que el cortafuegos está haciendo redirección de puertos *(port forwarding)* hacia el servidor web. Pero además, como el cortafuegos está en una red NAT, también necesitamos realizar redirección de puertos de nuestra red del aula hacia la dirección ip VERDE del cortafuegos.

Sigue estos pasos:

1. En VirtualBox, ve al menú 'Archivo'. Selecciona 'Herramientas' y, seguidamente, 'Administrador de red';
2. Selecciona la pestaña 'Redes NAT' y, a continuación, la red SEGI;
3. Selecciona la pestaña 'Reenvío de puertos';
4. En la sección «IPv4», añade una nueva regla con el icono '<span style="color:green;">**+**</span>'.
5. En la nueva regla, usa estos datos:
    - Nombre: «Acceso WEB»
    - Protocolo: ```TCP```
    - IP anfitrión: es mejor dejarlo vacío
    - Puerto anfitrión: escribe 4080.
    - IP invitado: la dirección IP del interfaz **RED** (ROJO) del cortafuegos, o sea, 10.0.2.X, donde X es tu n.º de PC en el aula
    - Puerto invitado: ```80```, el puerto estándar usado por el protocolo HTTP.
6. Haz clic en el botón «Aplicar».
7. Repite los pasos 4 a 6 con puerto anfitrión '4443', y puerto invitado '443'.

De este modo, el puerto 4080 de tu máquina anfitriona se redirige al puerto 80 del cortafuegos, y el puerto 4443 se redirige al puerto 443 del cortafuegos. El cortafuegos, a su vez, redirige estos puertos al servidor web.

No necesitas conectarte a ninguna de las dos máquinas (ni el cortafuegos ni el servidor web) a través de la línea de comandos. Deberías poder acceder a la página web de tu servidor web usando la dirección IP de tu ordenador del aula (o 'localhost' o '127.0.0.1'), con los puertos elegidos. Compruébalo abriendo tu navegador de la máquina anfitriona y poniendo:
- `http://127.0.0.1:4080` en la barra de dirección. Deberías poder ver la página por defecto de Apache.
- `https://127.0.0.1:4443` en la barra de dirección. Te dará un error de seguridad, porque el certificado es «inventado». Continúa adelante, haciendo caso omiso de la advertencia. Deberías poder ver de nuevo la página por defecto de Apache.

## Epílogo
Comentamos, después de configurar estáticamente el interfaz de red del servidor web, que algo «no cuadraba». Ese algo es que hemos hecho la instalación del servidor con una dirección IP dinámica, obtenida del cortafuegos por DHCP. Pero no tenemos definida ninguna regla en el cortafuegos que permita el acceso a actualizaciones (ni para instalar paquetes, lógicamente) para las direcciones del rango ofrecido por DHCP. Entonces, ¿cómo es posible que pudiéramos instalar el paquete *resolvconf,* si ninguna regla autorizaba ese tráfico?

La respuesta es que, por defecto, IPFire permite TODO el tráfico saliente desde la red segura (interfaz *GREEN)* hacia el exterior. Esto es muy cómodo, pero poco seguro. Lo ideal es definir reglas para todos los tipos de tráfico que deseamos (nada impide que alguna regla permita TODO el acceso hacia el exterior desde una parte de tu red, pero idealmente no desde TODA la red), y bloquear el resto del tráfico.

Si deseas comprobar si todo está bien, elimina el permiso por defecto para todo el tráfico de salida, y comprueba si aún puedes hacer actualizaciones del servidor web usando el servidor caché de paquetes:
1. Arranca de nuevo el servidor con el disco de instalación de Linux Mint, para poder usar el navegador.
2. Usa el navegador para acceder a la página de configuración del cortafuegos.
3. Vete al menú «Cortafuegos» → «Opciones del Cortafuegos».
4. Baja hasta el final de esa página, y en la sección «Comportamiento predeterminado del Cortafuegos» y cambia las dos opciones, «FORWARD» y «OUTGOING» a «Bloqueado».

Ahora, todo el tráfico saliente que no esté explícitamente autorizado por alguna regla, quedará bloqueado. Comprueba si puedes usar `apt update` y `apt upgrade` sin problemas. La primera regla que definimos debería permitírtelo.


&nbsp;

&nbsp;

&nbsp;

## Referencias

- [Página principal del proyecto IPFire](https://www.ipfire.org/)
- [Wiki de IPFire](https://wiki.ipfire.org/)

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2025 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)

### Imágenes usadas

Figs. 1, 3 y 6. Julio Garay. 2025. **Dominio público**.

Figs. 2 y 5. Generadas por IA. ChatGPT.

Fig. 4. Usuario [Rollopack](https://commons.wikimedia.org/wiki/User:Rollopack). *IPFire 2.21 - Web interface,* 2018. **CC-BY-SA**: https://commons.wikimedia.org/wiki/File:IPFire_2.21_-_Web_interface.png. Wikimedia Commons.

