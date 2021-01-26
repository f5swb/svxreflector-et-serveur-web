installation du serveur apache et php 

sudo apt update
sudo apt upgrade

Installer Apache

La première étape sera d’installer Apache sur votre Raspberry Pi
Pour ce faire, utilisez apt de cette manière :
sudo apt install apache2

Trouvez l’adresse IP de votre Raspberry Pi
Ouvrez votre navigateur et rendez-vous à la page http://IP
Par exemple, http://192.168.1.18

les pages web sont à placer dans le dossier /var/www/html

Pour installer PHP, vous aurez besoin de 2 paquets supplémentaires :
sudo apt install php libapache2-mod-php

cd /var/www/html
sudo nano test.php
<?php
phpinfo();
?>

http://IP/test.php

Virtual hosts

Si vous avez besoin d’héberger plusieurs sites ou applications sur votre Raspberry Pi, il existe deux possibilités :

    Créer des sous dossiers dans /var/www/html et y accéder via une url qui inclue le dossier en question
    Par exemple : http://IP/dossier1 et http://IP/dossier2
    Ou alors utiliser les virtual hosts de Apache pour avoir des adresses plus propres comme http://app1.mondomaine.com et http://app2.mondomaine.com

Si vous souhaitez utiliser les virtual hosts, vous aurez besoin de :

    Créer les sous-domaines dans votre serveur DNS ou dans le fichier hosts de votre ordinateur
    Puis créer les virtual hosts dans la configuration Apache, un pour chaque sous-domaine

La configuration est à faire dans le dossier /etc/apache2/sites-enabled
