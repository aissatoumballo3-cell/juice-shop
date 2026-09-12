Remédiation — IDOR / Broken Access Control
1. Identification

Vulnérabilité : IDOR / Broken Access Control
CWE : CWE-639
Composant : /rest/basket/:id
Niveau de risque : High

2. Description

L'application permet à un utilisateur authentifié d'accéder au panier d'un autre utilisateur en modifiant directement l'identifiant du panier dans la requête HTTP.

Par exemple, un utilisateur autorisé à consulter son propre panier peut modifier l'identifiant transmis dans l'URL afin de demander le panier associé à un autre utilisateur.

Cette situation constitue un défaut de contrôle d'accès côté serveur.

3. Impact

L'impact principal concerne la confidentialité des données.

Un utilisateur peut obtenir des informations appartenant à un autre compte alors qu'il ne devrait pas y avoir accès.

La confidentialité est donc fortement affectée.

Aucune atteinte à l'intégrité ou à la disponibilité n'a été démontrée lors des tests réalisés.

4. Cause

La cause principale est l'absence d'une vérification suffisante de la propriété de la ressource demandée.

L'application ne doit pas uniquement vérifier que le panier existe. Elle doit également vérifier que le panier appartient bien à l'utilisateur authentifié.

5. Remédiation proposée

Le serveur doit vérifier systématiquement l'association entre le panier demandé et l'utilisateur authentifié.

La logique attendue est :

Utilisateur authentifié
        |
        v
Récupération du panier demandé
        |
        v
Le panier appartient-il à l'utilisateur ?
       / \
     NON  OUI
      |    |
     403   Accès autorisé

Si le panier appartient à un autre utilisateur, le serveur doit retourner une réponse 403 Forbidden ou 404 Not Found sans divulguer les données du panier.

La vérification doit être effectuée côté serveur et ne doit pas dépendre d'une valeur fournie uniquement par le client.

6. Pourquoi cette correction est efficace

Cette correction empêche un utilisateur de contourner le contrôle d'accès simplement en modifiant l'identifiant du panier.

Même si l'utilisateur connaît ou devine l'identifiant d'une autre ressource, le serveur vérifiera son appartenance avant de retourner les données.

7. Vérification après correction

Après application de la correction, deux tests doivent être réalisés.

Test 1 — Panier de l'utilisateur connecté

Le panier appartenant à l'utilisateur authentifié doit rester accessible.

Résultat attendu :

HTTP 200 OK

avec les données correspondant uniquement à son propre panier.

Test 2 — Panier d'un autre utilisateur

Une tentative d'accès au panier d'un autre utilisateur doit être refusée.

Résultat attendu :

HTTP 403 Forbidden

ou :

HTTP 404 Not Found

Aucune donnée appartenant à l'autre utilisateur ne doit être retournée.

8. État actuel

Correction : proposée mais non encore appliquée.

Une nouvelle campagne de tests devra être réalisée après modification du code afin de confirmer la disparition de la vulnérabilité.
