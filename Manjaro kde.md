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
