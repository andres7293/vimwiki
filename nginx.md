# Nginx

nginx is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy server.

## Contraseña

Protect http routes with password. 
It is needed htpassword command from apache server.

```bash
pacman -S apache2-utils
# the user and password can be whatever you want
htpasswd -c /etc/nginx/htpasswd nginx
```

nginx.conf
```bash
auth_basic "Protected route";
auth_basic_user_file /etc/nginx/htpasswd;
```
