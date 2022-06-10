# S'affranchir de la Pyramide des Tests

Revoir le replay pour choper le principe 1. Conf intéressante, niveau debutant/medium.

Jonathan Boccara est développeur et architecte chez Doctolib où il travaille sur une vaste base de code dans plusieurs langages.

## intro

_La pyramide nous dit de faire le moins de test end-to-end possible car ils sont fragiles et lent._

### Les tests unitaires

**Pros**
 - Fast to run
 - Easy to locate
 - Easy to debug

**Cons**
 - hard to maintain

Nous devons commiter, nous engager lorsque nous écrivons des tests.

**Test Composant** (front like React)  
On mock la partie serveur (msw)

**Test API**  
On mock ce que nous envoie le front, mock Controller.

Mais si ça bouge ? comment on fait ?  
=> **Test de contrat**  
MAIS on se retrouve à avoir les tests fois 3 sur cette `interface`.

## Les tests E2E

Il nous donne accès à modifier tout ce qu'il y a à l'intérieur sans casser. 

**Pros** :
 - Flexible
 - Demande le moins de mock
 - Permet de tester la UX

Mais on nous dit fragile et lent, _vraiment_ ?

### Les tests E2E en 2022

En 2022 chez Doctolib : Cassandra + Capybara

=> Exemple, si le boutton change de place, de couleur, si d'autres éléments sont ajoutés **ça casse pas**.

D'ailleurs permet aussi de tester l'accessbilité.

En fait en ayant le bon tooling et si on investi dans la qualité de code de tests... **Les tests E2E peuvent faire l'affaire.**

**Principle 2 : have good test tooling and high test code quality.**

### Really slow ?

Oui si on compare à des tests unitaires oui.  
Mais on peut les lancer sur plusieurs machines ? Ou n'en lancer que certains ? Mais lesquels ?  

Speed is one of several variables. C'est un trade off.
**Principle 3 : Testing is about understanding tradeoff.**

## Improving tests

### Pourquoi on test ?

On veut pas casser : 
 - Le geste utilisateur
 - consommateur use cases (sending request to our backend)
 - les devs use cases (logs)
 - la data (usage metrics)

User use cases > User research > PM > Developer > Tested use cases

**Principle 4 : Good tests reflet usage of the products**

Il faut être plus sensible au produit.

## Comment on écrit des tests alors ?

Avant Unit > Integration > E2E, si on veut tester du sol au grenier why not.

Donc : **Voir les principes**.

**Principle 1 : be sure that interface contact don't move**  
**Principle 2 : have good test tooling and high test code quality.**  
**Principle 3 : Testing is about understanding tradeoff.**  
**Principle 4 : Good tests reflet usage of the products**  

Et **COMPRENDRE POURQUOI ON TEST**.


**Questions**

Pas de Code coverage sur le E2E (en tant que dev leader) ?  
Nous on regarde pas le code coverage, on regarde si on a couvert nos cas métier.   