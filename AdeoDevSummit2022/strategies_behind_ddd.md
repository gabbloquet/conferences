# Strategies behind DDD

[Slides](https://speakerdeck.com/lilobase/the-strategies-behind-ddd-adeodevsummit-2022)

## Sprint 0

Sprint 0 : Java ? React ? Micro-services ?
Mais en realite nous devons nous focus sur **ce que l'on doit faire**.

**DDD is about alignment. What we need to do before start a new project.**
Aujourd'hui on renverse le pattern, on fait des choix techs et apres on essaie de les faire rentrer.

"We need the put the need of database before the user needs ..."

## Domain 

**Domain Mapping** - Business Domain Classification  
_Only one VP should be angry at a time._

Your domains contain domains.

Core (sub)domain (In-House development with your best team, high cost and quality) > Supporting subdomain (in house development with externals)> Generic subdomain (external, pas notre valeur)

Par exemple on peut utiliser Shopify et stripe sur le site ecom, pour l'inventaire une autre solution du marché (ShipMonk).
Par contre le catalogue serait en interne.

**NOUS DEVONS ALIGNER LES TEAMS ET L'ARCHITECTURE DES SOFTS.**

## Context 

**DDD is all about context**

Discussion with experts, learn about company strategy.

On adapte l'architecture au besoin aue l'on veut adresser.
Faire de l'event storming, du CQRS sur le publishing, CRUD sur le e-commerce.

Attention CRUD a tendance à cacher le domain.

Bounded Context != Deployment.

## Boundaries

**DDD is all about boundaries**

Attention d'un context à un autre la comprehension d'un objet n'est pas la meme.

Exemple avec le context Authoring et Publishing. La definition d'un livre n'est pas la meme entre les 2.

REX sur la facturation : le prix ayant evolue entre le moment de l'achat et la facture les factures étaient caduques.

Nous devons avoir une strategie pour comprendre notre domain.

**DRY**, don't cross domains, on utilise des APIs ou des EventBus que l'on a traduit, des contract entre un context composé de domaines et un autre.

Aggregates > Entites sharing the same lifecycle.  
Aggregate root to interact with entities. Book, composé de chapitres, on dialogue avec le Book aui est le root pour ajouté un chapitre.  
Attention à ne pas faire de nos aggregates des four-tout.

ACL : piece of software que l'on met entre 2 domaines pour faire dialoguer les domaines.


## Compassion

**DDD is all about compassion**

On a tous des defn differentes des mots.  
On doit contextualisé et créer des glossaires.  

**DDD for any projects** (Dans Accelerate)
