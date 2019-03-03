Sommaire
1. [Les manipulations Git](#git)
2. [Devise](#devise)
3. [Mailer](#mailer)
4. [Stripe](#stripe)
5. [Intégration de template](#template)
6. [Les tests de Model](#model)
7. [Les tests de Controller](#controller)
8. [Les tests de Views](#views)
9. [Les tests d'Intégration](#integration)
10. [Structures des pages](#pages)
11. [Naming](#naming)
12. [Indentation](#indent)

## Les manipulations Git <a name="git"></a> :

**Manipulations de groupe**
Voici les étapes de la gestion/fusion de branche. Si tu suis cette démarche, tu ne devrais pas avoir de souci 😉.

  - Se positionner sur master : $ git checkout master
  - Mettre à jour master sur son ordi : $ git pull origin master
  - Créer une branche, depuis master, pour travailler : $ git branch nom_de_ta_feature
  - Se placer sur cette branche : $ git checkout nom_de_ta_feature
  - À partir de là, tu peux travailler sur ta branche OKLM, repasser d'une branche à l'autre avec git checkout, tout en n'oubliant pas de faire des commits (git add . et $ git commit -m "this is what i did")
  - Fusionner sa branche dans master (en 2 étapes de merge) :
    - $ git checkout master
    - $ git pull origin master
    - $ git checkout nom_de_ta_feature
    - $ git merge master + gérer les conflits si besoin
    - $ git checkout master
    - $ git merge nom_de_ta_feature + gérer les conflits si besoin
    - $ git push origin master

**Manipulations de base**
  - $ git init permet d'initialiser un répo git en local
  - $ git add nom_fichier permet d'ajouter nom_fichier à ton futur commit
  - $ git commit -m "message_commit" permet de faire un commit avec pour message message_commit
  - $ git status permet d'afficher l'état de tes fichiers par rapport au commit que tu es en train de réaliser (⚠ commande très pratique, à faire tout le temps)
  - $ git diff nom_fichier permet d'afficher les modifications de nom_fichier entre le dernier commit et le fichier actuel
  - $ git log permet d'afficher l'historique des commits
  - $ git checkout SHA permet de revenir en mode lecture uniquement au commit SHA
  - $ git checkout master permet de revenir au commit actuel
  - $ git reset --hard SHA permet de tout effacer pour revenir au commit SHA
  - $ git reset --hard permet de tout effacer pour revenir au dernier commit
  - $ git stash permet de sauvegarder provisoirement son travail sans le commit
  - $ git stash pop permet de récupérer un travail précédemment sauvegardé avec git stash
  - $ git remote -v permet de voir les différentes remotes de ton repo git
  - $ git remote add nom_remote url_remote permet d'ajouter la remote url_remote à la liste de tes remotes. Cette remote aura le nom nom_remote (par convention, la remote principale où il y a le code s'appelle origin)
  - $ git push nom_remote nom_branche permet de pousser ta branche nom_branche vers ta remote nom_remote
  - $ git pull nom_remote nom_branche permet de récupérer ta branche nom_branche de ta remote nom_remote


## Devise <a name="devise"></a> :

**Lien cours**
https://www.thehackingproject.org/dashboard/lessons/61?locale=fr

**Commandes**
- Installation
```
$ rails generate devise:install
```
- Pour le Mailer en phase de développement (à ajouter dans le fichier config/environments/development.rb)
```
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```
- Assigner Devise à un model 
```
$ rails g devise nom_du_model (User dans 99% des cas)
```
- Génration des views
```
$ rails generate devise:views
```
- Pour le Mailer en phase de production (à ajouter dans le fichier config/environments/production.rb)
```
config.action_mailer.default_url_options = { :host => 'YOURAPPNAME.herokuapp.com' }
```

## Mailer <a name="mailer"></a>:

**Lien du cours**
https://www.thehackingproject.org/dashboard/lessons/69?locale=fr

**En développement**
  - Générer un  model UserMailer et lui associer des views pour les emails souhaités
  - Mets letter_opener dans le groupe de développement de ton Gemfile puis bundle install
  - Maintenant va dans config/environments/development.rb (fichier contenant les paramètres de ton environnement de développement) et colle les lignes config.action_mailer.delivery_method = :letter_opener et config.action_mailer.perform_deliveries = true

**En production**
  - Générer des API (via Sendgrid)
  - Crée un fichier .env à la racine de ton application.
  - Ouvre-le et écris dedans les informations suivantes : SENDGRID_LOGIN='apikey' et SENDGRID_PWD='ta_clef_API' en remplaçant bien sûr ta_clef_API par la clef que tu viens de générer. Elle est au format SG.sXPeH0BMT6qwwwQ23W_ag.wyhNkzoQhNuGIwMrtaizQGYAbKN6vea99wc8. N'oublie pas les guillemets !
  - Rajoute gem 'dotenv-rails' à ton Gemfile et fait le $ bundle install
  - Et l'étape cruciale qu'on oublie trop souvent : ouvre le fichier .gitignore à la racine de ton app Rails et écris .env dedans.
  - Paramétrer Sendgrid (à ajouter dans le fichier /config/environment.rb)
``` ruby
ActionMailer::Base.smtp_settings = {
  :user_name => ENV['SENDGRID_LOGIN'],
  :password => ENV['SENDGRID_PWD'],
  :domain => 'monsite.fr',
  :address => 'smtp.sendgrid.net',
  :port => 587,
  :authentication => :plain,
  :enable_starttls_auto => true
}
```
**Tester le Mailer**
  - Enlève la ligne config.action_mailer.delivery_method = :letter_opener du fichier config/environments/development.rb ;
  - Va dans ta console Rails ;
  - Créé un utilisateur avec une adresse en @yopmail.com ;
  - Va vérifier que l’e-mail est bien arrivé sur http://www.yopmail.com/.

## Stripe <a name="stripe"></a> :

**Lien du cours**
https://www.thehackingproject.org/dashboard/lessons/245?locale=fr

**Lien du tuto stripe**
https://stripe.com/docs/checkout/rails

**Atelier de Felix sur stripe**
https://www.youtube.com/watch?v=o5c7WFyyJFc

## Intégration de template <a name="template"></a> :

**Lien du cours**
https://www.thehackingproject.org/dashboard/lessons/201?locale=fr

**La vidéo de Félix**
https://www.youtube.com/watch?v=JbnrWYYnJ7c

**L'Asset Pipeline**
  - app/assets: La partie permettant de stocker le css qu'on écrit à la main. c'est à dire si l'on veut un partial pour écrire du css spécifique à une view, c'est ici qu'on le créera.
  - lib/assets: Ici on met nos librairie perso.
  - vendor/assets: Ici on intègre nos librairie externes, comme Bootstrap par exemple, c'est donc de ce dossier qu'on aura besoin pour notre template.

**Appel des fichers**
  - Rajouter dans le fichier config/initializers/assets.rb :
  ```
  Rails.application.config.assets.paths << Rails.root.join('lib')
  Rails.application.config.assets.paths << Rails.root.join('vendor')
  ```
  - Dans le fichier application.css :
  ```
  *= require Dossier_de_ta_librairie/Ton_fichier
  ``` 
  - Dans le fichier application.js :
  ```
  //= require Dossier_de_ta_librairie/Ton_fichier
  ```

## Les tests de Model <a name="model"></a> :

**Lien du cours (avec les gems factory_bot, shoulda_matchers et faker)**

## Les tests de Controller <a name="controller"></a> :
https://www.thehackingproject.org/dashboard/lessons/239?locale=fr

**Lien du cours**
https://www.thehackingproject.org/dashboard/lessons/244?locale=fr

## Les tests de Views <a name="views"></a> :

**Lien du cours**
https://www.thehackingproject.org/dashboard/lessons/240?locale=fr

## Les tests d'Intégration <a name="integration"></a> :

**Lien du cours**
https://www.thehackingproject.org/dashboard/lessons/243?locale=fr

## Structure des pages <a name="pages"></a> :

**Les différentes pages de Locust**
  - Navbar/Footer sur toutes les pages
  - Landing page avec des cartes contenant les bars (et leurs events) et une search bar
  - Page Profil avec les préférences(?) et les évènements(en cours et passés)
  - Un Show de chaque bar avec ses events
  - Login/Logout/Forgot Password/Edit Profile (via Devise)
  - Un Show du résultat de recherche (pas urgent)
  - Formulaire de création de bar (pas urgent)
  - Formulaire de création d'event (pas urgent)

## Naming <a name="naming"></a> :
  - User (utilisateur)
  - Bar (bar)
  - Gig (concert)
  - Tag (style musical)

## Indentation <a name="indent"></a> :
  - Tabulation de 2 et rien d'autre !