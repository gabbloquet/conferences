# Kafka

Producer > Topic > partition > offset

## basics

Kafka est stateful.  
le producer c'est difficile `<->` comme ça, le consumer c'est difficile `<----->` comme ça.
On écrit et reçoit des records (topic, clé => valeur).  
Les records sont écris sur les partitions, à un offset (où dans la partition).  
Chaque topic a une retention. Du coup le earlier offset commence peut etre pas à 0.

Création : 

```java
Map config = new HashMap();
new KafkaProducer(config);
producer.send(new ProducerRecord<String, String>("TOPIC", partition = 0, "key", "value"))
```

Le producer fait plusieurs choses :
 - metadata (bloquant)
 - serialize => key/value (bloquant)
 - partition (bloquant)
 - record accumulator (bloquant)

Producer multi-thread, consumer mono-thread.

En java on a un Patitioner par default. (hash en murmur32). Sinon on peut créer le notre.  
Attention il a qq soucis avec ce partitioner des fois ça marche pas bien avec les autres languages, il peut écrire dans les partitioners différents malheureusement.  

Utiliser la clé `null` pour écrire sur différentes partitions.  
Sinon il faut créer un Partitioner qui arrive à dispatcher de manière équitable toutes les données. 

## Setup up kafka

Installation : 
 - Kafka broker
 - Metadata
   - Zookeeper
   - Or kafka controllers (new)
 - Upgrade ? 
 - Scale ?
 - Secure ?
 - Balance ?
 
Kubernetes via Strimzi. Kubernetes est une API d'intention, je souhaite un cluster avec Kafka, avec 3 replicats etc...  
Il y a beaucoup de types de ressources kafka que l'on peut hoster sur k8s.  
=> Kafka connect, ecoute ce qu'il se passe sur une bdd pour le pousser dans un topic.

## Back to app

`Acks config`: "All" => Plus secure "1" => Utiliser dans plus de 80% des cas
Quand je pousse dans le topic, en 0 => je ne connais pas le offset sur lequel a été écrit la ressource. 1 => je l'ai.
On peut avoir Strimzi en local.  
Attention l'ordre est garanti par partition, pas par topic !  
Donc avec le retry on peut se retrouver avec des choses duppliqués,
si on veut assurer l'unicité :  

```yaml
max.in.flight.requests.per.connection=1  
enable.idempotence=true (default depuis 3.0.1)  
```

## Mettre à jour 

En cas d'update mettre à jour les clients et server petit à petit pour voir si ça crack qq part.
Puis à la fin mettre à jour le schema.  

## Replication

Kafka est résilient et on peut configurer le nombre de sauvegarde que l'on veut par **partition**.

**Conseils** : 
 - Replication factor d'au moins 3.
 - sync replica to replica factor - 1.
 - podDisruptionBudget 0 car on doit killer nous meme les PODS, car Kafka est **STATEFUL**.

L'écrire (le commmit) est couteux. La question que l'on se pose c'est quand est-ce qu'on commit ?  
A ce moment là on écrira dans _consumer_offsets où on en est. D'ailleurs on peut perdre le fil si jamais on nous kill le pod car on reviendra à cet offset.

## Consumption

En fait poll ne poll pas, `elle s'appelle fetch en interne`. Le poll fait enormement d'actions.  
Le polling thread est très important dans le kafka consumer : 
 - **All operations must be perform on that thread**
 - poll **blocks**
 - **You must call poll frequently**
 - **Règle 1 : do not block while processing the records**
 - **Règle 2 : process the record synchronously** (Non bloquant et synchro ?)
   - Donc on doit disable auto-commit
   - Mais **DO NOT COMMIT AFTER EVERY RECORDS**

## Comment je sais que mon kafka marche ?

Un kafka qui marche => mes producers peuvent écrire et poller sans soucis.  
Waiting for update `714` pour avoir ses metrics.

Sinon utilisation d'un canary => 
 - il produce/consume, une partition par broker
 - il va ensuite envoyer ces infos dans le système de metric

## Les offsets

On a 3 manières de commit : auto-commint, commit sync, commit async.  
Application > _Write a record_ > partition (créer le offset __consumer_offets) > replication * 2  

**ATTENTION AU LATEST ! IL EST PAR DEFAULT ! UTILISEZ PLUTOT LE EARLIEST !**
`auto.offset.reset=earliest`