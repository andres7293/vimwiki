# Compilar openocd

```bash
apt-get install make libtool pkg-config autoconf automake texinfo libusb-1.0
cd /opt
git clone https://git.code.sf.net/p/openocd/code
cd /openocd-code/
./bootstrap
./configure --pregix=/opt/openocd-code
make 
make install
```

[Building OpenOCD from Sources for Linux — ESP-IDF Programming Guide v3.2-dev-1250-g221eced documentation](https://docs.espressif.com/projects/esp-idf/en/latest/api-guides/jtag-debugging/building-openocd-linux.html)
