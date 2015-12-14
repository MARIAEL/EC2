###Indice
  - [Install webmin](https://github.com/manviny/EC2/blob/master/README.md#install-webmin-on-ubuntu-ec2-httpsip10000)  
  - [Incrementar Swap](https://github.com/manviny/EC2/blob/master/README.md#incrementar-swap)  
  - [Instalar Odoo](https://github.com/manviny/EC2/blob/master/README.md#instalar-odoo-9) 
  - [Instalar Dreamfactory](https://github.com/manviny/EC2/blob/master/README.md#instalar-dreamfactory)
  - [Arrancar aplicaciones al reinicializar el servidor](https://github.com/manviny/EC2/blob/master/README.md#inicializa-aplicaciones-on-reboot)
  - [Instalar aws-cli](https://github.com/manviny/EC2/blob/master/README.md#instalar-aws-cli)
  - [Automated ec2 backups](https://github.com/manviny/EC2/blob/master/README.md#instalar-aws-cli)
  
  
  


## Connect to DB through localhost

```sh
#MySQL y RockMongo mediante conexión puente


sudo ssh -N -L 8888:127.0.0.1:80 -i ~/.ssh/bitnami-hosting.pem bitnami@deployd.bitnamiapp.com

acceder a las bases de datos
http://127.0.0.1:8888/rockmongo/
http://127.0.0.1:8888/phpmyadmin/
```

## First steps  
Si disponemos de una máquina pequeña (1GB RAM) -> incrementar swap
```sh
# (Opción A) Install UBUNTU LAMP server 
sudo apt-get update
sudo apt-get upgrade
sudo tasksel  #select LAMP

# (Opción B) Install BITNAMI LAMP server 
wget Lamp stack
...

# (Opción C) Install BITNAMI LAMP server 
# Seleccionar una AMI con bitnami LAMP


# install webmin / virtualmin
```

mirar crontab de dfodoo  
Probar instalar por ej codiad y si funciona seguir ya con dreamfactory en puerto 8080  



[volver](https://github.com/manviny/EC2/blob/master/README.md#indice)
##Conectar con el servidor
sudo ssh -i ~/.ssh/ec2.pem ubuntu@1.2.3.4  
Abrir puertos 80, 8080 y 8069  


##INCREMENTAR SWAP
[fuente](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

sudo nano /etc/fstab   #paste
    /swapfile   none    swap    sw    0   0
    
sudo nano /etc/sysctl.conf 
    vm.swappiness=10
    vm.vfs_cache_pressure=50

```


##Inicializa aplicaciones "on reboot"

Crear script starter.sh (en un sitio seguro, /root/blabla, hacerlo ejecutable )  
```bash
nano starter.sh
sudo chmod +x starter.sh

#Pegar este código
#!/bin/sh

#Inicializar deployd (se debe usar cada vez que se apaga el servidor)
if [ $(ps -e -o uid,cmd | grep $UID | grep node | grep -v grep | wc -l | tr -s "\n") -eq 0 ]
then
        export PATH=/opt/bitnami/nodejs/bin:$PATH
        forever start --sourceDir /home/bitnami/apps/deployd/CATIC production.js >> /home/bitnami/log.txt 2>&1
        #/opt/bitnami/nodejs/bin/forever start /home/bitnami/apps/deployd/CATIC/production.js  2>&1 > /dev/null
fi

#Inicializar DreamFactory
sudo -s /opt/dreamfactory-2.0.2-0/ctlscript.sh start

```
Crear un cron para ejecutar el script anterior cada vez que se apaga el servidor
```bash
sudo crontab -e
```
añadir
```bash

# Sync files to S3
#26 22 * * * cd /home/bitnami/apps/assets
#27 22 * * * s3cmd sync -r --delete-removed --acl-private files s3://bucket/assets/

# ejecuta tareas
@reboot /root/blabla/starter.sh
```

##Instalar Odoo 9
[fuente](http://toolkt.com/site/installing-odoo-9-in-ubuntu-14-04/)

```sh
sudo nano odoo-install.sh
    # PEGAR EL CONTENIDO DE -> https://github.com/Yenthe666/InstallScript/blob/9.0/odoo_install.sh
sudo chmod +x odoo-install.sh
sudo ./odoo-install.sh
```
[volver](https://github.com/manviny/EC2/blob/master/README.md#indice)

##Instalar Dreamfactory
[fuente](https://bitnami.com/stack/dreamfactory/installer)  

```sh
wget https://bitnami.com/redirect/to/83996/bitnami-dreamfactory-2.0.2-0-linux-x64-installer.run
chmod +x bitnami-dreamfactory-2.0.2-0-linux-x64-installer.run
sudo ./bitnami-dreamfactory-2.0.2-0-linux-x64-installer.run
```
[volver](https://github.com/manviny/EC2/blob/master/README.md#indice)

##Instalar AWS CLI
[fuente](https://alestic.com/2013/08/awscli/)   
```sh
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install -y python-pip  # sino lo tenemos instalado
sudo pip install awscli

aws configure
# AWS Access Key ID [None]: id
# AWS Secret Access Key [None]: llave
# Default region name [None]: zona si la zona es por ej eu-west-1a poner sin la última letra -> eu-west-1
# Default output format [None]:json
```
##Automated ec2 backups
[fuente](http://www.webmoves.net/blog/build/simple-automated-snapshots-of-multiple-ebs-volumes-3102/)    
[fuente mejor](https://www.drunksysadmin.com/automated-amazon-ebs-snapshots/) 


```sh
# Descarga ec2-automate-backup.sh
wget https://raw.githubusercontent.com/colinbjohnson/aws-missing-tools/master/ec2-automate-backup/ec2-automate-backup.sh
sudo chmod +x ec2-automate-backup.sh


$ crontab -e  #pegar las siguientes lineas

    PATH=/opt/someApp/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/root/blabla
    SHELL=/bin/bash
    39 13 * * * /home/ubuntu/aws-missing-tools/ec2-automate-backup.sh -r "eu-west-1" -v "vol-******"      #Crea backup






# Borra volumenes con mas de 7 dias que esten etiquetados como Backup true  
$ ./ec2-automate-backup.sh -r eu-west-1 -s tag -t 'Backup=true' -k 7 -p -n  

# Crea copia  
./ec2-automate-backup.sh -r "eu-west-1" -v "vol-******" 

#  Cron
"0 0 * * 0 ubuntu /home/ubuntu/aws-missing-tools/ec2-automate-backup.sh -s tag -t "Backup=true" > /home/ubuntu/aws-missing-tools/ec2-automate-backup_`date +"%Y%m%d"`.log"


/home/ubuntu/aws-missing-tools/cron-primer.sh


./ec2-automate-backup.sh -r "eu-west-1" -s tag -t "Backup=true" > ./ec2-automate-backup_`date +"%Y%m%d"`.log 
./ec2-automate-backup.sh -r eu-west-1 -s tag -t 'Backup=true' -k 7 -p -n -c /root/bin/cron-primer.sh  
0 22 * * * /root/bin/ec2-automate-backup.sh -r eu-west-1 -s tag -t 'Backup=true' -k 7 -p -n -c /root/bin/cron-primer.sh  
./ec2-automate-backup.sh -r "eu-west-1" -v "vol-******"  
```

## Convert snapshot to server



## Install webmin on ubuntu EC2 (https://ip:10000)  
[webmin](http://xcruft.com/content/install-webmin-ubuntu-1404-using-aws-ec2-free-tier)  
[virtualmin](http://thebroodle.com/web-control-panels/virtualmin/how-to-install-virtualmin-on-a-vps-running-ubuntu-14-04/#arvlbdata) 
```sh
#WEBMIN
#======
# open port 1000
Type: Custom TCP Rule
Protocol: TCP
Port Range: 10000
Source: (Select "My IP) - Makes it work only from your current location.

# install
 wget http://prdownloads.sourceforge.net/webadmin/webmin_1.770_all.deb  
 sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions python
 sudo dpkg --install webmin_1.770_all.deb
 
# add user  "adminuser"
sudo adduser adminuser
sudo usermod -a -G sudo adminuser

# VIRTUALMIN
============
# si tenemos instalado bitnami LAMP cambiar:
# /etc/webmin/mysql/config y /etc/webmin/apache/config 
# siguiendo los pasos de mas abajo
# Reiniciar servidor
sudo wget http://software.virtualmin.com/gpl/scripts/install.sh
sudo chmod +x install.sh
sudo ./install.sh
```
### WEBMIN + BITNAMI LAMP
/etc/webmin/mysql/config
```cfg
webmin_subs=0
login=root
date_subs=0
max_text=1000
perpage=25
stop_cmd=/etc/init.d/bitnami stop mysql >/dev/null 2>&1
mysqldump=/opt/bitnami/mysql/bin/mysqldump
nodbi=0
mysql_libs=/opt/bitnami/mysql/lib
max_dbs=50
start_cmd=/etc/init.d/bitnami start mysql >/dev/null 2>&1 &
mysql_data=/var/lib/mysql
mysqlimport=/opt/bitnami/mysql/bin/mysqlimport
access=*: *
style=0
my_cnf=/opt/bitnami/mysql/my.cnf
mysqlshow=/opt/bitnami/mysql/bin/mysqlshow
mysql=/opt/bitnami/mysql/bin/mysql
nopwd=0
add_mode=1
passwd_mode=0
blob_mode=0
mysqladmin=/opt/bitnami/mysql/bin/mysqladmin
```
/etc/webmin/apache/config  
```cfg
allow_virtualmin=0
defines_name=APACHE_ARGUMENTS
link_dir=/opt/bitnami/apache2/conf/sites-enabled
test_manual=0
show_list=0
mime_types=/opt/bitnami/apache2/conf/mime.types
access_conf=/opt/bitnami/apache2/conf/access.conf
auto_mods=1
stop_cmd=/etc/init.d/bitnami stop apache
virt_file=/opt/bitnami/apache2/conf/sites-available
test_apachectl=1
max_servers=100
srm_conf=/opt/bitnami/apache2/conf/srm.conf
httpd_dir=/opt/bitnami/apache2
start_cmd=/etc/init.d/bitnami start apache
show_order=0
test_always=0
httpd_conf=/opt/bitnami/apache2/conf/httpd.conf
defines_file=/opt/bitnami/apache2/bin/envvars
apachectl_path=/opt/bitnami/apache2/bin/apachectl
show_names=0
test_config=1
apply_cmd=/opt/bitnami/apache2/bin/apachectl graceful
httpd_path=/opt/bitnami/apache2/bin/httpd
```
 [volver](https://github.com/manviny/EC2/blob/master/README.md#indice)

