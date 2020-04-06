# Instalación ghdl

```bash
cd /opt/
git clone --depth=1 https://github.com/ghdl/ghdl
# Descargar compilador de Ada  https://www.adacore.com/download
wget http://mirrors.cdn.adacore.com/art/5b0d7bffa3f5d709751e3e04 -O gnat-community-2018-20180528-x86_64-linux-bin
sudo chmod +x gnat-community-2018-20180528-x86_64-linux-bin
#Instalar software en /opt. Solo instalar compilador de GNAT
./gnat-community-2018-20180528-x86_64-linux-bin
#Añadir al PATH
export PATH=$PATH:/opt/GNAT/2018/bin
#Compilar e instalar ghdl
./configure --prefix=/opt/ghdl/
make
make install
#Añadir binarios al PATH.
#Añadir binarios al bashrc para hacerlo accesible permanentemente en la consola
```

Error file format not recognized!!. 
