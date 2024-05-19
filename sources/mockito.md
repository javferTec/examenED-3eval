### Esquema del Material sobre Mockito

### Introducción
- **Definición:** Mockito es una biblioteca de Java utilizada para crear objetos simulados (mock objects) para pruebas unitarias.
- **Funcionalidad:** Ofrece una sintaxis sencilla para crear mock objects y especificar su comportamiento durante las pruebas.

---

#### Dependencias
- **Integración con Maven:**
  ```xml
  <dependency>
      <groupId>org.mockito</groupId>
      <artifactId>mockito-junit-jupiter</artifactId>
      <version>5.2.0</version>
  </dependency>
  ```
---

### Pruebas con Mockito

1. **Configuración Inicial:**
   - **@ExtendWith(MockitoExtension.class):** Utilizada para integrar Mockito en las pruebas.
   - **@Mock:** Para crear objetos simulados.
   - **@InjectMocks:** Para inyectar los objetos simulados en la clase que se está probando.

   ```java
   @ExtendWith(MockitoExtension.class)
   class CarServiceImplTest {
       @Mock
       private CarRepository carRepository;
       @Mock
       private IndicatorService indicatorService;
       @InjectMocks
       private CarServiceImpl carService;
   }
   ```

2. **Definición del Comportamiento de los Mock Objects:**
   - **when(mock.method(params)).thenReturn(value):** Define el comportamiento de un método simulado.
   - **when(mock.method(params)).thenThrow(Exception.class):** Define el lanzamiento de una excepción.

   ```java
   when(carRepository.findByPlate("1234ABC")).thenReturn(expectedCar);
   when(carRepository.findByPlate("9999ZZZ")).thenThrow(ResourceNotFoundException.class);
   ```

3. **Verificación de Comportamiento:**
   - **verify(mock).method(params):** Comprueba que un método ha sido llamado con los parámetros indicados.
   - **Número de invocaciones:**
     - **never():** Comprueba que el método no ha sido llamado.
     - **times(n):** Comprueba que el método ha sido llamado exactamente n veces.
     - **atLeast(n):** Comprueba que el método ha sido llamado al menos n veces.
     - **atMost(n):** Comprueba que el método ha sido llamado como máximo n veces.

   ```java
   verify(indicatorService).showMaxSpeedIndicator(true);
   verify(indicatorService, times(1)).showMaxSpeedIndicator(true);
   ```

---

#### Ejemplos de Pruebas con Mockito

1. **Pruebas de la Clase CarServiceImpl:**
   - **Test FindAll:**
     ```java
     when(carRepositoryMock.findAll()).thenReturn(new ArrayList<>());
     assertEquals(0, carService.findAll().size());
     ```

   - **Test FindByPlate:**
     ```java
     when(carRepositoryMock.findByPlate("1234ABC")).thenReturn(car1);
     Car resultCar = carService.findByPlate("1234ABC");
     assertEquals(car1, resultCar);
     ```

   - **Test Accelerate:**
     ```java
     carService.accelerate(car, 10.0);
     verify(indicatorServiceMock).showMaxSpeedIndicator(false);
     ```

[inicio](../README.md)