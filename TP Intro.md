# Réponces TP 2 :

## Exercice 1 : Création d’un jeu de données :

```

db["ecommerce_produits"].insertOne({
		nom: "Sweat",
    description: "Sweat a capuche",
    prix: 12,
    stock: 3,
		categorie: "Vetements",
		sous_categorie: "Pulls et vestes",
    caracteristiques_techniques: {
      couleur: "Noir",
      Taille: "L",
      Matiere: "Coton",
      pays_fabrication: "France"
    },
		avis: [
      { id: ObjectId("60d21b4667d0d8992e610c85"), date: new Date("2023-01-15"), note: 5 },
      { id: ObjectId("60d21b4667d0d8992e610c86"), date: new Date("2023-02-20"), note: 3 },
    ],
		tags: ["premium", "qualite", "hiver"]
})

```

## Exercice 2 : Requêtes et lecture :

### 2.1 :

```

db.ecommerce_produits.find({
  "categorie": "Vetements"
})

```

### 2.2 :

```

db.ecommerce_produits.find({
  prix: {
    $gte: 50,
    $lte: 200
  }
})

```

### 2.3 :

```

db.ecommerce_produits.find(
    { stock: { $gt: 0 } }
)

```

### 2.4 :

```

db.ecommerce_produits.find({ "avis.3": { $exists: 3 } })

db.ecommerce_produits.find({ avis: { $size: 3 } })

```

## Exercice 3 : Mise à jour :

### 3.1 :

```

db.ecommerce_produits.updateMany(
    { prix: { $lt: 18 } },
    { $mul: { prix: 1.05 } }
)

```

### 3.2 :

```

db.ecommerce_produits.updateMany({
        prix: {
            $gte: 50,
            $lte: 200
        }
    },
    { $set: { promotions: [] } }
)

```

### 3.3 :

```

db.ecommerce_produits.updateMany({
        "categorie": "Vetements"
    },
    { $push: { tags: "Promotion" } }
)

```

### 3.4 :

```

db.ecommerce_produits.updateOne({ name: "sweat" }, {$inc: { stock: -1} })

```

## Exercice 4 : Requêtes complexes :

### 4.1 :

```

db.ecommerce_produits.find({
  tags: {
    $all: ["premium", "hiver"]
  }
})

```

### 4.2 :

```

db.ecommerce_produits.find({
    "categorie": "Vetements",
    stock: { $lt: 5 }
})

```

### 4.3 :

```

db.ecommerce_produits.find({
    avis: {
    $elemMatch: {
      note: { $gte: 5 }
    }
  }
})

```

### 4.4 :

```

db.ecommerce_produits.find({"categorie": "Vetements"})
    .sort({ prix: -1 })
    .limit(5)

```
