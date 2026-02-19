![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEICE_IESPPM_Transparente.svg)

# Autentificación en 2 pasos en GNU/Linux, con *token*


En esta práctica vamos a configurar nuestra máquina virtual para que **sólo nos permita entrar en nuestra cuenta si cumplimos estas dos condiciones**:

1. Introducir nuestro **usuario y contraseña** (como se hace habitualmente)
2. Tenemos **conectado un pendrive dado** a la máquina.

Deben cumplirse **ambas** condiciones: si no está el pendrive conectado, no nos debe dejar acceder a nuestra cuenta. Si no introducimos bien nuestro usuario y contraseña, tampoco.

De esta manera, combinamos *algo-que-sabes* con *algo-que-tienes* para aumentar la seguridad en el acceso a la máquina.

> **NOTA:** este tutorial está desarrollado pensando que ya tienes el pendrive en tu poder. Si no es así, puedes seguir los pasos de instalación del software y, más adelante, usar el comando ```pamusb-conf``` para configurar el pendrive para tu usuario.

## Desarrollo de la práctica

Sigue los siguientes pasos para desarrollar esta práctica:

1. Abre el programa de virtualización (VirtualBox). Crea una máquina virtual según el tutorial facilitado por el profesor, usando la distribución que te indique el mismo.
2. En la sección USB de VirtualBox correspondiente a tu máquina virtual activa el servicio USB 3.0.
3. Inserta en el ordenador el pendrive que vas a usar como token para la autentificación.
4. En la misma sección de USB de VirtualBox, añade un filtro para que la máquina virtual tenga acceso al pendrive (si no puedes añadir el filtro, probablemente tu cuenta en el ordenador necesita ser añadida al grupo especial vboxusers con el comando ```sudo usermod -aG vboxusers NOMBRE_USUARIO```, pero **tú no podrás hacerlo. Pide al profesor que lo haga por ti**).
5. Inicia la máquina virtual y entra en tu cuenta con tu nombre de usuario y contraseña.
6. Comprueba que tu máquina virtual tiene acceso al pendrive: ejecuta el comando ```ls /dev``` para ver los dispositivos disponibles. Tendrás tu unidad de disco básica, ***sda**,* con sus particiones (***sda1**,* ***sda2**,* etétera). Tu pendrive estará identificado como la última de las unidades **sd\*** (probablemente ***sdb**)* y seguramente podrás ver también las particiones que contenga, como ***sdb1**,* etc.
7. Para poder hacer esta práctica, necesitamos instalar en la máquina un paquete de software especial: **libpam-usb**. Durante varios años el paquete dejó de ser mantenido por su creador, y acabó desapareciendo de los repositorios estándar de Debian. Pero un técnico lo ha retomado para volver a hacerlo activo y, aunque no está aún en los repositorios oficiales de Debian, al menos ya se puede instalar usando su repositorio personal. El primer paso es configurar tu sistema para aceptar la clave de cifrado de su repositorio:

```bash
wget -qO- "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x913558C8A5E552A7" | gpg --dearmor | sudo tee /usr/share/keyrings/apt.mcdope.org.gpg > /dev/null
```

>**🔴 NOTA:** si hay problemas de red y el comando anterior no funciona correctamente, puedes descargarte el archivo de [este enlace](http://julio.iespacomolla.es/SMR2.SEGI/apt.mcdope.org.gpg) y copiarlo en el directorio ```/usr/share/keyrings/``` de tu máquina virtual. Puedes hacerlo con estos comandos (como _root):_

```bash
cd /usr/share/keyrings
wget "https://julio.iespacomolla.es/SMR2.SEGI/apt.mcdope.org.gpg"
```

8. A continuación añadimos su repositorio personal a nuestro archivo ```/etc/apt/sources.list```.
Edita dicho archivo con el comando ```sudo nano /etc/apt/sources.list``` y añade al final la siguiente línea:
```bash
deb [signed-by=/usr/share/keyrings/apt.mcdope.org.gpg] https://apt.mcdope.org/ ./
```

9. Guarda el archivo y cierra el editor.

10. Ahora puedes actualizar la caché de paquetes e instalar el paquete como lo haríamos normalmente:
```bash
sudo apt update
sudo apt install libpam-usb
```

11. Añade tu _pendrive_ como dispositivo para autentificar (lo último de la línea es un nombre que eliges tú; yo uso «llave-de-julio» en este ejemplo):
```bash
sudo pamusb-conf --add-device llave-de-julio
```

Si tienes más de un dispositivo disponible (por ejemplo, para que más de un usuario puedan usar este sistema, con diferentes _pendrives),_ se te mostrarán, para que selecciones el que quieras usar.

12. Añade todos los usuarios que requerirán usar un _pendrive_ para entrar al sistema (en este caso, al menos tu propio nombre de usuario):
```bash
sudo pamusb-conf --add-user jgg
```

En este caso, «jgg» es mi nombre de usuario. Tú tendrás que poner el tuyo, lógicamente.  
El programa te preguntará en este punto qué token quieres usar (en esta práctica sólo debería aparecer uno, porque sólo hemos definido uno en el paso 11).

13. Si todo está bien, en este momento el sistema te autentificará con contraseña **O** con el pendrive. Es decir, si el pendrive está conectado, ni siquiera te pedirá la contraseña. Vamos a cambiarlo para que nos pida AMBAS cosas. Edita el archivo ```/etc/pam.d/common-auth```. Este archivo contendrá una línea como esta:
```
auth    sufficient  pam_usb.so
```

Debes cambiarla a:
```
auth    required    pam_usb.so
```

14. Ahora, tanto para conectarte a tu cuenta como para ejecutar cualquier comando con *sudo,* deberás tener el pendrive conectado e introducir tu contraseña. ¡Compruébalo!  
Ten en cuenta que si has hecho ```sudo su``` correctamente, puedes sacar el pendrive y volver a hacerlo sin problemas, porque sudo recuerda durante un tiempo que te has autentificado correctamente, y no hace ninguna comprobación.


## Ampliación optativa

- Una vez que has configurado todo correctamente, no podrás conectarte a la máquina por SSH fácilmente, ya que la autentificación remota no funcionará por no disponer del pendrive. Investiga cómo resolver esto.
- Investiga cómo hacer que el sistema realice alguna acción (por ejemplo, cerrar tu sesión) al retirar el pendrive de la máquina.

## Preguntas

1. ¿Cómo podrías hacer algo similar para una máquina Windows?
2. ¿Puedes jugar con el PAM y hacer combinaciones más complejas? ¿Por ejemplo, que pida el pendrive para hacer sudo pero no para hacer login?

&nbsp;

&nbsp;

&nbsp;


## Referencias



## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2024 Julio Garay, [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)


