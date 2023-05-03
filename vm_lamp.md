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
![Apache2](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/Apache2.jpeg?raw=true)


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

![apache2.conf](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/apache2.conf.jpeg?raw=true)

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

## Installation et configuration de la base de données

installation de mysql8 en lieu et place de maria

![mysql](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/mysql.jpeg?raw=true)

```
wget https://repo.mysql.com/mysql-apt-config_0.8.23-1_all.deb
dpkg -i mysql-apt-config_0.8.23-1_all.deb 
apt update
apt upgrade
apt install mysql-server
```

test de mysql en cli

```
mysql -u root -p
```

## Installation et configuration du langage PHP7.X

installation php 7.4 sur deb 11

![php7.4](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/version_php.jpeg?raw=true)

```
apt install php
# édition du fichier php.ini pour modifier le upload max 
nano /etc/php/7.4/apache2/php.ini
```

faite une recherche de "filesize" dans "upload_max_filesize=2M", remplacer le 2 par 32.

```
service apache2 restart
```

création d’un fichier test dans /home/david/www/

```
nommé test.php avec le compte user
```

```
su david (user)
nano /home/david/www/test.php
```
![test.php](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/test_php1.jpeg?raw=true)

lancer un navigateur sur https://votreFQDN/test.php

```
apt install php-curl php-gd php-zip php-apcu php-xml php-ldap php-mbstring
apt install php-mysqlservice apache2 restart
```

![test php](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/test.php.jpeg?raw=true)

## Installation et configuration de PhpMyAdmin

Installe de PMA deb 11 

```
apt install phpmyadmin 
```

Pour changer l'alias, il faut aller dans "/etc/apache2/conf-enabled" puis "nano phpmyadmin.conf"

Remplacer  phpmyadmin par pma

![Alias](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/alias.jpeg?raw=true)

![phpmyadmin](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/phpmyadmin_pma.jpeg?raw=true)

Changement des droits root avec % et création d'un utlisateur

![root et user](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/root_et_user.jpeg?raw=true)

Pour crée un compte utilisateur, il faut être dans l'accueil puis "compte utilisateurs" puis "Ajouter un compte d'utilisateur" puis taper la commande pour recharger les privilèges

```
flush privileges
```
## Installation et configuration du service FTP

Installe du service ftp

```
apt install vsftpd
cp /etc/vsftpd.conf /etc/vsftpd.bak
rm /etc/vsftpd.conf
```

configuration du service avec chrootage et passv et ssl

```
nano /etc/vsftpd.conf
```

**Example config file /etc/vsftpd.conf**

```
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
#
# Run standalone?  vsftpd can run either from an inetd or as a standalone
# daemon started from an initscript.
listen=NO
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
listen_ipv6=YES
#
# Allow anonymous FTP? (Disabled by default).
#anonymous_enable=YES
#anon_root=/var/ftp/pub
#
# Uncomment this to allow local users to log in.
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
use_localtime=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
ftpd_banner=David Banner
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
chroot_local_user=YES
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
allow_writeable_chroot=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd.chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# Customization
#
# Some of vsftpd's settings don't fit the filesystem layout by
# default.
#
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
#
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
#
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
rsa_cert_file=/etc/apache2/apache.pem
rsa_private_key_file=/etc/apache2/apache.pem
ssl_enable=YES
pasv_enable=yes
pasv_min_port=65000
pasv_max_port=65500
```
```
service vsftpd restart
nmap 127.0.0.1
nmap 192.168.1.25
service vsftpd status
```
![vsftpd](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/vsftpd.jpeg?raw=true)

![filezilla](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/filezilla.jpeg?raw=true)

## Installation et configuration du partage SAMBA

```
mv /etc/samba/smb.conf /etc/samba/smb.bak
nano /etc/samba/smb.conf
```

```
[global]workgroup = home
netbios name = debian
comment = Serveur de fichier de thomas pour le lamps
interfaces = ens33encrypt
passwords = true
security = user
smb passwd file = /usr/bin/smb
passwdpassdb backend = tdbsamobey pam restrictions = yes
[www]
path = /home/david/www
valid users = david
writeable = Yes
create mask = 777
directory mask = 777
```
relancer les 2 services

```
service nmbd restart
service smbd restart
```

tester notre fichier de conf

```
testparm
```

chiffrer le user de la base de compte local Linux à la sauce Windows

```
smbpasswd -a user
```

**il n' y a plus qu'à tester via l'explorateur de fichier du genre**

![samba](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/Samba.jpeg?raw=true)

## Script de sauvegarde des bases de données

Le but est de créer une tâche automatique qui compresse en zip le dossier contenant les bases et qui le sauve dans un dossier à la date du jour ainsi que le contenu du dossier www.

```
cd /root					
touch backup.sh
chmod +x backup.sh 					
nano backup.sh
```

```
#!/bin/bash
#Script de backup auto de la base et du www pour le site user
#V1.1 par D.Ursulet mai 2023
clear
echo Compression : zip -rq /home/david/$(date +%Y%m%d%H%M)_www_backup-fichier.zip  /home/david/www
echo Dump de la base
mysqldump -u root -p'dadfba16' --databases user > /home/david/$(date +%Y%m%d%H%M)_www_backup-base.sql
echo Terminé
```
Installation du certificat dans Webmin

![certificat](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/certificat.jpeg?raw=true)

## Installation de Wordpress

```
cd /home/david
rm -R /home/david/www
wget https://fr.wordpress.org/latest-fr_FR.zip
unzip latest-fr_FR.zip
mv wordpress www

chown -R www-data:www-data /home/david/www
chmod -R 775 /home/david/www
```
![Wordpress](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/Worpress.jpeg?raw=true)

![wordpress](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/wordpress2.jpeg?raw=true)

Les ports

![les ports](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/les_ports.jpeg?raw=true)
