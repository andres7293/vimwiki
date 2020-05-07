# opencv

[instructions](https://docs.opencv.org/4.3.0/d7/d9f/tutorial_linux_install.html)
[download opencv](https://opencv.org/releases/)

## build opencv

```bash
git clone --depth=1 https://github.com/opencv/opencv_contrib
# cd to opencv
mkdir build
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
mkdir build
cd build

#con opencv_contrib
#fijarse que la ruta extra_modules se refiere a la carpeta modules de opencv_contrib
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/opt/opencv -D OPENCV_EXTRA_MODULES_PATH=/media/andres/5EEC0A85EC0A5823/opencv/opencv_contrib/modules -D BUILD_opencv_legacy=OFF ..

#sin opencv_contrib
#cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/opt/opencv ..

#TESTS, DOCS OFF
cmake \
-DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/opt/opencv \
-DOPENCV_EXTRA_MODULES_PATH=/media/andres/5EEC0A85EC0A5823/opencv/opencv_contrib/modules \
-DBUILD_opencv_legacy=OFF \
-DBUILD_EXAMPLES=OFF \
-DBUILD_TESTS=OFF \
..
```

## cmake

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
