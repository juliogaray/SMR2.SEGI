![Generalitat Valenciana - CEUO / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEUO_IESPPM_NOFSE_Transparente.svg)

# Sistemas de alimentación ininterrumpida

![Imagen de un SAI](https://upload.wikimedia.org/wikipedia/commons/b/b0/UPSAPC.jpg)

Un SAI (en ingles *UPS: Uninterruptible Power Supply)* es un dispositivo que cuenta con una batería o conjunto de baterías con el que se alimenta uno o varios sistemas.

En caso de fallo del suministro eléctrico, los equipos conectados al SAI siguen funcionando porque consigue electricidad de la(s) batería(s). La capacidad de suministro depende del SAI y del consumo de los equipos, pero es habitual que garanticen un suministro de al menos diez minutos.

Si es posible, conviene aplicar redundancia e instalar un doble juego de equipos SAI, para es­tar cubiertos en caso de que uno fallara. Esto es posible porque la mayoría de los servidores vienen con doble fuente de alimentación, lo que nos permite conectar una
fuente a cada grupo de SAI.

En caso de fallo de suministro eléctrico, el SAI:

1. Espera unos minutos por si el corte ha sido puntual y vuelve el suministro en poco tiempo.
2. Si el suministro no vuelve, ejecuta una parada ordenada de los equipos conectados al SAI.

Un SAI tiene otra importante ventaja: suele llevar un **estabilizador de corriente** que suprime los picos de tensión que nos llegan por el cableado, que también pueden ser perjudiciales.

Los SAI suelen incorporar algún sistema de **monitorización** de los mismos, que se aprovecha gracias a su conexión a algún ordenador. Esto nos permite ver el estado de las baterías, así como una lista de incidentes que el SAI guarda, en la que podemos ver cuándo ha habido pro­blemas de suministro, y cuánto han durado (y si el sistema ha tenido que apagar equipos o no).

Los SAI avanzados nos permiten configurar **disparadores** *(**triggers**,* en inglés), que respon­den a determinadas circunstancias con los comandos que deseemos: por ejemplo, que en caso de fallo eléctrico, se apaguen determinados equipos, o se suspendan, cuánto esperar antes de hacerlo, etc.

En cuanto al **mantenimiento**, hay que tener en cuenta que **las baterías se degradan con el tiempo y el uso**, funcionando cada vez peor… o dejando de funcionar. Es importante comprobar regularmente su estado y, en caso necesario, sustituirlas (suele ser un procedimiento relativamente sencillo).

&nbsp;

&nbsp;

## Tipos de SAI

Se distinguen generalmente tres tipos de SAI:

| Tipo de SAI                    | Características principales                                                                                                                                                 | Ejemplo               |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| **En espera *(Stand-by)***         | La corriente va directamente del enchufe al equipo. Solo cambia a batería cuando hay un corte o una caída de tensión. Tiene un pequeño retardo (2–10 ms) en la conmutación. | *Salicru Home 650VA*  |
| **De línea interactiva *(Line-Interactive)***           | Similar al SAI en espera, pero con regulación automática de tensión (AVR) para compensar pequeñas variaciones sin pasar a batería.                                               | *APC Back‑UPS BX700U‑GR*      |
| **En línea o de doble conversión *(On-line)*** | Siempre convierte la corriente: de alterna (CA, la que llega al enchufe) a continua (CC, la que se almacena en las baterías), y de nuevo a alterna. Los equipos que dependen del SAI están recibiendo energía constantemente de las baterías, que al mismo tiempo se están cargando. En caso de corte de la luz no hay retardo, la alimentación es constante y pura.                                                            | *Riello Sentinel Pro 1500VA* |

&nbsp;

&nbsp;

## Mantenimiento de un SAI

El mantenimiento de un SAI tiene como finalidad **garantizar su disponibilidad** ante cortes o perturbaciones eléctricas y **evitar fallos** por envejecimiento, sulfatación o mala calibración del sistema.


### Comprobación del estado de las baterías

#### 1. Indicadores del propio SAI

Casi todos los SAI modernos *(APC, Eaton, Salicru,* etc.) disponen de:

- **Indicador(es) LED o pantalla LCD** con iconos de batería baja o defectuosa.
- **Software de gestión** (p. ej., *PowerChute, Eaton UPS Companion, ViewPower,* etc.) que muestra:

  - Capacidad estimada (%)
  - Voltaje actual de batería
  - Temperatura interna
  - Autonomía estimada

**Interpretación básica:**

| Síntoma                               | Posible causa                      | Acción recomendada       |
| ------------------------------------- | ---------------------------------- | ------------------------ |
| Autonomía reducida significativamente | Batería degradada                  | Sustituir batería        |
| El SAI se apaga al pasar a batería    | Sulfatación o celda abierta        | Sustituir batería        |
| Olor ácido o deformación del bloque   | Fuga o sobrecalentamiento          | Sustituir inmediatamente |
| Tiempo de recarga anormalmente largo  | Envejecimiento o fallo de cargador | Revisar sistema          |

---

#### 2. Medición con multímetro

En mantenimiento profesional, puede hacerse una comprobación directa:

1. Apagar y desconectar el SAI de la red.
2. Medir el voltaje en los bornes de la batería (normalmente 12 V nominal).

  * Valor normal en reposo: **12,6 – 13,6 V** (batería cargada).
  * Si baja de **11,8 V**, la batería está descargada o degradada.

3. Si hay varias baterías en serie, comprobar cada una individualmente.

---

#### 3. Prueba de autonomía

Consiste en desconectar el SAI de la red eléctrica con carga real conectada (por ejemplo, un PC o una bombilla de prueba) y medir:

* **Duración real** hasta el apagado.
* **Comparar** con la autonomía nominal (consultar ficha técnica).

Si la duración real es < 50 % de la esperada, se recomienda reemplazar la(s) batería(s).

---
### Sustitución de baterías

#### ⚠️ Precauciones básicas

* Desconectar completamente el SAI de la red eléctrica.
* Esperar unos minutos para que los condensadores se descarguen.
* No usar herramientas metálicas sin aislamiento (riesgo de cortocircuito).
* Sustituir **todas las baterías del conjunto**, no solo una, si están en serie o paralelo (para evitar desbalances).

#### Procedimiento general

1. Retirar la tapa o panel frontal (según el modelo).
2. Extraer el bloque de baterías (suelen ser 12 V 7 Ah o 9 Ah tipo VRLA).
3. Conectar las nuevas respetando polaridades (rojo → **+**, negro → **–**).
4. Cerrar y conectar de nuevo a la red.
5. Cargar completamente (normalmente 8–12 h).

---

### Calibrado del sistema de medición

Los SAI «inteligentes» almacenan internamente un **perfil de capacidad de batería**, que puede desajustarse con el tiempo.

#### Síntomas de descalibración

* El software indica «100 % de carga» pero la autonomía es muy baja.
* El software indica un porcentaje menor de carga, pero que nunca sube aunque se mantenga cargando.
* El indicador de batería desciende bruscamente al pasar a batería.

#### Procedimiento habitual de calibración

*(varía según la marca, pero el principio es el mismo)*

1. **Cargar completamente** el SAI (mínimo 8 h).
2. **Desconectarlo de la red** y dejarlo descargarse, preferiblemente con una carga constante (30–50 % de su potencia nominal).
3. **Dejarlo funcionar hasta que se apague por batería baja.**
4. **Reconectarlo a la red y recargar al 100 %.**

**Esto fuerza una nueva calibración interna del nivel de carga.**

💡 En algunos modelos (p. ej. APC Smart-UPS), se puede hacer desde software con la función *Runtime Calibration* o *Battery Calibration*.

---

### Tipos de baterías más habituales

| Tipo                        | Denominación                                                   | Características                                                                         | Usos comunes                                                               |
| --------------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **VRLA AGM**                | (Valve-Regulated Lead Acid, Absorbent Glass Mat)               | Electrolito absorbido en fibra de vidrio. Sin mantenimiento. Resistentes a vibraciones. | La mayoría de los SAI de gama doméstica/profesional (APC, Eaton, Salicru). |
| **VRLA Gel**                | (Electrolito gelificado)                                       | Mejor tolerancia a descargas profundas y temperatura. Más caras.                        | SAI industriales, telecomunicaciones.                                      |
| **Li-ion (iones de litio)** | Modernas, ligeras, vida útil 8–10 años, carga rápida.          | SAI de gama alta o portátiles (Eaton 5PX Lithium, APC Smart-UPS Lithium).               |                                                                            |
| **NiCd / NiMH**             | Antiguas o específicas. Requieren ventilación y mantenimiento. | Prácticamente en desuso en SAI modernos.                                                |                                                                            |

**La mayoría de los SAI convencionales usan baterías selladas tipo VRLA AGM de 12 V / 7–9 Ah.**

---

### Vida útil de las baterías y recomendaciones

| Condición                          | Vida estimada               |
| ---------------------------------- | --------------------------- |
| Temperatura ambiente 20–25 °C      | 3–5 años                    |
| Temperatura elevada (>30 °C)       | Se reduce a la mitad        |
| Ciclos de descarga frecuentes      | Menor duración              |
| Mantenimiento y calibrado correcto | Mayor precisión y autonomía |

Las temperaturas bajas también pueden perjudicar el rendimiento y, sobre todo por debajo de 0 °C, las celdas de las baterías pueden dañarse irreversiblemente si no están cargadas all 100 %.

**Recomendación general:** sustituir las baterías **cada 3–5 años** o antes si la autonomía cae por debajo del 50 % nominal.

---

### Buenas prácticas de mantenimiento preventivo

* Mantener el SAI en lugar ventilado y sin polvo.
* No bloquear las rejillas de ventilación.
* Realizar una **prueba de autonomía cada 3–6 meses**.
* Verificar temperatura interna (ideal: 20–25 °C).
* Calibrar tras cada sustitución de batería.
* Registrar la fecha de cambio y el número de serie del bloque de batería.

&nbsp;

&nbsp;

&nbsp;

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es) 2025 [J. Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollà](https://iespacomolla.es/), Alicante (España)

