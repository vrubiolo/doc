# <img src="/images/wallabag2_logo.svg" height="80px" alt="logo de wallabag2"> Wallabag

[![Install wallabag2 with YunoHost](https://install-app.yunohost.org/install-with-yunohost.png)](https://install-app.yunohost.org/?app=wallabag2) [![Integration level](https://dash.yunohost.org/integration/wallabag2.svg)](https://dash.yunohost.org/appci/app/wallabag2)

### Index

- [Liens utiles](#liens-utiles)

Wallabag est une application de lecture différée : elle  permet simplement d’archiver une page web en ne conservant que le contenu. Les éléments superflus (menus, publicités, etc.) sont supprimés. Sont disponibles : une interface web, des add-ons pour navigateurs (Firefox / Chrome / Opera), des applications pour mobile (Android / iOS / Windows Phone) et même sur liseuse (PocketBook / Kobo).

### Fonctionnalités

En plus des fonctionnalités principales de Wallabag, ce paquet propose également :

* Une intégration avec le système de gestion des utilisateurs et le SSO de Yunohost - e.g. un bouton de déconnexion
* De permettre à un utilisateur d'être administrateur (réglage lors de l'installation)
* Un import asynchrone utilisant Redis (à activer dans les *Paramètres Internes*). L'import via RabbitMQ n'est pas (encore ?) supporté.

## Liens utiles

+ Site web : [www.wallabag.org](https://www.wallabag.org/)
+ Documentation officielle : [doc.wallabag.org](https://doc.wallabag.org/)
+ Démonstration : [Démo](https://vimeo.com/video/167435064)
+ Dépôt logiciel de l'application : [github.com - YunoHost-Apps/wallabag2](https://github.com/YunoHost-Apps/wallabag2_ynh)
+ Remonter un bug ou une amélioration en créant un ticket (issue) : [github.com -YunoHost-Apps/wallabag2/issues](https://github.com/YunoHost-Apps/wallabag2_ynh/issues)

----

### Mettre à niveau depuis la v1.x

La mise à niveau depuis le paquet Yunohost de Wallabag v1 demande une opération manuelle, c'est pourquoi un nouveau paquet est fourni. Pour le processus de migration, merci de vous référer à [la documentation officielle 
de Wallabag](https://doc.wallabag.org/fr/user/import/wallabagv1.html).

