# Tests d'interface

A voir ! Ci-dessous les notes de @Flo

## Intro 

Tester ce que font les utilisateurs, et pas ce que fait votre code
Avoir confiance en ses tests

### Contexte   
coverage 100% - toutes les méthodes exporté et testé unitairement - avec de gros composants
100% de coverage, pq ? Impression de projet fiable, satisfaisant, pas contre mais ne pas avoir un faux sentiments de sécurité
100% de coverage, attention : facile à atteindre en TU attention de tout tester ensemble - test très très technique - attention a ne pas faire de mauvais test juste pour avoir 100% de coverage
Ne pas tester le composant sans tester son utilisation
Tester les features et pas l'implem
Rends la modification de l'implemantation plus simple, la feature et ses tests fonctionnent toujours peut importe l'implemantation

### Live Doc  
100% de coverage n'est pas un objectif, mais va être atteind progressivement en testant chaques features
Mock dépendance externe (api browser)
Chaques nouvelles features doit être testé

### Correction de bugs
- caractérisation
- créé un test pour reproduire le bug
- corrige ce test en tdd
- optionnellement tester son ui

(A)TDD -> tests plus petit possible
=> Sur du legacy, test e2e parfois plus simple que d'écrire des TU ou TI

E2E ne pas en faire trop, **ne pas mock**, lourd à faire, a maintenir, idéalement tourne sur la prod pour un maximum de viabilité

=> Écrire ses tests en **gherkin pour le métier**, lors des démos par exemple, ou avec le QA
=> Scénario écrit avec le QA
  
Jest cuncumber pour écrire en gherkin

### Inconvénients du TDD
- le dev doit être à l'aise avec l'env de test
- bien comprendre le métier
- impossible en Snapshot testing 
- Être pragmatique sur le TDD, ne pas en être nazi
  
**Attention aux** :
  - composants trop large
  - A l'asynchrone
  - Peut-être décourageant pour un nouveau développeur
  - confiance au 100% de coverage
  - performance de tests
