# Manjaro KDE

Experiencias con manjaro linux.

1. Instalación

Hacer caso al consejo del instalador y crear una partición fat32 de alrededor de 500MB hasta 1Gb en adelante, y ponerle el flag esp y montado sobre boot/??

2. SWAP y kde

Por alguna razón kde usaba memoria swap teniendo ram libre. Deshabilitando la swap una vez, dejaba de dar tirones

```bash
swapoff -a
#esperar que se libere la swap 
swapon -a
```

3. Virtualbox

```bash
#Observar version del kernel
uname -r
pacman -S virtualbox
#instalar la versión de virtualbox que coincida con la version del kernel
sudo vboxreload
```

## lenovo ideapad330

El wifi no funciona en este pc. Falta el driver de la tarjeta de wifi para que se pueda reconocer la interfaz de red inalambrica

### AUR

```bash
uname -r
# la version del kernel que tengo en el momento de escribir: 5.6.15-1-MANJARO
# entonces hay que instalar la version 56 de las cabeceras del kernel
pacman -S linux56-headers
pamac build rtl8821ce-dkms-git
```

### desde las fuentes

```bash
pacman -S linux56-headers
git clone https://github.com/tomaspinho/rtl8821ce
cd rtl8821ce
make
make install
```
