Remédiation — Feedbacks / Broken Access Control
1. Identification

Vulnérabilité : Broken Access Control / accès non autorisé aux feedbacks
Composant : fonctionnalité de gestion des feedbacks
Niveau de risque : High

2. Description

La fonctionnalité de gestion des feedbacks présente un défaut de contrôle d'accès.

Un utilisateur ne devrait pouvoir consulter, modifier ou supprimer que les ressources auxquelles son rôle ou son compte lui donne accès.

Une vérification insuffisante des autorisations côté serveur peut permettre à un utilisateur standard d'accéder à des fonctionnalités ou à des données réservées à un autre utilisateur ou à un administrateur.

3. Impact

L'exploitation de cette faiblesse peut entraîner :

une divulgation d'informations ;
la modification de données appartenant à d'autres utilisateurs ;
la suppression de données ;
une atteinte à la confidentialité et potentiellement à l'intégrité des données.

Le niveau d'impact dépend des opérations réellement accessibles à l'utilisateur.

4. Cause

La cause principale est un contrôle d'accès insuffisant côté serveur.

L'application ne doit pas se contenter de vérifier qu'un utilisateur est authentifié. Elle doit également vérifier que son rôle et ses permissions lui permettent d'effectuer l'opération demandée.

5. Remédiation proposée

Le serveur doit appliquer un contrôle d'accès systématique avant chaque opération sur les feedbacks.

La logique attendue est :

Utilisateur authentifié
|
v
Demande d'accès au feedback
|
v
Vérification du rôle et des permissions
/
NON OUI
| |
403 Accès autorisé

Les contrôles d'autorisation doivent être effectués côté serveur et ne doivent jamais dépendre uniquement des informations envoyées par le navigateur.

Les opérations réservées aux administrateurs doivent notamment vérifier le rôle de l'utilisateur avant leur exécution.

6. Pourquoi cette correction est efficace

Cette correction empêche un utilisateur authentifié mais non autorisé de contourner les restrictions simplement en modifiant une requête HTTP ou un identifiant de ressource.

L'authentification permet de déterminer qui est l'utilisateur, tandis que l'autorisation détermine ce qu'il a le droit de faire.

7. Vérification après correction

Plusieurs tests doivent être réalisés après l'application de la correction.

Test 1 — Utilisateur autorisé

L'utilisateur disposant des permissions nécessaires doit pouvoir effectuer l'opération prévue.

Résultat attendu :

HTTP 200 OK ou réponse correspondant à l'opération demandée.

Test 2 — Utilisateur non autorisé

Un utilisateur ne disposant pas des permissions nécessaires doit être refusé.

Résultat attendu :

HTTP 403 Forbidden.

Aucune donnée protégée ne doit être retournée.

Test 3 — Tentative de modification de la requête

Une modification manuelle des paramètres ou de l'identifiant de la ressource ne doit pas permettre de contourner le contrôle d'accès.

8. État actuel

Correction : proposée mais non encore appliquée.

Une nouvelle campagne de tests devra être réalisée après modification du code afin de confirmer que l'accès non autorisé n'est plus possible.
