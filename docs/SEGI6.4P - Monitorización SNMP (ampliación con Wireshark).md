
------------------------------------------------------------------------------------------
AGREGAR Y COMPLETAR
------------------------------------------------------------------------------------------

## Recursos necesarios
4. Optativamente **Wireshark**: Para capturar y analizar tráfico SNMP.


## Paso 7 (optativo): captura y análisis de tráfico SNMP
ESTA PARTE NO ESTÁ DESARROLLADA
Para realizar este paso necesitaríamos una máquina adicional con [Wireshark](https://es.wikipedia.org/wiki/Wireshark), configurando los interfaces de red de forma que esta máquina tenga acceso a todo el tráfico SNMP que le llega al servidor Zabbix.

- Utilizar Wireshark para capturar el tráfico SNMP entre las máquinas virtuales y el software de monitoreo.
- Analizar los paquetes SNMP para identificar:
  - Las solicitudes (GET) y respuestas (RESPONSE) SNMP.
  - La estructura de los objetos OID (Object Identifier).
  - La comunidad SNMP utilizada en la comunicación.


