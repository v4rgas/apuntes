# Introducción

## Componentes de un computador

- CPU
- control
- datapath
- memoria
- input/output

Los programas se escriben en lenguajes ed alto nivel y se van traduciendo a "niveles más bajos"

## Operaciones de un computador

- Sumar dos números
- Revisar un numero para ver si es cero
- Copiar datos desde una parte de la memoria a otra

## On y Off

el alfabeto de un computador tiene dos letra: 0 y 1

el computador entiende y obedece secuencias de bits

## Assembly

Es un lenguaje simbólico que se puede traducir a instrucciones binarias

Assembler se encarga de hacer esta traducción

## Compilador

Traduce un programa escrito de un lenguaje de alto nivel a uno de bajo

# Lógica digital

La cantidad de bits disponible depende del computador, por lo que la precision es finita

## Representación de números enteros (Unsigned Integers)

### Base 10

Las personas pensamos en base 10 (10 símbolos). El valor numérico de un dígito depende del símbolo y la posición
$$421 = 4 \cdot 10^2 + 2 \cdot 10^1 + 1 \cdot 10^0$$

### Base 2

Los computadores usan solo dos digitos (0 y 1)
$$1101 = 1 \cdot 2^3 + 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0$$

Los bits se cuentan de izquierda a derecha partiendo del 0

## Compuertas

Son la base eléctrica sobre la que se construyen los computadores, estan hechas de transistores

0 a 0.5 volts equivalen a 0
1 a 1.5 volts equivalen a 1

### Transistores

Un transistor es un semiconductor que sirve como amplificador o interruptor de señal

Tres puntos de contacto:

- Colector: Voltaje > Tierra
- Base: Si el voltaje es menor que cierto valor transistor actúa como resistencia infinita
- Emisor: Tierra

### Compuertas Fundamentales

**UN** transistor es equivalente a la compuerta **NOT**

Si conectamos los transistores en serie corresponde a un **NAND**

Si los conectamos en serie corresponde a un **NOR**

### Circuitos Combinaciones

Corresponde a los circuitos formados por compuertas lógicas (solo importan los inputs del instante, no tienen memoria)

### Half Adder

La suma de dos bits se comporta como un XOR
El carry de esta suma se comporta como un AND

Ambos juntos forman un HALF ADDER

Dos Half Adders forman un Full Adder

## Representación de números negativos

### Signo y Magnitud

El bit de mas a la izquierda al signo y el resto la magnitud
$$101 = -2$$

### Complemento de dos

0001 = 1,
0010 = 2,
0011 = 3,
....
011 = 7,
1001 = -7,
1011 = -6,
...

Para pasar de positivos a negativos

1. Se invierten los bits: 0110 -> 1001
2. Se suma uno: 1001+0001 = 1010

### Resta

Para restar se suman los números positivos con los negativos en complemento de dos

### Circuito sumador restador

Para hacer una resta con un ADDER se debe hacer NOT a cada uno de los bits del numero negativo y poner el carry como 1

## Programacion

### Enabler

Si E está activo, deja pasar A
| E | A |
|---|---|
| 0 | 0 |
| 1 | A |

### Multiplexor

Si S esta activo pasa B, sino pasa A

| S   | M0  |
| --- | --- |
| 0   | A   |
| 1   | B   |

Permite elegir uno de los dos inputs. Si es 0 pasa A si es 1 pasa B

### Decodificador

Convierte la información de n bits a uno de los 2^n outputs únicos

### Comparador

Compara bits 1 a 1 y si son iguales output es 1, sino 0

### Unidad Lógica Artimética (ALU)

- Recibe dos operandos de K bits cada uno
- Devuelve el resultado de K bits
- Recibe un input de N bits para selección $2^n$ operaciones distintas

#### Operaciones

- Suma
- Resta
- Bitwise (And, Or, Not, Xor)
- Shift Left: $0001 \rightarrow 0010$ (EQUIVALENTE A MULTIPLICAR POR 2)
- Shift Right: $0010 \rightarrow 0001$ (EQUIVALENTE A DIVIDIR POR 2)

### Latch S-R

Consiste en dos NAND o NOR interconectadas que permiten guardar un estado dependiendo del ultimo pulso enviado

# Procesador

Se utilizan dos registros como input para la ALU y y el output se vuelve a conectar a los registros

Se agrega un AND entre el output de la ALU y el reloj

Además de los registros existe una memoria del programa (Instruction Memory)

## Instruction Memory

Consiste muchos registros ordenados. Cada uno de estos registros almacena una instrucción que posteriormente se dará al procesado

## PC

A la memoria de instrucciones se conecta un PC (Program Counter) el cual entrega la dirección de la próxima instrucción que se quiere dar

## Opcode

En vez de referenciar cada instrucción con una secuencia de bits de 8 de largo, podemos numerar cada una de estas instrucciones para disminuir la cantidad de bits necesaria para referirse a la instrucción

## Assembly
Es un version simbolica de los opcode

## Data Memory
Sirve para para guardar las variables que vana  ser usadas en el programa

Tiene un data in, un address y un data out

## if y while
Para poder implementar control de flujo se necesitan añadir mas instrucciones al assembly.

CMP A,B compara contenidos
JMP (Jump); JLE(Jump if less than or equal), JNE (Jump if not equal)

Para añadir estas instrucciones se necesita añadir un ouput que sale de la alu y entra a la control unit para especificar si saltar o no

## Subrutinas
Son funciones que se pueden llamar en cualquier linea, mueven el PC para ejecutarse y luego vuelven a donde fueron lalmadas habiendo sido ejecutadas


