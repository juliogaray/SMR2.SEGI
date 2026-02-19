![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEICE_IESPPM_Transparente.svg)

# Sistemas de alimentación ininterrumpida (SAI)

![Imagen de un SAI](https://upload.wikimedia.org/wikipedia/commons/b/b0/UPSAPC.jpg)

Un **SAI** (en ingles ***UPS: Uninterruptible Power Supply**)* es un dispositivo que cuenta con una batería o conjunto de baterías con el que se alimenta uno o varios sistemas.

En caso de fallo del suministro eléctrico, los equipos conectados al SAI siguen funcionando porque consigue electricidad de la(s) batería(s). La capacidad de suministro depende del SAI y del consumo de los equipos, pero es habitual que garanticen un suministro de al menos diez minutos.

Si es posible, conviene aplicar redundancia e instalar un doble juego de equipos SAI, para es­tar cubiertos en caso de que uno fallara. Esto es posible porque la mayoría de los servidores vienen con doble fuente de alimentación, lo que nos permite conectar una
fuente a cada grupo de SAI.

En caso de fallo de suministro eléctrico, el SAI:

1. Espera unos minutos por si el corte ha sido puntual y vuelve el suministro en poco tiempo.
2. Si el suministro no vuelve, ejecuta una parada ordenada de los equipos conectados al SAI.

Un SAI tiene otra importante ventaja: suele llevar un **estabilizador de corriente** que suprime los picos de tensión que nos llegan por el cableado, que también pueden ser perjudiciales.

Los SAI suelen incorporar algún sistema de **monitorización** de los mismos, que se aprovecha gracias a su conexión a algún ordenador. Esto nos permite ver el estado de las baterías, así como una lista de incidentes que el SAI guarda, en la que podemos ver
cuándo ha habido pro­blemas de suministro, y cuánto han durado (y si el sistema ha tenido que apagar equipos o no).

Los SAI avanzados nos permiten configurar **disparadores** *(**triggers**,* en inglés), que respon­den a determinadas circunstancias con los comandos que deseemos: por ejemplo, que en caso de fallo eléctrico, se apaguen determinados equipos, o se suspendan, cuánto esperar antes de hacerlo, etc.

En cuanto al **mantenimiento**, es importante comprobar regularmente el estado de las baterías, que con el tiempo y el uso tienden a degradarse y funcionar cada vez peor… o no funcionar. En caso necesario, suelen ser sencillas de sustituir.

## Cómo elegir un SAI

Los factores más importantes para elegir un SAI es la **potencia consumida** (en vatios) por los equipos que debe proteger, y la **capacidad** de las baterías, que determinan **cuánto tiempo** podrá mantener el suministro eléctrico a la potencia dada. Otras consideraciones, como el precio, la disponibilidad de servicio técnico, repuestos, etcétera, también deben ser tenidas en cuenta, obviamente. Pero aquí nos vamos a centrar en los aspectos técnicos.

### 1. Definir los datos principales
1. **Potencia del equipo (P, en vatios)**: especifica cuánta potencia consume el equipo conectado al SAI.
2. **Duración de respaldo requerida (T, en minutos)**: el tiempo durante el cual el SAI debe suministrar energía.

### 2. Convertir la potencia a [VA (voltio-amperios)](https://es.wikipedia.org/wiki/Voltiamperio)
La potencia del equipo en VA (**S**) se calcula a partir de la potencia en vatios (**P**) y un término llamado [*factor de potencia*](https://es.wikipedia.org/wiki/Factor_de_potencia) (**FP**). Si no sabes el factor de potencia de los equipos que debes proteger, **asume un valor medio de 0,8**. El cálculo se realiza así:

$$S = \frac{P}{FP}$$

Por ejemplo, si el equipo tiene una potencia de 500 W y un factor de potencia de 0,8:

$$S = \frac{500}{0{,}8} = 625 \, \text{VA}$$

Ahora sabemos la potencia de dos maneras: 500 W o 625 VA (los SAI suelen anunciar su potencia en VA).

### 3. Calcular la capacidad requerida (en [Wh —vatios-hora—](https://es.wikipedia.org/wiki/Vatio-hora) o VAh —voltio-amperios-hora—)
La capacidad requerida de la(s) batería(s) (**E**, de energía) se calcula como:

$$E = P \times T \, \text{(en horas)}$$

Si el tiempo está en minutos, conviértelo a horas:

$$T \, \text{(en horas)} = \frac{T \, \text{(en minutos)}}{60}$$

Por ejemplo, si el equipo consume 500 W y necesita funcionar durante 30 minutos:

$$E = 500 \times \frac{30}{60} = 250 \, \text{Wh}$$

Si necesitas la capacidad en VAh:

$$E = \frac{500}{0{,}8} \times \frac{30}{60} = 312{,}5 \, \text{VAh}$$

### 4. Ajustar la capacidad en función de la eficiencia del SAI
Los SAI no son 100 % eficientes; suelen tener una eficiencia del 90% al 95%. Si la eficiencia es del 90 %, se ajusta la energía requerida:

$$E_{\text{ajustada}} = \frac{E}{\text{Eficiencia}}$$

Para una eficiencia del 90 %:

$$E_{\text{ajustada}} = \frac{250}{0{,}9} = 278 \, \text{Wh}$$

o, usando voltio-amperios:

$$E_{\text{ajustada}} = \frac{312{,}5}{0{,}9} \approx 348 \, \text{VAh}$$

### 5. Elegir el SAI con margen de seguridad

Se recomienda siempre elegir un SAI con capacidad un 20-30 % mayor que la calculada, por seguridad (entre otros factores, cuando la batería se va descargando por debajo del 20-30 %, su rendimiento baja bastante, y deja de entregar la potencia requerida).

La capacidad nominal del SAI (en VA) debe ser mayor que la potencia aparente (S) del equipo, y la capacidad de la batería debe garantizar la energía ajustada.

Con un margen razonable del 30 %, nuestra capacidad de 278 Wh se transforma así:

$$278 \, \text{Wh} \times 1{,}3 \approx 360 \, \text{WAh}$$

(De forma análoga, los 348 VAh se transforman en aprox. 450 VAh).

En este caso, un SAI de **625 VA** con baterías capaces de almacenar al menos **360 Wh** (o 450 VAh) sería suficiente (para alimentar un equipo que consume 500 W durante media hora).

### **Consideraciones adicionales**

- **Picos de potencia:** Si el equipo genera picos de potencia (por ejemplo, servidores), asegúrate de que el SAI los pueda soportar.
- **Baterías:** Verifica la autonomía ofrecida por las baterías del SAI en función de su capacidad (en Wh o VAh). Muchas veces se ofrecen datos en gráficos, porque el tiempo de autonomía depende de la potencia consumida.

Este enfoque permite dimensionar correctamente el SAI para tus necesidades.

&nbsp;

&nbsp;

&nbsp;


## Referencias



## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2024 Julio Garay, [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)


