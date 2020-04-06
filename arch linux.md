# Instalación arch linux.

* Seguir este [tutorial](https://wiki.archlinux.org/index.php/Installation_guide)

La dificultad está en instalar el grub. Logré que arrancase el SO haciendo lo siguiente.
* Antes de crear las particiones, formatear el disco a msdos. 

``` bash
mkfs.msdos
```

* La partición para / marcarla como booteable. 

``` bash
cfdisk /dev/sda
```

* Utilicé tres particiones :/ swap y /home.

* Montar e instalar el sistema.

```bash
mount /dev/sdaX /mnt
pacstrap /mnt base
```

* Generar fstab. 

``` bash
genfstab -U /mnt >> /mnt/etc/fstab
```

* Cambiar root

``` bash
arch-chroot /mnt
```

* Initramfs

``` bash
mkinitcpio -p linux
# Sino existe el comando mkinitcpio, instalarlo
# pacman -S mkinitcpio
```

* Cambiar contraseña

```bash
passwd 
```
* Instalar bootloader

``` bash
pacman -S grub os-prober
grub-mkconfig -o /boot/grub/grub.cfg
grub-install --target=i386 /dev/sda (Sino funciona utilizar --force)
```

## Post instalación

Después de instalar el sistema, no habrá conexión a red. Hay que activar el dhcp para ello. En cada arranque el servicio dhcp estará desactivado. Con lo que hay que *activar* el servicio. Lo mismo ocurre con el servicio ssh.

### Activar dhcp

``` bash
systemctl enable dhcpcd
```

### SSH

``` bash
pacman -S ssh
```
Activar puerto 22 y escuchar por cualquier interfaz. Editar fichero : */etc/ssh/sshd_config*
Descomentar las siguientes líneas:

``` bash
Port 22
AddressFamiliy any
PermitRootLogin yes
```
Habilitar servicio ssh
``` bash
systemctl enable sshd
```

## Crear usuario y carpeta /home 

``` bash
useradd -d /home/andres -m andres
```

### Instalar sudo y añadir usuario a visudo

```bash
pacman -S sudo
```
```bash
$ visudo
#Editar lineas:
root ALL=(ALL) ALL
andres ALL=(ALL) ALL
```


## Actualización
 
 Los pasos de la wiki de arch son muy explicativos
 
 ```bash
 timedatectl set-ntp true
 #formatear discos : Crear dos particiones / y swap. Marcar / como booteable. Label dos
 cfdisk /dev/sda 
 # Dar formato a las particiones
 mkfs.ext4 /dev/sda1
 mkswap /dev/sda2
 swapon /dev/sda
 # montar particion /
 mount /dev/sda1 /mnt
 pacstrap /mnt base
 # Generar fstab
 genfstab -U /mnt >> /mnt/etc/fstab
 arch-chroot /mnt
 # zona horaria
 ln -sf /usr/share/zoneinfo/Atlantic/Canary /etc/localtime
hwclock --systohc
locale-gen
# Elegir distribución teclado
pacman -S vim
# Descomentar en_US UTF-8
vim /etc/locale.gen
locale-gen
# Configurar red 
vim /etc/hostname # escribir arc
vim /etc/hosts # localhost 127.0.0.1
# initramfs
mkinitcpio -p linux
# password
passwd
# instalar grub
pacman -S grub
# Especificar disco, NO PARTICION
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
 ```
