#   APACHE

## I/ Qu’est-ce qu’un serveur Web Apache? <br>
<p>
Apache est le serveur Web le plus couramment utilisé sur les systèmes Linux. Les serveurs Web sont utilisés pour servir les pages Web demandées
par les ordinateurs clients. Cette configuration est appelée LAMP (Linux, Apache, MySQL et Perl/Python/PHP) et forme une plate-forme puissante 
et robuste pour le développement et le déploiement d’applications Web,
selon <a href="https://www.lojiciels.com/reponse-rapide-comment-configurer-le-serveur-web-apache-sous-linux-etape-par-etape/">LOJICIELS.Com</a>
</p> 
<br>

<p>
Ou encore, Apache est un logiciel de serveur web gratuit et open-source qui alimente environ 46% des sites web à travers le monde.
Le nom officiel est Serveur Apache HTTP et il est maintenu et développé par Apache Software Foundation.
</p><br>

II/ Comment installer et configurer le serveur Web Apache?

<h4> Étape 1 — Installation d’Apache </h4>
   - Mettez à jour votre index local des packages :
       
     sudo apt update
    

  Installez le package apache2 :

      sudo apt install apache2
    

<h4>Étape 2 — Réglage du pare-feu :</h4>
  -  Consultez les profils d’application ufw disponibles :

     sudo ufw app list


  - Activons le profil le plus restrictif qui permettra toujours le trafic que vous avez configuré, en autorisant le trafic sur le port 80 :

     sudo ufw allow 'Apache'
    

> **Note** Vérifiez le changement :

     sudo ufw status
    

<h4>Étape 3 - Vérification de votre serveur Web</h4>
  - Vérifiez avec le système systemd init pour vous assurer que le service fonctionne en tapant :

     sudo systemctl status apache2
    
  - Accédez à la page d’accueil par défaut d’Apache pour confirmer que le logiciel fonctionne correctement grâce à votre adresse IP :

      ip addr
    

<h4>Étape 4 — Configuration des hôtes virtuels (recommandé)</h4>
Lorsque vous utilisez le serveur web Apache, vous pouvez utiliser des hôtes virtuels (similaires aux blocs de serveurs dans Nginx) pour encapsuler les détails de la configuration et héberger plusieurs domaines à partir d’un seul serveur.
  Créez le répertoire pour `your_domain`

     sudo mkdir /var/www/your_domain
    

Attribuez la propriété du répertoire :

     sudo chown -R $USER:$USER /var/www/your_domain
    
    
    
Les autorisations de vos racines web devraient être correctes si vous n’avez pas modifié votre valeur unmask, mais vous pouvez vous en assurer en tapant :

     sudo chmod -R 755 /var/www/your_domain
    

Créez un exemple de page `index.html`en utilisant `nano` ou votre éditeur préféré :

     nano /var/www/your_domain/index.html
    

À l’intérieur, ajoutez l’exemple de HTML suivant :
{% filename %}command-line{% endfilename %}

    <html>
    <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
    

Enregistrez et fermez le fichier lorsque vous avez terminé.

Activez le fichier avec  :

     sudo nano /etc/apache2/sites-available/your_domain.conf
    

Collez dans le bloc de configuration suivant, mis à jour pour notre nouveau répertoire et nom de domaine :

    <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName your_domain
    ServerAlias your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


Enregistrez et fermez le fichier lorsque vous avez terminé.

Activez le fichier avec  `a2ensite`

     sudo a2ensite your_domain.conf
    

Désactivez le site par défaut défini dans  `000-defaul.conf `:

     sudo a2dissite 000-default.conf
    

Effectuez un test pour trouver d’éventuelles erreurs de configuration :

     sudo apache2ctl configtest
    

Vous devriez voir la sortie suivante :

    Output : Syntax OK



Redémarrez Apache pour implémenter vos modifications :

     sudo systemctl restart apache2
    
  Apache devrait maintenant vous présenter votre nom de domaine. Vous pouvez vérifier cela en allant sur http://your_domain, où vous devriez voir quelque chose de similaire à ceci :
  
  ## SUCCESS! THE YOUR_DOMAIN VIRTUAL HOST IS WORKING !


    

    
