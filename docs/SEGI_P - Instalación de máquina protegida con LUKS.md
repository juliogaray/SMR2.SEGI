![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](https://raw.githubusercontent.com/juliogaray/recursos/main/img/Cabecera_CEICE_IESPPM_Transparente.svg)

# Práctica: Cifrado Completo de Debian con LUKS

## Objetivos

* Instalar Debian con cifrado completo (LVM+LUKS).
* Comprender los conceptos teóricos del cifrado a nivel de bloque.
* Gestionar claves LUKS.
* Analizar el papel del área de intercambio (swap) en la seguridad.
* Realizar comprobaciones y ejercicios prácticos.

---

## 1. Introducción

> NOTA: esta introducción es para tu información, no hace falta que la incluyas en la práctica.

### ¿Qué es LUKS?

LUKS _(Linux Unified Key Setup)_ es el estándar para el cifrado de discos en Linux. Cifra **bloques de disco**, no archivos, por lo que ofrece protección al sistema completo cuando la máquina está apagada.

### ¿Qué protege LUKS?

* Protege contra **acceso físico**: si el equipo es robado o se extrae su disco.
* Cifra **todo el contenido**, incluyendo `/home`, `/var`, `/tmp` y **swap**.

### ¿Qué NO protege LUKS?

* No protege un sistema **encendido**.
* No evita el acceso remoto si la máquina está iniciada.

### ¿Qué es LVM?

LVM _(Logical Volume Manager)_ permite manejar el almacenamiento de forma flexible, creando volúmenes lógicos dentro de un mismo contenedor cifrado.

En esta práctica, usaremos la arquitectura:

```
Disco físico
 └── Partición cifrada (LUKS)
     └── LVM
         ├── lv-root
         └── lv-swap
```

---

## 2. Instalación de Debian con cifrado completo

### Paso 1: Arranque del instalador

Arranca la máquina virtual desde la ISO de Debian y selecciona:

**Install** (no **Graphical Install**).

### Paso 2: Seleccionar el particionado

En el menú de particionado, elige:

**Guiado – utilizar todo el disco y configurar LVM cifrado**

Esta opción:

* Crea una única partición cifrada.
* Dentro de ella monta LVM.
* Evita que datos sensibles queden sin cifrar.

En el _esquema del particionado_ puedes seleccionar, como de costumbre, **Todos los ficheros en una partición**. Recuerda que, en un sistema real, generalmente optaríamos por **separar la partición /home** (y de hecho, si lo prefieres, puedes hacerlo también en esta práctica).

### Paso 3: Configurar la _passphrase_ de cifrado

Se te pedirá que introduzcas una **frase de contraseña** (o _passphrase):_

> "Frase de contraseña de cifrado"

Esta _passphrase:_

* Se usa para desbloquear la clave maestra del disco.
* No es la clave real que cifra los datos: LUKS usa una **master key** aleatoria.
* Si pierdes la _passphrase,_ pierdes el acceso **sin posibilidad de recuperación**.

### Paso 4: Confirmación del particionado

Cuando se te pregunte _Cantidad en el grupo de volumen a usar en el particionado guiado,_ dale a [Continuar], para usar todo el disco.

Después confirma la definición de particiones (finaliza el particionado y escribe los cambios en el disco).

El instalador creará automáticamente:

* Partición física → `crypto_LUKS`
* LUKS mapeado → `/dev/mapper/sdaX_crypt`
* Grupo LVM → `vg0`
* Volúmenes lógicos → `lv-root` y `lv-swap`

### Paso 5: Instalación normal

Instala Debian y crea un usuario.

Reinicia la máquina.

---

## 3. Primer arranque: desbloqueo del sistema

Aparecerá:

**"Please unlock disk sdaX_crypt:"**

Introduce la _passphrase._

Este prompt aparece antes de cargar el kernel completo porque:

* El sistema raíz está **dentro del volumen cifrado**.
* El initramfs contiene el código para abrir LUKS.

Hasta que se introduce la clave, el sistema operativo literalmente **no existe para la máquina**.

---

## 4. Verificación del cifrado

Ejecutar:

```
lsblk -f
```

Debes ver una estructura con `crypto_LUKS` y volúmenes `dm-*`.

> `dm-*` indica dispositivos gestionados por _device-mapper,_ usados por LUKS y LVM.

---

## 5. Administración de claves LUKS

### 5.1 Ver los _slots_ de clave

```
sudo cryptsetup luksDump /dev/sdaX
```

LUKS permite **hasta 8 _passphrases_ distintas**.

### 5.2 Añadir una segunda clave

```
sudo cryptsetup luksAddKey /dev/sdaX
```

Cada _slot_ almacena una copia cifrada de la _master key._ El cifrado de datos **no cambia**, solo la forma de desbloquearlo.

### 5.3 Eliminar una clave

```
sudo cryptsetup luksRemoveKey /dev/sdaX
```

### 5.4 Cambiar una clave correctamente

1. Añadir la nueva clave.
2. Probar reiniciando.
3. Eliminar la antigua.

Nunca uses `luksChangeKey` sin entenderlo: puede sobrescribir claves.

---

## 6. Verificación del _swap_ cifrado

Ejecutar:

```
cat /etc/crypttab
```

Debe mostrar que el área de intercambio _(swap)_ está dentro del volumen cifrado.

**Importante**: el _swap_ puede contener:

* contraseñas
* documentos
* fragmentos de memoria

Si no se cifra, es un objetivo prioritario para análisis forense.

---

## 7. Ejercicios prácticos

### A) Añadir una segunda passphrase LUKS.

Confirmar con `luksDump`.

### B) Cambiar la _passphrase_ principal.

Usar método seguro explicado antes.

### C) Explicar por escrito:

* Qué cifra LUKS.
* Qué no cifra.
* Por qué protege únicamente con el sistema apagado.
* Por qué LVM es útil en sistemas cifrados.

---

## 8. Extensión opcional: ataque forense simulado

1. Apagar la VM.
2. Arrancar con un Live-CD.
3. Ejecutar:

```
lsblk
sudo mount /dev/sdaX /mnt
```

4. Demostrar que **no se puede montar** porque la partición está cifrada.

---

## 9. Conclusiones

* LUKS ofrece una protección muy sólida contra acceso físico.
* El cifrado completo evita fugas en logs, tmp o swap.
* La comprensión de LUKS, LVM y la _master key_ es esencial para administradores reales.

---

Fin de la práctica.


&nbsp;

&nbsp;

&nbsp;

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2025 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)

