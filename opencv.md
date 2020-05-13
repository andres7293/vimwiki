# opencv

[instructions](https://docs.opencv.org/4.3.0/d7/d9f/tutorial_linux_install.html)

[download opencv](https://opencv.org/releases/)

## Compilar opencv

Instalar paquetes necesarios:

```bash
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
```

```bash
wget https://github.com/opencv/opencv/archive/4.3.0.zip -O opencv-4.3.0.zip
unzip opencv-4.3.0.zip
cd opencv-4.3.0/
mkdir build

cmake \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/opt/opencv \
-DBUILD_opencv_legacy=OFF \
..
make -j4
make install
```

### opencv_contrib

**Es necesario recompilar todo opencv para añadir opencv_contrib**
```bash
rm -rf opencv-4.3.0/build
```

```bash
git clone --depth=1 https://github.com/opencv/opencv_contrib
# La ruta extra_modules es la carpeta modules del repo opencv_contrib
cd opencv-4.3.0
mkdir build
cmake \
-DCMAKE_TOOLCHAIN_FILE=../trd4-opencv.cmake \
-DCMAKE_BUILD_TYPE=Release \
-DBUILD_opencv_legacy=OFF \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
-DBUILD_opencv_java=OFF \
-DBUILD_opencv_python=OFF \
-DCMAKE_INSTALL_PREFIX=/opt/rpi/sysroot \
-DWITH_FFMPEG=OFF
-DOPENCV_EXTRA_MODULES_PATH=/opt/rpi/src/opencv_contrib/modules/ \
..
make -j4
make install
```

### compilar opencv soporte para python

[doc python opencv](https://docs.opencv.org/4.3.0/d2/de6/tutorial_py_setup_in_ubuntu.html)

Paquetes previos

```bash
sudo apt-get install cmake g++ gcc make
```

dependencias
```bash
sudo apt-get install python3-dev python3-numpy
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
#optional
sudo apt-get install libpng-dev
sudo apt-get install libjpeg-dev
sudo apt-get install libopenexr-dev
sudo apt-get install libtiff-dev
sudo apt-get install libwebp-dev
```

Compilar opencv
```bash
git clone --depth=1 https://github.com/opencv/opencv.git
cd opencv
mkdir build
cd build
cmake \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
..
make -j3
make install
```

Probar compilación desde python:
```bash
import cv2 as cv
print(cv.__version__)
```

### Compilación cruzada

[cmake cross compile](https://cmake.org/cmake/help/latest/manual/cmake-toolchains.7.html#cross-compiling-for-linux)

#### libav

```bash
wget https://libav.org/releases/libav-12.3.tar.xz
xz -d libav-12.3.tar.xz
tar xf libav-12.3.tar
cd libav-12.3/
. /opt/rpi/src/entorno.sh
./configure \
--prefix=/opt/rpi/sysroot   \
--sysroot=/opt/rpi/sysroot  \
--enable-cross-compile  \
--enable-shared \
--target-os=linux   \
--arch=arm  \
--cc=arm-linux-gnueabihf-gcc    \
```

#### ffmpeg

```bash
wget https://ffmpeg.org/releases/ffmpeg-4.2.2.tar.bz2
tar xf ffmpeg-4.2.2.tar.bz2
cd ffmpeg-4.2.2/
 ./configure \
 --prefix=/opt/rpi/sysroot \
 --sysroot=/opt/rpi/sysroot \
 --enable-cross-compile \
 --enable-shared \
 --target-os=linux \
 --arch=arm \
 --cc=arm-linux-gnueabihf-gcc \
 --strip=arm-linux-gnueabihf-strip
 make -j4
 make install
```

#### opencv-4.3.0

Crear fichero trd4-opencv.cmake en la ruta dentro de la carpeta opencv-4.3.0

```bash
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)
set(CMAKE_SYSROOT /opt/rpi/sysroot)
set(CMAKE_STAGING_PREFIX /opt/rpi/sysroot)
set(tools /opt/rpi/bin)
set(CMAKE_C_COMPILER ${tools}/arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER ${tools}/arm-linux-gnueabihf-g++)
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```

```bash
wget https://github.com/opencv/opencv/archive/4.3.0.zip -O opencv-4.3.0.zip
unzip opencv-4.3.0.zip
cd opencv-4.3.0/
mkdir build
cd build
cmake \
-DCMAKE_TOOLCHAIN_FILE=../trd4-opencv.cmake \
-DCMAKE_BUILD_TYPE=Release \
-DBUILD_opencv_legacy=OFF \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
-DBUILD_opencv_java=OFF \
-DBUILD_opencv_python=OFF \
-DCMAKE_INSTALL_PREFIX=/opt/rpi/sysroot \
-DWITH_FFMPEG=OFF
..
make -j4
make install
```

##### opencv con soporte ffmpeg

```bash
#actualizar pkgconfig
export LD_LIBRARY_PATH=/opt/rpi/sysroot/lib/pkgconfig/
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/opt/rpi/sysroot/lib/pkgconfig/
export PKG_CONFIG_LIBDIR=$PKG_CONFIG_LIBDIR:/opt/rpi/sysroot/lib/pkgconfig/
cd opencv-4.3.0/build
cmake \
-DCMAKE_TOOLCHAIN_FILE=../trd4-opencv.cmake \
-DCMAKE_BUILD_TYPE=Release \
-DBUILD_opencv_legacy=OFF \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
-DBUILD_opencv_java=OFF \
-DBUILD_opencv_python=OFF \
-DCMAKE_INSTALL_PREFIX=/opt/rpi/sysroot \
-DWITH_FFMPEG=ON
..
make 
make install
```

## Compilar código usando opencv

[CMakeLists.txt](https://docs.opencv.org/4.3.0/db/df5/tutorial_linux_gcc_cmake.html)

Para compilar es necesario la variable de entorno *OpenCV_DIR* apunte al directorio donde se encuentra el fichero *OpenCVConfig.cmake*

Se puede especificar de dos formas:
```bash
#1. Añadir a CMakeLists.txt
set("OPENCV_DIR", "<ruta al directorio de OpenCVConfig.cmake>")
#2. Por variables de entorno
export OpenCV_DIR=<ruta al directorio de OpenCVConfig.cmake>

#Luego hacer
cmake . && make
```

### Compilar código usando opencv de forma cruzada.

Aplican las mismas reglas que las vista anteriormente:

1. Especificar la ruta de instalación de opencv.
2. Indicar la ruta del toolchain file (Donde se especifica la ruta nuestro compilador, sysroot, etc)
3. Navegar hasta el directorio donde se encuentre CMakeLists.txt


```bash
export OpenCV_DIR=<ruta al directorio de OpenCVConfig.cmake>
cmake -DCMAKE_TOOLCHAIN_FILE=../trd4-opencv.cmake .
make
```

Nota:
Si se está compilando de forma cruzada y nativa, es mejor borrar la caché de cmake para evitar errores.

```bash
rm -rf CMakeCache.txt CMakeFiles cmake_install.cmake
```


## entorno.sh

```bash
#!/bin/bash
export PATH=/opt/rpi/bin:${PATH}
export CC=arm-linux-gnueabihf-gcc
export CXX=arm-linux-gnueabihf-g++
export AR=arm-linux-gnueabihf-ar
export AS=arm-linux-gnueabihf-as
export RANLIB=arm-linux-gnueabihf-ranlib
export LD=arm-linux-gnueabihf-ld
export STRIP=arm-linux-gnueabihf-strip
```
