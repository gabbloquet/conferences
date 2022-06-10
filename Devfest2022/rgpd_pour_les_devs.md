# RGPD pour les développeurs

@fdelbrayelle

## Historique

 - Safari 1974
 - Loi informatique et liberté 1978 => Création de la CNIL
 - Représentation de la donnée 
 - Directive de 95
 - RGPD 2018 + droit national

La donnée est une mine d'or pour les entreprises. Donnée => information brute, information => transformée

## Outils

 - DPO (Personne responsable des exigeances de la RGPD)
 - Registre des activités de traitement (obligatoir à partir de 2500 salariés)
 - Analyse d'impacts sur la vie privée

## Personnes concernés

 - Personne concernée
 - Responsable de traitement
 - Sous-traitant
 - Qui controle => CNIL en France

cnil.fr/fr/guide-rgpd-du-developpeur

## Comment

 - Développer en conformité avec le RGPD 
 - Connaitre ce qu'est une donnée sensible / personnelle

=> Tout ce qui identifie une personne
=> Anonymisation (non reversible) ou pseudonymisation (reversible)

 - Préparer son développement

=> Mettre la RGPD dans son quotidien, dans son développement

 - Sécuriser ses environnements

=> Securisation homogène
=> Outils d'automatisation (Terraform)

 - Gérer son code source

=> 

 - Faire un choix éclairer de son architecture

=> OnPremise ? Cloud ?
=> Choix des supports de stockage
=> Cycle de vie des données personnelles
=> Localisation géographique, la CNIL met à dispo une carte

 - Sécuriser les sites web et applications

=> Sécu BDD
=> Sécu les communications (Kafka on fait du SSL, TLS 1.2 1.3)
=> Sécu les authent
=> Sécu les infra (backup, DRP)
=> Veille sur les vulné (Bulletin CERT)

 - minimiser les données collectées

=> YAGNI
=> réduire la précision
=> champs libres
=> eviter les données perso dans les logs
=> purge des données (dureedeconservation.fr) > Lifecycle policy sur certains cloud provider

 - Gérer les utilisateurs

=> ID uniques
=> Auth forte
=> Principe du moindre privilege
=> Tracer les activités

 - Maitriser les dépendances
 - QUalité du code et documentation
 - Tester vos applications
 - Informer les personnes

=> En pleine conscience, transparence

5000 indcidents par an référencés. 60% liés au cyber-attaque.

 - Gérer la durée de conservation
 - Développer les features de droits des personnes

=> droit à l'oubli, portabilité, non profilage, limitation => à développer, je souhaite que mes données soient supprimés

 - se protéger des attaques informatiques => (OWASP)

## Conclusion

 - (Se) sensibiliser en tant que dev
 - Avoir les bons reflexes
 - Appliquer les mesures de protection et de sécurité
