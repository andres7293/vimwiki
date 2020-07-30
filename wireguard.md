# WireGuard

Kernel VPN server

## Raspberry

### Instalar wireguard desde las fuentes

```bash
cd /opt/
git clone https://git.zx2c4.com/WireGuard
cd WireGuard/src
make
make install
```

### Configuración raspberry

```bash
#Enable ip forwarding
perl -pi -e 's/#{1,}?net.ipv4.ip_forward ?= ?(0|1)/net.ipv4.ip_forward = 1/g' /etc/sysctl.conf
reboot
#Verify. Output should be 1
sysctl net.ipv4.ip_forward 

cd /etc/wireguard
umask 077
#generate keys
```
