# mongo_db_rabatel

## Qu'est ce que :

- Un language qui permet d'intéragir avec la base de données

## NoSQL ?

- Not Only SQL
- Structure des tables (schéma) flexibles
- Scalabilité horizontale : répartir la charge du flux de données important

Collection est un équivalant d'une table

## Index

- Améliore les perfrmances de lecture

## Pourquoi MongoDB ?

- Schéma flexible (tables)
- Utilisation de JSON
- Performances +
- multi-documents

- Application web
- CMS
- E-commerce (catalogues produits)
- BigData Analytique
- Métadonnées
- Caching / sessions

## BSON :

- Extension binaire de JSON
- Encodage + éfficace
- Optimisé pour la traversé rapide
- support type additionnels

![Image](img/bson.png)

## Installation :

![Image](img/install.png)

## Projction et Limitation ;

- Filtrer
- Séléctionner avec conditions
- ![Image](img/projection.png)

## DB :

password : eABFKskjLRvfaz13

mongodb+srv://martyrabatel2:eABFKskjLRvfaz13@cluster0.sgzft.mongodb.net/

## L'agrégation : 

- L'agrégation est un mécanisme puissant dans MongoDB qui permet de transformer, filtrer et regrouper des données dans une collection de manière avancée. Chaque "étape" (ou stage en anglais) dans le pipeline d'agrégation effectue une opération particulière sur les données.

- Exemple : 

```

db.livres.aggregate([
  { 
    $group: {
      _id: "$genre",  // Grouper par genre
      nombre_de_livres: { $sum: 1 },  // Nombre de livres par genre
      note_moyenne: { $avg: "$note_moyenne" },  // Moyenne des notes par genre
      prix_moyenne: { $avg: "$prix" },  // Moyenne des prix par genre
      prix_min: { $min: "$prix" },  // Prix minimum par genre
      prix_max: { $max: "$prix" }   // Prix maximum par genre
    }
  }
])

```