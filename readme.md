# SYMFONY (Introduction)

# Jour 1    

## Utilisation vscode

`ctrl + p` : pour éditer un fichier 

`ctrl + shift + p` : accéder aux commandes

`ctrl + b` : affichage et désactivation barre latérale gauche

`ctrl + j` : ouverture et fermeture du terminal intégré


## Installation Symfony
Aller dans Google et chercher le site officiel de Symfony (pour l'installer)

- Installer ou vérifier si PHP est déjà installé sur l'ordinateur (version supérieure à 7.4) => enter la commande suivante dans le terminal :
`php -v`

- Installer ou vérifier si COMPOSER est déjà installé sur l'ordinateur) => entrer la commande suivante dans le terminal : `composer -v`
Pour l'installer voir :
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

- Installer GIT (dans le terminal : `git`)

- Installer ensuite Symfony (depuis le site officiel)
https://symfony.com/doc/current/setup.html

- Ajouter la ligne suivante au fichier de config (coller dans le terminal) :
`export PATH="$HOME/.symfony/bin:$PATH"`

Attention, commande à renseigner systématiquement dans chaque terminal ouvert (car l'option ne reste pas en mémoire)

- Dans le cas où le message suivant apparaît :
*simplexml_import_dom() must be available*
Entrer la commande suivante dans le terminal :
`sudo apt-get install php-xml`

## Création projet Symfony
1. Créer un dossier projet en Symfony 4.4
Utiliser la commande suivante dans le terminal :
 `composer create-project symfony/website-skeleton:"^4.4" my_project_name`

2. Connecter ce projet à une base de données
Lancer phpmyadmin.
Ouvrir le fichier .env et remplir les champs nécessaires de la ligne suivante :
DATABASE_URL="mysql://`db_user`:`db_password`@127.0.0.1:3306/`db_name`?serverVersion=5.7"

# customize this line!
DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=5.7"
# to use mariadb:
DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=mariadb-10.5.8"
# to use sqlite:
DATABASE_URL="sqlite:///%kernel.project_dir%/var/app.db"
# to use postgresql:
DATABASE_URL="postgresql://db_user:db_password@127.0.0.1:5432/db_name?serverVersion=11&charset=utf8"
# to use oracle:
DATABASE_URL="oci8://db_user:db_password@127.0.0.1:1521/db_name"
 
3. Créer la base de données
Entrer la commande suivante :
`php bin/console doctrine:database:create`

4. Créer les entités (entité utilisateur + autres entités)
Entrer la commande :
`php bin/console make:user`
Générer les requêtes SQL
`php bin/console make:migration`

Des requêtes SQL sont alors préparées, pour les valider entrer la ligne :
`php bin/console doctrine:migrations:migrate`

Attention: La création de l'entité "user" est traitée de manière particulière par Symfony. Mais pour créer d'autres entités, il faudra utiliser la commande suivante :
 `php bin/console make:entity`

Valider puis entrer le nom de l'entité souhaitée en commençant par une lettre majuscule.

Entrer ensuite les noms de propriétés ou caractéristiques et se laisser guider pour le paramétrage (titre en string, booléen etc...)

Une fois toutes les propriétés ou caractéristiques créées, reprendre les commandes de requêtes SQL :

`php bin/console make:migration`

`php bin/console doctrine:migrations:migrate`


# Jour 2

5. Créer une page de login 
Entrer la commande suivante :

`php bin/console make:auth`

Choisir le style d'authentification (création de formulaire) puis nommer la classe de l'authentificateur et la classe du contrôleur.

Enfin, générer une URL pour la page de déconnection.

Pour visualiser le résultat à ce stade, entrer la commande :

`symfony server:start`

Ouvrir le lien donné ou ouvrir dans le navigateur (e.g : http://127.0.0.1:8000)
On arrive sur une page "Welcome to Symfony"


6. Créer une page d'enregistrement
Entrer la commande :

`php bin/console make:registration-form`

Ne pas renseigner d'adresse email personnelle pour la vérification des inscriptions car ces dernières seront visibles lors de la mise en prod.

Attention, lors de la création de la page, le terminal peut demander d'installer des composants supplémentaires, penser à bien lire les messages d'alertes en jaune.

Exemple : composer require symfonycasts/verify-email-bundle

Ensuite modifier la variable d'environnement dans le fichier .env comme ceci :

`MAILER_DSN=smtp://localhost`

devient 

`MAILER_DSN=smtp://user:pass@smtp.example.com:port`

Attention, il faudra encoder les caractères spéciaux comme arobase, dièse etc...pour ne pas entrer en conflit avec l'arobase principal.
Aller sur le site : https://www.urlencoder.org/
et encoder l'adresse mail choisie ainsi que le mot de passe comme suit :

`MAILER_DSN=smtp://test-projet@philiance.com:SMTP#test@@mail.gandi.net:587`

devient

`MAILER_DSN=smtp://test-projet%40philiance.com:SMTP%23test%40@mail.gandi.net:587`

Pour tester l'envoi de mails, il est préférable d'utiliser une adresse mail temporaire. Par exemple, le site https://temp-mail.org/fr/ permet de générer une adresse et de consulter la messagerie correspondante directement en ligne.

Attention, après tout changement effectué au niveau des entités, il est impératif de relancer les 2 commandes ci-dessous pour mettre à jour l'application et valider les dernières modifications :

`php bin/console make:migration`

`php bin/console doctrine:migrations:migrate`

Cas d'erreur : 
`TODO: provide a valid redirect inside /home/office-1/Projects/newproject/tasklist/src/Security/LoginFormAuthenticator.php`

Il faut créer une route pour rediriger l'utilisateur vers une page précise après sa connexion.

Exemple :
` return new RedirectResponse($this->urlGenerator->generate('some_route'));`


7. Créer des CRUD (Create Read Update Delete)
Entrer la commande suivante :

`php bin/console make:crud`

Renseigner le nom de classe de l'entité pour laquelle la méthodeen est créée et valider. (`The class name of the entity to create CRUD`)

Symfony propose automatiquement la création du controller associé, si ce n'est pas le cas, voici la commande pour le créer manuellement :

`php bin/console make:controller ProductController`

Lancer l'application dans le navigateur (rappel, entrer la commande suivant s'il s'agit d'un nouveau terminal :

`export PATH="$HOME/.symfony/bin:$PATH"`

Puis lancer le serveur 
`symfony server:start`

En cas d'erreur sur l'objet non trouvé dans l'entité, ouvrir le fichier d'entité et entrer la ligne de code suivante :

    public function __toString()
    {
        return $this->email;
    }

8. Design de l'application (HTML CSS BOOTSTRAP)



