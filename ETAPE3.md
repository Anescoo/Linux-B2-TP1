# Installation d'un serveur GLPI
---
![](Img/glpilogo.png)

**Quest-ce que GLPI ?**

GLPI est un logiciel libre de gestion des services informatique.
Il permet de gérer des tickets, des postes, des domaines, des lignes, etc...


### Comment installer GLPI sur sa VM ?

Suite à notre connexion via SSH on va utiliser pouvoir utiliser notre invite de commandes pour faciliter la manipulation des commandes.


#### Installation de Apache, PHP, MariaDB configuration MySQL
- Dans un premier temps on va mattre a jour notre système avec la commande suivante :
  
```
apt-get update && apt-get upgrade 
```
- On va ensuite installer **Apache2** avec la commande suivante :

```
apt-get install apache2 php libapache2-mod-php
```

- Après cela nous allons installer PHP avec la commande suivante : 

``` 
apt-get install php-imap php-ldap php-curl php-xmlrpc php-gd php-mysql php-cas
```

- Il s'agira ensuite d'installer **MariaDB** qui est un système de base de données. Il faudra entrer les commandes suivantes : 

``` 
apt-get install mariadb-server
``` 

**&** 

```
mysql_secure_installation
```

- Il faudra ensuite installer les modules complémentaires avec la commande suivante : 

``` 
apt-get install apcupsd php-apcu
```

- Redémarrer les services :

```
/etc/init.d/apache2 restart
``` 

**&** 

```
 /etc/init.d/mysql restart
```

- Créer la base de données qui nous permettra ensuite d'installer GLPI :

``` 
mysql -u root -p
```

- Une fois cette commande exécuter il faudra insérer ceci :

```
CREATE DATABASE glpi CHARACTER SET UTF8 COLLATE UTF8_BIN;
CREATE USER 'glpi'@'%' IDENTIFIED BY 'glpi';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'%';
FLUSH PRIVILEGES;
quit;
```

- On va ensuite éditer le fichier **apache2.conf** et ajouter les lignes suivantes à la fin du fichier : 

```
sudo nano /etc/apache2/apache2.conf 
```

*(commande pour ouvrir le fichier)*

```
<Directory /var/www/html>
AllowOverride All
</Directory>
```

##### Installation GLPI 

- Après avoir installer et configurer MySQL/Apache, nous pouvons alors débuter l’installation de GLPI.

  On télécharge la dernière version de GLPI et l’extraire dans le dossier /tmp : 

```
cd /tmp
```

```
wget (https://github.com/glpi-project/glpi/releases/download/9.5.6/glpi-9.5.6.tgz)
```

```
tar -zxvf glpi-9.5.6.tgz
```

- Donnez à l’utilisateur www-data le contrôle total sur le répertoire GLPI et ses fichiers : 

```
sudo chown -R www-data /var/www/html/glpi
```

- Créez un fichier de configuration Apache nommé glpi.conf et insérez le texte suivant :

```
sudo nano /etc/apache2/conf-available/glpi.conf
```
(création du fichier, la commande nous permet de rentrer dedans)

  Insérer ce texte :

```
<Directory /var/www/html/glpi>
  AllowOverride All
</Directory>

<Directory /var/www/html/glpi/config>
  Options -Indexes
</Directory>

<Directory /var/www/html/glpi/files>
  Options -Indexes
</Directory>
```

- Activez la configuration et redémarrez le serveur Apache :

```
sudo a2enconf glpi
sudo systemctl reload apache2
sudo service apache2 restart 
```


#### Configuration de GLPI 

- Ouvre le navigateur web et entrez l’adresse IP du serveur GLPI (http://192.168.47.135/glpi)

- On sélectionne la langue suivante :

```
Français
```

- Ensuite on accepte les termes de la license :
![](Img/glpi1.PNG)

- On va ensuite installer GLPI en cliquant sur "**Installer**"
![](Img/glpi2.PNG)

- On vérifie le résultat de tous les tests puis si tout est bon ✅ on clique sur **Continuer**
![](Img/glpi3.PNG)

- Ensuite on renseignes les informations de connexion **MySQL** pour qu'on puisse se connecter à la base de données GLPI :
![](Img/glpi4.PNG)

- On sélectionne ensuite la base de données **glpi**, créée dans une étape plus haut : 
![](Img/glpi5.PNG)

- Une fois la base initialisée, on clique sur **Continuer** :
![](Img/glpi6.PNG)

- On clique à nouveau sur **Continuer** : 
![](Img/glpi7.PNG)

- De même sur **Continuer** :
![](Img/glpi8.PNG)

- L'installation est enfin terminée il est désormais possible d'accéder à l'interface **GLPI** en cliquant sur "**Utiliser GLPI**: 
![](Img/glpi9..PNG)

**Voici l'interface de GLPI !**
![](Img/glpinterface.PNG)

---

[III - 1 | page suivante ](https://github.com/Anescoo/Linux-B2-TP1/blob/main/ETAPE5.md) >