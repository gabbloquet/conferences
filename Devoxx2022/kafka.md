# Kafka

3h, niveau medium.

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

En faite le canary se comporte comme une vraie app.

## Les offsets

On a 3 manières de commit : auto-commint, commit sync, commit async.  
Application > _Write a record_ > partition (créer le offset __consumer_offets) > replication * 2  

**ATTENTION AU LATEST ! IL EST PAR DEFAULT ! UTILISEZ PLUTÔT LE EARLIEST !**
`auto.offset.reset=earliest`

## Event driven Microservices

Commit periodically => Throttled strategy (by default)

### Failure management ?

Ack strategy : 
 - fail fast => on a une erreur on arrete tout (default)
 - ignore => logué mais rien de spé
 - DLQ, write the record in a DLQ topic en attente
   - Par contre il faudra penser à rejouer ces records que l'on a mis en stand by
 - @Retry, pas retry du producer, à faire que si on est sur que ça va fonctionner et pas péter ma prod

## Alerts

Metrics les plus utiles ?

 - Prometheus
 - Grafana pour le dashboard

Pod up ? check les logs ? prometheus status ? check selector in prometheus ? check errors

Attention aux problèmes de disk plein, c'est ça le pire sur kafka, vraiment mauvais pour les brokers.

### SLO rates

Burn rate  
Google recommande : si on a cramé 2% en 1h / 5H en 10h ... On réveille quelqu'un pour qu'il se penche sur le probleme.

Budget on fire  
On check le dashboard, les logs du broker, roll update pods via strimzi, check canary...

### Rebalance

 - producer side
   - concurrent write to multiple partitions
 - consumer side
   - ..
   - ..

Le rebalance permet d'orchestrer le rebalance protocol lorsqu'un consumer arrive ou part.  
On redispatch nos partitions parmis les consumers restant. (a set of resources among the members).

findCoordinator (gets the broker coordinating the group) => joinGroup (init the rebalance protocol, lui passe les conf interval et timeout ainsi que la partition à consommer).  

C'est le consumer leader discute avec le coordinateur, il ordonne ensuite les rebalance, toi consumer 2 tu restes sur la partition 2, toi le 3 tu passes sur le 4 etc... (assignments).

Each consumer sends periodically un hearthbreak pour dire qu'il est encore là.

 - Problème 1 : **Freeze the world** quand un gars part, ça bloque TOUS les consommateurs. Rebalance etc. (si on est sur K8s, 2 instances fois 3 consumers, 6 rebalance).
 - Problème 2 : Heartbeat missed, si on rame un peu et qu'on rate le ping pour dire qu'on est up, ah bah trop tard, on est balancé.

## Authent

SASL Auth with OAUTHBEARER : https://docs.confluent.io/platform/current/kafka/authentication_sasl/authentication_sasl_oauth.html

Conf:  
protocol=SASL_SSL  
... 

Module maven => [io.strmizi](https://mvnrepository.com/artifact/io.strimzi/strimzi)  
Pour l'authent précisément : [Kafka OAuth Client](https://mvnrepository.com/artifact/io.strimzi/kafka-oauth-client)

[Exemple](https://medium.com/egen/how-to-configure-oauth2-authentication-for-apache-kafka-cluster-using-okta-8c60d4a85b43)

## Conclusion

Un broker kafka se lance quand meme facilement.  
Difficile à scaler si on commence à etre beaucoup consommé.  
PUB => Redhat offre le truc managé (pay to go). Starts with qui aide bien à la config.