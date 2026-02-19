![Generalitat Valenciana - CEUO / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEUO_IESPPM_NOFSE_Transparente.svg)

# Centros de Proceso de Datos (CPD)

## 1. Definición

Un **Centro de Proceso de Datos (CPD)** —también llamado Centro de Cálculo o *Data Center*— es una instalación física donde se concentran los **servidores, sistemas de almacenamiento, redes y equipos de telecomunicaciones** que permiten el funcionamiento de los servicios informáticos de una organización.

Su objetivo es **garantizar la disponibilidad, seguridad y eficiencia** de los recursos tecnológicos que gestionan la información crítica de la empresa.

---

## 2. Ventajas de centralizar los servidores en un CPD

- **Seguridad física y lógica:** los equipos están protegidos frente a accesos no autorizados, incendios, humedad, etc.
- **Gestión centralizada:** facilita el mantenimiento, monitorización y administración de sistemas.
- **Optimización de recursos:** ahorro energético, de espacio y de costes operativos.
- **Alta disponibilidad:** sistemas redundantes que reducen el riesgo de interrupciones.
- **Escalabilidad:** es más fácil añadir nuevos equipos o servicios.
- **Cumplimiento normativo:** permite aplicar políticas de protección de datos y de seguridad.

---

## 3. Ubicación del CPD

La localización física del CPD es un factor clave, porque determina en gran parte su seguridad y las posibilidades de acondicionamiento técnico.

Para su protección, optaremos por localizarlo ...

### Lejos de:

- Ríos, playas, presas, y otras zonas **inundables** o con riesgo de catástrofes naturales.
- **Fábricas, aeropuertos o carreteras** con alta vibración o contaminación y posibilidades de accidentes.

### De forma que:

- **Tenga dos accesos por caminos o calles diferentes**, para asegurarnos el acceso en caso de que se bloquee uno por cualquier razón.
- Tenga **fácil acceso de cableado** desde el punto de entrada de las líneas de comunicaciones y energía.
- Disponga de **salidas de emergencia**, cumpliendo la normativa de seguridad.

### Planta del edificio:

- Idealmente en **una de las primeras plantas, pero no la planta baja**
   - Nunca en el sótano (riesgo de inundación)
   - Ni en la planta baja (más expuesta a daños como impacto de vehículos o acceso por asaltantes).
   - Ni en plantas altas (más expuestas a accidentes aéreos, y las de mayor riesgo en caso de incendio)


---

## 4. Características del CPD

- **Control ambiental:** temperatura y humedad controladas (± 20–25 °C, 40–60 % [HR](https://conceptos.es/humedad-relativasuelo)).
- **[Suelo técnico](https://es.wikipedia.org/wiki/Suelo_t%C3%A9cnico)** para canalización de cables y ventilación inferior.
- **Techo técnico o bandejas** para cableado estructurado y electricidad.
- **Paredes ignífugas y con aislamiento acústico.**
- **Puertas de seguridad** con control de acceso electrónico.
- **Sin ventanas**, que suponen un riesgo de intrusiones de personas no autorizadas, pero también de lluvia y otros riesgos climáticos (además, hoy en día tampoco hay que descartar accidentes o ataques con drones).
- **Iluminación de emergencia y señalización clara.**

---

## 5. Estructura básica

Un CPD suele dividirse en áreas:

| Zona                                 | Función                                                   |
| ------------------------------------ | --------------------------------------------------------- |
| **Sala de servidores**               | Aloja *[racks](https://es.wikipedia.org/wiki/Bastidor_de_19_pulgadas),* servidores y equipos de red.               |
| **Sala técnica**                     | Espacio para [SAI](http://julio.iespacomolla.es/SMR2.SEGI/SEGIT2%20-%20SAI.md), cuadros eléctricos, climatización, etc. |
| **Área de control o monitorización** | Desde donde los técnicos supervisan los sistemas.         |
| **Almacén técnico**                  | Guarda repuestos, herramientas y materiales.              |
| **Centro de respaldo o backup**      | Instalación duplicada para emergencias.                   |

---

## 6. Medidas de protección

### A. Protección para los equipos

- **Sistemas de alimentación ininterrumpida (SAI)** y **[grupos electrógenos](https://es.wikipedia.org/wiki/Grupo_electr%C3%B3geno)**.
- **Control de temperatura y humedad**: climatización, a ser posible con equipos redundantes, de forma que siempre haya uno de reserva.
- **Detección y extinción automática de incendios** ([por gas inerte](https://es.wikipedia.org/wiki/INERGEN), no agua).
- **Sistemas antiincendios en bandejas de cables.**
- **Protección frente al polvo**: el polvo tiende a acumularse en los radiadores y ventiladores, dificultando la refrigeración de los equipos, y también puede introducirse en las ranuras de conexión de componentes, provocando fallos frecuentes. Es importante que el aire del CPD esté convenientemente filtrado.
- **Interferencias electromagnéticas**: el CPD debe estar alejado de (o protegido frente a) equipos que generen estas interferencias (p. ej. determinados aparatros industriales o médicos, generadores eléctricos, etc.), bien de la propia organización, bien de alguna empresa vecina.
- **Monitorización 24/7** (temperatura, humedad, energía, intrusión).
- **Redundancia en red eléctrica y comunicaciones.**

### B. Protección para los usuarios

- **Suelos y equipos con toma de tierra.**
- **Protección contra descargas eléctricas.**
- **Alumbrado de emergencia.**
- **Normas de acceso seguro y [EPIs](https://es.wikipedia.org/wiki/Equipo_de_protecci%C3%B3n_individual) (por ejemplo, zapatos antiestáticos).**
- **Plan de evacuación y señalización de salidas.**
- **Contra el ruido.** Aislamiento acústico para los usuarios que están en el exterior del CPD, ya que las máquinas del mismo suelen ser bastante ruidosas.

---

## 7. Esquema de ventilación y climatización

- El aire frío se impulsa **por el suelo técnico** hacia la parte frontal de los *racks* (pasillo frío).
- El aire caliente se extrae **por la parte trasera y superior** de los mismos (pasillo caliente).
- Así se genera una **disposición en pasillos fríos y pasillos calientes** para mejorar la eficiencia.
- Control automático de temperatura y humedad mediante sensores.
- Filtros antipolvo y mantenimiento periódico.

---

## 8. Suministro eléctrico y de comunicaciones

### Suministro eléctrico:

- **Líneas independientes y redundantes.**
- **SAI** para evitar cortes breves.
- **Grupo electrógeno** para cortes prolongados.
- **Cuadros eléctricos protegidos y señalizados.**
- **Tomas de tierra y protección contra sobretensiones.**

### Comunicaciones:

- **Conexión redundante a Internet y proveedores distintos.**
- **Cableado de red estructurado.**
- **Redes internas separadas (producción, gestión, almacenamiento, etc.).**
- **Monitorización y gestión del tráfico de red.**

---

## 9. Control de acceso

- **Acceso restringido** solo a personal autorizado.
- **Sistemas de autenticación:** tarjetas, PIN, biometría, etc.
- **Registro de entradas y salidas.**
- **Cámaras de videovigilancia.**
- **Zona de exclusión:** acceso controlado con cerradura o doble puerta.
- **Política de visitantes** (acompañamiento y registro obligatorio).

---

## 10. Centro de respaldo (Centro de contingencia)

Un **[centro de respaldo (CR)](https://es.wikipedia.org/wiki/Centro_de_respaldo)** es un segundo CPD que **duplica los sistemas** del principal para garantizar la continuidad del servicio ante una avería grave, incendio o desastre. Puede duplicar todos los sistemas (más costoso, pero ofrece mayor redundancia) o sólo aquellos considerados críticos (más económico, pero la capacidad de recuperación del servicio es más limitada).

### Tipos de centro de respaldo:

| Tipo          | Descripción                                               | Tiempo de recuperación |
| ------------- | --------------------------------------------------------- | ---------------------- |
| ***Hot site***  | Totalmente operativo y sincronizado en tiempo real.       | Minutos                |
| ***Warm site*** | Equipado parcialmente, necesita sincronización de datos.  | Horas                  |
| ***Cold site*** | Solo infraestructura física, sin datos ni equipos listos. | Días                   |

### Requisitos de un centro de respaldo:

- Estar **físicamente alejado** del CPD principal (para no verse afectado por el mismo riesgo).
- Tener **conectividad redundante y segura** con el CPD principal.
- Contar con **copias de seguridad actualizadas y probadas.**
- Permitir **arranque rápido** de los sistemas críticos.
- Disponer de **procedimientos documentados** de activación y retorno.

&nbsp;

&nbsp;

&nbsp;

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es) 2025 [J. Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollà](https://iespacomolla.es/), Alicante (España)

