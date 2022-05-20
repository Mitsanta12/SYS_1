# VSFTPD


## I/ Qu’est-ce que VSFTPD ? <br>
<p>
VsFTPd est un serveur FTP(File Transfer Protocol) conçu avec la problématique d'une sécurité maximale. Contrairement aux autres serveurs FTP (ProFTPd, PureFTPd, etc.),
aucune faille majeure de sécurité n'a jamais été décelée dans VsFTPd.
</p> 

## II/ Comment installer VSFTPD?

<h4> Étape 1 —  Installer VSFTPD </h4>
      - Comme tous les autres, commencez par mettre à jour l'index des packages apt:
       
    sudo apt update
    

  Installez le package Samba avec :
  
    sudo apt install vsftpd -y
    
Pour vérifier si l'installation du serveur est réussi, tapez :
  
    sudo systemctl status vsftpd

Pour redémarrer le service, utilisez la commande:
       
    sudo systemctl restart vsftpd
    

<h4> Étape 2— Installation de FileZila </h4>
  Afin d'y parvenir au partage de fichier, il est nécessaire d'utiliser un application comme FileZila, 
  qui est une application FTP multiplateforme gratuite et open source, composée de FileZilla Client et FileZilla Server.:
  
 1/ Installation de fileZila :  
  - vous pouriez télécharger gratuitement fileZila dans <a href="https://filezilla-project.org/">download fileZila</a>
    
  2/ Configuration de fileZila :
  <p align="left">
  <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/fileZila_config_1.jpg" />
</p>

<p align="left">
  <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/fileZila_config_2.jpg" />
</p>

<p align="left">
  <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/fileZila_config_3.jpg" />
</p>

<p align="left">
  <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/fileZila_config_4.jpg" />
</p>


3/ Ajout d'un nouveau utilisateur:
Pour ajouter un utilisateur, tapez:
```
sudo adduser ftpuser
```

Pour créer un nouveau dossier pour l'utilisateur, saisir la commande:
```
sudo mkdir /home/ftpuser/ftp
sudo mkdir /home/ftpuser/ftp/fichier
```

Pour changer la permission et la propriétaire:
  
    sudo chown nobody:nogroup /home/ftpuser/ftp 
    sudo chmod a-w /home/ftpuser/ftp
    sudo chown ftpuser:ftpuser /home/ftpuser/ftp/fichier
    
Pour vérifier si l'installation du serveur est réussi, tapez :
  
    sudo systemctl status vsftpd

Pour redémarrer le service, utilisez la commande:
       
    sudo systemctl restart vsftpd
    
<h4> Étape 3 —  Configuration du serveur VSFTPD </h4>
Commencez par créer un backup pour le ficher vsftpd.conf:
       
    sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
    

  Puis modifiez ce ficher avec :
  
    sudo nano /etc/vsftpd.conf
    
Et pour finir,vérification et redémarage du serveur vsftpd avec la commande :
  
    sudo systemctl status vsftpd
    sudo systemctl restart vsftpd

<h4> Étape 4 —  Dérnière étape,test du serveur VSFTPD </h4>
On va essayer de transférer un fichier provenant d'un utilisateur externe vers l'utilisateur admin:
       
<p align="left">
 <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/vsftpd_test_1.jpg" />
 </p>
    

  Les fichiers a transferé sont:
  
    Dossier win10 
    Cours online.txt
    fichier_via_windows.txt 
 
 <p align="left">
  <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/vsftpd_test_2.jpg" />
</p>

Maintenant, si on vérifie les fichiers dans le dossier de destination, les fichiers transferés devront être dans la destination.
```
$sudo cd /home/ftpuser/ftp/fichier
$/home/ftpuser/ftp/fichier# ls -al
```
<p align="left">
  <img src="https://github.com/Herizoran/SYS1/blob/main/vsftpd/images/vsftpd_test_3.jpg" />
</p>


## MAINTENANT C'EST A VOUS DE JOUER!
