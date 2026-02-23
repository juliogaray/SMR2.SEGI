![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)

# Instalación y uso de *Zabbix* II

Recuerda que **Zabbix** es una herramienta de monitorización de [código abierto](https://es.wikipedia.org/wiki/C%C3%B3digo_abierto) que permite obtener información continua sobre el rendimiento de muy variados servicios de red, servidores y dispositivos de todo tipo. Zabbix puede recolectar una gran cantidad de datos de los tipos más diversos y mostrar gráficas que den una idea inmediata del estado de los dispositivos conectados a la red y de su evolución.

En la [primera parte](https://juliogaray.github.io/SMR2.SEGI/SEGI6.4P%20-%20Zabbix%20II.md) de este tutorial creamos el servidor, instalamos Zabbix (así como todo el software adicional que requiere) y ejecutamos todo el proceso de instalación desde la línea de comandos.

En esta segunda parte vamos a configurar **Zabbix** desde el interfaz web.

## Configuración inicial

Sigue estos pasos para tener tu Zabbix listo para monitorizar lo que desees:

1. Accede a la página web de tu Zabbix (```http://D.I.M.A:PuertoAsociado/zabbix/```, donde *D.I.M.A* es la dirección IP de tu máquina anfitriona, y *PuertoAsociado* es el puerto que asociaste en la Red NAT SEGI al puerto 80 de tu servidor Zabbix). Verás la página de inicio de configuración de Zabbix:

![Página de bienvenida y selección de idioma de Zabbix](https://kifarunix.com/wp-content/uploads/2021/09/zabbix-welcome-page-1068x674.png?v=1630820413)

2. Selecciona el idioma que desees (si no puedes, consulta en la primera parte de este tutorial cómo activar otros idiomas). En este tutorial asumiremos que has elegido «español». Haz clic en «Siguiente paso».

3. Ahora verás la pantalla de «Comprobación de requisitos previos». Si has seguido los pasos de la primera parte correctamente, todos los requisitos deberían estar marcados como «<span style="color:green">OK</span>», indicando que están cumplidos. Si no es así, deberás corregir los requisitos que den problemas desde la línea de comandos del servidor antes de poder continuar. Haz clic en «Siguiente paso».

4. Ahora vamos a proceder a configurar la conexión del servidor Zabbix a la base de datos que creamos en la primera parte. Recuerda que, durante el proceso de *Creación de una base de datos para Zabbix*, creamos una BD (a la que en el tutorial dimos el nombre «bdzabbix»), un usuario (que llamamos «usrzabbix») y una contraseña (en nuestro ejemplo, «psswrdzabbix»). Ahora deberás introducir estos datos en esta pantalla (no hace falta que cambies ninguna otra cosa). Después, haz clic en «Siguiente paso». Si te da algún error, deberás corregirlo nuevamente en la línea de comandos.

5. El siguiente diálogo es el de «Ajustes». Ponle a tu servidor el nombre que prefieras, asegúrate de que la zona horaria es «Europe/Madrid». Cambia el tema si no puedes resistir la tentación. Y haz clic en «Siguiente paso».

6. En el siguiente paso veremos el diálogo de «Resumen de preinstalación». Si no ves nada raro, haz clic en «Siguiente paso».

7. Si no han surgido problemas, llegarás a la página «Instalar», en la que deberías ver el texto «<span style="color:green">¡Felicidades! Has instalado la interfaz web de Zabbix satisfactoriamente.</span>». Haz clic en «Finalizar».

## Inicio de sesión

Ahora deberías ver el diálogo de autentificación, pidiéndote un nombre de usuario y una contraseña para acceder a tu Zabbix. El usuario inicial es «**Admin**», y la contraseña es «**zabbix**». Inicia la sesión. Verás la pantalla de «Vista Global» (o *«Global View»)*, algo parecido a esto:

![Global View de Zabbix](https://pplware.sapo.pt/wp-content/uploads/2021/02/zabbix_01-1-1024x496.jpg)

## Usuarios

El primer paso (debería ser obvio) será cambiar la contraseña del administrador: ¡es importante que la información que ofrece Zabbix no caiga en las manos equivocadas!

1. En el menú de la izquierda, selecciona «Administración» &rarr; «Usuarios». Podrás ver que Zabbix admite una gran cantidad de usuarios, todos ellos con diferentes roles (de forma que puedan acceder a cierta información pero no a otra, y tengan diferentes permisos para hacer cosas).

2. Haz clic en «<span style="color:#0275b8">Admin</span>» para acceder a la página de configuración del usuario Admin.

3. Haz clic en «Cambiar la contraseña» y, a continuación, introduce (dos veces, como es habitual) la nueva contraseña (que sea segura, por favor). **No hagas clic aún** en «Actualizar».

4. Selecciona la pestaña «Medio», en la parte de arriba de la página.

5. Haz clic en «<span style="color:#0275b8">Añadir</span>».

Aquí podrás seleccionar cómo puede contactar Zabbix contigo (el administrador) cuando surjan emergencias. Como mínimo selecciona el correo electrónico (aunque, como puedes ver, Zabbix ofrece una plétora de medios para enviar notificaciones).

6. En «Tipo», selecciona «Email».

7. En «Enviar a», escribe tu dirección de correo electrónico.

8. En «Cuándo está activo» puedes indicar a qué horas deseas recibir notificaciones por este medio (por defecto, los 7 días de la semana, las 24 horas). Pero si configuras otros medios, puedes decidir que no te molesten a ciertas horas.

9. En «Utilizar la gravedad» puedes filtrar qué tipos de notificaciones se te enviarán. Déjalo como está.

Es razonable que elijas diferentes combinaciones de medios, horarios y gravedades. Por ejemplo, puedes crear un medio que te envíe un email con cualquier notificación a cualquier hora, y otro que te envíe sólo notificaciones «Promedio», «Altas» y «Críticas» a cualquier hora al móvil. En una empresa grande, con varios administradores, os podríais repartir los días y las horas de notificaciones. Las combinaciones son casi infinitas.

10. Haz clic en «Añadir» y, a continuación, en «Actualizar».

## Configuración de equipos

Ha llegado el momento de añadir a Zabbix equipos que queremos monitorizar.

1. Haz clic en «Configuración» &rarr; «Equipos».

Inicialmente verás un equipo configurado: el propio servidor Zabbix en el que, como recordarás, instalamos el agente de Zabbix en la primera parte del tutorial. Puedes instalar agentes Zabbix en multitud de equipos, y todos ellos ofrecerán información detallada al servidor.

2. Marca la casilla a la izquierda del servidor (para seleccionarlo) y haz clic en «<span style="color:#0275b8">Activar</span>». Confima tu elección cuando se te pregunte.

Verás que el «Estado» ha cambiado a «Activado», y ahora el servidor está siendo monitorizado. En este momento se empiezan a recoger datos, y dentro de unos minutos podrás verlos.

Ahora vamos a añadir los servidores que tenemos en la red (el servidor Web Debian, y el otro equipo con Windows).

3. Haz clic en el botón «Crear equipo».

4. En el nuevo diálogo, Asigna un nombre significativo al equipo (como «Servidor Web» o «Servidor Windows»).

5. En «Plantillas», selecciona la más adecuada para cada sistema.

6. En «Grupos», selecciona el que mejor se adapte.

8. Configura la dirección IP de los dispositivos que vas a monitorizar. Para el servidor Debian, `10.0.2.100`. Para el equipo con Windows, `10.0.2.250`.

9. Puedes añadir una descripción si lo deseas. En nuestro caso no es necesario.

11. En la parte de abajo del diálogo, haz clic en «<span style="background-color:#0275b8;color:white">Añadir</span>» (no te confundas con el enlace de «<span style="color:#0275b8">Añadir</span>» que está justo encima de «Descripción», que sirve para añadir otro interfaz de monitorización del dispositivo).

Ahora deberías tener dos equipos configurados y activados. En «Disponibilidad» podrás ver cómo accede Zabbix a la información de cada uno de ellos («ZBX» para todos los que tienen instalado el agente Zabbix; sería «SNMP» para dispositivos que no dispusiesen del agente, y sólo fuesen accesibles por SNMP). Si monitorizas algún dispositivo con SNMP, ten en cuenta que puede tardar unos minutos en poner «SNMP» en verde, puesto que SNMP funciona a base de consultas periódicas, y no se pondrá en verde hasta la siguiente consulta.

## Monitorización

Esta es la principal utilidad de Zabbix. Aparte de darte un tablero con un resumen de la información más importante en cada momento, te permite observar es estatus de tus dispositivos en tablas de datos y en gráficos.


1. Acude, en el menú de la izquierda, a «Monitorización» &rarr; «Equipos».

2. Selecciona los equipos que has configurado, haciendo clic en el enlace «Últimos datos» correspondiente.

4. Comprueba qué datos se han obtenido, y cuáles pueden ser de interés (esto depende del dispositivo).

5. Vuelve a «Monitorización» &rarr; «Equipos».

6. Selecciona ahora, de los servidores, el enlace «Gráficos». Echa una ojeada a la información que proporcionan.

Haz algunas capturas de pantalla de los «Últimos datos» y de los «Gráficos» para entregar en la práctica.

&nbsp;

&nbsp;

&nbsp;


## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es) 2025 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)


