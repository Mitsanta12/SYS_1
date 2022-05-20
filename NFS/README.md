#   NFS

## I/ Qu’est-ce que NFS ? <br>
<p>
Le NFS, pour Network File System, est un protocole permettant à un ordinateur d'accéder à des fichiers extérieurs via un réseau.
Développé dans les années 80, le NFS s'est ensuite imposé comme la norme en matière de serveur de fichiers.
</p> 
<br>

## II/ Comment installer et configurer le serveur NFS?

<h4> Étape 1 —  Installer un serveur NFS </h4>
   - Comme tous les autres il faut d'abord mettre à jour votre index local des packages :
       
    apt-get install nfs-kernel-server
    

  Normalement il devrait y avoir un avertissement qui va s'affiché :
  
    [warn] Not starting NFS kernel daemon: no exports. ... (warning)
    
> **Note** Pas de panique, ceci est normal, car nous n'avons pas encore configuré les dossiers à "partager":

   
   - Pour commencer, créez un dossier dans "/var" et donnez tous les droits pour l'utilisateur root. Les autres auront uniquement les droits de lectures et d'accès.
       
    mkdir /var/shared-data
    chown root:root /var/shared-data
    chmod 755 /var/shared-data
    

  Pour partager ce dossier, indiquez cette possibilités dans le fichier "/etc/exports" :
  
    # Dossier partagé en lecture seul et accessible par tout le monde
     /var/a-public-folder *(ro,insecure,all_squash)
     

  Pour finir, redémarrez le serveur NFS:
  
    service nfs-kernel-server restart
    
    
 <h4> Étape 2 —  Installer un client NFS </h4>
 
 
   - Pour installer le client NFS pour Linux, taper la commande suivante :
       
    apt-get install nfs-common
    

  Puis créez un dossier pour le point de montage :
  
    mkdir -p /mnt/nfs/var/shared-data

   
   Listez les systèmes de fichiers disponibles sur le client.
       
    df -h
    

  créer un fichier dans le dossier "/mnt/nfs/var/shared-data" :
 
     touch /mnt/nfs/var/shared-data/fichier.txt
     

  Ensuite, sur le serveur, listez le contenu du dossier "/var/shared-data/":
  
    ls -l /var/shared-data/
    
> **Note** Attention, si vous redémarrez l'ordinateur client, le point de montage disparaitra :
    
  Pour monter le dossier au démarrage de l'ordinateur, modifiez le fichier "/etc/fstab" en ajoutant ceci :
  
    10.0.0.102:/var/shared-data  /mnt/nfs/var/shared-data   nfs      rw,sync,hard,intr  0     0

   
   Pour tester cette configuration, démontez le point de montage actuel:
       
    umount /mnt/nfs/var/shared-data
    

  Pour tester la configuration du fichier "/etc/fstab" sans devoir redémarrer la machine linux, utilisez:
 
     mount -a
     

  Si tout est OK, le dossier "/mnt/nfs/var/shared-data" contiendra le fichier créé précédemment:
  
    ls -l /mnt/nfs/var/shared-data
    
Et le point de montage sera affiché par la commande :
  
    df -h


<h4> Étape 3 —   Partage FTP pour les clients non linux</h4>
   Comme Windows ne permet pas nativement de se connecter à un serveur NFS, nous allons utiliser le protocole FTP pour accéder au dossier partagé par le serveur NFS: <br>
  Il faut installer ProFTPD en suivant le tutoriel que vous pourriez trouver dans le lien suivant :
 <a href="https://www.informatiweb-pro.net/admin-systeme/linux/debian-ubuntu-installer-un-serveur-ftp.html">Debian / Ubuntu - Installer un serveur FTP<a>
    


   
   - Une fois installé et configuré, créez un utilisateur en indiquant le dossier souhaité comme dossier home.
Ainsi, lorsque vous vous connecterez en utilisant cet utilisateur, vous accéderez au dossier souhaité.
       
    useradd --home /var/shared-data my_ftp_user
    

  Spécifiez un mot de passe pour cet utilisateur avec :
  
    passwd my_ftp_user
     

  Définir cet utilisateur comme le propriétaire de ce dossier:
  
    chown -R my_ftp_user:my_ftp_user /var/shared-data
    
   Pour vérifiez les droits de ce dossier, utilisez la commande:
  
    ls -l /var/shared-data
   
   
   
#### Voilà, vous pouvez accéder à ce dossier depuis n'importe quel OS en utilisant un simple client FTP.

<a href="https://github.com/Mitsanta12/SYS_1"> RETOUR </a>
   
## VOUS ETES MAINTENANT CAPABLE D'INSTALLER ET DE CONFIGURER VOTRE SERVEUR NFS. 
