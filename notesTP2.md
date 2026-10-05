# Q1 
C'est le serveur MongoDB qui attribue automatiquement un _id 
Pour Pixel, l'id 4 avait été choisi par Postgre pendant l'insertion du joueur 

# Q2
Il aurait fallu faire une jointure avec une table Plateforme hors ici on peut directement recherhcer sans faire de jointure 

# Q3 
Car sa a était créer dans la base admin 
Le compte pixelhub se trouve dans le docker-compose.yml

# Q4 
Pour un projet avec plusieurs développeurs, cela peut être dangereux car chacun peut utiliser des noms ou des champs différents, ainsi il peut être difficile de tester des requêtes et sa peut rendre les résultats incohérents

# Q5 
La requête ne renvoie aucun document car ici on a marqué "Jeux" et non "jeux", comme Jeux n'existe pas ici on ne renvoie aucun document et ainsi sa ne renvoie aps d'erreur

# Q6
MongoInvalidArgumentError: Update document requires atomic operators
Mongo refuse car updateOne attend un document car ici one sait pas vraiment si l'élément a modifier est le titre ou la note, il aurait fallu faire sa 
db.jeux.replaceOne(
  { titre: "Valorant" },
  { titre: "Valorant", note: 4.4 }
)

# Q7 
ALTER TABLE Jeux ADD COLUMN nbVotes INTEGER DEFAULT 0;

# Q8 
Non, le mot "mongo" n'apparaît pas, c'est important car l'interface ne dépend pas du stockage (découplage)

# Q9 
L'erreur concerne le champs nombreClasses 
Le document MongoDB contient bien le champs mais la classe C# Game ne possède aucune propriété correspondante 

# Q10
[BsonIgnoreExtraElements] supprime l'erreur en demandant au driver d'ignorer les champs présents dans MongoDB mais absents de la classe Game 
JoueursParEquipe n'est pas chargé dans l'objet C# il n'apparait donc pas dans /games

# Q11
[BsonIgnoreExtraElements] continue de fonctionner même si MongoDB contient de nouveaux champs 
[BsonExtraElements] conserve les champs inconnus dans un BsonDocuement, ce qui permet tout de même de les récupérer
Une hiérarchie des classes permet de réprésenter précisément les propriétés propres pour chaque type de jeu 

# Q12
Voici la requête équivalente 
SELECT genre, AVG(note) AS note_moyenne, COUNT(*) AS nombre
FROM jeux
GROUP BY genre
ORDER BY note_moyenne DESC;
MongoDB n'apporte pas d'avantage particulier, dans ce tp l'apport de MongoDB se situe dans la souplesse des schémas, les documents peuvent avoir des champs différents sans modifier une structure de table commune 
