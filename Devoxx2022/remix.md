# Remix


## Intro 

Modèle 3-tiers ajd.  
Interface <> Application <> Données  

=> Ajd on a donc énormément de data qui transite.  

Ce qui implique : 
 - securité
 - temps
 - garantie
 - performances

### Solution ?

On supprime le dialogue interface <> application ?
> Application Multi-tiers

Client/Serveur <> données

Pragramme multi-tiers > Compilateur > 1 client, 1 serveur.

### Avantages du multi-tiers
Programmation multi tiers :
- programme qui va passer par une phase de compilation qui va Split le code en différents tier
- back/front co localisé
- moins d'erreur possible
- gain de temps
- moins de code
- meilleures maintenabilité
- refacto plus facile
- on garde les types, plus à soucier du type des appels serveurs
- partage de code (validation de formulaire)

### Outils 

**Remix** :
- SSR super facile (pré render, excellent pour l'ux)
- gestion des états
- progressive enhancement

Utilisation de hook dans la vu qui utilise le serveur.  
_On a mit en place des conventions dans l'équipe._

**Links** :
- communication bi directionnel
- contexte stocké côté client
- concurrence côté front

**Lambdera**:
- 3 tiers + infra
- ci cd automatiquement intégré
- élimination code mort
- excellent tooling
- evergreen migration
- gros vendor locking
- limite aux grosse charge (max 1Go ou 1000 requête par seconde)
- page "resons to not use lambdera)

