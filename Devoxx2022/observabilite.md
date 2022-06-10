# Observabilité

Bonne intro, niveau débutant.

Pierre ZEMB de Clever Cloud.  

## Introduction

On se fait des representations mentals de nos produits, le problème c'est quelles peuvent être fausses.  

L'application ne créer de la valeur que si elle tourne en prod, on veut donc s'assurer que ça tourne bien, on va observer ce qu'il se passe.  
Il y a beaucoup de soucis qui peuvent se produire. L'observabilité c'est :  
_A quel point mon service fonctionne bien ? Est ce que je réponds vite ? Est ce que ma mémoire est OK ?_  

## De nombreuses problématiques

 - On peut avoir des symptomes loin de la cause parfois
 - Plusieurs root cause
 - le system est dynamique 
 - Trouver un bug sur un gros service peut etre vraiment difficile

Le debugging c'est trouvé la root cause.

## Debugguer

 - Poser des questions
 - Faire des observations
 - goto 1

Un senior en astreinte pose des questions et il connait les questions à poser.  
L'objectif est donc d'écrire le code, les logs qui vont nous permettre de répondre à ces questions.  

### Méthode USE

[Lien](https://www.brendangregg.com/usemethod.html)

Terminology definitions:

 - resource: all physical server functional components (CPUs, disks, busses, ...)
 - utilization: the average time that the resource was busy servicing work
 - saturation: the degree to which the resource has extra work which it can't service, often queued
 - errors: the count of error events

### Méthode RED

 - Rate
 - Errors
 - Duration

### Méthode Four golden signals

 - **Latence**, combien de temps mon app met à répondre
 - **Trafic**, charge de mon app
 - **Nombre d'erreurs** et quel type ?
 - **Saturation**, à quel point mon systeme manque de capacité ?

## Poser ses points d'observation

 - Static instrumentation
   - Log
   - Metrics
   - Trace
 - Dynamic instrumentation (j'ai un programme qui tourne, je me branche dessus et je cherche des infos dessus)
   - ebpf
   - dtrace

### Logs

On affiche des messages d'erreurs : 
 - Contexte
 - Erreur en elle-même (j'arrive pas à ouvrir le fichier)
 - **Et pontentiellement la solution pour résoudre si problem il y a !**

### Metrics

Metrics: These are the values represented as counts or measures that are often calculated or aggregated over a period of time. Metrics can originate from a variety of sources, including infrastructure, hosts, services, cloud platforms, and external sources.

 - Temps de réponse
 - Trafic
 - Erreurs 400, 500...
 - Charge CPU, charge RAM...

### Tracing

On voit se qu'il se passe pendant une requete. 
Natif sur dynatrace on peut voir combien de temps le client passe sur tel ou tel service, tel repository, controller... PurePath.  

## Visualisation

Le cerveau humain est très fort pour trouver des erreurs, servez-vous en.  

Dashboard  
Flame graph  

