# TP Final jour 1 :

## Partie 1 :

### Insertion de documents (Create) :

#### Livres :

```

db["livres"].insertMany([
  {
    titre: "Le Petit Prince",
    auteur: "Antoine de Saint-Exupéry",
    annee_publication: 1943,
    editeur: "Gallimard",
    genre: ["Conte", "Philosophie"],
    nombre_pages: 96,
    langue: "Français",
    disponible: true,
    stock: 3,
    note_moyenne: 4.8,
    description: "Un pilote d'avion, qui s'est écrasé dans le désert du Sahara, rencontre un jeune prince venu d'une autre planète...",
    prix: 7.50,
    isbn: "9782070612758",
    date_ajout: new Date("2023-01-15")
  }
])

```

#### Utilisateurs :

```

db["utilisateurs"].insertMany([
  {
    nom: "Dupont",
    prenom: "Marie",
    email: "marie.dupont@example.com",
    age: 28,
    adresse: {
        rue: "123 Avenue des Livres",
        ville: "Lyon",
        code_postal: "69002"
    },
    date_inscription: new Date("2022-12-10"),
    livres_empruntes: [
        {
        livre_id: ObjectId("67c6b6a153ee5b576572eb14"),
        titre: "Le Petit Prince",
        date_emprunt: new Date("2023-02-15"),
        date_retour_prevue: new Date("2023-03-15")
        }
    ],
    tags: ["fiction", "histoire"]
  }
])

```

## Partie 2 :

### Requêtes de lecture (Read) :

#### 2.1 Listez tous les livres disponibles (si vous utilisez le dataset Amazon, ajoutez d'abord un champ `disponible` à vos documents) :

```

db.livres.find({
  stock: { $gt: 0 },
  disponible: true
})

```

#### 2.2 Trouvez les livres publiés après l'an 2000 :

```

db.livres.find({
  annee_publication: { $gt: 1800 }
})

```

#### 2.3 Trouvez les livres d'un auteur spécifique :

```

db.livres.find({
  auteur: "Alexandre Dumas"
})

```

#### 2.4 Trouvez les livres qui ont une note moyenne supérieure à 4 :

```

db.livres.find({
  note_moyenne: { $gt: 4 }
})

```

#### 2.5 Listez tous les utilisateurs habitant dans une ville spécifique :

```

db.utilisateurs.find({
  "adresse.ville": "Lyon"
})

```

#### 2.6 Trouvez les livres qui appartiennent à un genre spécifique :

```

db.livres.find({
  genre: {
    $in: ["Fantasy", "Aventure"]
  }
})

```

#### 2.7 Trouvez les livres qui ont à la fois un prix inférieur à 15€ et une note moyenne supérieure à 4 :

```

db.livres.find({
  note_moyenne: { $gt: 4 },
  prix: { $lt: 15 }
})

```

#### 2.8 Trouvez les utilisateurs qui ont emprunté un livre spécifique (par titre) :

```

db.utilisateurs.find({
    "livres_empruntes.titre" : "Le Petit Prince"
})

```

## Partie 3 :

### Mise à jour de documents (Update) :

#### 3.1 Mettez à jour le titre d'un livre spécifique :

```

db.livres.updateMany(
    { titre : "Le Petit Prince" },
    { $set: { titre : "Le Petit Prince 2" } }
)

```

#### 3.2 Ajoutez un champ `stock` à tous les livres avec une valeur par défaut de 5 :

```

db.livres.updateMany(
    {},
    { $set: { stock : 5 } }
)

```

#### 3.3 Marquez un livre comme indisponible (disponible = false) :

```

db.livres.updateMany(
    { titre : "Le Petit Prince 2" },
    { $set: { disponible : false } }
)

```

#### 3.4 Ajoutez un nouvel emprunt dans la liste `livres_empruntes` d'un utilisateur :

```

db.utilisateur.updateOne( {
    name: "Dupont" }, { $set:  }
)

```

#### 3.5 Changez l'adresse d'un utilisateur :

```

db.utilisateurs.updateMany(
    { nom : "Dupont" },
    { $set: { "adresse.rue" : "42 Quais Perrache" } }
)

```

#### 3.6 Ajoutez un nouveau tag à un utilisateur :

```

db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $addToSet: { tags: "Horreur" } }
)

```

> note : $addToSet : Ajoute au tableau sauf si valeur deja present.

#### 3.7 Mettez à jour la note moyenne d'un livre :

```

db.livres.updateOne(
    { titre: "Le Petit Prince 2" },
    { $set: { note_moyenne: 5 } }
)

```

## Partie 4 :

### Suppression de documents (Delete)

#### 4.1 Supprimez un livre spécifique par son titre :

```

db.livres.deleteOne(
    { titre: "Le Petit Prince 2" }
)

```

#### 4.2 Supprimez tous les livres d'un auteur spécifique :

```

db.livres.deleteMany(
    { auteur: "George Orwell" }
)

```

#### 4.3 Supprimez un utilisateur par son email :

```

db.utilisateurs.deleteOne(
    { email: "test-email@random-email.com" }
)

```

## Partie 5

### Requêtes avancées et projection :

#### 5.1 Listez tous les livres triés par note moyenne (ordre décroissant) :

```

db.livres.find({}).sort({ note_moyenne: 1 })

```

#### 5.2 Trouvez les 3 livres les plus anciens :

```

db.livres.find({}).sort({ annee_publication: 1 }).limit(3)

```

#### 5.3 Comptez le nombre de livres par auteur :

```

db.livres.aggregate([
    { $group: { _id: "$auteur", count: { $sum: 1 } } }
])


```

#### 5.4 Affichez uniquement le titre, l'auteur et la note moyenne des livres (sans l'id) :

```

db.livres.aggregate([
    { $project: { _id: 0, titre: 1, auteur: 1, note_moyenne: 1 } }
])

```

#### 5.5 Trouvez les utilisateurs qui ont emprunté plus d'un livre :

```



```

## Partie 6 : 

### Modélisation de données (Mini-exercice) : 

#### 6.1 Modèle embarqué vs référence : 

##### 6.1.1 Créez une nouvelle collection `emprunts` qui utilise des références vers les livres et les utilisateurs : 

```

db["emprunts"].insertMany([
    {
        utilisateur_id: ObjectId("67c6b81fc75697932d43b45e"),
        livre_id: ObjectId("67c6b6a153ee5b576572eb16"),
        date_emprunt: new Date("2023-02-20"),
        date_retour_prevue: new Date("2023-03-20"),
        date_retour_effective: null,
        statut: "en cours"
    }
])

```

##### 6.1.2 Insérez au moins 3 emprunts dans cette collection : 

```

db["emprunts"].insertMany([
    {
        utilisateur_id: ObjectId("67c6b81fc75697932d43b45e"),
        livre_id: ObjectId("67c6b6a153ee5b576572eb16"),
        date_emprunt: new Date("2023-02-20"),
        date_retour_prevue: new Date("2023-03-20"),
        date_retour_effective: null,
        statut: "en cours"
    }
])

```

##### 6.1.3 Comparez cette approche avec celle où les emprunts sont directement intégrés dans le document utilisateur : 

```

db.utilisateurs.updateOne(
    { nom: "Dupont" },
    { $push: { livres_empruntes: { livre_id: ObjectId("67c6b6a153ee5b576572eb16"), date_emprunt: new Date("2023-02-20"), date_retour_prevue: new Date("2023-03-20"), date_retour_effective: null, statut: "en cours" } } }
)

```

#### 6.2 Réflexion sur la modélisation

##### 6.2.1 Quels sont les avantages et inconvénients de chaque approche ? : 

- Avantages : Moins de duplications et plus facile à mettre à jour. Si un livre change, nous ne sommes pas obligés de modifier tous les emprunts associés.

- Inconvénient : On doit faire des jointures ou utiliser des aggrégations pour récupérer des données. (Requête plus complexe)

##### 6.2.2 Quelle approche privilégieriez-vous pour une application réelle et pourquoi ? : 

- Il vaut mieux privilégier l'approche avec une collection "emprunts", car elle est plus flexible et permet de mieux gérer ses données pour une mise en production.

##### 6.2.3 Comment modéliseriez-vous les cas où un même livre peut exister en plusieurs exemplaires ? : 

- Il faudrait créer une collection "exemplaires" qui fait référence à chaque livre. Puis ensuite lier les emprunts à des exemplaires spécifiques.