# Compilar avrgcc

Descargar paquetes:

* binutils
* gcc
* avrlibc
* avrdude (programador)

```bash
apt-get install bison flex
#Descargar paquetes
 mkdir /opt/avrgcc/src
 mkdir /opt/avrgcc/build
 # Binutils
 mkdir /opt/avrgcc/build/binutils
 cd /opt/avrgcc/build/binutils
../../src/binutils-2.31/configure --prefix=/opt/avrgcc/ --disable-nls --target=avr
make 
make install
# gcc
export PATH=$PATH:/opt/avrgcc/bin
#download prerequisites
cd /opt/avrgcc/src/gcc-8.2.0/
contrib/download_prerequisites
mkdir /opt/avrgcc/build/gcc
cd /opt/avrgcc/build/gcc
 ../../src/gcc-8.2.0/configure --prefix=/opt/avrgcc/ --target=avr --enable-languages=c,c++ --disable-nls --disable-libssp --disable-dwarf2
 make 
 make install
 #avr libc
 mkdir /opt/avrgcc/build/avrlibc
 cd /opt/avrgcc/build/avrlibc
  ../../src/avr-libc-2.0.0/configure --prefix=/opt/avrgcc/ --build=`./config.guess` --host=avr
  make 
  make installl
  #avrdude
 cd /opt/avrgcc/src/avrdude-6.3
./configure --prefix=/opt/avrgcc
make
make install
```

[Building and Installing the GNU Tool Chain](https://www.nongnu.org/avr-libc/user-manual/install_tools.html)
