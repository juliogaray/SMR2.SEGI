![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)
# Introducción a la Seguridad Informática

La **seguridad informática** es la práctica de proteger la información mitigando los riesgos a que puede verse sometida. Se suele considerar que incluye la prevención (o al menos la reduc­ción de la probabilidad) de que la información se vea afectada por:

- el acceso no autorizado o inapropiado
- Uso ilegal
- Divulgación (revelación, inspección, copia)
- Alteraciones (disrupción, borrado, corrupción, modificación)

La seguridad informática involucra acciones y procedimientos definidos para garantizar la **pre­ven­ción, detección y represión** de cualquiera de los incidentes mencionados, si tuviesen lugar. Ade­más, debe planear acciones **correctivas**, para los casos en que las medidas anterio­res no hayan sido su­fi­cientes para impedir un incidente.

Aunque en Seguridad Informática nos ocupamos de la protección de la información digital, debemos tener en cuenta que la información puede tomar cualquier forma, y no sólo electró­nica: también se encuentra en forma **física** o **tangible** (p. ej. documentos de papel), o **intangi­ble** (conocimiento por parte de seres humanos).

En estos apuntes vamos a ver una serie de conceptos básicos de Seguridad Informática.

## I. Conceptos básicos

### 1. CID

La seguridad informática busca una protección equilibrada de la **confidencialidad**, la **integri­dad** y la **disponibilidad** (CID), centrándose en una implementación de normas y protocolos eficaces, pero que no afecten negativamente la productividad organizacional.

**Confidencialidad**

➤ Consiste en garantizar **que la información sea accesible solamente para un grupo de per­so­nas autorizadas**, determinado por el propietario de dicha información. En otras palabras, consiste en **«guar­dar el secreto»** de esa información, fuera de las personas que tienen derecho a acceder a ella.

**Integridad**

➤ Se define como el mantenimiento y garantía de la **exactitud** y **consistencia** de la infor­mación durante su ciclo de vida. En otras palabras, consiste en **«mantener los datos de forma que sean correctos y completos»**.

**Disponibilidad**

➤ Es la cualidad de los datos de ser **accesibles** por parte de los usuarios que los necesiten. En otras palabras, significa **«hacer que los datos sean accesibles para los usuarios en todo momento»**.

Debe tenerse en cuenta que el concepto «en todo momento» es utópi­co: hasta los sistemas más fiables tienen algún porcentaje de tiempo, por pequeño que sea, de inaccesibilidad. El porcentaje de tiempo de accesibilidad requerido depende de las necesidades del sistema y sus usuarios: para algunos sistemas, una disponibilidad del 95% puede ser suficiente. En otros, se puede requerir el 99% o más.

Algunas medidas convencionales de alta disponibilidad son las de «los nueves»:
- «Dos nueves», 99% de disponibilidad. los datos pueden permanecer inaccesi­bles hasta 3,65 días al año.
- «Tres nueves», 99,9% de disponibilidad. los datos pueden permanecer inaccesi­bles hasta 8¾ horas al año.
- «Cuatro nueves», 99,99% de disponibilidad. los datos no pueden permanecer inaccesibles más de **52,56 minutos al año**.
- «Cinco nueves», 99,999% de disponibilidad. los datos no pueden permanecer inaccesibles más de **5,26 minutos al año**. Se considera un estándar de excelen­cia. Muy costoso y muy difícil de alcanzar.

Cada «nueve» que añadimos incrementa enormemente los requerimientos para poder cumplir con la dis­po­ni­bi­li­dad comprometida. Por poner un ejemplo real, Amazon se compromete a una dis­po­ni­bi­li­dad del 99,95% en sus sistemas, a medio camino entre los «tres nueves» y los «cuatro nueves». Y no siempre ha sido capaz de cumplir con dicho compromiso.

> A los conceptos anteriores se suelen añadir la **Autentificación** (o Autenticación) que, por su importancia, verem­os en más detalle en el siguiente punto, y el **No repudio**, formando el conocido acrónimo **CIDAN**.

**No repudio**

Aunque ha tenido diferentes definiciones, algunas de ellas un tanto crípticas, en térmi­nos generales el concepto No repudio se puede interpretar, en una definición relativa­mente accesible, como:

➤ Una propiedad lograda por medios criptográficos que impide que un individuo o entidad niegue haber realizado una acción particular relacionada con ciertos datos (como mecanismos de no-rechazo o autoridad (origen); de prueba de obligación, intención o compromiso; o de prueba de pertenencia).

Es decir, el no repudio se centra en **«ofrecer una garantía de que “alguien” ha hecho “al­go” con ciertos datos»** (por ejemplo, que los ha recibido o leído, o creado, o modifica­do), sin que ese «alguien» pueda negarlo.

### 2. Autentificación — AAA

Como todos intuimos, la **Autentificación** (o «autenticación») es una parte básica de la seguridad informática. Consiste en el **conjunto de medidas tomadas para garantizar que un agente que accede a un sistema es quien dice ser**. Un «ægente» puede ser una persona, una aplicación, un sistema, etc.

Ampliando un poco el concepto, llegamos a la tríada **AAA**: **Autentificación**, **Autorización**, y **Registro** (en inglés _**Authentication**, **Authorization**, **Accounting**,_ de ahí las tres aes).

**Autentificación**

➤ Como hemos dicho, consiste en garantizar que un agente es quien dice ser. No debemos limitarnos a pensar en personas, sino tambien en sistemas (cuando se intenta establecer una conexión, asegurarse de que el sistema que la inicia y el que la recibe son los sistemas que se quie­re conectar).

**Autorización**

➤ Establece los **privilegios** de un usuario autentificado: una vez que hemos garantizado que el agente que se conecta es quien dice ser, los mecanismos de autorización deter­minan qué puede hacer dicho agente en nuestro sistema: a qué datos puede acceder, y con qué «poderes»: ¿puede leerlos? ¿Puede también modificarlos, añadir datos nuevos, eliminar datos existentes? ¿Puede incluso alterar la estructura de los datos, añadiendo campos nuevos en una base de datos, por ejemplo?

**Registro**

➤ El registro es un sistema de recopilación de información con respecto a qué agente(s) ha(n) accedido a qué datos, y qué ha(n) hecho con ellos. El nivel de detalle puede variar según los requerimientos de seguridad que tengamos: puede ser suficiente que el siste­ma anote que el agente X accedió a la BD Y en tal fecha y hora, o puede ser necesario que se recoja también a qué tablas accedió, qué datos leyó, o qué datos creó, modificó o eliminó.

## II. Seguridad física y seguridad lógica

Una de las clasificaciones más habituales de tipos de seguridad es la que distingue entre segu­ridad física y seguridad lógica.

### 1. Seguridad física

La **seguridad física** de un sistema informático consiste en la aplicación de barreras físicas y procedimientos de control frente a **amenazas físicas al hardware**.

#### 1.1 Acceso de personas no autorizadas

En seguridad informática hay un dicho según el cual «si alguien tiene acceso físico a tu siste­ma, ya no es tu sistema». Esto es así porque, cuando dispone de acceso físico a un ordenador, puede extraer la información que contiene, puede eliminarla, puede instalar herramientas de malware, etc. Es posible tomar medidas para reducir el riesgo o retrasar la labor de alguien que acceda con intenciones oscuras a nuestro sistema, pero su efectividad puede ser limitada. Por eso la seguridad física es clave: si impides el acceso físico al sistema, la única posibilidad de ataque es a través de las redes y del software, y ahí es más fácil proteger nuestro equipo.

Aparte de los **ataques dirigidos** expresamente contra la información almacenada o trans­mi­ti­da­, debemos considerar también amenazas como **robos** o **sabotajes**.

#### 1.2 Protección frente a otras amenazas

La seguridad física también debe proteger nuestros sistemas frente a otras amenazas, tanto ocasionadas por el hombre como por la naturaleza del medio físico en que se encuentran ubicados los sistemas. Las principales amenazas que hay que tener en cuenta son:

- Desastres naturales, incendios accidentales, inundaciones… y cualquier variación pro­ducida por las condiciones ambientales (fundamentalmente exceso de calor y/o hume­dad);
- Cortes en el suministro eléctrico;
- Cortes en el acceso a red y/o Internet;
- Disturbios internos y externos (aunque causados por personas, no los consideramos aquí como específicamente dirigidos contra nuestros sistemas, aunque pueden afectarl­os, como es lógico).

Evaluar y controlar permanentemente la seguridad física del sistema es la base para comenzar a integrar la seguridad como función primordial del mismo. Tener controlado el ambiente y acceso físico permite disminuir siniestros y tener los medios para luchar contra accidentes. 

### 2. Seguridad lógica

La **seguridad lógica** de un sistema informático consiste en la aplicación de barreras y proced­imientos que protejan el acceso a los datos y a la información contenida en dicho sistema.

El activo más importante de un sistema informático es la información, lo que convierte su protección en un objetivo vital. La seguridad lógica, por tanto, es un elemento clave de todo sistema informático.

La seguridad lógica trata de conseguir los siguientes objetivos:
- Restringir el acceso a los programas y archivos.
- Asegurar que los usuarios puedan trabajar sin supervisión y no puedan modificar los programas ni los archivos que no correspondan a su autorización.
- Garantizar que se estén utilizados los datos, archivos y programas correctos en y según el procedimiento correcto.
- Verificar que la información transmitida sea recibida sólo por el destinatario al cual ha sido enviada (confidencialidad) y que la información recibida sea la misma que la transmitida (integridad).
- Disponer de métodos alternativos de emergencia para la transmisión de información.

## III. Vulnerabilidades

Definimos **vulnerabilidad** como una **debilidad de cualquier tipo que compromete la segu­ridad del sistema informático**
.
Podemos clasificar las vulnerabilidades de los sistemas informáticos (SI) en función de diver­sos criterios:
- Vulnerabilidades de **diseño**
   - Debilidad en el diseño de protocolos utilizados en las redes.
   - Políticas de seguridad deficientes e inexistentes.
- Vulnerabilidades de **implementación**
   - Errores de programación.
   - Existencia de puertas traseras en los sistemas informáticos.
   - Descuido de los fabricantes.
- Vulnerabilidades en el **uso**
   - Configuración inadecuada de los sistemas informáticos.
   - Desconocimiento y falta de sensibilización de los usuarios y de los responsables de informática.
   - Disponibilidad de herramientas que facilitan los ataques.
   - Limitación gubernamental de tecnologías de seguridad.

### 1. Vulnerabilidad de día cero _(zero-day_ o _0-day)_

Una vulnerabilidad de día cero (en inglés _zero-day_ o _0-day)_ es aquella que es desconocida por quien debería estar interesado/a en mitigarla (incluido/a quien haya creado el software vulne­rable), pero de la que se sabe cómo explotarla. En tanto la vulnerabilidad no sea mitigada, puede ser explotada por hackers para afectar al software, los datos, o quizá para acceder a otros dispositivos conectados a la misma red. Un _exploit_[^1] dirigido a obtener ventaja de una vul­ne­ra­bi­li­dad de este tipo, se llama, lógicamente, _exploit zero-day,_ o ataque _zero-day._

El término «día cero» se refería, originalmente, al número de días desde que un desarrollo de software se publicaba. El software «día cero» era el que se había obtenido hackeando los or­denadores de la persona o compañía creadora del software (PoCCS), _antes_ de su publicación.

Más adelante se aplicó a las vulnerabilidades que permitían este tipo de ataque, y al número de días que la PoCCS había tenido para fijarlo, desde que lo conoce (por tanto, «día cero» se refiere a vulnerabilidades que la PoCCS aún no conoce; a partir del momento en que es cons­ciente de la vulnerabilidad, comienzan a contar los días hasta la publicación de una solución para mitigar la vulnerabilidad).

### 2. Ingeniería social

El mayor riesgo de un sistema informático suele estar entre el teclado y la silla. Generalmente es mu­cho más fácil engañar a una persona que a una máquina.

**Ingeniería social**

➤ Conjunto de técnicas usadas por los (ciber)criminales para engañar a los usuarios in­cau­t­os con el fin de manipular sus sistemas informáticos u obtener datos confidencia­les.

Por lo general se explota la falta de conocimiento de los usuarios; debido a la velocidad a la que avanza la tecnología, la mayoría de los usuarios de sistemas informáticos no son cons­cientes del valor real de los datos personales ni saben muy bien cómo proteger dicha informa­ción. Además, puesto que la Ingeniería social no se basa en atacar a la máquina, sino al ser hu­mano, no requiere conocimientos técnicos; más bien se centra en el uso de habilidades tan hu­manas y omnipresentes como la empatía, el engaño, el uso de medias verdades, el abuso de la confianza del prójimo, etc.

La [Oficina de Seguridad del Internauta](https://www.incibe.es/ciudadania/) tiene una página bastante completa dedicada a la Inge­niería social: [www.incibe.es/ciudadania/tematicas/ingenieria-social-fraudes-online](https://www.incibe.es/ciudadania/tematicas/ingenieria-social-fraudes-online)

## IV. Amenazas

En el punto anterior hemos visto los diferentes tipos de vulnerabilidades de un sistema infor­mático. Ahora vamos a relacionar el concepto de vulnerabilidad con el de amenaza.

**Amenaza**

➤ Una amenaza es un elemento o acción que puede perjudicar la seguridad de la in­for­ma­ción. Su existencia depende de la presencia de vulnerabilidades de las que se puedan apro­ve­char.

Cuando en un sistema informático se detecta una vulnerabilidad y existe una amenaza asocia­da a dicha vulnerabilidad, puede ocurrir que la amenaza se haga realidad, poniendo en riesgo el sistema.

**Evento**

➤ En este contexto, un evento es un suceso en el que una amenaza se pone en práctica, como un ataque.

Si dicho evento se produce y el riesgo que era probable ahora es real, el sistema informático sufrirá daños que habrá que valorar cualitativa y cuantitativamente:

**Impacto**

➤ Daños sufridos por un sistema informático que ha sido víctima de un evento de segurida­d.

Resumiéndolo en una frase, **un evento producido en el sistema informático por una ame­naza asociada a una vulnerabilidad del sistema, produce un impacto sobre él**.

Si queremos eliminar las vulnerabilidades del sistema informático o queremos disminuir el impacto que puedan producir sobre él, hemos de proteger el sistema mediante una serie de medidas que podemos llamar defensas o salvaguardas. Toda la idea de Seguridad Informática gira en torno a estos conceptos.

## V. Seguridad activa y seguridad pasiva

El otro gran criterio para clasificar las medidas de seguridad consiste en el momento en que las medidas se activan ante un evento de seguridad.

### 1. Seguridad activa

➤ La seguridad activa incluye todas las medidas de seguridad que tratan de impedir que un evento de seguridad tenga lugar. Su objetivo es eliminar las vulnerabilidades y/o limitar las amenazas.

Medidas de seguridad activa son, por ejemplo, el uso de cortafuegos, la limitación de acceso a los sistemas con medidas de acreditación, el uso de unidades de almacenamiento redundantes, la limitación de uso de dispositivos externos, etc.

### 2. Seguridad pasiva

➤ La seguridad pasiva agrupa todas las medidas de seguridad que, una vez producido un evento de seguridad (es decir, cuando la seguridad activa ha fallado), permiten que el sistema se recupere con el menor impacto posible.

El ejemplo más obvio de medidas de seguridad pasiva es el uso de copias de seguridad _(back-up)._ Si algún evento ha afectado a nuestros sistemas, bases de datos, etcétera, siempre podre­mos usar copias de seguridad recientes para restaurar el sistema a un estado correcto de fun­cionamiento reciente.


&nbsp;

&nbsp;

&nbsp;

## Colofón
 
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es) 2025 [J. Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollà](https://iespacomolla.es/), Alicante (España)

### Referencias

Aula Mentor. [Curso de seguridad Informática](http://descargas.pntic.mec.es/mentor/visitas/demoSeguridadInformatica/index.html). Ministerio de Educación, Formación Pro­fe­sional y Deportes.

### Notas

[^1]: _Exploit_ es una palabra inglesa que significa «explotar» o «aprovechar». En el ámbito de la informática es un fragmento de software, fragmento de datos o secuencia de comandos o acciones, utilizada con el fin de apro­ve­char una vulnerabilidad de seguridad de un sistema de información para conseguir un comportamiento no deseado del mismo.


