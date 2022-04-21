# Programmation reactive

## Intro

**La programmation impérative**

Déjà la programmation impérative, on fait une requete, on attend la réponse.

Problèmes : 
 - cout memoire et cpu
 - bridage serveur ou explosion de la JVM

Solutions ?
 - Scalling, augmentation des instances
   - mais du coup plus de maintenance

**La programmation reactive**

 - Orienté autour des flux de données
 - async et donc non bloquant
 - piloté par des messages.

Plusieurs messages et donc potentiellement plusieurs thread pour les traiter.
1 thread par coeur ou 2 thread.

**Performance** => 1000, 2000, 6000 users toujours la meme perf. On gagne pas en temps de chargement mais on scale bien plus.

**Reactive stream**, ses fonctions :
 - Publisher
 - Subscriber
 - Subscription
 - Processor (Publisher + Subscriber)

Subscriber (sub)=> publisher (onSubscribe)=> Subscriber (request)=> 

**Technos** : 
 - Flow (JDK9)
 - Reactor (spring)
 - RxJava

Spring => Spring MVC (tomcat), Spring WebFlux (netty)

**Typage** :   
n éléments : Flux<> 
0, 1 élément : Mono<>

**Operation** : 
 - merge
 - concat
 - flatmap
 - flatMapAny
 - ...
 
Backpressure :   
_Sur le flux on peut configurer un timeout, une duration, un nombre d'éléments à traiter..._
On peut donc controller son flux.

**Threads** :  

Exemple :
```java
Flux.just("a","b","c", "")
  .delayElements(Duration.ofMillis(1000))
  .map(String::toUpperCase)
  .take(3)
  .doAfterTerminate(() -> Log.info("DONE"))
  .subsribe((log) -> ...)

// result : A B C DONE
```

Exemple concret :
```java
repository.getAll()
  .onErrorHandler(() -> Log.info("toto"))
```

On peut tester ses flux :  
```java
flux
  .actionOne()
  actionTwo()
  .verify()
```

[Reactor test](https://projectreactor.io/docs/core/release/reference/#testing)

<img width="904" alt="image" src="https://user-images.githubusercontent.com/25029077/164452950-ddf65665-b133-4ff3-b417-e2dd0c9d57e9.png">

## Conclusion

 - Synthaxe deroutante
 - API Contaminante
 - Plus difficile à mettre en place sur un legacy (from scratch bien)


WebClient est l'alternative reactive au RestTemplate.
R2DBC est l'alternative reactive à JDBC. ATTENTION PAS DE PAGINATION, PAS DE JOINTURES, PAS DE FETCH.
SpringData, ReactiveCrudRepository.

```java
public interface ReactivePersonRepository
  extends ReactiveCrudRepository<Person, String> {

  Flux<Person> findByLastname(Mono<String> lastname);

  @Query("{ 'firstname': ?0, 'lastname': ?1}")
  Mono<Person> findByFirstnameAndLastname(String firstname, String lastname);
}
```

[Article](https://spring.io/blog/2016/11/28/going-reactive-with-spring-data)

Pour contourner le soucis de pagination spring a créer un fonction à l'API pour dealer avec. afterCallbackThread mais pas la même API.  

**Autres blocages** : 
 - Flux bloquant JRE
 - UUID generation
 - Toutes les librairies externes utilisants les flux bloquants (Logs ou autre, [Outil pour checker si on a des trucs bloquants](https://github.com/reactor/BlockHound)).

**Sécurité**

`ReactiveSecurityContextHolder` => `getContext` => `map(SecurityContext::getAuthentication)` => `map(principal...)` 

**Logging**

Actuator OK, pas de changement c'est facile.


## Alors ? On migre ?

Scale => OUI  
Vitesse => NON  

**Attention** 
 - Synthaxe spéciale (reactive)
 - Pas mal d'impact dans les services, dans le code, Mono, FLux, Reactor Context...
 - Deps : R2DBC mais pas de jointures, donc NoSQL ?
 - Attention aux libraries externes 

**Bon point**  
 - Peu ou pas d'impact sur la sécurité et devops