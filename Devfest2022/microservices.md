# Microservices

## Historique

Refonte d'une plateforme de paiement => **SOA**
=> Transaction ACID locale. Si une action fail, rollback.

Atomicity - Consistency - isolation - Durability
=> Tansaction manager => simplicité de dev mais Couplage technique, coût d'isolation, pas possible de faire du NoSQL

**Opération de compensation** => Ce qui change c'est la manière de les développer
Un peu moins ACID => LRA/SAGA, Confirmation / Compensation, on doit coder les opérations de compensation
=> Attention peut etre compliqué avec des systemes tiers, autres APIs, voir comment on peut opti

 - MicroProfile LRA (Défini des specs pour écrire des applications) 
 - Eventuate SAGA

[microservices.io](htpps://www.microservices.io)

**Versus**

Micropofile (Spec) => SpringBoot Jersey
Eventuate  (Plateforme OpenSource) => SpringBoot, Micronaut, Quarkus

Annotation **@LRA** à poser sur les endpoints, on configure la nature de la transaction, on a aussi **@Compensate** et **@Complete**.

Le LRA receptionne la commande, on fait notre logique métier, on COMPLETE.
La methode Compensate peut se lancer de manière concurrente.

Une Saga est composée d'étape, on vient invoquer les actions métier et on glisse dedans potentiellement les compensation. (Code placé au niveau du service)
Le compensation flow => On prend le fil d'execution à revert. Le flow normal est appelé Flow standard d'execution.
Eventuate défini ses propres tables en BDD pour fonctionner.

Microprofile => Plus facile à configurer, moins de couplage fonctionnel, courbe d'apprentissage un peu meilleur
SAGA => Moins de couplage technique, observabilité, meilleure généricité