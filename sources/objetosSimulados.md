# Introducción a Objetos Simulados:
   - Los objetos simulados se utilizan para simular el comportamiento de objetos reales en el sistema.
   - Son útiles para probar el comportamiento de una clase que depende de otras clases o componentes sin necesidad de utilizar las implementaciones reales.

   ---

## Funcionamiento (Mocks manuales)
- Test de `Servicio` -> Mock de Repositorio.
    - Ejemplo: (se comprueba que se hace las llamadas al repositorio entre otras cosas, <i>como que devuelva lo deseado, métodos fuera de la interfaz, etc</i>).

        ```java
        public class UserServiceImplTest {

            // Se declaran las variables de la clase de prueba
            private UserServiceImpl userServiceImpl; // Implementación del servicio de usuario
            private UserRepository userRepositoryMock; // Repositorio simulado de usuario
            private CartRepository cartRepositoryMock; // Repositorio simulado de carrito

            // Antes de cada prueba, se configuran los objetos necesarios
            @BeforeEach
            public void setUp() {
                userRepositoryMock = new UserRepositoryMock(); // Se crea un mock del repositorio de usuario
                cartRepositoryMock = new CartRepositoryMock(); // Se crea un mock del repositorio de carrito
                userServiceImpl = new UserServiceImpl(userRepositoryMock, cartRepositoryMock); // Se instancia la implementación real del servicio de usuario, integrando los objetos mock
            }

            // Aquí se implementarían el resto de las pruebas...
        }
        ```

- Test de `Repositorio` -> Mock de DAO.
    - Ejemplo: (se comprueba que se hace las llamadas al DAO entre otras cosas, <i>como que devuelva lo deseado, métodos fuera de la interfaz, etc</i>).

        ```java
        public class UserRepositoryImplTest {

            // Se declaran las variables de la clase de prueba
            private UserRepositoryImpl userRepositoryImpl; // Implementación del repositorio de usuario
            private UserDao userDaoMock; // Mock del objeto de acceso a datos (DAO) de usuario

            // Antes de cada prueba, se configuran los objetos necesarios
            @BeforeEach
            public void setUp() {
                userDaoMock = new UserDaoJDBCMock(); // Se crea un mock del DAO de usuario utilizando JDBC
                userRepositoryImpl = new UserRepositoryImpl(userDaoMock); // Se instancia la implementación real del repositorio de usuario, integrando el mock del DAO
            }

            // Aquí se implementarían el resto de las pruebas...
        }
        ```
- Test de `DAO` -> Mock de la BD (h2).
    - ¿Qué es H2 y para qué sirve?
        - <b><u>H2</u></b> es una base de datos relacional que se puede incrustar en aplicaciones Java. Es útil para pruebas y desarrollo, permitiendo crear y destruir fácilmente bases de datos sin afectar la principal. Con H2, puedes ejecutar consultas SQL y procedimientos almacenados de forma rápida y eficiente, siendo ideal para pruebas unitarias e integración en entornos Java.
        
        <br>
    - Ejemplo:
        ```java
        public class OrderDetailDaoJDBCTest {

            // Se declara la instancia de la implementación JDBC para los detalles de orden
            private OrderDetailJDBCImpl orderDetailJDBCImpl = new OrderDetailJDBCImpl();
            
            // Se declara una conexión estática para todas las pruebas
            private static Connection connection;

            // Antes de todas las pruebas, se configura la conexión y se ejecuta un script de inicialización
            @BeforeAll
            public static void setUp() throws SQLException {
                connection = DBConnection2.getInstance().getConnection(); // Se obtiene una instancia de la conexión a la base de datos
                DBConnection2.getInstance().executeScript("filMarketH2.sql"); // Se ejecuta un script SQL para inicializar la base de datos
                connection.setAutoCommit(false); // Se desactiva el autocommit para las pruebas
            }

            // Después de cada prueba, se realiza un rollback para revertir los cambios en la base de datos
            @AfterEach
            void tearDown() throws SQLException {
                connection.rollback(); // Se revierten los cambios en la base de datos
            }

            // Aquí se implementarían el resto de las pruebas...
        }

        ```
