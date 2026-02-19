![Generalitat Valenciana - CEUO / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEUO_IESPPM_NOFSE_Transparente.svg)

# Monitorización de servidores con SNMP

## Objetivos
1. Aprender a configurar SNMP en sistemas operativos profesionales (GNU/Linux) y en otros inferiores (…).
2. Instalar y configurar herramientas de monitorización para observar el comportamiento de estos dispositivos virtuales.

![Esquema de la práctica](http://julio.iespacomolla.es/SMR2.SEGI/imágenes/SEGI-P6.3-esquema.png)

### Entrega
Deberás:
- Documentar el proceso seguido, **incluyendo capturas de pantalla.**
- **Explica en cada paso lo que estás haciendo y por qué es necesario o para qué sirve.**

Elabora y entrega un único archivo PDF con todo lo especificado, incluidas las preguntas y sus respuestas correspondientes.


## Recursos necesarios
1. **VirtualBox**: Para crear y gestionar máquinas virtuales.
2. **Sistemas Operativos**:
   - Una máquina virtual, servidor **Linux** (por ejemplo, Debian) con el software de servidor web instalado;
   - Una máquina virtual, **Windows** (por ejemplo, Windows 10 u 11);
3. **Software de monitorización SNMP: [Zabbix](https://www.zabbix.com/)** (aunque, a criterio tuyo, puedes usar alternativas como [Cacti](https://www.cacti.net/), [LibreNMS](https://www.librenms.org/), [Monitorix](https://www.monitorix.org/), etcétera);
   - Zabbix (o el software que prefieras de monitorización) irá montado sobre una tercera máquina virtual, con la dirección IP ```10.0.2.X```, donde X es tu n.º de PC en clase.

---

## Paso 1: instalación de las máquinas virtuales

Instala una máquina virtual con Debian, sin interfaz gráfico, pero con el software de servidor web (Apache). Ya sabes que dispones de [nuestro tutorial habitual](http://julio.iespacomolla.es/comunes/Tutorial%20de%20instalación%20Servidor%20Debian.md) para hacerlo.

Instala otra máquina virtual con Windows 10 u 11.

Por supuesto, puedes usar máquinas ya creadas (y usadas para otras prácticas).


## Paso 2: configuración de SNMP en máquinas virtuales
- **En el servidor web (Debian)**:
  1. Instala el servidor SNMP:
     ```bash
     sudo apt update
     sudo apt install snmpd snmp
     ```
  2. Configura el archivo `/etc/snmp/snmpd.conf`:
     - Define una comunidad SNMP (por ejemplo, la que se suele usar por defecto, «public»).
     - Permite acceso desde la IP del host de monitorización (10.0.2.X, donde X es tu n.º de PC).
  3. Reinicia el servicio SNMP:
     ```bash
     sudo systemctl restart snmpd
     ```
  4. Verifica que el servicio está activo:
     ```bash
     sudo systemctl status snmpd
     ```

- **En Windows**:
  1. Instala el servicio SNMP:
     - Ve a «Panel de Control» &rarr; «Programas» &rarr; «Activar o desactivar características de Windows».
     - Marca «Servicio SNMP» y «Cliente SNMP».
  2. Configura SNMP:
     - Ve a «Herramientas Administrativas» &rarr; «Servicios».
     - Busca «Servicio SNMP», haz clic derecho y selecciona «Propiedades».
     - En la pestaña «Seguridad», agrega la comunidad «public» (o la que uses) y permite acceso desde la IP del servidor de monitorización.
  3. Reinicia el servicio SNMP.

## Paso 3: instalación del agente Zabbix en las máquinas virtuales

El **agente Zabbix** es un software que, basado en el servicio SNMP, amplía notablemente las posibilidades de monitorización de los dispositivos en que se instala.

Zabbix permite monitorizar cualquier dispositivo que use SNMP, pero si se puede instalar el software agente, su capacidad se amplía enormemente.

### 3.1: Instalación del agente Zabbix en servidor Debian

1. Actualiza el sistema con el comando `apt update && sudo apt upgrade -y`.
2. Instala el paquete correspondiente con el comando `sudo apt install zabbix-agent`.
3. Configura el agente Zabbix: edita el archivo `/etc/zabbix/zabbix_agentd.conf` y busca la línea que dice **`Server=`** para poner la IP o el nombre de host de tu servidor Zabbix. Esto le indica al agente Zabbix en qué servidor buscará el servidor de monitorización. En nuestro caso deberá quedar así (recuerda que `X` es tu número de PC):

   ```ini
   Server=10.0.2.X
   ```
4. Si tienes un nombre poco específico para tu servidor (como por ejemplo «Debian»), edita la línea que dice `Hostname=` y pon un nombre más significativo después del símbolo `=`.
5. Guarda el archivo y cierra el editor.
6. Activa e inicia el servicio con el comando `systemctl enable zabbix-agent --now`.
7. Asegúrate de que está funcionando correctamente con el comando `systemctl status zabbix-agent`.

### 3.2: Instalación del agente Zabbix en Windows

1. Obtén el agente Zabbix:
   1. Ve a la [página de descargas de agente Zabbix](https://www.zabbix.com/download_agents)
   2. En la sección *"Download pre-compiled Zabbix agent binaries"*, asegúrate de que esté seleccionada la versión para Windows
   3. Haz clic en el botón verde gordo **DOWNLOAD**. No tiene pérdida.
   4. Puedes descargar el agente original o el agente 2. Las [diferencias entre ambas versiones](https://www.zabbix.com/documentation/current/en/manual/appendix/agent_comparison) no nos importan mucho, así que **escoge la versión original, y NO el 'Agent 2'.**

2. Una vez descargado el archivo `.msi`, instálalo de la forma habitual.

3. Durante la instalación deberías tener la opción de configurar la IP o el nombre de host de tu servidor Zabbix, igual que en el servidor Linux (recuerda nuevamente que `X` es tu número de PC):

   ```ini
   Server=10.0.2.X
   ```

## Paso 4: instalación y configuración del software de monitorización (Zabbix)

Realiza la instalación incial del servidor Zabbix siguiendo [este tutorial](http://julio.iespacomolla.es/SMR2.SEGI/SEGI6.4P%20-%20Zabbix%20I.md).

Durante esta faase de instalación, realiza únicamente las siguientes capturas:
- Página por defecto de Apache2 (desde el navegador de tu máquina anfitriona);
- Página de información de PHP (ídem);
- Página inicial de configuración de Zabbix (ídem).

Guarda las capturas para entregarlas como parte del resultado de esta práctica.

## Paso 5: Configura el servidor Zabbix para monitorizar tus dos servidores

Usa [este tutorial](http://julio.iespacomolla.es/SMR2.SEGI/SEGI6.4P%20-%20Zabbix%20II.md) para configurar Zabbix desde el interfaz web, y agregar los dos servidores a tu servidor Zabbix.

> NOTA: el tutorial está adaptado de otra práctica, y es posible que no todos los pasos estén completamente claros, o que falten algunos. Si es así, avisa a tu profesor(a).

## Paso 6: monitorización de las máquinas virtuales

Utiliza Zabbix para:
  - Visualizar información básica de las máquinas virtuales (nombre, descripción, etc.).
  - Monitorizar métricas como el uso del procesador (CPU), memoria, tráfico de red, etc.
  - Generar gráficos y alertas basadas en umbrales (por ejemplo, alertar si el uso de CPU supera el 80%).
  - Definir alertas en caso de que el disco duro se llene por encima del 85 %.
  - Definir alguna otra alerta que te parezca relevante.

&nbsp;

&nbsp;

&nbsp;


## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es) 2025 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)



