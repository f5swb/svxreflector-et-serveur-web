1 - installation de svxlink/svxreflector : 

cd /usr/src

wget https://github.com/sm0svx/svxlink/archive/19.09.1.tar.gz

tar -xzf 19.09.1.tar.gz -C /usr/src/

rm 19.09.1.tar.gz

wget -O - ftp://ftp.f5nlg.ovh/DEV/SERVEUR_17.12/ReflectorClient.cpp > /usr/src/svxlink-19.09.1/src/svxlink/reflector/ReflectorClient.cpp

wget -O - ftp://ftp.f5nlg.ovh/DEV/SERVEUR_17.12/ModuleMetarInfo.cpp > /usr/src/svxlink-19.09.1/src/svxlink/modules/metarinfo/ModuleMetarInfo.cpp

mkdir /usr/src/svxlink-19.09.1/src/build && cd /usr/src/svxlink-19.09.1/src/build

useradd -r -g daemon svxlink

groupadd svxlink

gpasswd -a svxlink svxlink

gpasswd -a svxlink gpio

gpasswd -a svxlink audio

cmake -DCMAKE_INSTALL_PREFIX=/usr -DSYSCONF_INSTALL_DIR=/etc -DLOCAL_STATE_DIR=/var -DUSE_OSS=NO -DUSE_QT=NO -RELEASE_CFLAGS=-g -O2 -mfloat-abi=softfp -mfpu=vfp -mcpu=native ..

make doc

make install

make clean

rm -rf /usr/src/svxlink-19.09.1/src/build

ldconfig

2 - Lancement automatique au démarrage de svxreflector :

nano /etc/rc.local 
svxreflector –config=/etc/spotnik/svxreflector.conf –daemon –logfile=/tmp/svxreflector.log
sleep 1

ensuite on edite le fichier svxreflector afin de rendre compatible le format de l’heure avec le dashboard : 

nano /etc/svxreflector.conf
###################################################################
#
# Configuration file for the SvxReflector SvxLink conference node
#
###################################################################

[GLOBAL]
#CFG_DIR=svxreflector.d
TIMESTAMP_FORMAT="%d.%m.%Y %H:%M:%S"
LISTEN_PORT=5300
SQL_TIMEOUT=120
SQL_TIMEOUT_BLOCKTIME=30
CODECS=OPUS
AUTH_KEY=motdepasse

#[USERS]
#[PASSWORDS]

3 - Installation d’Apache :
sudo apt update
sudo apt upgrade
sudo apt update
sudo apt install apache2

droits au dossier d’apache : 
sudo chown -R pi:www-data /var/www/html/
sudo chmod -R 770 /var/www/html/

Vérifier qu’Apache fonctionne
wget -O verif_apache.html http://127.0.0.1
cat ./verif_apache.html
Si vous voyez marqué à un endroit dans le code « It works! », c’est qu’Apache fonctionne.

4 - Installation de PHP :
On va encore une fois faire appel à l’administrateur pour installer PHP avec la ligne de commande.
sudo apt install php php-mbstring
Vérifier que PHP fonctionne
sudo rm /var/www/html/index.html
Puis créez un fichier « index.php » dans ce répertoire, avec cette ligne de commande
echo "<?php phpinfo(); ?>" > /var/www/html/index.php

5 - Installation du dashboard en français :

cd /var/www
sudo rm /var/www/html/index.html

git clone https://github.com:f5swb/html.git

sudo /etc/init.d/apache2 restart

ensuite se rendre sur la page web https://l’adresse.ip.de.mon.raspberry

vous devriez voir le dashboard s’afficher 

6 - coté client : 

nano /etc/spotnik/restart.reg

# copie du host pour le reflector
echo « https://l’adresse.ip.de.mon.raspberry.serveur» >>/etc/spotnik/svxlink.reg
echo « AUTH_KEY=Motdepassedesvxreflector.conf » >>/etc/spotnik/svxlink.reg
echo « PORT=5300 » >>/etc/spotnik/svxlink.reg
