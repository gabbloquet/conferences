# REX: TDD avec TestContainers

Clever cloud, 
- Zookeeper clé/valeur distribué (souvent utilisé pour les logs)
- Pulsar qui permet d'échanger des messages qu'on veut pas perdre
- PostgreSQL

## Statégie de test

TDD, test d'intégration directement (plus gros que test unitaire)

Grace à testcontainers.  
Bibliotheque java qui pilote du docker, s'intègre à junit.

Avantage de testcontainers => dans vos tests, dans quel environement je veux que sois mon application quand je lance mes tests.  

## Implem

```java
@Testcontainers
public class RedisBackedCacheIntTest {

  private RedisBackedCache underTest;

  // container {
  @Container
  public GenericContainer redis = new GenericContainer(DockerImageName.parse("redis:5.0.3-alpine"))
    .withExposedPorts(6379);
  // }


  @BeforeEach
  public void setUp() {
    String address = redis.getHost();
    Integer port = redis.getFirstMappedPort();

    // Now we have an address and port for Redis, no matter where it is running
    underTest = new RedisBackedCache(address, port);
  }

  @Test
  public void testSimplePutAndGet() {
    underTest.put("test", "example");

    String retrieved = underTest.get("test");
    assertEquals("example", retrieved);
  }
}
```

On vient charger un container mock de notre bdd via `@Container`, sans oublier `@Testcontainers` sur la classe.

## Cas d'usage intéressant

 - Requête SQL 
   - Typos
   - Erreurs de logique
   - Différents états de base
 - Communication entre les services
   - Copier/coller hâtif
   - Effets de bord attendus
   - tester l'ajout d'une facture en e2e
 - Non-reg
 - APIs
   - Tester tous les endpoints

[Exemple implem](https://github.com/CleverCloud?q=test&type=all&language=&sort=).