# 10 ans de Devoxx FR et de Java 

Jean michel DOUDOUX, [Video](https://www.youtube.com/watch?v=mQV4NVZlFnc)

## Intro

Pas mal de versions java depuis : 
 - 8 bien reçue
 - 11 mouais
 - 17 Oh yeah !

 - 9 Flop
 - Le reste RAS

## Le passé

### 8

 - Lambdas, références de méthode (ClassName::staticMethodName)
 - Stream
 - Date/Time
 - Optional
 - Annotations répétées

### 9
 - Les modules ! C'est surement ce qui va devenir une norme dans le futur mais peu utilisé ajd
 - JShell
 - List.of()...
 - Reactive Streams (Flow API)

### 10
 - var

### 11
 - API HTTP Client, async/sync, web sockets

### 14
 - Switch expressions (With -> )

### 15
 - Blocs de texte (with `""" """`)
 - Amélioration du NullPointerException pour plus de lisibilité

### 16
 - Pattern matching pour `instanceof`
 - **records** (Nouveau type, ce qui est rare), encapsulation **immuable**
 - Stream::toList()

### 17
 - Classe scellées : Restreindre la déclaration (`Sealed`, classes peuvent étendre, classes peuvent implémenter les interfaces scellées)
 - LTS passage à 2ans, donc plus de migrations

### 18
 - UTF-8 par défaut (sauf sortie console)
 - Serveur web minimaliste
 - Améliorations dans le ramasse-miettes et la sécu

## Le présent

Le Java évolue oui, mais la JVM aussi => **Performance** et **sécurité**.

Pour migrer => **jdeps**, cartographie de ses dépendances. **jdeprscan** pour voir les APIs qui sont dépréciées.

## Futur
 - Projet Amber
 - Pattern matching
 - Panama
 - Loom
 - Valhalla