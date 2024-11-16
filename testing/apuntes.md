# Unit Testing
## Mocking objects
Test doubles son objetos que se utilizan en lugar de objetos reales en pruebas de software. Hay varios tipos de test doubles, como stubs, spies, mocks y fakes.


- Dummy: objetos que se pasan al método de prueba pero no se utilizan.
- Fake: implementaciones funcionales pero simplificadas de objetos de producción.
- Stubs: respuestas limitadas y predefinidas a llamadas de métodos.
- Mocks: objetos preprogramados con expectativas de llamadas de métodos.

# Coverage

## Statement coverage
Cantidad de instrucciones ejecutadas en el código respecto al total de instrucciones.

## Branch coverage
Cantidad de ramas ejecutadas en el código respecto al total de ramas.

## Function coverage
Cantidad de funciones ejecutadas en el código respecto al total de funciones.

## Black Box Testing
Pruebas de caja negra, se centran en la entrada y salida del sistema.

Son independientes de la implementación y se centran en el comportamiento del sistema.

## White Box Testing
Pruebas de caja blanca, se centran en la estructura interna del sistema.

Se basan en el código fuente y se centran en la lógica interna del sistema.

# Mutation based fuzzing
## Idea general
Si tenemos una entrada valida para un programa, las variaciones de esta tienen más probabilidad de ser válidas que una entrada aleatoria.

# Search based fuzzing
## Idea general
A veces no solo nos interesa tener la mayor cantidad de entradas posibles, sino buscar alguna que alcance un objetivo específico.

Si la condicion es muy compleja, puede ser dificil encontrar una entrada que la cumpla.

## Hill climbing algorithm
### Espacio de busqueda
Son todas las posibles entradas que se pueden generar.

Si es una tupla (x,y), el espacio de busqueda es el plano cartesiano, donde cada punto tiene 8 vecinos.

### Función objetivo
Es una función que toma una entrada y devuelve un valor numérico que indica que tan buena es esa entrada.

## Genetic algorithm
### Idea general
1. Inicializar una población de entradas aleatorias.
2. Evaluar cada entrada con la función objetivo.
3. Seleccionar las mejores entradas.
4. Cruzar las mejores entradas.

## Tournament selection
### Idea general
Se seleccionan dos parejas de entradas al azar y se elige la mejor de cada pareja.

Luego se elige la mejor de las dos.

## Roulette wheel selection
La probabilidad de que se seleccione un individuo es proporcional a su fitness.

# Mutation analysis
## Por que el coverge no es suficiente
El coverage no garantiza que el código sea correcto.

## Mutation analysis
Se introducen mutaciones en el código y se ejecutan los tests.

Si un test falla, la mutación es detectada y se considera que el test es efectivo.

## Mutation score
Es la cantidad de mutaciones detectadas dividido la cantidad de mutaciones totales.

## Mutantes equivalentes
Son mutaciones que no cambian el comportamiento del programa, por lo que no deberían ser detectadas. Este es un problema de la mutación analysis.

# Black Box Testing
## Tablas de decisión
| Condiciones | Regla 1 | Regla 2 | Regla 3 | Regla 4 |
| ----------- | ------- | ------- | ------- | ------- |
| Condición 1 | 1       | 0       | 1       | 0       |
| Condición 2 | 0       | 1       | 1       | 0       |
| Condición 3 | 1       | 0       | 0       | 1       |
| Condición 4 | 0       | 1       | 0       | 1       |
| Outputs     |         |         |         |         |
| Resultado   | A       | B       | C       | D       |

## State transition testing
Se modela el sistema como un autómata finito y se prueban las transiciones entre estados.

### Ventajas
- Es una buena represetancion del comportamiento del sistema.
- Un tester puede verificar si se han probado todas las transiciones.

### Desventajas
- No todos los sistemas se pueden modelar como autómatas finitos.
- Es dificil definir todos los posibles estados invalidos.

# Static Analysis
## AST
Un árbol de sintaxis abstracta es una representación intermedia de la estructura de un programa.

# Fault Location
## Spectrum based fault localization
Se localizan los elementos que pueden contener errores mediante la ejecución de tests. Se asigna un score a los elementos del programa.

Existen varios algoritmos para calcular el score, como el Tarantula, Ochiai, Jaccard, etc.

## Mutation based fault localization
Se introducen mutaciones en el código y se ejecutan los tests, la intuición es que si mutar una cierta parte del codigo cambia el comportamiento del programa, entonces esa parte del código es sospechosa.

## Stack Trace Fault Localization
Se analizan los stack traces de los tests fallidos para encontrar la causa del fallo.

## Predicate Switching Fault Localization
Se analizan las condiciones de los if para encontrar la causa del fallo.

# Taint Analysis
Existen varios niveles de taint analysis.

Los taints basados en cadenas y caracteres permiten rastrear la propagación de datos a través de cadenas y caracteres.

Verificar taints permite rastrear la propagación de datos a través de las verificaciones de seguridad.

Las cadenas sin taint deben ser tratadas como si tuviesen el taint más alto.

Los taints pueden ser utilizados junto al fuzzing para proporcionar una indicación robusta del comportamiento incorrecto del programa.

# Concolic Fuzzing
## Idea general
La idea es comenzar con una entrada de ejemplo y ejecutar la funcion bajo traza.

En cada punto en que la ejecución pasa por una condición, guardamos la condicion encontrada en forma de relación simbólica.

## Limitaciones
El flujo de control implicito puede oscurecer la relación simbólica.

## Lecciones aprendidas
La ejecución concolic puede proporcionar mas información quee el analisis taint en cuanto al comportamiento del programa.

Conlleva a un costo computacional mayor.

# Symbolic Fuzzing
## Idea general
Se utilizan valores simbólicos en lugar de valores obtenidos a partir de la ejecución.






