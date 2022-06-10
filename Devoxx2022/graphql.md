# Comprendre GraphQL

3h, niveau débutant.

BFFs fails to scale. On se sert du BFF pour qu'on est côté front qu'une seule API à appeler, qui va lui appeler tout le monde. En ajoutant des APIs et des APIs ça devient vite compliqué à maintenir.
C'est là que GraphQL apparait.

## Comprendre GraphQL

Créer par Facebook en 2012, 2018 incorporé dans la fondation Linux.
[Spec website](https://spec.graphql.org)

### Basics

Language de requête, typé.

Le schema le plus petit possible : 
```
schema {
  query: Query
}
```
JSON like.  
Création de type : 
```
type Query {
  propertyName: MyType
}
```

Il y a aussi le type Mutation pour les mises à jour en BDD (POST).  
Ainsi que les Subscriptions qui est du steam de requête. (Flux de l'API reactor en kotlin, en dessous websocket chez spring)

**Les types de bases** : Int, Float, Boolean, String, enum, List. (Pas de MAP en GraphQL)  

```
type SpaceCat {
  name: String! => Ne peut être null
  age: int
  missions: [Mission!]
}

type Mission {
  departure: DepartureSite;
  ship: Rocket;
}
```

Ce qui est cool c'est que côté client on decide complement ce que l'on veut / attend de l'API. L'ordre des éléments, les éléments que l'on veut... 

En GraphQL le plus important c'est le SCHEMA. C'est le contrat que vous avez avec vos clients.

### Demo

[gscheibel/understanding_graphql-live](https://github.com/gscheibel/understanding_graphql-live)

#### Typescript

Deps : `appolo-server` & `appolo-server-core`.   
Resolver en JS, Data solver sur la JVM.  

```typescript
const typeDefs = gql`
  type {
    spaceCat: SpaceCat;
    spaceCatById(id: Int = 42): SpaceCat
  }
  
  type SpaceCat {
    name: String!
    age: int
    missions: [Mission!]
  }
`

const resolvers = {
  Query:  {
    spaceCat: () => {
      return {
        name: "neil carstrong",
        age: null
      }
    },
    spaceCatById: (parent, args, context, info) => {
      return {
        name: `neil carstrong ${args.id}`,
        age: null
      }
    }
  }
}

new AppoloServer({
  typeDefs,
  resolvers
})
```

le serveur se lance donc sur le port 4000 et donne accès au playground qui permet de requeter le serveur décrit.

#### Kotlin

[graphql-kotlin-demo](https://github.com/gscheibel/graphql-kotlin-demo)

Deps : `graphql-kotlin-spring-server` & `graphql-kotlin-hooks-provider` pour les fonctionnalités avancées.  

conf (application.yml) : 
```yaml
graphql:
  packages:
    - "com.apollographql.understanding_graphql"
```

On ne peut pas avoir de schema sans requête.

```kotlin
@Component
class SpaceCatQueries: Query {
  fun spaceCat() = SpaceCat() // Attention le premier élément doit être une fonction, sinon ça compile pas
}

data class SpaceCat(
  val name: String = "Neil catstrong",
  val lowMission: Mission = NearOrbitMission(orbit = Orbit.LOW), 
  val farAwayMission: Mission = HighOrbitMission(distance = 42) 
)

interface Mission

data class NearOrbitMission {
  val id: Int = 42,
  override val orbit: Orbit = Orbit.LOW
}: Mission

enum Orbit {
  LOW;
  HIGH;
}

data class HighOrbitMission {
    val distance: int;
}
```

=> Impossible d'utiliser les UUID car il est composé de beaucoup d'éléments publiques, il ne comprend pas l'élément à prendre donc génère une erreur.

### Advanced

=> Les appels ne sont pas asynchrones, si une propriété prend 2sec à charger, une autre 1sec, la query prendra donc un peu plus de 3 secondes.
=> On peut utiliser l'annotation `@defer` (directive côté client) pour décupéré les data dès que le serveur les a reçu (async) => N'est pas encore implementer dans les libs. (derrière c'est un multipart qui vient mettre au bon endroit ce qui a été reçu, on reçoit plusieurs json)
=> On peut déclarer une propriété comme étant `suspend` dans le schema pour ne pas bloquer le chargement (Asynchrone)

```kotlin
class Mission {
    suspend fun all(): List<String> { // du coup ici on charge tout et on aura le resultat d'un coup (en 2sec)
        val where = async { where() }
        val orbit = async { orbit() }
        
        List.of(where.await(), orbit.await()).flatten()
    }
    suspend fun where(): String = coroutineScope { // ASYNC, la data arrive petit à petit si on demande la mission, orbit puis where
        delay(2000)
        "Somewhere"
    }
    fun orbit(): String = coroutineScope {
        delay(100)
        Orbit.LOW
    }
}
```

=> On peut passer à la requete des conditions : 

```
query myQuery($condition: Boolean!) {
  spaceCat{
    mission {
      orbit @include(if: $condition)
      where
    }
  }
}
```

J'ai modélisé dynamiquement ma requete, en l'appelant si je passe : 
```
{
  "condition": false
}
```
je n'aurai donc pas orbit dans la réponse.

=> Utilisation des contexts, permet par exemple de définir des annotations que l'on pourra utiliser dans la requete @AdminOnly par exemple pour que le champs ne soit visible que par les admins.

Une requete contient 3 éléments :
 - query (string)
 - operationName (string)
 - variables {...}

la response aussi : 
 - data {...}
 - error []
 - extensions [] (meta données => tracing, tel champ a mis x ms pour être résolu)

**IMPORTANT** => **toutes les requetes sont des `POST`** et **retourne des 200**. Si on a un 500 c'est qu'on a vraiment un soucis, server eteint.

HATEOAS, on a beaucoup de requete/response, sur mobile en 3G c'est limite. Ici on a une requete qui peut en cacher X.

**Compatabilité ascendante** (on rajoute des champs des types pas de soucis, on le requete pas on sait pas qu'il existe) / **descendante** ? Versionning n'existe pas ! Très simple ! Sinon utilisation de la directive `deprecated` (nutilisez plus ce champs là, utilisez ce champs).  
En GraphQL on peut requeter X ressources en un coup (X champs de top niveau, on peut diguer sur des props à dix niveaux...).

### Archi - composition

On peut créer un **gateway** qui va dialoguer avec X APIs graphql qui appartiennent à X équipes, on va donc faire de la jointure de graphs.  
En gros, on fait un back for front :D
**Stitching** => composition de 2 schema (reverse proxy graphql, node ou brade sur la JVM powered by Atlasian mais...)

```javascript
const composedSchema: GraphQLSchema = stitchSchema({subSchemas, typeDefs, resolvers}) => {
  subSchemas: [missionSubGraph, catSubGraph],
  typeDef: gql`extend type SpaceCat {mission: Mission}`,
  resolvers : {
	  spaceCat: {
			
    }
	}
}
```

Chaque équipe bosse sur son graph, on sait que ça va pas péter en plus car la CI peut gérer les regressions.
On merge les schemas et ensuite on est capable de joindre les données. Grace au router (dev par appolo), utilisation de @link.
Il collecte des infos sur les sous APIs et fait des liens. Pour le consommateur c'est un seul graph, géré par le router.

Nombres de requêtes ? 1 on recupere les IDs (IDs des speakers des talks), puis l'autre api est appelée pour chercher les infos de ces IDs. (les appels sont optimisés, on a que 2 appels).

Chez Appolo => Algo de composition Rover (command) ou Studio (interface)

**ATTENTION, LA SECURITE EST TRES DIFFICILE A METTRE EN PLACE ! On peut se retrouver à consommer des props sur lesquels on a pas les droits !!**

### Notes hors conf

Exemple de consommation côté client : 
```javascript
request
  .get('http://localhost:3000/data')
  .query({
    query: `{
      hello,
      user(id: "${userId}") {
        name
        friends {
          name
        }
      }
    }`
  })
  .end(function (err, res) {
    debug(err || res.body);
    debug('friends', res.body.data.user.friends);
  });
```
Implémentation basique côté server (NodeJS - express) : 

```javascript
import {graphql} from 'graphql';

routes.get('/data', function* () {
  var query = this.query.query;
  var params = this.query.params;

  var resp = yield graphql(schema, query, '', params);

  if (resp.errors) {
    this.status = 400;
    this.body = {
      errors: resp.errors
    };
    return;
  }

  this.body = resp;
});
```

[Exemple de schemas](https://github.com/RisingStack/graphql-server/blob/master/src/server/schema.js)