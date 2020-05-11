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
git clone --depth=1 https://github.com/opencv/opencv_contrib
# La ruta extra_modules es la carpeta modules del repo opencv_contrib
-DCMAKE_INSTALL_PREFIX=/opt/opencv \
-DOPENCV_EXTRA_MODULES_PATH=/media/andres/5EEC0A85EC0A5823/opencv/opencv_contrib/modules \
-DBUILD_opencv_legacy=OFF \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
..
make -j4
make install
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

Para compilar es necesario la variable de entorno *OpenCV_DIR* apunte al directorio donde se encuentra el fichero *OpenCVConfig.cmake*
En mi caso se encuentra en la ruta siguiente:

```bash
export OpenCV_DIR=/media/andres/5EEC0A85EC0A5823/opencv/opencv-4.3.0/build/
```

También se puede añadir a CMakeList.txt, la ruta de instalación de opencv:

```bash
set("OPENCV_DIR", "/media/second_drive/projects/mhp/opencv_install")
```

Luego hacer:
```bash
cmake .
make
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
