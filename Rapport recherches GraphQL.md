# Rapport de Recherche sur GraphQL

## Introduction

GraphQL est un langage de requête pour les API et un runtime pour exécuter ces requêtes par rapport à vos données existantes. Développé initialement par Facebook en 2012 et devenu open-source en 2015, GraphQL offre une alternative flexible et efficace aux API REST traditionnelles.

GraphQL a été créé pour résoudre certains des problèmes rencontrés avec les API REST, tels que la sur-récupération et la sous-récupération de données, ainsi que la nécessité de multiples appels pour récupérer des données reliées. Depuis que sa spécification est devenue open-source, il a gagné en popularité et est maintenant utilisé par de nombreuses grandes entreprises, notamment GitHub, Twitter, Shopify...

## Historique et Contexte

1. Origines et Création
2012: L'origine de GraphQL remonte à Facebook encore lui ;) , où l'équipe de développement travaillait sur des solutions pour améliorer la performance et la flexibilité des API. À cette époque, les développeurs de Facebook cherchaient une méthode plus efficace pour récupérer des données pour leurs applications mobiles, qui devenaient de plus en plus complexes et exigeantes.
2015: Facebook a officiellement annoncé GraphQL au public et a publié le projet en open source. L'objectif était de proposer une alternative aux API REST en permettant aux clients de spécifier exactement les données dont ils avaient besoin, évitant ainsi la surcharge de données souvent associée aux API REST.
2. Adoption et Croissance
2015-2016: Après sa publication, GraphQL a rapidement gagné en popularité dans la communauté des développeurs. Plusieurs entreprises et développeurs individuels ont commencé à expérimenter et à adopter GraphQL pour ses avantages en termes de performance et de flexibilité.
2016: La GraphQL Foundation a été créée sous l'égide de la Fondation Linux pour gérer le développement et la direction futurs de GraphQL. Cette structure a permis d'assurer que GraphQL resterait ouvert et serait développé en collaboration avec la communauté.
3. Évolution et Innovations
2017: L'écosystème GraphQL a commencé à s'enrichir avec de nouveaux outils et bibliothèques. Des frameworks serveurs comme Apollo Server, GraphQL Yoga et des outils de développement comme GraphiQL ont été développés pour faciliter l'utilisation et le déploiement de GraphQL.
2018: De grandes entreprises comme GitHub, Twitter et Shopify ont adopté GraphQL, démontrant ainsi sa robustesse et sa capacité à gérer des charges de travail importantes et complexes.
2019-2020: GraphQL a continué à évoluer avec des améliorations apportées au langage de requête et de nouveaux outils pour le développement, la surveillance et la sécurité des API GraphQL. La spécification GraphQL a également été mise à jour pour inclure de nouvelles fonctionnalités et optimisations.
4. Adoption Généralisée et Standards
2020 et au-delà: Aujourd'hui, GraphQL est largement adopté par une grande variété d'industries et d'applications, allant des startups aux grandes entreprises. La communauté continue d'innover et de développer de nouveaux outils et pratiques autour de GraphQL, facilitant ainsi son intégration et son utilisation.

GraphQL a radicalement changé la manière dont les API sont conçues et consommées, offrant une flexibilité et une efficacité accrues par rapport aux méthodes traditionnelles comme REST. Son adoption continue de croître, et il est maintenant un standard de facto pour de nombreuses nouvelles applications API.

## Fonctionnement de GraphQL

### Langage de Requête

GraphQL permet aux clients de spécifier précisément les données dont ils ont besoin, et l’ordre dans lequel ils souhaitent les recevoir. Une requête GraphQL ressemble à cela :

```graphql
query {
  user(id: "1") {
    name
    email
    posts {
      title
      content
    }
  }
}
```

GraphQL spécifie la syntaxe mais on peut utiliser à peu près n’importe quelle base de données pour le stockage et n’importe quel langage de programmation pour l’API :
PHP a graphql-php
Scala a Sangria
Python a Graphene Python
Ruby a graphql-ruby
Javascript a graphql.js
und so weiter.

### Schéma et Types

#### Schéma

Le schéma est au cœur de GraphQL. Il définit les types de données disponibles et les relations entre eux. Lorsque les requêtes arrivent, GraphQL les compare au schéma et exécute celles qui sont valides (si quelque chose qui n’est pas contenu dans le schéma est demandé, ce n’est pas valide). Le client peut interroger l’API sur son schéma, ce qui lui fournit une documentation automatique, avant de lancer sa requête, ce qui évite les requête invalides.

Voici un exemple de schéma simple :

```graphql
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
}
```

#### Types

Le schéma est constitué de types d’objets, qui définissent le genre d’objet qu’il est possible de demander et les champs qu’il contient.
Les typages sont forts, ce qui permet de vérifier les données récupérées.
Les types scalaires (ou primitifs) admis dans GraphQL sont `Int`, `Float`, `String`, `Boolean` et `ID`, et ils servent à définir le contenu des objets qu’on veut utiliser (entier, décimal, chaîne, booléen et ID).
On décrit les objets en leur donnant un nom (ex : Fruit) et une liste d’attributs appelés “champs”, dont la valeur doit donc correspondre à un des types scalaires (ex : nom du fruit -> String ; comestible -> Boolean ; prix -> Int ou Float). (en réalité, pour un prix, il vaut mieux enregistrer un Int en centimes, ça évite bien des erreurs causées par le Float)
ID est spécial, il sert d’identifiant unique pour chaque item (par exemple s’il y a 2 pommes, on les différencie par leur ID).
Pour définir un objet type, on utilise le mot-clé “type” comme dans le code juste au-dessus, et le nom de l’objet prend une majuscule. Le contenu de l’objet est décrit dans des accolades.

On peut avoir besoin d’une liste d’objets (ex : la liste des commandes qu’a passées un client, ou la liste des fruits dans une de ces commandes). La liste est caractérisée par des crochets, ce qui n’est guère dépaysant.

On peut aussi utiliser des énumérations qui se définissent par le mot-clé enum, et dont le contenu est écrit en majuscules et _ (tiret du bas), par exemple :
```
enum SeasonEnum {
    FALL
    WINTER
    SPRING
    SUMMER
}
```

Un champ peut être “nullable” ou pas (a-t-il le droit d’être vide ?), ce que l’on précise avec un !
Ex : ID! signifie que l’objet a forcément un ID.

Le schéma contient deux espèces de super-types que sont Query et Mutation.Ils sont considérés comme un type “racine” ou “root” parce que c’est le point d’entrée pour récupérer toutes les données auxquelles on peut accéder. Ils sont définis de la même manière que les autres types, avec le mot-clé et les accolades. 


### Résolveurs

Les résolveurs sont des fonctions qui connectent les types de GraphQL à vos données réelles. Un résolveur pour récupérer un utilisateur par ID pourrait ressembler à ceci :

```javascript
const resolvers = {
  Query: {
    user: (parent, args, context, info) => {
      return dataSource.getUserById(args.id);
    }
  },
  User: {
    posts: (user, args, context, info) => {
      return dataSource.getPostsByUserId(user.id);
    }
  }
};
```

## Avantages de GraphQL

### Flexibilité

GraphQL permet aux clients de demander exactement les données dont ils ont besoin, réduisant ainsi la sur-récupération et la sous-récupération de données.
**Sur-récupération** : même si un objet contient une multitude d’attributs, on ne récupère que les attributs précis que l’on a demandé. 
**Sous-récupération** : ce qui en REST se présente sous plusieurs endpoints peut être récupéré en une seule requête (le serveur se débrouille pour rapporter les données demandées).

### Performance

En réduisant le nombre de requêtes nécessaires pour récupérer des données reliées, GraphQL peut améliorer les performances, en particulier sur les réseaux mobiles lents.

### Écosystème

GraphQL a un écosystème florissant avec de nombreux outils et bibliothèques pour faciliter son utilisation, comme Apollo Client pour les clients JavaScript et GraphiQL pour tester et documenter les API GraphQL.

## Inconvénients de GraphQL

### Complexité

La mise en place d'un serveur GraphQL peut être plus complexe que les API REST traditionnelles, nécessitant une compréhension approfondie des schémas et des résolveurs.

### Mise en Cache

La mise en cache des réponses peut être plus compliquée avec GraphQL par rapport aux API REST, car les réponses peuvent être très spécifiques à la requête.

## Écriture de requêtes et de mutations

Les requêtes sont les demandes de données et les mutations les ajouts, suppressions ou mises à jour de données. Dans les deux cas il s’agit d’une méthode POST (car GET est trop passif pour adapter la récupération des données au besoin réel).

## Mise en place d'un serveur GraphQL

On utilise un outil GraphQL côté client, comme Apollo, et un outil côté serveur, comme express-graphql ou graphql

## Cas d'Utilisation

### GitHub API v4

GitHub a migré son API vers GraphQL pour permettre aux développeurs de récupérer plus efficacement les données complexes. Par exemple, récupérer les informations sur les dépôts, les issues, et les pull requests en une seule requête.

### Shopify

Shopify utilise GraphQL pour permettre aux développeurs de personnaliser les expériences de commerce en ligne, offrant une grande flexibilité dans la récupération des données nécessaires pour les vitrines et les applications personnalisées.

## Conclusion

GraphQL représente une avancée significative dans la manière dont les API peuvent être conçues et utilisées. Sa flexibilité et son efficacité en font un outil précieux pour les développeurs modernes, bien que sa mise en œuvre puisse être complexe. À mesure que l'écosystème continue de croître, il est probable que GraphQL deviendra de plus en plus intégré dans les stratégies de développement d'API.

---

**Références**

1. [GraphQL Documentation](https://graphql.org/)
2. [Red Hat sur GraphQL](https://www.redhat.com/fr/topics/api/what-is-graphql)
2. [“comprendre en 5 minutes”](https://www.jesuisundev.com/comprendre-graphql-en-5-minutes/)
2. [GraphQL pour quoi faire](https://blog.octo.com/graphql-et-pourquoi-faire)
2. [Apollo GraphQL](https://www.apollographql.com/)
3. [GitHub GraphQL API](https://docs.github.com/en/graphql)
4. [Shopify GraphQL API](https://shopify.dev/api/admin-graphql)
