# Réponses – Partie 02 : Packages et classes modèles

## Q2.1 – Rôle des packages

- **sqflite :** permet à Flutter de communiquer avec une base de données SQLite pour ajouter, lire, modifier et supprimer des données.
- **path :** permet de construire correctement les chemins des fichiers, notamment le chemin de la base de données.
- **sqflite_common_ffi :** permet d'utiliser SQLite sur Windows et Linux grâce à FFI.

## Q2.3 – Classe Categorie

**Pourquoi `id` est-il de type `int?` ?**

Parce que l'identifiant peut être nul lors de la création d'une nouvelle catégorie. Il sera attribué automatiquement par la base de données lors de l'insertion.

**Pourquoi `toMap()` ajoute-t-il `id` uniquement s'il n'est pas null ?**

Pour laisser SQLite générer automatiquement l'identifiant si la catégorie est nouvelle.

## Q2.4 – Classe Produit

**Pourquoi utilise-t-on un constructeur `factory` plutôt qu'un constructeur classique ?**

Parce que le constructeur `factory` permet de créer un objet Produit à partir d'une Map provenant de la base de données et de convertir les valeurs dans les types Dart appropriés.

**Pourquoi écrit-on `(map['prix'] as num).toDouble()` au lieu de `map['prix'] as double` ?**

Parce que SQLite peut renvoyer le prix sous forme d'entier ou de nombre décimal. Le type `num` accepte les deux, puis `toDouble()` convertit la valeur en double.


# Partie 03 – Ouvrir et créer la base depuis Flutter

## Q3.2 – Questions de compréhension

**1. Que se passe-t-il si on écrit deux fois `DatabaseHelper()` dans le code ? Combien d'objets sont créés ?**

Un seul objet est créé. Grâce au constructeur `factory`, chaque appel à `DatabaseHelper()` renvoie la même instance. C'est le principe du Singleton.

**2. Que fait l'opérateur `??=` ?**

L'opérateur `??=` affecte une valeur à une variable uniquement si elle est égale à `null`. Ici, la base est ouverte seulement si `_db` est encore null.

**3. Pourquoi le `PRAGMA foreign_keys = ON` est-il placé dans `onConfigure` et non dans `onCreate` ?**

Parce que `onConfigure` est exécuté à chaque ouverture de la base. Cela permet d'activer les clés étrangères à chaque connexion, même lorsque la base existe déjà. `onCreate`, lui, est exécuté uniquement lors de la création initiale.

**4. Combien de fois `onCreate` est-il appelé dans toute la vie de l'application ?**

Il est appelé une seule fois pour un fichier de base de données donné, lors de sa première création. Il ne sera pas rappelé aux ouvertures suivantes, sauf si le fichier est supprimé ou si une nouvelle base est créée.
