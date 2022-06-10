

Christophe Furmaniak

Kubernetes for the many.

# GitOps

 - Déclaratif, je veux un bucket comme ci, comme ça (On fait pas un enchainement de requêtes)
 - Versionné et immutable
 - Des déploiements automatisés
 - Reconcialiation permanente (les agents logiciels s'assurent qu'il n'y a pas de divergeance)

La source de vérité c'est **GIT**.

# Outils 

 - ArgoCD
 - FluxCD (supporte HELM, le seul qui supporte les release HELM)
 - Les autres => Faros, Kapp-controller

# Observabilité

Comment je sais que ça s'est bien passé ? QUe ce que j'ai décrit fonctione bien ?

=> ligne de commande `flux` ou `kubectl`

# Mes secrets

Git, pas Git ?

## Git

=> **Chiffrer** dans GIT

`sops` ou `kubeseal`, seul l'opérateur qui tourne dans le cluster pourra décrypter la clé. Seul celui qui l'a mise la connait en faite.  

## Coffre fort

**Vault**, mais les cloud providers gère leur truc

Je stock mon secret dans mon coffre fort, et dans **git => je stock le descripteur** qui va me permettre d'y accéder.  

=> Vault si j'ai déjà vault, sinon à voir la solution du cloud provider

# Scalling

HorizontalScalling => minReplicas & maxReplicas

# Retour en arrière

`git revert`

`git commit` pour pousser les modifs

# Comment je gère mes dépôts Git

Mes desc de stack ? mes desc applicatifs ? par équipe ? par BU ? par env ?

1. **pas dans le même repo que mon code** => Pas le même cycle de vie, change moins rarement, SoC...
2. **pas mettre la partie infra et la partie applicatives dans le même** => Pas le même cycle de vie, plus facile à auditer
3. **1 dépot application pour une équipe et ses différents environnements** (Création un par BU)

Autre options 
 - **Branche par environnement ET répertoires**, demande plus de gymnastique Git (PR Auto sur d'autres envs)
 - Branches orphelines par environnement => Encore plus de gym (cherry pick), peu recommandé

# Mise à jour de l'environnement

Est ce que je peux détecter les images quand je suis en PR, en dev ?

 - Option que GIT (je mets à jour le descripteur, l'opérateur gitops met à jour l'image)
 - **Scan de la registry**, ImageUpdateAutomation avec Flux, ArgoCD Image Updater

Date et heure
Nommage image : <branch>-<sha1>-<timestamp>

# Les PRs

Je veux créer un namespace dédié à ma PR, je veux lancer des tests de perf, des tests fonctionnels.

 - Utiliser des namespaces hierarchiques, qui permet de créer des **sous namespaces** qui partageront une conf commune.

Comment je pousse les descripteurs ?
 - Full GitOps (WebHooks pour déclencher la synchro)
 - Pas GitOps `kubectl apply`, d'ailleurs maintenant git gère le lien entre repo et kubernetes

# Conclusion

 - **Fail Fast, Shift left**
 - Step by step, progressivement on améliore
 - définir la manière dont on veut que dev et ops collaborent