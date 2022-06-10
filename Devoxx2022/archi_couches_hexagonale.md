# La fin des architectures en couches avec l’approche hexagonale

[Video](https://www.youtube.com/watch?v=p029gSnlnfU).

## Intro

Le classique modèle 3-tiers, Controller (Presentation) - Service - Repository (Persistance). Repository Hibernate => Magique.  

## Arrivée du nouveau PO

Mon nouveau PO a des besoins. Il faut que j'y réponde !  
Soucis de référence cyclique donc : `@JsonIgnore` bon marche pas... `@JsonView` marche !

Mais en faite on a fortement coupler mes objects de domaine et les objects qui transitent.

## La règle d'or

### Ports et Adapters

Alistair Cockburn

L'archi hexagonale c'est faire des ports et adapters.  
On a notre domaine que l'on protège et on veut qu'il puisse communiquer avec plein de trucs, persistance, presentation, brokers...  
On va faire des ports pour qu'ils puissent communiquer avec eux, adapters qui vont transformer notre domaine en objects techniques.

Il y a deux types de ports, les ports driven et driving.

<img width="715" alt="image" src="https://user-images.githubusercontent.com/25029077/167594566-5b4b16cf-2bd0-4d32-b5bc-6c9ab2668976.png">

### L'architecture Hexagonale

On décorréle la logique métier et la communication avec l'exterieur qui est technique.  
On isole notre domaine ce qui le rend facilement testable.

<img width="766" alt="image" src="https://user-images.githubusercontent.com/25029077/167595311-1bd90196-15bd-49a2-a35d-15fd77f794a2.png">

### DDD

#### Ubiquitous Language

Ubiquitous Language, Language commun avec le Business.  
Le UUID n'a pas de sens métier par exemple, donc on va créer une classe pour l'expliciter. D'ailleurs il est interessant de noter qu'en Java 17 on peut utiliser les records qui vont nous permettre de dégager les getters, pas de setters car `IMMUABLE`. (Osef de Hibernate)

<img width="802" alt="image" src="https://user-images.githubusercontent.com/25029077/167596197-7a0f5cb8-2cc6-4649-855f-039d96ed245f.png">

#### Services

Traduction de business Logique. Les règles : 

<img width="1090" alt="image" src="https://user-images.githubusercontent.com/25029077/167596697-0ddc6380-19e0-4f1c-bc37-5b40d9de9ae9.png">

#### CQRS

En parlant DDD on arrive souvent au CQRS, on vient séparer les parties lectures et écritures, séparation des usages, Event driven => avec les actions que font nos utilisateurs on peut etre capable de générer des actions par rapports à des événements.  
=> Event sourcing, Compte en banque. On a des mutations unitaires et on les joue une à une pour arriver à un état final.  

<img width="786" alt="image" src="https://user-images.githubusercontent.com/25029077/167597048-76502cfb-7a64-40af-b9bc-ef50b4c45240.png">

## Code

### Modulariser

 - Separation de dépendances

### Domaine

 - Classes métiers
 - Services associés
 - Interfaces

Contient : Libraries fonctionnelles et de tests.  
=> Pourquoi pas Lombok, Spring Context... Mais l'objectif est d'avoir le moins de deps possible.  

[Exemple concret à 29min](https://youtu.be/p029gSnlnfU?t=1740)

### Adapters

#### Hibernate 

**Classes**  
ArticleEntity...

**Mappers**  
ArticleEntityMapper...

#### Implémentation

`JpaArticleRepository` extends `ArticleRepository` 

Ce repository initialise `ArticleDao` qui est une interface extends de `JpaRepository<ArticleEntity, UUID>` et le mapper défini ci-dessus.  
Les fonctions du repo font donc la concatenation des 2, par exemple pour chercher un élément :    
`dao.findById(ID_TO_SEARCH).map(mapper::fromEntity);`

De même si on travail avec une queue : 

`RabbitArticleEventProducer` implements `ArticleEventProducer`.

#### L'application

 - la configuration (Une par couche / module)
 - L'application mère vient `@Import` les autres confs pour éxecuter toute l'application.

## Conclusion

Implique pas mal de choses (CONS) : 
 - Objects à courte durée de vie
 - Beaucoup de mappers
 - beaucoup de classes et donc de démultiplication
 - Overhead applicatif

PROS :
- Flexibilité du domaine
- Lisibilité
- Tests en isolation
- Gestion de contraintes de couches plus simples
- PLus facile de mettre en place DDD et CQRS
- Flexibilité avec les micro-services

Attention ça n'est pas pour vous : 
 - Pas de grosse complexité business
 - Petite app
 - Mauvaise gestion des objects à courte durée
 - Scripting

Pour vous si :
- Beaucoup de consomation d'APIs différentes, de databases...
- Agrégation d'objects complexes
- Event sourcing / CQRS

L'architecture, c'est ce qui impact le plus la manière d'écrire le code. 

