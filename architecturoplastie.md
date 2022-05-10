# Architecturoplastie hexagonale d’un backend Node.js : Opération à code ouvert

[Vidéo](https://www.youtube.com/watch?v=r2XMwAUqZBA).

L'objectif est de refactorisé ce projet OpenSource. Il est un réseau social de musique.

## Les mains dans le code

On commence par faire du refactoring de surface, **renaming**.  
On change quelques noms, BOOM ça casse...  

On peut aussi commencer par installer les linters :)

### Diagnostic

 - Problème de lisibilité du code
 - Couplage fort entre les couches (On touche un truc ça casse ailleurs)
 - Problème de SoC
 - Problème au niveau des tests (manque de pertinence et de ciblage, filet de sécu défaillant)

<img width="839" alt="image" src="https://user-images.githubusercontent.com/25029077/167640820-db1bdf03-0da0-4c5d-b2dd-448c5dcc77a4.png">

Opacité : Difficile de rentrer dans le code, peu de lisibilité.  
Rigidité : On change quelque chose on casse quelque chose d'autres.  
Immobilité : Difficile de bouger quelque chose pour faire plus propre.  

=> Pourriture logicielle

### Approval tests

On a des tests mais attention, ça peut être un piège les tests si ils ne sont pas pertinents.  

_Il nous faut un filet de sécurité pour pouvoir refactor_.  
On peut grâce aux tests graver dans le marbre ce que l'on attend de notre app. (APIs requests ?)  
**=> Approval tests, on va venir définir ce que l'on attend.**
On peut d'ailleurs avec [une librarie](https://github.com/approvals/ApprovalTests.Net) vous les différences entre ce qui était ce que l'on avait avant et maintenant.  

**Bénéfices** :
 - Cette technique nous permet de refacto quand on a perdu l'intelligence métier. (Baisse de l'opacité)
 - Filet de sécurité (Baisse de la viscosité)
 - Ciblage des erreurs (Baisse de l'opacité)

On part des APIs pour voir ce qui en sort, c'est un peu des tests d'inté mais attention ici on devine un peu ce qu'on attend en sortie on le sait pas vraiment. Dans le test d'inté on sait exactement ce que l'on veut en sortie.  

### Début du refactoring

On commence donc notre refacto :
 - Renommage de variables / de fonction (Baisse de l'opacité)
 - Reduction de charge cognitive, un seul niveau d'abstraction (Baisse de l'opacité)
 - Extraction dans des méthodes
 - Suppression des try/catch pour deal avec des promesses `await new Promise((reseolve) => monModel.createToto())`

### Soucis d'architecture

La solution pour pouvoir travailler les couches de manière indépendante s'appelle : **L'architecture Hexagonale**.
On inverse les dépendances, le domaine ne dépend plus de rien.  

<img width="753" alt="image" src="https://user-images.githubusercontent.com/25029077/167651153-627da063-cce0-4e87-aa5e-afced3986b0e.png">

**Complexité Essentielle / Complexité obligatoire**

<img width="763" alt="image" src="https://user-images.githubusercontent.com/25029077/167651451-97e3f1b7-4f5c-4191-a968-da2bfd61ecaf.png">

### Travail sur le Domaine

On travaille sur du petit et on grossit au fur et à mesure.  

1. On va d'abord faire un passe-plat tout bête, en y intégrant les interfaces et les types métiers, sinon copier/coller de l'existant
2. Bon on va essayer déjà de mettre des types d'entrée sorties pour nos interface donc **TYPESCRIPT** [TUTO](https://youtu.be/r2XMwAUqZBA?t=4440)

Maintenant, il faut faire en sorte que notre domaine ne dépende de rien, on va le découpler de notre BDD.  
1. On va créer des tests fonctionnels qui vont mettre en avant nos cas métiers (Le cas où on ajoute une musique à une playlist existante, et le cas non existant)
2. On va ensuite créer un contrat qui va nous permettre de dialoguer avec la BDD, aller cherche un truc, sauvegarder...
3. On ajoute les typages pour le Repository
4. On vient mettre à jour les tests fonctionnels en utilisant les nouvelles fonctions écrites, et utilisation de stub pour stuber les repositories

On arrive donc à ça :

<img width="711" alt="image" src="https://user-images.githubusercontent.com/25029077/167663715-a996a087-55ff-42b3-a6de-5b27b3462a8a.png">

On voit bien qu'on ne sort pas du domaine, c'est assez cyclique. D'ailleurs pour le moment il n'est pas connecté au reste du monde.  
On voit aussi la dépendance avec le Stub.

On a donc :
 - Créer des tests fonctionnels (Baisse de l'opacité et viscosité)
 - Défini et détourer les responsabilités (Baisse de l'opacité, immobilité et viscosité)
 - Isolé le domaine (Baisse de la rigidité et de la fragilité)
 - Typé, grâce à JsDOC (Baisse de l'opacité, fragilité et viscosité)

### Persistance

1. Création d'un repository infrastructure, création de la `collection` (car ici mongo)
2. On type de repository avec l'interface Repository définie dans le domaine et on défini les méthodes 
3. On lance les tests approvals pour voir si on a bien cablé
4. Création du type qui va représenter ce que l'on aura en BDD, en Mongo des objets `Document` 
5. On rempli les fonctions qui sont défini avec le contrat du domaine, et on créer les **Mappers** qui vont transformer les Documents en objets métier
6. On migre la donnée legacy que nous voulons fixer pour mieux la manipuler 

<img width="738" alt="image" src="https://user-images.githubusercontent.com/25029077/167729231-8f8dac85-92a3-45f3-a42b-2626b4c4ae17.png">

On a donc :
- Rendu la structure de données de la persistence explicite (Baisse de l'opacité et viscosité)
- Mis en place l'anti corruption layer (Baisse de la fragilité, rigidité et viscosité)
- Externalisé la migration de données (Baisse de la fragilité, rigidité et viscosité)

### Découpage du controller

1. Injection des dépendances pour le framework (Nest ou Spring, dans l'application souvent)

On a donc :
- Découplé les différentes couches, controller, domaine, persistance | Composition Root (Application) (Baisse de l'immobilité et la rigidité)

### Enfin

> Suppression des Approvals tests et créer des tests sur les **controllers** et la couche **persistance**.


## Conclusion

<img width="696" alt="image" src="https://user-images.githubusercontent.com/25029077/167730727-7c87b768-1342-49a1-a320-5198878eaaa3.png">