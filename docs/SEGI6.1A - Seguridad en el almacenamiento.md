![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)
# Seguridad en el almacenamiento

Para cualquier organización, la parte más importante de la informática son los datos. El hardware, el personal, los sistemas y aplicaciones… Todo se puede remplazar fácilmente, excep-
to los datos, que no se pueden adquirir ni contratar fuera de la organización: si se pierden, no hay de dónde recuperarlos.

Para reforzar la integridad y disponibilidad de los datos, se pueden aplicar varias estrategias.

## 1. Almacenamiento redundante

Los mecanismos más comunes en la actualidad para proporcionar redundancia en el almacenamiento son los esquemas RAID.

RAID define varios niveles, pero los más usados son:

- RAID 0: no ofrece redundancia. Sólo agrupar varios discos en una unidad, repartiendo los datos entre ellos, aumentando la capacidad y el rendimiento.
- RAID 1: discos en espejo. Se usan dos discos, manteniendo copias idénticas de los datos en ambos. El 50% de la capacidad total se pierde en redundancia.
- RAID 5: Discos agrupados en bandas con paridad. Se pueden agrupar 3 o más discos iguales, y el espacio equivalente a uno de ellos se pierde en redundancia (por ejemplo, con 4 discos se pierde el 25% del espacio total; con 10 discos, se pierde el 10%).
- RAID 6: Similar al RAID 5, pero con doble paridad. Se pierde el equivalente a dos discos en redundancia, pero se aumenta la seguridad.
- RAID 10 (o RAID 1+0): Consiste en crear dos (o más) espejos RAID 1, y luego asociarlos en un RAID 0 de nivel superior. Ofrece la misma seguridad y redundancia que RAID 1, pero aumenta el rendimiento por el uso de RAID 0.

Puedes leer más información sobre los niveles RAID 5 y 6 en [este enlace](https://www.mercadoit.com/blog/analisis-opinion-it/niveles-de-almacenamiento-de-datos-raid-5-y-raid-6/).

## 2. Almacenamiento en red: NAS y SAN

Una opción muy interesante es el almacenamiento en red. Esto nos permite trabajar con espacio de almacenamiento instalado en el CPD, donde están especialmente protegidos, y donde
podemos usar técnicas de redundancia y balanceo de carga para hacer el servicio más eficiente.

Las dos formas básicas son NAS y SAN, pero no forman parte del temario de este módulo.

## 3. Copias de seguridad e img de respaldo

Una **copia de seguridad** o (en inglés, aunque el término también se usa habitualmente en castellano) ***back-up*** es una copia de los datos contenidos en un dispositivo de almacenamiento, efectuada con el objetivo de recuperarlos si el dispositivo de almacenamiento sufre alguna pérdida de datos.

Aunque el término se suele considerar asociado al almacenamiento masivo en ordenadores (discos duros), también tiene sentido en relación con otros dispositivos como una tarjeta SIM,
con el almacenamiento de bases de datos (que puede estar distribuido en varios discos duros, por ejemplo), etc.

En caso de necesidad, los datos contenidos en la copia de seguridad se pueden **restaurar** sobre el dispositivo que contenía los datos originalmente, o sobre uno que lo sustituya.
Dependiendo del tipo de incidente del que se quiera proteger los datos, los métodos de copia de seguridad pueden variar:

- El riesgo de que un usuario borre o dañe un archivo inintencionadamente se puede mitigar simplemente manteniendo una copia de los archivos importantes en otro directorio del mismo disco duro.
- Una avería del disco duro se puede mitigar manteniendo una copia de seguridad en otro disco de la misma máquina, o en otra máquina.
- Ante el riesgo de robo de un ordenador o de un dispositivo de almacenamiento, será necesario hacer una copia de seguridad en un medio externo.
- Para el caso de riesgo de grandes calamidades, como incendios, inundaciones, etcétera, habrá que hacer copias de seguridad en una localidad remota.

A la hora de definir una estrategia de copias de seguridad, debemos considerar los riesgos que enfrentamos. Además es necesario considerar que, **frecuentemente, los daños sufridos por archivos de datos no son advertidos inmediatamente**, por lo que **es probable que las copias de seguridad más recientes incluyan ya los datos dañados**, y será necesario acudir a copias más antiguas para recuperar (lo que se pueda de) los datos. Por ello es clave conservar varios *back-ups* realizados a lo largo del tiempo, para poder acudir a versiones anteriores de
archivos que queramos recuperar.

### 3.1. Métodos de copia de seguridad

Hay diversas maneras de abordar la realización de copias de seguridad. En primer lugar, podemos clasificarlos en dos métodos básicos:

1. Copia de seguridad del dispositivo completo, a menudo llamadas img, en referencia a que la copia sería una imagen perfecta del estado del dispositivo en el momento en que se hace;
2. Copias de seguridad a nivel de archivos.

&nbsp;

&nbsp;

&nbsp;

## Colofón
 
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es) 2023 [J. Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollà](https://iespacomolla.es/), Alicante (España)

### Imágenes usadas

Fig. 2. Ivo Baran. *Unshielded twisted pair cable with different twist rates,* 2007. **Dominio público**: https://commons.wikimedia.org/wiki/File:UTP_cable.jpg. Wikimedia Commons.

Figs. 3 a 10. Age Bosma. Diversas versiones de *twisted pair cable shielding,* 2021. **CC-BY-SA**: https://en.wikipedia.org/wiki/Twisted_pair#/media/File:U-UTP_twisted_pair_cable_shielding.svg. Wikimedia Commons.

Fig. 11. Usuario [Mike1024](https://commons.wikimedia.org/wiki/User:Mike1024). *Close-up photo of an uncrimped, transparent RJ-45 plug,* 2005. **Dominio público**: https://commons.wikimedia.org/wiki/File:Uncrimped_rj-45_connector_close-up.jpg. Wikimedia Commons.

Fig. 13. Ivo Baran. *network cable TP crimping tool RJ-45,* 2007. **Dominio público**: https://commons.wikimedia.org/wiki/File:Crimping_tool_TP2.jpg. Wikimedia Commons.

Fig. 14. Ivo Baran. *rj-45 pins colored,* 2007. **Dominio público**: https://commons.wikimedia.org/wiki/File:Rj-45_pins.jpg. Wikimedia Commons.

Figs. 15 y 16. Miha Ulanov. *Connection of ISDN cable to RJ45,* 2022. **Dominio público**: https://commons.wikimedia.org/wiki/File:RJ-45_ISDN_Right.png (variación)

Fig. 17. Julio Garay. 2023. Modificación de la figura 11 arriba mencionada. **Dominio público**.

Figs. 1, 12, 17 y 18. Julio Garay. 2023. **Dominio público**.

