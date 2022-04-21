# Model driven design

Eric Evans. Domain driven Design. Tackling bounded context. Ubiquitous Language.  
 - Autonomie
 - Gagner en clarté, lisibilité
 - Aligner besoin et contexte

## Rex

### Ubiquitous language

Rendre l'implicite explicite en premier. Qu'est ce qui est coeur ?  
> **Event storming** > Détection d'un domaine > Des sous domaines liés

Dans leur use case : 
 - Le domaine parent **suggestion**
 - Sous domaine **Réservation**

Exemple mapping.  
Responsibility mapping.  
Tous ces ateliers ont des impacts sur les domaines mentaux. On les fait au début car c'est méga important mais on continue pendant la création.  

On est attaché au geste métier que l'on veut modeliser, on établi un lien avec le métier.
On fait du **refactoring** constant, on itère !

### Model driven design

Le MDD c'est 2 choses : Deep model, supple model.  
Modeling developer, client developer.
Est ce que le code fait transparaitre le geste métier ?

#### Supple design

Supple design : 
 - Classe autonome
 - nom correspondant au business
 - assertions, test acceptances

[Article supple design](https://blog.engineering.publicissapient.fr/2018/09/06/craft-le-supple-design-en-ddd/).

Tous les éléments du supple design nous mène au **value object** (comparable selon ses propriété (non le hashcode))  
Les VO sont vos meilleurs mais quand vous codez, rendez les les plus populaires possibles dans votre code.  

#### Deep model

On commence par un scenario, on commence par un **PROTOTYPE** (on commence donc à l'extérieur de notre produit).  
Il faut s'abstraire de tous les éléments qui ne sont pas dépendant de ce scope.  

> Rien de mieux que de commencer par un exemple mapping

Après ça raffinement, mais en pensant au **métier**. Il est impossible de faire ça si on court, il faut prendre du temps.  
_Il faut sortir, aller courir, prenez du temps._  
ça créer de la cohésion, de la confiance, on créer une bonne ambiance d'équipe.  

2 écoles du TDD : 
 - **Outside/in**, la manière la plus naturel de répondre à une US
   - Assertion, créer le prototype, KISS, mettre les mots sur le domaine
   - Mettre le **deep model** dans un **directory dédié**

Quel plaisir de voir un code dans lequel il est facile de naviguer, onboard, dont on sait que ça représente au mieux le business. On connait mieux les règles au final que les experts domaines.  

#### Breakthrough

Breakthrough 
=> On va trouver des cas que les gens du business ont oublié.  
=> On va réduire le code en terme de complexité  
 - long conversation avec les gens du métier
 - Et même parfois de voir les domaines qui gravitent autour

#### MDD Take away

1. BOUNDED CONTEXT 
2. RENDRE EXPLICITE L'IMPLICITE => se comprendre 
3. DEEP MODEL / SUPPLE DESIGN 
4. ITERRATIVE REFACTORING 
5. PARTNERS OF DOMAIN EXPERTS => On va devenir fort 
6. BREAKTHROUGH => Réduire le code en terme de complexité

Responsibility mapping

