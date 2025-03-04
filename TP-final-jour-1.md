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

db.utilisateur.deleteOne(
    { email: "test-email@random-email.com" }
)

```
