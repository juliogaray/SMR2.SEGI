![Generalitat Valenciana - CEICE / IES Poeta Paco Mollá (Alicante)](http://julio.iespacomolla.es/Recursos-Comunes/Cabecera_CEICE_IESPPM_Transparente.svg)

# Instalación y configuración de un servidor VPN


En esta práctica vamos a instalar y configurar un servidor VPN en casa, para poder acceder a él desde el instituto.

Necesitaremos una máquina virtual con GNU/LInux en la que instalaremos OpenVPN (alternativamente, si lo deseas, puedes usar el mismo software sobre Windows, pero en esta práctica veremos los pasos para hacerlo en Linux):

## Por desarrollar: abrir puerto 80 (o 443) en el router hacia la máquina

## Instalación y configuración de *OpenVPN*

Sigue ls siguientes pasos para instalar y configurar el software servidor de conexiones VPN:

1. Instala OpenVPN:

```bash
sudo apt update
sudo apt install openvpn
```

2. Copia los archivos de configuración de ejemplo de OpenVPN al directorio de configuración:

```bash
sudo cp -r /usr/share/doc/openvpn/examples/easy-rsa/ /etc/openvpn
```

3. Inicializa la configuración PKI. Este paso configura una Autoridad de Certificación (CA) propia (se te pedirán algunos detalles como el Nombre Común (o sea, el nombre de tu servidor), etcétera:

```bash
cd /etc/openvpn/easy-rsa/3.0
./easyrsa init-pki
./easyrsa build-ca
```


## Referencias

[Página principal del proyecto IPFire](https://www.ipfire.org/)
[Wiki de IPFire](https://wiki.ipfire.org/)

## Colofón
[![CC-BY-NC-SA](https://upload.wikimedia.org/wikipedia/commons/5/55/Cc_by-nc-sa_euro_icon.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es)
2024 [Julio Garay](mailto:juliogaray.informatica@iespacomolla.es), [IES Poeta Paco Mollá](https://iespacomolla.es/), Alicante (España)


