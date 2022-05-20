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
    
  Pour que le serveur Web Nginx démarre automatiquement, il faut exécuter la commande suivante   :

    sudo systemctl enable nginx

    
 Les fichiers de configuration de nginx se trouves dans le dossier :
 
     /etc/nginx
    
   Mais les fichiers qui nous intéresse sont  :
       
   - `nginx.conf` : Le fichier de configuration globale du serveur.
   - `mime.types` : La liste des types MIME résolus par les extensions de fichiers
   - `sites-available` : Contient les fichiers de configurations de vos sites ou services (un fichier par préoccupation/site/service).
   - `sites-enabled` : Doit contenir des liens symboliques vers les fichiers de site-available que vous souhaitez activer.
   - `conf.d` : Emplacement pour appliquer les paramètres communs à tous les sites.

     -  Le dossier qui contient les fichiers de configuration des sites disponibles:

    /etc/nginx/sites-available/

  Le dossier qui contient les fichiers de configuration des sites actifs est:

     /etc/nginx/sites-enabled/

<h4>Étape 3 — Accéder à la page par défaut du serveur Web :</h4>
  - Si votre machine où est installé Nginx dispose d'une interface graphique, on peut y accéder en local :

    http://127.0.0.1


  - À partir d'une machine distante, utilisez l'adresse IP de votre serveur Web plutôt que l'adresse de loopback (127.0.0.1). Voici la page qui doit s'afficher :

    Image
    

> **Note** Le contenu de cette page Web correspond au fichier suivant :

    /var/www/html/index.nginx-debian.html
    
  le site par défaut de Nginx est déclaré dans le fichier de configuration suivant :

    /etc/nginx/sites-enabled/default

    

La racine de ce site est :

    /var/www/html
    
    
    
On peut le vérifier grâce à la directive suivante :

    root /var/www/html;
    


<h4>Étape 4 — Créer un premier site dans Nginx</h4>
Nous allons déclarer un nouveau site sur notre serveur Web Nginx. Vous pourriez choisir le site de votre choix.
 Il sera stocké dans :

    /var/www/le nom de votre site
    

Commençons par créer le dossier qui va accueillir notre site Web :

    sudo mkdir /var/www/your_domain
    
    
    
Ensuite, on va déclarer l'utilisateur www-data comme propriétaire de ce dossier. Il s'agit de l'utilisateur par défaut de Nginx (correspondant à la propriété "user www-data" du fichier nginx.conf).

    sudo chown -R www-data:www-data /var/www/your_domain/
    

Pour définir les droits de ce dossier  :

    sudo chmod 755 /var/www/your_domain/
    

Attribuez la propriété du répertoire :

    $ sudo chown -R $USER:$USER /var/www/your_domain
    
    
    
Ensuite, c'est le moment de créer notre fichier "index.html" : cela correspond à la page d'accueil de notre site Web.

    sudo nano /var/www/your_domain/index.html
    

Dans ce fichier, insérez le code suivant :
{% filename %}command-line{% endfilename %}

    <html>
    <head>
        <title>Welcome to Nginix!</title>
    </head>
    <body>
        <h1>Success! The your_domain virtual host is working!</h1>
    </body>
</html>
    
Enregistrez et fermez le fichier.

    
Il est temps maintenant de créer le fichier de configuration de notre site Internet. Dans le dossier "sites-available", on va créer le fichier "your_domain" : grâce à ce nom, il sera facilement identifiable.

    sudo nano /etc/nginx/sites-available/your_domain
    

Dans ce fichier, intégrez la configuration suivante :
{% filename %}command-line{% endfilename %}

   server {

    listen 80;
    listen [::]:80;

    root /var/www/your_domain;

    index index.html;
    server_name it-connect.tech www.your_domain;

    location / {
        try_files $uri $uri/ =404;
    }
}
    
 #### Signification des différentes directives de la configuration que l'on vient de créer:
   - La directive "listen" : la première ligne permet d'indiquer que le serveur Nginx écoute sur toutes ses adresses IPv4, sur le port 80, ce qui correspond au protocole HTTP. La seconde ligne est similaire, mais pour toutes les adresses IPv6 du serveur, toujours sur le port 80.

    listen 80; 
    listen [::]:80;
    

   - La directive "root" permet de déclarer la racine du site Internet : en toute logique, on précise la racine que l'on a créée précédemment et où se situe la page index.html.

    root /var/www/your_domain;
    
    
    
   - La directive "server_name" sert à déclarer le nom de domaine, ou les noms de domaine, concerné par ce bloc "Server". On peut également utiliser une adresse IP. En l'occurrence dans cet exemple, on souhaite que Nginx traite les requêtes qui arrivent sur your_domain et www.your_domain.
   
    server_name it-connect.tech www.your_domain;
    

   - La directive "location" permet d'indiquer un chemin relatif dans l'URL. En indiquant "/", on cible toutes les requêtes puisqu'une requête commence toujours par "/" après le nom de domaine pour spécifier le chemin vers une page.

   - Enfin, grâce à la directive "try_files" suivie de $uri et $uri/, nous allons chercher à vérifier l'existence du fichier ou du dossier (d'où le "/") passé en paramètre dans l'URL. La variable $uri reprend automatiquement l'URL saisie par le client qui accède au site. En fait, la règle "try_files $uri $uri/ =404;" permet de retourner une erreur 404 (page introuvable) au client s'il essaie d'accéder à un fichier ou un dossier qui n'existe pas.

    location / { 
    try_files $uri $uri/ =404; }
       


<h4>Étape 5 — activation et configuration chargée par Nginx</h4>
Nous devons créer un lien symbolique : 
    Pour créer un lien symbolique et renvoyer "/etc/nginx/sites-enabled/it-connect.tech" vers "/etc/nginx/sites-available/your_domain", voici la commande :
  voici la commande :

    ln -s /etc/nginx/sites-available/your_domain     /etc/nginx/sites-enabled/your_domain.tech
 
       
       
  - Avant de redémarrer le service Nginx, vérifier la syntaxe de la configuration avec :

    sudo nginx -t
    

  - Si tout est bon, on peut redémarrer le service Nginx avec la commande:

    sudo systemctl restart nginx
    
  - On peut aussi arrêter Nginx et le démarrer, en deux temps :
    pour démarer :

    sudo systemctl start nginx 

    
  Pour arrêter:

    sudo systemctl stop nginx

 
 Le site doit être accessible sur:

    http://your_domain
    
 <a href="https://github.com/Mitsanta12/SYS_1"> RETOUR </a>   
  
  ## BRAVO VOUS AVEZ BIEN INSTALLER ET CONFIGURER NGNIX!
