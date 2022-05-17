#   NGINIX

## I/ Qu’est-ce que NGINIX? <br>
<p>
Nginx a été créé à l’origine par Igor Sysoev,il est prononcé comme « engine-ex », est un serveur web open-source qui, depuis son succès initial en tant que serveur web, est maintenant 
aussi utilisé comme reverse proxy, cache HTTP, et load balancer.
</p> 


## II/ Comment installer et configurer Nginx?

<h4> Étape 1 — Installation de NGINIX </h4>
   - Avant de commençé mettons à jour le cache de paquets sur notre machine  :
       
    sudo apt update -y
    

   - Puis passons à l'installation du paquet Nginx:

    sudo apt install nginx -y
    
    

<h4>Étape 2 — Configuration de NGINIX</h4>
     -  Pour regarder la version de NGNIX on peut faire :

    sudo nginx -v

   Suite à l'installation, le serveur Nginx est déjà démarré, on peut le vérifier avec la commande ci-dessous. Cela permettra de voir qu'il est bien actif :

      sudo systemctl status nginx
    
  - Pour que le serveur Web Nginx démarre automatiquement, il faut exécuter la commande suivante   :

    sudo systemctl enable nginx



  - Dès à présent, on peut accéder à la page par défaut du serveur Web à partir d'un navigateur. Si votre machine où 
  est installé Nginx dispose d'une interface graphique, on peut y accéder en local :

    http://127.0.0.1

- À partir d'une machine distante, utilisez l'adresse IP de votre serveur Web plutôt que l'adresse de loopback (127.0.0.1). Voici la page qui doit s'afficher :
    image 
    

 
  
  ## SUCCESS! THE YOUR_DOMAIN VIRTUAL HOST IS WORKING !
