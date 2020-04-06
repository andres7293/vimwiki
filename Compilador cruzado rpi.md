# Compilación cruzada gcc para la Rpi

Paquetes necesarios:
* binutils
* gcc
* glibc
* gmp
* mpc
* mpfr
* Cabeceras del kernel de linux.

## Enlaces 
Todo!!

## Explicación

El paquete binutils contiene el software necesario para realizar operaciones de bajo nivel como el ensamblador (ar), enlazador (ld) ,etc.

Durante la primera compilación de gcc, o fase 1, se genera una compilador SIN la librería estándar de C (sin printf, malloc, memcpy, etc). Con este primer compilador, compilamos la librería estándar de C (glibc u otra), junto con las cabeceras del kernel.

Con la glibc compilada, volvemos a compilar gcc, esta vez con soporte para glibc. Este compilador de fase final, puede compilar los programas que hagan uso de la librería estándar.

## Building

### Binutils 2.29

```bash
cd /opt/rpi/src
wget https://ftp.gnu.org/gnu/binutils/binutils-2.29.tar.bz2
tar xf binutils-2.29.tar.bz2  #(OJO  sino funciona hacer un bzip2 -d binutils-2.29.tar.bz2  y luego tar x binutils-2.29.tar)
mkdir -p /opt/rpi/build/binutils-2.29
cd /opt/rpi/build/binutils-2.29
apt-get install gcc -y
apt-get install gcc-c++ -y
../../src/binutils-2.29/configure --with-sysroot=/opt/rpi/sysroot --prefix=/opt/rpi --disable-nls --target=arm-linux-gnueabihf --disable-multilib
make
make install
export PATH=/opt/rpi/bin:${PATH}
```

### GCC 7.2.0 (fase 1)

``` bash
cd /opt/rpi/src
wget https://ftp.gnu.org/gnu/gcc/gcc-7.2.0/gcc-7.2.0.tar.gz
wget https://ftp.gnu.org/gnu/gmp/gmp-6.1.2.tar.bz2
wget https://ftp.gnu.org/gnu/mpc/mpc-1.0.3.tar.gz
wget https://ftp.gnu.org/gnu/mpfr/mpfr-3.1.5.tar.gz
tar xf gcc-7.2.0.tar.gz
tar xf gmp-6.1.2.tar.bz2
tar xf mpc-1.0.3.tar.gz
tar xf mpfr-3.1.5.tar.gz
mv gmp-6.1.2 gcc-7.2.0/gmp
mv mpc-1.0.3 gcc-7.2.0/mpc
mv mpfr-3.1.5 gcc-7.2.0/mpfr
mkdir -p /opt/rpi/build/gcc-7.2.0
cd /opt/rpi/build/gcc-7.2.0
../../src/gcc-7.2.0/configure --prefix=/opt/rpi --target=arm-linux-gnueabihf --enable-languages=c --without-headers --disable-nls --disable-multilib --disable-threads --disable-shared
make all-gcc all-target-libgcc
make install-gcc install-target-libgcc
```

### Cabeceras del kernel

``` bash
cd /opt/rpi/src
git clone https://github.com/raspberrypi/linux.git
cd linux
mkdir -p /opt/rpi/sysroot/usr
make headers_install ARCH=arm INSTALL_HDR_PATH=/opt/rpi/sysroot/usr
```

### Glibc 2.26

``` bash
cd /opt/rpi/src
wget https://ftp.gnu.org/gnu/glibc/glibc-2.26.tar.bz2
tar xf glibc-2.26.tar.bz2
mkdir -p /opt/rpi/build/glibc-2.26
cd /opt/rpi/build/glibc-2.26
../../src/glibc-2.26/configure --prefix=/usr --host=arm-linux-gnueabihf --with-headers=/opt/rpi/sysroot/usr/include --disable-multilib
make
make install DESTDIR=/opt/rpi/sysroot
```

### GCC 7.2.0 (fase final)

``` bash
mkdir -p /opt/rpi/build/gcc-7.2.0-final
cd /opt/rpi/build/gcc-7.2.0-final
../../src/gcc-7.2.0/configure --prefix=/opt/rpi --target=arm-linux-gnueabihf --enable-languages=c,c++ --disable-nls --disable-multilib --with-sysroot=/opt/rpi/sysroot
make
make install
```

## Enlaces
[how to build a gcc cross compiler](http://preshing.com/20141119/how-to-build-a-gcc-cross-compiler/)
[building gcc cross compiler raspberry pi](https://solarianprogrammer.com/2018/05/06/building-gcc-cross-compiler-raspberry-pi/)
