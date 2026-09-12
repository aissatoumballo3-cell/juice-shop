Remédiation — Exposition de fichiers via FTP
1. Identification

Vulnérabilité : Exposition de fichiers sensibles / mauvaise configuration du répertoire FTP
Composant : /ftp
Niveau de risque : High

2. Description

L'application expose un répertoire /ftp permettant de consulter et de télécharger des fichiers.

Lors de l'analyse, plusieurs fichiers présents dans ce répertoire peuvent être accessibles directement depuis l'application web.

L'exposition de fichiers contenant des informations confidentielles, des sauvegardes ou des documents internes constitue un risque pour la confidentialité des données.

3. Impact

Un attaquant pouvant accéder à des fichiers exposés peut récupérer des informations qui ne devraient pas être accessibles publiquement.

Les conséquences peuvent notamment être :

divulgation d'informations confidentielles ;
fuite de données internes ;
récupération de fichiers de sauvegarde ;
exposition d'informations utiles à une attaque ultérieure ;
atteinte à la confidentialité des données.
4. Cause

La cause principale est l'exposition directe du répertoire de fichiers par l'application web.

La présence d'un index de répertoire facilite également la découverte des fichiers disponibles.

Les fichiers destinés à un usage interne ne doivent pas être accessibles directement depuis une interface web publique.

5. Remédiation proposée

Le répertoire contenant les fichiers sensibles doit être placé en dehors du répertoire publiquement accessible par le serveur web.

L'accès aux fichiers doit être contrôlé par une logique d'autorisation côté serveur.

Il est également recommandé de :

désactiver le listing des répertoires ;
supprimer les fichiers sensibles de l'espace web public ;
empêcher l'accès direct aux fichiers de sauvegarde ;
vérifier les extensions et les types de fichiers autorisés ;
appliquer une politique de contrôle d'accès appropriée ;
journaliser les accès aux fichiers sensibles.

La logique recommandée est :

Demande d'accès au fichier
|
v
Le fichier est-il public ?
/
NON OUI
| |
403 Accès contrôlé

Pour les fichiers non publics, l'application doit refuser l'accès sans révéler leur contenu.

6. Pourquoi cette correction est efficace

Le fait de retirer les fichiers sensibles de l'espace web public empêche leur téléchargement direct par simple connaissance ou découverte de leur nom.

La désactivation du listing empêche également un utilisateur de parcourir facilement le contenu du répertoire.

Les contrôles côté serveur ajoutent une protection supplémentaire lorsque certains fichiers doivent rester accessibles à des utilisateurs authentifiés.

7. Vérification après correction
Test 1 — Accès à un fichier public

Un fichier explicitement destiné à être public doit rester accessible.

Résultat attendu :

HTTP 200 OK.

Test 2 — Accès à un fichier sensible

Une tentative d'accès direct à un fichier confidentiel doit être refusée.

Résultat attendu :

HTTP 403 Forbidden ou HTTP 404 Not Found.

Le contenu du fichier ne doit pas être retourné.

Test 3 — Listing du répertoire

L'accès au répertoire ne doit pas afficher la liste des fichiers disponibles.

Résultat attendu :

Le listing doit être désactivé ou l'accès doit être refusé.

8. État actuel

Correction : proposée mais non encore appliquée.

Une nouvelle campagne de tests devra être réalisée après modification de la configuration et du code afin de confirmer que les fichiers sensibles ne sont plus accessibles publiquement.
