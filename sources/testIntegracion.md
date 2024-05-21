# TEST DE INTEGRACIÓN

*Comprueban las unidades (clases) funcionando de forma conjunta.*

---

# Niveles
- Servicio + Repositorio -> `Mock de DAO`.
- Repositorio + DAO -> `Mock de la BD (con h2)`.
- Servicio + Repositorio + SAO -> `Mock de la BD (con h2)`.

---

# Notas:
- Siempre se mockea la última capa. Se le dice con el "when().then()" cómo se deberá comportar en cada caso.

- Las llamadas del test siempre son a la primera capa del test de integración. Si se está haciendo de:
  - Servicio y Repositorio -> se llama a servico, ya que este llama al repositorio y este al DAO mockeado.
  - Repositorio y DAO ->  se llama a repositorio, ya que este llama al DAO y este a la BD mockeada con H2.
  - Servicio, Repositorio y DAO -> se llama a servico, ya que este llama al repositorio, el repositorio al DAO y este a la BD mockeada con H2.


---

# Ejemplos
### - Servicio + Repositorio
```java
@ExtendWith(MockitoExtension.class)
public class ActorServiceRepositoryMockDao {
    // Mock
    private final ActorDao actorDaoMock = mock(ActorDao.class);

    // Injections
    private final ActorRepository actorRepository = new ActorRepositoryImpl(actorDaoMock); // llamada al DAO mock
    private final ActorService actorService = new ActorServiceImpl(actorRepository); // Llamada al repositorio

    // Test data
    private final List<ActorEntity> actorEntityList = new ArrayList<>();
    private ActorEntity actorEntity;

    @BeforeEach
    void setUp() {
        actorEntity = new ActorEntity(UUID.fromString("00000000-0000-0000-0000-000000000001"), "Actor 1");
        actorEntityList.add(actorEntity);
    }

    @Test
    void testFindAll() {
        // ARRANGE
        List<Actor> expectedActorList = new ArrayList<>(List.of(new Actor(UUID.fromString("00000000-0000-0000-0000-000000000001"), "Actor 1")));
        when(actorDaoMock.findAll()).thenReturn(actorEntityList);
        // ACT & ASSERT
        assertEquals(expectedActorList, actorService.findAll());

    }
}
```

### - Repositorio + DAO
```java
@ExtendWith(MockitoExtension.class)
public class ActorRepositoryDaoMockDB {
    private final ActorDao actorDao = new ActorDaoJDBCImpl();
    private final ActorRepository actorRepository = new ActorRepositoryImpl(actorDao);

    // Test data
    Actor actor;
    ActorEntity actorEntity;
    List<Actor> actorList;
    List<ActorEntity> actorEntityList;
    private static Connection connection;
    private PreparedStatement ps;
    private ResultSet rs;
    private String sql;

    @BeforeEach
    void setUp() throws SQLException {
        actor = new Actor(UUID.fromString("00000000-0000-0000-0000-000000000001"),"Actor 1");
        actorEntity = new ActorEntity(UUID.fromString("00000000-0000-0000-0000-000000000001"),"Actor 1");
        actorList = List.of(actor);
        actorEntityList = List.of(actorEntity);
        connection = DBConnection.getInstance().getConnection();
        DBConnection.getInstance().executeScript("filMarketH2.sql");
        connection.setAutoCommit(false);
    }

    @AfterEach
    void tearDown() throws SQLException {
        connection.rollback();
    }

    @Test
    void testFindAll() {
        // ARRANGE
        actorRepository.create(actor);
        // ACT & ASSERT
        assertEquals(actorList, actorRepository.findAll());
    }
}
```

### - Servicio + Repositorio + DAO
```java
@ExtendWith(MockitoExtension.class)
public class ActorServiceRepositoryDaoMockDB {
    private final ActorDao actorDao = new ActorDaoJDBCImpl();
    private final ActorRepository actorRepository = new ActorRepositoryImpl(actorDao);
    private final ActorService actorService = new ActorServiceImpl(actorRepository);

    // Test data
    Actor actor;
    ActorEntity actorEntity;
    List<Actor> actorList;
    List<ActorEntity> actorEntityList;
    private static Connection connection;
    private PreparedStatement ps;
    private ResultSet rs;
    private String sql;

    @BeforeEach
    void setUp() throws SQLException {
        actor = new Actor(UUID.fromString("00000000-0000-0000-0000-000000000001"),"Actor 1");
        actorEntity = new ActorEntity(UUID.fromString("00000000-0000-0000-0000-000000000001"),"Actor 1");
        actorList = List.of(actor);
        actorEntityList = List.of(actorEntity);
        connection = DBConnection.getInstance().getConnection();
        DBConnection.getInstance().executeScript("filMarketH2.sql");
        connection.setAutoCommit(false);
    }

    @AfterEach
    void tearDown() throws SQLException {
        connection.rollback();
    }

    @Test
    void testFindAll() {
        // ARRANGE
        actorService.create(actor.getName());
        // ACT & ASSERT
        assertEquals(actorList.get(0).getName(), actorRepository.findAll().get(0).getName());
    }
}
```

[inicio](../README.md)