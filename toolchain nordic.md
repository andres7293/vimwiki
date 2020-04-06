# Instalar toolchain nordic

## Enlaces 

[GNU Toolchain | GNU-RM Downloads – Arm Developer](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
[Nordic SDK](https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/)
[SEGGER - The Embedded Experts - Downloads - J-Link / J-Trace](https://www.segger.com/downloads/jlink/#J-LinkSoftwareAndDocumentationPack)
[nRF5 Command Line Tools - nordicsemi.com](https://www.nordicsemi.com/Software-and-Tools/Development-Tools/nRF5-Command-Line-Tools)


```bash
cd Documentos
mkdir nordic-toolchain
cd nordic-toolchain

# Descargar compilador arm-none-eabi
wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/8-2018q4/gcc-arm-none-eabi-8-2018-q4-major-linux.tar.bz2?revision=d830f9dd-cd4f-406d-8672-cca9210dd220?product=GNU%20Arm%20Embedded%20Toolchain,64-bit,,Linux,8-2018-q4-major -O gcc-arm-none-eabi-8-2018-q4-major-linux.tar.bz2
tar xvf gcc-arm-none-eabi-8-2018-q4-major-linux.tar.bz2

# Descargar Nordic SDK y documentación
wget https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/nRF5_SDK_15.2.0_9412b96.zip
wget https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/nRF5_SDK_15.2.0_offline_doc.zip
unzip nRF5_SDK_15.2.0_9412b96.zip
mkdir doc
mv nRF5_SDK_15.2.0_offline_doc.zip doc
cd doc
unzip nRF5_SDK_15.2.0_offline_doc.zip

#Descargar JLink via web y añadir ejecutables al PATH del sistema
tar xzvf JLink_Linux_V640_x86_64.tgz
# Sino funciona, descargar fichero .deb e instalarlo con dpkg

#Descargar nRF5 command line tools
mkdir cli-tools
cd cli-tools
mv nRF-Command-Line-Tools_9_8_1_Linux-x86_64.tar cli-tools
cd cli-tools 
tar xvf nRF-Command-Line-Tools_9_8_1_Linux-x86_64.tar
#añadir en bashrc 
PATH=$PATH:/home/andres/Documentos/nordic-toolchain/cli-tools/mergehex
PATH=$PATH:/home/andres/Documentos/nordic-toolchain/cli-tools/nrfjprog

# Añadir ruta compilador al sdk de nordic
cd nRF5_SDK_15.2.0_9412b96/components/toolchain/gcc/

Editar Makefile.posix
GNU_INSTALL_ROOT ?= /home/andres/Documentos/nordic-toolchain/gcc-arm-none-eabi-8-2018-q4-major/bin/
GNU_VERSION ?= 8.2.1
GNU_PREFIX ?= arm-none-eabi
```

En el caso de la placa nordic de aliexpress no trae soldado el reloj de 32KHz que utiliza el softdevice. Para ello es necesario configurar el softdevice para usar el reloj rc interno.
En el caso del sdk12 hay que escribirlo por código. En el sdk15, haciendo make sdk_config podemos configurar mediante una aplicación el tipo de reloj a utilizar.

## Depuración con Jlink de Segger RTT.

[How to use RTT Viewer (or similar) on GNU/Linux? - Nordic DevZone](https://devzone.nordicsemi.com/f/nordic-q-a/14512/how-to-use-rtt-viewer-or-similar-on-gnu-linux)

En el config del proyecto habilitar que el log vaya por RTT y deshabilitar la uart. Compilar y tostar.

En un terminal ejecutar :
```bash
JLinkExe
# Dentro de la consola escribir
connect
?
# Aparece diversas opciones, elegimos SWD (porque es la que estamos usando)
# intro varias veces y escribimos connect otra vez
connect
#intro varias veces y debería encontrar automaticamente la cpu. En el caso de este momento un Cortex M4.
```

Abrimos otra terminal y ejecutamos

```bash
JLinkRTTClient
#Ya tenemos una consola con la depuración activada
```
