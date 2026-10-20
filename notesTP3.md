# Q1
Cela signifie que chaque incrémentation est exécutée entièrement, sans interférence avec les autres. Ainsi, si 1 000 joueurs terminent une partie simultanément, aucune incrémentation n’est perdue. En SQL, on utiliserait UPDATE compteur SET parties = parties + 1. La différence est que Redis est généralement plus rapide en mémoire.

# Q2 
Avec une String JSON, il aurait fallu récupérer le JSON (GET), le désérialiser, ajouter 250 XP, le re-sérialiser puis enregistrer le résultat (SET).
Si deux requêtes font cela simultanément, elles peuvent lire la même valeur initiale et s’écraser mutuellement : une seule augmentation serait conservée. Le Hash permet d’utiliser HINCRBY, une opération atomique qui évite ce problème.

# Q3
ZREVRANGE trie les scores par ordre décroissant, tandis que ZRANGE les trie par ordre croissant.

Pour afficher le top 3, j’utiliserai ZREVRANGE classement:global 0 2 WITHSCORES, afin d’obtenir les trois joueurs ayant les scores les plus élevés.

# Q4 
ZREVRANK renvoie un rang commençant à 0. Il faut donc ajouter 1 pour afficher la position réelle du joueur.

# Q5
RPOP retire Nova, le premier joueur ajouté, car LPUSH insère les joueurs au début de la liste. Sa applique la règle du premier entré, premier sorti.
LPOP aurait retiré Ombre, le dernier joueur ajouté.

# Q6
Avant 30 secondes : TTL renvoie le temps restant (entre 0 et 30) et EXISTS renvoie 1.
Après 30 secondes : TTL renvoie -2 et EXISTS renvoie 0, car la clé a expiré.

# Q7
Il aurait fallu stocker une date d’expiration et lancer une tâche qui se répéte pour supprimer les clés expirées. Cela ajoute de la complexité, consomme des ressources et risque de laisser des données expirées en base si la tâche échoue.

# Q8
ConnectionMultiplexer gère les connexions entre l’application et Redis. Il est thread-safe et conçu pour être partagé et réutilisé.
Créer une instance à chaque requête HTTP gaspillerait des ressources, multiplierait les connexions et pourrait épuiser les sockets.
Ce bug serait invisible en TP car il y a peu de requêtes simultanées. Il apparaîtrait surtout sous forte charge.

# Q9 
Oui, le résultat est visiblement faux : les joueurs ayant le score le plus faible apparaissent en premier au lieu des meilleurs joueurs.
Ce bug est dangereux car le classement reste cohérent techniquement, sans erreur ni message d’alerte, mais affiche des résultats incorrects. Il faut donc remettre Order.Descending pour afficher les meilleurs scores en premier.

# Q10


# Q12
Aucune ligne des endpoints n’a été modifiée, car le cache implémente la même interface IGameCatalog que MongoGameCatalog.
Le choix de la séance 2 est l’injection de dépendances via une interface, qui permet de remplacer l’implémentation sans modifier le code qui l’utilise.

# Q13
Le premier appel est plus lent que les suivants. Le gain peut être faible avec peu de jeux et peu de requêtes. Un cache devient réellement décisif avec beaucoup de données, des accès fréquents et une base source lente ou fortement sollicitée.

# Q14
 Les joueurs peuvent voir l’ancien titre pendant 60 secondes maximum, car le cache conserve les données jusqu’à expiration du TTL.

Deux stratégies pour réduire ce délai :

Invalider le cache après une modification : les données sont actualisées immédiatement, mais il faut gérer l’invalidation correctement.

Réduire le TTL : les données deviennent plus rapidement à jour, mais les accès à MongoDB augmentent.