# Guía Completa de Instalación y Configuración Debian 13 Trixie

Configuración básica y programas esenciales para una instalación nueva de Linux.


## Agregar repositorios

> Configuración de los repositorios oficiales de Debian Trixie, incluyendo `contrib`, `non-free` y `non-free-firmware`.

---

## Abrir la configuración de APT

Accede como `root` y edita el archivo principal de repositorios:

```bash
su -
nano /etc/apt/sources.list
```

---

## Repositorios

Añade las siguientes líneas al archivo `/etc/apt/sources.list`:

```text
# Debian Trixie
deb https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware

# Debian Trixie Updates
deb https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware

# Debian Trixie Proposed Updates
deb https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware

# Debian Trixie Backports
deb https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware

# Debian Security
deb https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware
# deb-src https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware
```

---

## Guardar los cambios

En `nano`:

```text
CTRL + O    → Guardar
ENTER       → Confirmar
CTRL + X    → Salir
```

---

## Actualizar El Sistema

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Códecs, Tipografias, GDebi, Synaptic, Compresores, Desescompresores y Drivers

```bash
sudo apt-get install -y libavcodec-extra curl nano wget git ffmpeg ttf-mscorefonts-installer gdebi gdebi-core synaptic p7zip-full p7zip-rar rar unrar printer-driver-all 
```

---

## Programas y paquetes

```bash
sudo apt install -y cmatrix btop htop fastfetch mousepad evince eog obs-studio kdenlive qbittorrent vlc tilix gnome-shell-extension-manager gparted
```

---

## Flatpak

```bash
sudo apt install -y flatpak
```

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

## Iconos y Cursores

```bash
sudo apt install -y papirus-icon-theme bibata-cursor-themes
```

## VirtualBox

Actualiza los repositorios e instala las dependencias necesarias para compilar los módulos de VirtualBox:

```bash
sudo apt update

sudo apt install -y \
    build-essential \
    dkms \
    linux-headers-$(uname -r) \
    linux-headers-amd64 \
    perl \
    wget \
    gnupg \
    ca-certificates \
    apt-transport-https
```

Comprueba que los headers del kernel están disponibles:

```bash
ls -ld /usr/src/linux-headers-$(uname -r)
```

---

## Añadir la clave de Oracle

Crea el directorio para las claves de APT:

```bash
sudo install -d -m 0755 /etc/apt/keyrings
```

Descarga la clave oficial de Oracle:

```bash
wget -O /tmp/oracle_vbox.asc \
    https://www.virtualbox.org/download/oracle_vbox_2016.asc
```

Convierte la clave al formato utilizado por APT:

```bash
sudo gpg --dearmor \
    -o /etc/apt/keyrings/oracle-virtualbox.gpg \
    /tmp/oracle_vbox.asc
```

---

## Añadir el repositorio

Añade el repositorio oficial de VirtualBox para Debian Trixie:

```bash
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/oracle-virtualbox.gpg] https://download.virtualbox.org/virtualbox/debian trixie contrib" | \
sudo tee /etc/apt/sources.list.d/virtualbox.list
```

Actualiza la información de los repositorios:

```bash
sudo apt update
```

Comprueba la versión disponible:

```bash
apt policy virtualbox-7.2
```

---

## Instalar VirtualBox

Instala VirtualBox:

```bash
sudo apt install -y virtualbox-7.2
```

Comprueba la instalación:

```bash
VBoxManage --version
```

---

## Añadir usuario a VirtualBox

Añade tu usuario al grupo `vboxusers`:

```bash
sudo usermod -aG vboxusers "$USER"
```

Aplica el cambio de grupo sin cerrar la sesión:

```bash
newgrp vboxusers
```

Comprueba los módulos de VirtualBox:

```bash
lsmod | grep vbox
```

---

## Solución de conflictos con KVM

Si VirtualBox presenta problemas relacionados con los módulos KVM, comprueba si están cargados:

```bash
lsmod | grep kvm
```

Si utilizas un procesador AMD:

```bash
sudo modprobe -r kvm_amd
sudo modprobe -r kvm
```

> Si utilizas Intel, el módulo correspondiente normalmente será `kvm_intel`.

---

## Desactivar KVM

Crea un archivo de configuración para los módulos:

```bash
sudo nano /etc/modprobe.d/virtualbox.conf
```

Añade:

```text
blacklist kvm_amd
blacklist kvm
```

Guarda el archivo y actualiza el `initramfs`:

```bash
sudo update-initramfs -u
```

Reinicia el sistema:

```bash
sudo reboot
```

Prueba VirtualBox

---

## Limpieza final

```bash
sudo apt autoremove -y
sudo apt clean
sudo apt autoclean
```
