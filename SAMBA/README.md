# SAMBA

## I/ Qu’est-ce que SAMBA ? <br>
<p>
Samba est une réimplémentation gratuite et open-source du protocole de partage de 
fichiers réseau SMB / CIFS qui permet aux utilisateurs finaux d'accéder aux fichiers, imprimantes et autres ressources partagées.
</p> 

<br>
<p>
Samba est un logiciel d'interopérabilité qui implémente le protocole propriétaire SMB/CIFS de Microsoft Windows dans les ordinateurs tournant sous le système d'exploitation Unix et ses dérivés de
manière à partager des imprimantes et des fichiers dans un réseau informatique.(Source : Wikipedia)
</p>
<br>

<p>
Samba est un ensemble d'outils permettant de partager des ressources telles que des imprimantes
et des fichiers sur un réseau hétérogène. (C’est un serveur fichier)

Un serveur fichier : Un serveur de fichiers permet de partager des données à travers un réseau.
</p>

## II/ Comment installer Samba sur Ubuntu?

<h4> Étape 1 —  Installer SAMBA </h4>
      - Commencez par mettre à jour l'index des packages apt:
       
    sudo apt update
    

  Installez le package Samba avec :
  
    sudo apt install samba
    
> **Note** Une fois l'installation terminée, le service Samba démarre automatiquement. Pour vérifier si le serveur Samba fonctionne, tapez:
       
    sudo systemctl status smbd
    

 <h4> Étape 2 — Configuration du pare-feu </h4>
      vous pouvez ouvrir les ports en activant le profil 'Samba',si vous utilisez UFW pour gérer votre pare-feu:
       
    sudo ufw allow 'Samba
    

<h4> Étape 3 — Configuration des options globales de Samba </h4>
  Avant de modifier le fichier de configuration Samba, créez tout d'abord sauvegarde pour référence future: :
  
    sudo cp /etc/samba/smb.conf{, .backup}
    
Le fichier de configuration par défaut fourni avec le package Samba est configuré pour le serveur Samba autonome. 
Ouvrez le fichier et assurez-vous que server role est défini sur standalone server:
       
    sudo nano /etc/samba/smb.conf /etc/samba/smb.conf
 
... # Most people will want "standalone sever" or "member server". # Running as "active directory domain controller" will require first
#running "samba-tool domain provision" to wipe databases and create a # new domain. server role = standalone server ... 
    
  Si vous souhaitez restreindre l'accès au serveur Samba uniquement à partir de votre réseau interne,
 décommentez les deux lignes suivantes et spécifiez les interfaces à lier :
       
    /etc/samba/smb.conf
    
Une fois cela fait, exécutez l'utilitaire testparm pour rechercher des erreurs dans le fichier de configuration Samba.
S'il n'y a pas d'erreurs de syntaxe, vous verrez Loaded services file OK.    

  Pour finir,  redémarrez les services Samba avec: :
  
    sudo systemctl restart smbd sudo systemctl restart nmbd
    
<h4> Étape 4 — Création d'utilisateurs Samba et d'une structure de répertoires </h4>
  Pour faciliter la maintenance et la flexibilité au lieu d'utiliser les répertoires de base standard ( /home/user ), tous les répertoires et données Samba seront situés dans le répertoire /samba .
  - Création d'un répertoire /samba :
  
    sudo mkdir /samba
    
Définissez la propriété du groupe sur sambashare . Ce groupe est créé lors de l'installation de Samba, nous ajouterons plus tard tous les utilisateurs Samba à ce groupe:
       
    sudo chgrp sambashare /samba
 
<p>Samba utilise des utilisateurs Linux et un système d'autorisation de groupe mais possède son propre mécanisme d'authentification distinct de l'authentification Linux standard.
Nous allons créer les utilisateurs à l'aide de l'outil Linux useradd standard, puis définir le mot de passe utilisateur avec l'utilitaire smbpasswd . </p> 
<br>
<p>Maintenant  nous allons créer un utilisateur régulier qui aura accès à son partage de fichiers privé et à un compte administratif avec un accès en lecture et 
en écriture à tous les partages sur le serveur Samba.</p>

<h4> Étape 5 — Création d'utilisateurs Samba </h4>
  Pour créer un nouvel utilisateur tapez la commande suivante (on va nommé l'utilisateur M.ANDRIAH) :
  
    sudo useradd -M -d /samba/M.ANDRIAH -s /usr/sbin/nologin -G sambashare M.ANDRIAH
    

 Créez le répertoire personnel de l'utilisateur et définissez la propriété du répertoire sur l'utilisateur josh et le groupe sambashare : :
       
    sudo mkdir /samba/M.ANDRIAH sudo chown M.ANDRIAH:sambashare /samba/M.ANDRIAH
    
    
 La commande suivante ajoutera le bit setgid au répertoire /samba/M.ANDRIAH afin que les fichiers nouvellement créés dans ce répertoire héritent du groupe du répertoire parent.:
       
    sudo chmod 2770 /samba/M.ANDRIAH
 
Ajoutez le compte utilisateur M.ANDRIAH à la base de données Samba en définissant le mot de passe utilisateur:
  
    sudo smbpasswd -a M.ANDRIAH
    
 Vous serez invité à saisir et à confirmer le mot de passe utilisateur :
  
    New SMB password: Retype new SMB password: Added user M.ANDRIAH
    

 Une fois le mot de passe défini pour activer l'exécution du compte Samba:
       
    sudo smbpasswd -e M.ANDRIAH

    Enabled user M.ANDRIAH
    
    
 ###### Ensuite, créons un utilisateur et un groupe sadmin . Tous les membres de ce groupe auront des autorisations administratives. Plus tard, si vous souhaitez accorder des autorisations administratives à un autre utilisateur, ajoutez simplement cet utilisateur au groupe sadmin .
  Créez l'utilisateur administratif :
  
    sudo useradd -M -d /samba/users -s /usr/sbin/nologin -G sambashare sadmin
 
Définir le mot de passe et activez l'utilisateur:
   
    sudo smbpasswd -a sadmin sudo smbpasswd -e sadmin
    
 Puis créez le répertoire de partage des Users:
  
    sudo mkdir /samba/users
    
 Definir la propriété du répertoire sur l'utilisateur sadmin et le groupe sambashare :
  
    sudo chown sadmin:sambashare /samba/users
    

 Ce répertoire sera accessible à tous les utilisateurs authentifiés:
       
    sudo chmod 2770 /samba/users
  
## III/ Configuration des partages Samba:

 Ouvrez le fichier de configuration Samba et ajoutez les sections:
       
    sudo nano /etc/samba/smb.conf /etc/samba/smb.conf

path = /samba/users browseable = yes read only = no force create mode = 0660 force directory mode = 2770 valid users = @sambashare @sadmin path = /samba/M.ANDRIAH browseable = no read only = no force create mode = 0660 force directory mode = 2770 valid users = M.ANDRIAH  @sadmin


    

  Une fois cela fait, redémarrez les services Samba avec la commande:
  
    sudo systemctl restart smbd sudo systemctl restart nmbd
    
## IV/Connexion à un partage Samba depuis Linux:

 Les utilisateurs Linux peuvent accéder au partage samba à partir de la ligne de commande, à l'aide de deux methodes:
  - du gestionnaire de fichiers et 
  - montage du partage Samba
  
Ici je vais vous montrer la première méthode, c'est-à-dire la méthode du gestionnaire de fivhiers.
  
 ###### Cela nécessite l'utilisation du client smbclient(un outil qui vous permet d'accéder à Samba à partir de la ligne de commande) :
 smbclient n'est pas pré-installé sur la plupart des distributions Linux donc il faut l'installé avec:
 
    sudo apt install smbclient
 
La syntaxe pour accéder à un partage Samba est la suivante:
   
    mbclient //samba_hostname_or_server_ip/share_name -U username
    
 Par exemple, pour vous connecter à un partage nommé M.ANDRIAH sur un serveur Samba avec l'adresse IP 192.168.121.118 tant qu'utilisateur M.ANDRIAH, vous devez exécuter:
  
    smbclient //192.168.121.118/josh -U M.ANDRIAH
    
 Vous serez invité à saisir le mot de passe utilisateur :
  
    Enter WORKGROUP\M.ANDRIAH's password:
    

 Une fois le mot de passe entré, vous serez connecté à l'interface de ligne de commande Samba:
       
    Try "help" to get a list of possible commands. smb: \>
    
  <a href="https://github.com/Mitsanta12/SYS_1"> RETOUR </a>
    
 ## VOUS POUVEZ MAINTENANT UTILISER SAMBA! COURAGE! 
