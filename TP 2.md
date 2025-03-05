# TP 2 :

## Exercice 1.1 Préparation et analyse des performances sans index :

### 1.1.1 :

- Création d'un jeux de données

### 1.1.2 :

#### Analyse des performances des requêtes suivantes sans index en utilisant expalin("executionStats") :

```

db.livres.find({ prix: { $gt: 25 } })
  .explain("executionStats")

```

#### Recherche par titre exacte :

```

db.livres.find({ titre: "Le Nom de la Rose - Édition 41"}).explain('executionStats')

```

#### Recherche par auteur :

```

db.livres.find({ auteur: "Emile Zola"}).explain('executionStats')

```

#### Recherche par page de prix entre 10 et 20 euros et note minimale de 3 :

```

db.livres.find({
  prix: {
    $gte: 10,
    $lte: 20
  },
  note_moyenne: {
    $gte: 3
  }
}).explain('executionStats')

```
#### Recherche filtrée par genre et langue avec tri par note décroissante : 

```

db.livres.find({
  genre: "Conte",
  langue : "Allemand"
}).sort({ prix: -1 }).explain('executionStats')

```

### 1.1.3 : 

|   Question   |   Commande |   Resultat |
|---    |:-:    |:-:    |:-:    |--:    |
|   Nombre de documents examinés   |   db.livres.find({ prix: { $gt: 25 } }).explain("executionStats")   |  1010  |

## Exercice 1.2 : 

#### 1.2.1 : 

```

db.livres.createIndex({ titre: 1 })

```

