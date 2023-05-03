# 							VM Lamps locale Debian 11

## CONFIGURATION DEBIAN 11

- Mémoire 2 GB
- Processeur 1 GB
- Disque dur 30 GB
- Reseau Bridge
- IP: 192.168.1.25

### Utilitaires installés  (apt install)

ssh, nmap, zip, dnsutils, net-tools, tzdata, lynx, sudo, curl, git, screen,locate, ncdu

## WEBMIN

```
wget https://github.com/webmin/webmin/releases/download/2.021/webmin_2.021_all.deb
```

```
dpkg -i webmin_2.021_all.deb
```

```
apt -f install
```

```
apt install winbind samba
```
![webmin](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/Webmin.jpeg?raw=true)

je vérifie et modifie au cas ou les dns

```
resolv.conf
```
![resolv.conf](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/resolv.conf.jpeg?raw=true)
```
hosts
```
![hosts](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/nano%20hosts.jpeg?raw=true)
```
hostname
```
![hostname](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/nano%20hostname.jpeg?raw=true)
```
nsswitch.conf
```
![nsswitch.conf](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/nano%20nsswitch.conf.jpeg?raw=true)
```
date
```
![date](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/date.jpeg?raw=true)

## APACHE2

Installation d'apache2

```
apt install apache2
```

```
a2enmod ssl
a2ensite default-ssl
```

```
service apache2 restart
```

```
openssl req $@ -new -x509 -days 3650 -nodes -out /etc/apache2/apache.pem -keyout /etc/apache2/apache.pem
```

Pays: fr

Etat/Province: CVDL

Localité: Tours

Organisation: CEFIM

Unité organisationnelle: IT

Nom Courant: debian.home

Mail:

```
a2enmod rewrite
```

```
a2enmod headers
```

Nouveau certificat ssl dans la conf apache

```
cd /etc/apache2/sites-available/
```

```
nano default-ssl.conf 
```
![ssl](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/ssl.jpeg?raw=true)

```
nano default-ssl.conf 
```

```
service apache2 restart
```

Redirection et changement du documentroot

<VirtualHost *:80>

​					Redirect Permanent/https://debian.home

<VirtualHost *>
  


```
nano /etc/apache2/sites-available/default-ssl.conf 
```

DocumentRoot /home/david/www

```
nano /etc/apache2/apache2.conf
```

commenter

<Directory /var/www/html>

​	option Indexes FollowsSymlink

​	AllowOverride None

​	Require all granted

<Directory*>

Création en mode user du dossier www et d’un index dans /home/user/

### 

```
su david
```

```
mkdir /home/david/www
nano /home/david/www/index.html (créer du contenu pipo dans votre index)
```

changement récursif de propriétaire et c'est apache le nouveau proprio

```
chown -R www-data:www-data /home/user/www
usermod -a -G www-data user
```

changement récursif des droits pour le dossier www pour ftp et samba

```
chmod -R 775 /home/user/www
```

fusion des 2 fichiers de conf 

```
cd /etc/apache2/sites-available
cat 000-default.conf >site.conf
cat default-ssl.conf >>site.conf
```

désactivation des 2 fichiers de conf qui ne vont plus servir

```
a2dissite 000-default
a2dissite default-ssl
```

effacement de ces 2 fichiers

```
rm 000-default.conf
rm default-ssl.conf
```

activation de la nouvelle et unique conf

```
a2ensite site.conf
service apache2 restart
```
![site.conf](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/site-conf.jpeg?raw=true)

![HTML](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/site_html.jpeg?raw=true)






![légende]()
