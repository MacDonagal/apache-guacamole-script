<p align="center"><a href="URL_REDIRECT" target="blank"><img align="center" src=Apache_Guacamole_logo.png height="150" /></a></p>


# Apache Guacamole Installation Script

J'ai repris le script d'installation de MysticRyuujin pour l'adapter aux dernières version d'Apache Guacamole

Attention, le(s) script(s) ne fonctionnent pas sous Debian 12..


## FIXES

Les fixs ci-dessous ne sont à utiliser si vous avez des problèmes

### Les utilisateurs ayant des problèmes sous **Ubuntu** avec le protocole RDP :
```
sudo add-apt-repository ppa:remmina-ppa-team/remmina-next
sudo apt-get update
sudo apt-get install freerdp2-dev freerdp2-x11
```
### Les utilisateurs ayant des problèmes sous **Debian** avec le protocole RDP :
```
sudo bash -c 'echo "deb http://deb.debian.org/debian buster-backports main" >> /etc/apt/sources.list.d/backports.list'
sudo apt update
sudo apt -y -t buster-backports install freerdp2-dev libpulse-dev
```



## Installation

Le script permet l'installation sous Ubuntu/Debian principalement ainsi que d'autres distribution (voir ci dessous pour la compatibilité)

```bash
 $ wget https://raw.githubusercontent.com/MacDonagal/apache-guacamole-script/main/guac-install-server.sh
 $ wget https://raw.githubusercontent.com/MacDonagal/apache-guacamole-script/main/guac-install.sh
 $ chmod +x guac-install-server.sh
 $ chmod +x guac-install.sh
 $ ./guac-install-server.sh
 $ ./guac-install.sh
```
Attention à installer le guac-server avant le webclient !

## MFA/2FA
Le script n'installe pas le 2FA, il faut modifier la variable `installTOTP=true` ou `installDuo=true`

## Upgrade
Il existe également un script pour mettre à jour Guacamole, ce dernier met à jour les extensions aussi.
    
## Variable d'environnement
Il est possible d'installer le script de façon non interactive

Exemple:
 `./guac-install.sh --mysqlpwd password --guacpwd password --nomfa --installmysql`

Install MySQL:

`-i or --installmysql`

Do NOT install MySQL:

`-n or --nomysql`

MySQL Host:

`-h or --mysqlhost`

MySQL Port:

`-p or --mysqlport`

MySQL Root Password:

`-r or --mysqlpwd`

Guacamole Database:

`-db or --guacdb`

Guacamole User:

`-gu or --guacuser`

Guacamole User Password:

`-gp or --guacpwd`

No MFA (No TOTP + Duo):

`-o or --nomfa`

Install TOTP:

`-t or --totp`

Install Duo:

`-d or --duo`

### NOTE: Only the switches for MySQL Host, MySQL Port and Guacamole Database are available in the upgrade script.



## PostInstall - Reverse Proxies
Make sure that you configure your reverse proxy (NGinx or Apache) as per the Official Documentation

For Nginx:
```
location /guacamole/ {
    proxy_pass http://HOSTNAME:8080/guacamole/;
    proxy_buffering off;
    proxy_http_version 1.1;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $http_connection;
    access_log off;
}
```
For Apache:
```
<Location /guacamole/>
    Order allow,deny
    Allow from all
    ProxyPass http://HOSTNAME:8080/guacamole/ flushpackets=on
    ProxyPassReverse http://HOSTNAME:8080/guacamole/
</Location>
```
## Authors

- [@MysticRyuujin](https://github.com/MysticRyuujin)


## Testé sur

Le script fonctionne sous:

- Ubuntu 22.04 LTS
- Debian 11

