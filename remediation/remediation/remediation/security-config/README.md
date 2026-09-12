Security Configuration

Ce dossier regroupe les éléments de configuration liés à la sécurité du projet.

Les configurations peuvent notamment concerner :

l'analyse statique du code (SAST) ;
l'analyse des dépendances (SCA) ;
l'analyse dynamique de l'application (DAST) ;
les contrôles de sécurité intégrés au pipeline Jenkins ;
les paramètres nécessaires à l'automatisation des contrôles de sécurité.

Les secrets, mots de passe, clés API et informations d'authentification ne doivent jamais être stockés directement dans ce dépôt GitHub.

Les informations sensibles doivent être configurées dans Jenkins à l'aide des mécanismes de gestion des credentials.
