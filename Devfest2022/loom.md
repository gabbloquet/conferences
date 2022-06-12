# Loom

Still WIP, preview en Java 19.  
Surement l'un des plus gros projets, 100 000 lines.

# Historique

Modèles de prog concurrente :
 - 1995 : Thread, Runnable
 - 2004 : Callback, locks
 - 2014 : Fork / Join, parallel streams, CompletableFuture

Loom vient revisiter l'asynchronicité des taches

La concurrence, 2 modèles : 
 - Process in memory data en //, (faire des actions sur une liste en //)
 - Processing data I/O, threads

Un thread c'est 1ms pour être exec, 2MB of stack de mémoire

 - CompletionStage / CompletableFuture
 - Asynchronous / Reactive programming
 - Mono / Multi (Spring)
 - ...

**Soucis / Cons** :  
 - Hard to read and write
 - dur à débuguer
 - dur à tester
 - impossible to profile

Loom amène une nouvelle typologie de thread, des **virtual threads** qui sont bien plus léger et beaucoup plus rapide à executer.  
On peut potentiellement créer 1 millions de virtual threads. (1000* Cheaper)

`Thread virtualThread = Thread.ofVirtual().name("toto le thread ").start(runnable);`
`ExecutorService service = Executors.newVirtualThreadPerTaskEecutor()`

=> **Structured Concurrency**, c'est ce pattern que l'on va maintenant utiliser pour développer nos solutions

```java
scope.fork(() => readWeartherFromA()
scope.fork(() => readWeartherFromB()
scope.fork(() => readWeartherFromC()

scope.join()

return scope.result()
```

`Future<Quotation> future`, avec un switch case on peut faire des actions en fonction de l'état de `future.state()` 

