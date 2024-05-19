# Introducción al Testing

## ¿Que son los Test? ¿Cuáles son sus partes?

El testing o pruebas de software es la realización de pruebas sobre el código con el fin de detectar errores y mejorar su calidad.

Se divide en 3 partes:

- **Arrange:** Es la parte del Test Unitario en donde se configura todo el código para ejecutar la prueba unitaria, declaramos las variables y creamos la instancia de los objetos de las clases.

- **Act:** Esta es la fase del Test Unitario en donde se ejecuta el código a probar, donde se llama al método que estamos probando (verificando). En este apartado, es donde se pasan los parámetros de entrada al método y donde se recoge el resultado devuelto por el método.

- **Assert:** En esta parte compara el resultado real de la llamada con el resultado esperado. La prueba se pasa o no depende de esta parte de la afirmación. Por lo general, se usa la clase Assert para esta operación.

---

## Test Unitarios VS Test de Integración

Los pruebas unitarias son una forma de verificar cómo funciona cada parte individual de un programa o sistema. Se uso para detectar problemas dentro de una unidad funcional y para garantizar que cada función or componento de un programa sea correcta en sí mismo.

Las pruebas de integración, por otra parte, verifican cómo se pueden combinar los elementos individuales de un programa y cómo funcionan juntos. Se utilizan para detectar problemas que pueden surgir cuando se combinan diferentes partes o componentes de  

---

## Para qué sirven JUnit Y Mockito

- **JUnit:** Nos permite evaluar el resultado de la ejecución de un método. Es decir, nos permite comparar el resultado esperado, con el que realmente estamos obteniendo después de ejecutar el método.

- **Mockito:** Nos ayuda a simular la respuesta de otro método necesario para ejecutar el método que necesitamos probar. Esto nos permite centrarnos únicamente en el método que deseamos probar. 

[inicio](../README.md)
