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

 -  Pour commencer, créez un dossier dans "/var" et donnez tous les droits pour l'utilisateur root. Les autres auront uniquement les droits de lectures et d'accès.

   
