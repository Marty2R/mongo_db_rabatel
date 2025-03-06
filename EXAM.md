# EXAM :

## Q1 :

A:
Il n'y a pas d'index pour optimiser la requête.

B :
db.produits.createIndex({ categorie: 1, prix: 1, date_ajout: -1 })

C:
db.produits.find({ categorie: "électronique", prix: { $gt: 300 } }).sort({ date_ajout: -1 }).explain("executionStats")

## Q2 :

Il faut utiliser $sum au lieu de $add dans : 
total: { $sum: "$montant" }

## Q3 :

Création du restaurant :

```
db.geofences.insertOne(
{
  "nom": "Restaurant Delices",
  "emplacement": {
    "type": "Point",
    "coordinates": [2.3488, 48.8530]
  },
  "zone_livraison": {
    "type": "Polygon",
    "coordinates": [
      [
        [2.3450, 48.8500],
        [2.3520, 48.8500],
        [2.3520, 48.8550],
        [2.3450, 48.8550],
        [2.3450, 48.8500]
      ]
    ]
  }
})

db.restaurants.find({
  "zone_livraison": {
    $geoIntersects: {
      $geometry: {
        "type": "Point",
        "coordinates": [2.3508, 48.8567]
      }
    }
  }
})

```

## Q4 :

Le model B est plus adapter a un blog avec un grand flux de données et offre une meilleure séparation des données, ce qui ameliore les performances et offre une plus grande flexibilitég.

## Q5 :

```

db.ventes.aggregate([
  {
    $match: {
      date_vente: {
        $gte: ISODate("2023-01-01T00:00:00.000Z"),
        $lt: ISODate("2024-01-01T00:00:00.000Z")
      }
    }
  },
  {
    $group: {
      _id: { $month: "$date_vente" },
      total_ventes: { $sum: 1 },
      montant_moyen: { $avg: "$montant" }
    }
  },
  {
    $sort: { _id: 1 }
  }
])

```

## Q6 :

On doit utiliser un index TTL.

db.sessions.createIndex({ derniere_activite: 1 }, { expireAfterSeconds: 86400 })

db.sessions.dropIndex({ derniere_activite: 1 });
db.sessions.createIndex({ derniere_activite: 1 }, { expireAfterSeconds: 172800 });
