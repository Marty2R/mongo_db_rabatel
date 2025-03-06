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

| Requête | Temps avant indexation | Temps après indexation | Nombre de documents examinés|
|---------------------------------------------|-----------------------|----------------------------------|
| Recherche par prix et note minimale         | < 1ms                 | < 1ms                            |  53             
| Recherche par genre et langue               | < 1ms                 | < 1ms                            |  61
| Recherche de texte sur titre et description | < 1ms                 | < 1ms                            |  82
| Recherche avec index sur ISBN               | < 1ms                 | < 1ms                            |  1
| Recherche de livres disponibles             | < 1ms                 | < 1ms                            |  542

## Exercice 1.2 : 

#### 1.2.1 : 

```

db.livres.createIndex({ titre: 1 })

```

### Créez une collection livraisons : 

```
db.livraisons.insertOne(
  {
    utilisateur_id: ObjectId("67c7523ac13133e1feb21e6a"), 
    livre_id: ObjectId("67c71dbfd68f87729363ae53"),
    depart: {
      type: "Point",
      coordinates: [4.8557, 45.758] 
    },
    arrivee: {
      type: "Point",
      coordinates: [2.35, 48.85] 
    },
    position_actuelle: {
      type: "Point",
      coordinates: [2.351, 48.8602] 
    },
    itineraire: {
      type: "LineString",
      coordinates: [
        [2.3522, 48.8566], 
        [2.3510, 48.8585],
        [2.3505, 48.8600],
        [2.3499, 48.8641]
      ]
    },
    statut: "En cours",
    date_livraison: ISODate("2025-03-05T10:30:00Z") 
  }
);

```

### Implémentez une fonction pour mettre à jour la position d'une livraison.

```
function updatePositionLivraison(livraisonId, nouvellePosition) {
  db.livraisons.updateOne(
    { _id: ObjectId("67c7ebce75599966318d30be") }, 
    { 
      $set: {
        position_actuelle: {
          type: "Point",
          coordinates: [2.33,48.83]
        }
      }
    }
  );
}

```

### Créez une requête pour trouver toutes les livraisons en cours dans un rayon de 1km autour d'un point
donné.

```
db.livraisons.find({
  position_actuelle: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [4.8557,45.758] 
      },
      $maxDistance: 1000, 
    }
  }
});

```


# TP 3 : 

## 3.1 : 

### 1. Créez une pipeline d'agrégation pour calculer les statistiques par genre : 

```

db.livres.aggregate([
  { 
    $group: {
      _id: "$genre",
      nombre_de_livres: { $sum: 1 },
      note_moyenne: { $avg: "$note_moyenne" },
      prix_moyenne: { $avg: "$prix" },
      prix_min: { $min: "$prix" },
      prix_max: { $max: "$prix" } 
    }
  }
])


```

### 2. Analysez la répartition des lires par éditeur : 

```

db.livres.aggregate([
  { 
    $group: {
      _id: "$editeur",
      nombre_de_livres: { $sum: 1 },
      auteur: { $sum: 1 },
      genre: { $sum: 1 },
      note_moyenne: { $avg: "$note_moyenne" },
    }
  }
])


```

### 3. Enrichissez votre collection d'utilisateurs avec des données d'emprunt si ce n'est pas deja fait, puis créez une pipeline d'agrégation pour analyser les habitudes d'emprunt par ville d'utilisateur : 

```

db.emprunts.aggregate([
  { 
    $group: {
      _id: "$ville",
      total_emprunts: { $sum: 1 },
      total_livres_empruntes: { $sum: 1 },
      dates_emprunt: { $push: "$date_emprunt" },
    }
  },
  { 
    $project: {
      _id: 1,
      total_emprunts: 1,
      total_livres_empruntes: 1,
      dates_emprunt: { $size: "$dates_emprunt" },
    }
  }
])

```

## 3.2 : 

### 1. Analysez les durées d'emprunt en calculant la durée moyenne, la durée minimale et maximale et le % d'enprunt retourné en retard : 

```



```

### 2. Utilisez l'opérateur $lookup pour joindre les collections livres et utilisateurs afin d'analyser quels livres sont les plus empruntés et par qui : 

```

db.emprunts.aggregate([
  {
    $lookup: {
      from: "livres",
      localField: "livre_id", 
      foreignField: "_id", 
      as: "livre_details" 
    }
  },
  {
    $lookup: {
      from: "utilisateurs",  
      localField: "utilisateur_id", 
      foreignField: "_id", 
      as: "utilisateur_details" 
    }
  },
  {
    $unwind: "$livre_details"
  },
  {
    $unwind: "$utilisateur_details"
  },
  {
    $group: {
      _id: "$livre_details.titre",  // Regrouper par le titre du livre
      nombre_emprunts: { $sum: 1 },  // Compter le nombre d'emprunts pour chaque livre
      utilisateurs: { $addToSet: "$utilisateur_details.nom" }  // Ajouter les noms des utilisateurs ayant emprunté ce livre
    }
  },
])

```

### 3. utilisez l'opérateur $facet pour créer un tableau de bord complet avec plusieurs analyses en une suel pipeline : 

- Statistiques globales ( nombre total delivres, prix moyen, notes moyenne )
- Top 5 des livres les mieux notés
- Repartition par langue
- Répartition par décennie de publication

```

db.livres.aggregate([
  {
    $facet: {
      statistiques_globales: [
        {
          $group: {
            _id: null,
            nombre_de_livres: { $sum: 1 },
            prix_moyen: { $avg: "$prix" },
            note_moyenne: { $avg: "$note_moyenne" }
          }
        }
      ],
      
      top_5_livres_notes: [
        { $sort: { note_moyenne: -1 } },
        { $limit: 5 },  
        {
          $project: {
            titre: 1,
            note_moyenne: 1
          }
        }
      ],
      
      repartition_par_langue: [
        {
          $group: {
            _id: "$langue",
            nombre_de_livres: { $sum: 1 }
          }
        },
        { $sort: { nombre_de_livres: -1 } } 
      ],
      
      repartition_par_decennie: [
        {
          $project: {
            titre: 1,
            decade: { $floor: { $divide: [{ $year: "$date_publication" }, 10] } }
          }
        },
        {
          $group: {
            _id: "$decade",
            nombre_de_livres: { $sum: 1 }
          }
        },
        { $sort: { _id: 1 } }
      ]
    }
  }
])

```

## 3.3 : 

### 1. Créez un système de recommandation basique qui suggère des livres à un utilisateur en fonction des genres qu'il à deja empruntés : 

```



```