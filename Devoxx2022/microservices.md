# Et si les micro-services n'avaient rien à voir avec la technique

[Video](https://www.youtube.com/watch?v=_yTNqp3wMuU)

On a des fois des problèmes liées à la technique comme la performance par exemple, ou la sécurité (On veut isoler un composant tiers dont on a besoin).  
Mais parfois, non. Et si c'est pas technique c'est **humain**.

## Loi de conway

**les organisations qui conçoivent des systèmes [...] tendent inévitablement à produire des designs qui sont des copies de la structure de communication de leur organisation.**

On a des équipes distribuer => On fait des systèmes distribués.  
Et c'est souvent l'aspect humain qui va gagner.

=> Le management va décider de l'organisation technique.  
Qui communique avec qui ? Comment ?  

=> On va définir, restreindre, contractualiser la communication.    
On veut optimiser les communication avec les personnes avec lesquels j'ai besoin de travailler tous les jours.  

[Outil qui permet de mettre en avant les flux](https://github.com/TeamTopologies/Team-API-template)

## Equipe

### Taille

Plus on a d'interactions plus on a de bruit. Scrum 5 à 9, si on analyse les flux de com : `5 à 6`.  
=> Dumbar's number

### Charge cognitive

 - Charge intrinsèque : complexité du code
 - Extrinseque : Deploiement / config
 - Essentielle : Interactions avec les autres équipes

### Ownership

 - L'équipe possède le logiciel, en est maitre
 - Responsabilité de tout le monde

### Diversité 

 - Plus de créativité

### Equipe et non individu

 - Individus < Dynamique de l'équipe
 - Etat d'esprit

### Responsabilités
 - Limiter les charges cognitivees (Interessant comme point de vue)

## Team topologies

#### Topologies d'équipes

 - Stream-aligned team (Sur un segment métier, à peu près feature team | **Principal**)
 - Enabling team (Aide, support)
 - Complicated subsystem team (Expertise spécifique requise, complexité de BDD ...)
 - Platform team (Qui aide à la livraison des autres équipes, Vitamin, CPE Stack, Turbine...)

#### Modes de communication

- Collaboration (Sur une période données, 2 équipes aligned )
- Facilitation (Aide, mentoring, ressources suppl...)
- X-as-a-service (Consomme le produit d'un autre)

### Réduction de la charge cognitive
 - Meilleurs pratiques de code : Enabling team
 - Meilleurs outils et processus : Complicated subsystem team | Platform team
 - Meilleurs docs, team API : X-as-a-service


=> Une equipe, gère son service, il est interessant de venir piocher dans les typos ci-dessus pour tacler des complexités. 