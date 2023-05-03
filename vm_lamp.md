# 							VM Lamps locale Debian 11

## CONFIGURATION DEBIAN 11

- Mémoire 2 GB
- Processeur 1 GB
- Disque dur 30 GB
- Reseau Bridge

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
![webmin](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/Webmin.jpeg)

je vérifie et modifie au cas ou les dns

```
resolv.conf
```
![resolv.conf](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/resolv.conf.jpeg)
```
hosts
```
![hosts](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/nano%20hosts.jpeg)
```
hostname
```
![hostname](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/nano%20hostname.jpeg)
```
nsswitch.conf
```
![nsswitch.conf](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/nano%20nsswitch.conf.jpeg)
```
date
```
![date](https://github.com/baaldees/David-ursulet-VM-Lamps-localeDebian-11/blob/main/image_linux/date.jpeg)

![légende]()
