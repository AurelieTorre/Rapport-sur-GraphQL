
# GraphQL

## Langage de requête alternatif aux API REST

[documentation officielle](https://graphql.org)

# Concepts de base et avantages de GraphQL

GraphQL est un langage de requête pour les API et un environnement d'exécution pour exécuter ces requêtes sur vos données existantes. Développé par Facebook en 2012 et open-source depuis 2015, GraphQL permet aux clients de demander exactement les données dont ils ont besoin, rien de plus et rien de moins.

### Avantages de GraphQL

- **Flexibilité des requêtes** : Les clients peuvent spécifier exactement quelles données ils veulent recevoir, réduisant ainsi la surcharge de données.
- **Évolutivité** : Les schémas de GraphQL permettent de faire évoluer les API sans casser les requêtes existantes.
- **Récupération de données optimisée** : Les clients peuvent récupérer plusieurs ressources en une seule requête.
- **Types forts** : Le système de types de GraphQL permet de valider les requêtes avant leur exécution.

# Schémas et types dans GraphQL

Le schéma est au cœur de GraphQL. Il définit les types de données disponibles dans l'API, les relations entre eux, et les opérations que les clients peuvent effectuer.

### Types de base

- **Scalar Types** : `Int`, `Float`, `String`, `Boolean`, `ID`
- **Object Types** : Définit un type d'objet avec ses champs.
- **Enum Types** : Utilisé pour un ensemble fixe de valeurs possibles.
- **List Types** : Indique qu'un champ est une liste d'un certain type.
- **Non-nullable Types** : Spécifie qu'un champ ne peut jamais être nul.

### Exemple de schéma

```graphql
type Query {
  user(id: ID!): User
  posts: [Post]
}

type User {
  id: ID!
  name: String!
  age: Int
  posts: [Post]
}

type Post {
  id: ID!
  title: String!
  content: String
  author: User
}

```





----------

lien vers le figjam : [veille GraphQL](https://www.figma.com/board/TLVGpFIAWzaIyEB7VQqCwP/Veille-GraphQL?node-id=2-149&t=4vKE4oxVkVcw2aYa-0)

