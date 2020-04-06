# Compilación GCC x86

[building gcc ubuntu linux](https://solarianprogrammer.com/2016/10/07/building-gcc-ubuntu-linux/)

```bash
# Descargar versión de GCC de : <https://gcc.gnu.org/releases.html>
mkdir gcc-XX
cd gcc-XX
mkdir src
mkdir build
cd src
# wget https://gcc.gnu.org/gcc-8/
cd gcc-8
# Descargar mpfr mpc gmp
contrib/download_prerequisites
cd ../../build
../src/gcc-8.1.0/configure --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu 
prefix=/opt/gcc-XX --enable-languages=c,c++ --disable-multilib
make -j4
make install
```

*GCC debe ser construido fuera del directorio principal. Es necesario crear un directorio y generar el ejecutar configure en ese directorio.*
