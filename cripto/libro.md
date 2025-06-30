# Apuntes de Introduction to Modern Cryptography

## Chapter 2: Perfectly secret encryption

### Sistemas de encriptación

Se definen por tres algoritmos:

- **Gen**: Generador de claves, que produce una clave aleatoria.
- **Enc**: Algoritmo de encriptación, que toma una clave y un mensaje y produce un mensaje cifrado.
- **Dec**: Algoritmo de desencriptación, que toma una clave y un mensaje cifrado y produce el mensaje original.

Denotamos como $\mathcal{K}$ el espacio de claves, $\mathcal{M}$ el espacio de mensajes y $\mathcal{C}$ el espacio de mensajes cifrados.

Entonces:

- $\text{Enc}: \mathcal{K} \times \mathcal{M} \rightarrow \mathcal{C}$, es decir, $\text{Enc}(k, m) = c$
- $\text{Dec}: \mathcal{K} \times \mathcal{C} \rightarrow \mathcal{M}$, es decir, $\text{Dec}(k, c) = m$

$k$ y $m$ deben ser independientes, es decir, no debe haber relación entre ellos.

## Perfect secrecy

Un sistema de encriptación $(\text{Gen}, \text{Enc}, \text{Dec})$ con un espacio de mensajes $\mathcal{M}$ es perfectamente secreto si:

$$P(M = m | C = c) = P(M = m)$$

O sea, que no ganas información del mensaje con la clave.

### Indistinguibilidad adversarial perfecta

Presentamos una definición equivalente de perfect secrecy basada en un experimento que involucra a un adversario que observa pasivamente un texto cifrado y luego trata de adivinar cuál de dos posibles mensajes fue encriptado.

En este experimento:

1. Un adversario $A$ especifica dos mensajes arbitrarios $m_0, m_1 \in \mathcal{M}$
2. Se genera una clave $k$ usando $\text{Gen}$
3. Se elige uno de los dos mensajes (cada uno con probabilidad $1/2$) y se encripta usando $k$
4. El texto cifrado resultante se le da a $A$
5. $A$ produce una "conjetura" sobre cuál de los dos mensajes fue encriptado

Un esquema de encriptación es **perfectamente indistinguible** si ningún adversario $A$ puede tener éxito con probabilidad mejor que $1/2$.

## One Time Pad

Es un sistema de encriptación que es perfectamente secreto. Se define como:

Gen: Genera una clave aleatoria de la misma longitud que el mensaje.
Enc: Encripta el mensaje $m$ con la clave $k$ usando la operación XOR: $c = m \oplus k$.
Dec: Desencripta el mensaje cifrado $c$ con la clave $k$ usando la operación XOR: $m = c \oplus k$.

Es solamente perfectamente secreta si la clave se utiliza una sola vez y es tan larga como el mensaje.

## Pseudorandom permutations

1. Verificador elige $b \in \{0, 1\}$ con distribución uniforme.
   1. Si $b = 0$, elige una clave k con Gen y define f(x) = Enc(k, x).
   2. Si $b = 1$, elige una permutacion aleatoria $\pi$ y define f(x) = $\pi(x)$.
2. El adversario elige una palabra $x$ y el verificador le devuelve $f(x)$.
3. Se repite el paso 2, q veces.
4. El adversario devuelve una conjetura sobre el valor de $b$.

(Gen, Enc, Dec) es una pseudorandom permutation si el adversario no puede distinguir entre los dos casos con probabilidad significativamente mejor que 1/2.

## Chapter 6: Hash functions and Applications

Las funciones de hash son funciones que toman una entrada de longitud variable y producen una salida de longitud fija.

$$ h:  M \rightarrow H $$

donde $M$ es el espacio de mensajes y $H$ es el espacio de hashes.

### Resistencia a colisiones

Una funcion de hash es resistente a colisiones si es inviable encontrar  para cualquier algoritmo en tiempo polinomial probabilistico encontrar una colision.

### Resistencia a preimágenes

Una función de hash es resistente a preimágenes si es inviable encontrar, dado un $x \in H$, un $y \in M$ tal que $h(y) = x$.

### Funciones de hash criptográficas

Una función de hash es criptográfica si es resistente a colisiones y resistente a preimágenes

### Funciones despreciables

Una función $f: \mathbb{N} \rightarrow \mathbb{R}$ es despreciable si:

$$(\forall \text{ polinomio } p: \mathbb{N} \rightarrow \mathbb{N})(\exists n_0 \in \mathbb{N})(\forall n \geq n_0)\left(f(n) < \frac{1}{p(n)}\right)$$

En otras palabras, $f$ decrece más rápido que el inverso de cualquier polinomio.

### Resistencia a colisiones con funciones despreciables

La resistencia a colisiones también se puede definir usando funciones despreciables mediante un juego formal.

#### Definición del juego Hash-Col(n)

1. El adversario elige mensajes $m_1$ y $m_2$ con $m_1 \neq m_2$
2. El adversario gana el juego si $h(m_1) = h(m_2)$, y en caso contrario pierde

#### Formalización de la resistencia a colisiones

Una función de hash $\{h_n\}_{n \in \mathbb{N}}$ se dice resistente a colisiones si para todo adversario que funciona como un algoritmo aleatorizado de tiempo polinomial, existe una función despreciable $f(n)$ tal que:

$$\Pr[\text{Adversario gane Hash-Col}_n] \leq f(n)$$

En otras palabras, la probabilidad de que cualquier adversario eficiente encuentre una colisión es despreciable.

## Chapter 4: Message authentication codes

La encriptacion no es suficiente para garantizar la integridad de los mensajes (o sea, que no hayan sido alterados). Para esto se utilizan los códigos de autenticación de mensajes (MACs).

### Definición de MAC

El objetivo de un MAC es evitar que un adversario pueda modificar un mensaje sin que el receptor lo detecte.

Se utiliza de la siguiente manera:

1. El emisor genera un mensaje $m$ y un tag $t$ usando una clave secreta $k$
2. El emisor envía el mensaje $m$ junto con el tag $t$ al receptor
3. El receptor recibe el mensaje y el tag, y verifica que el tag es correcto usando la misma clave $k$

Esto se hace con un algoritmo de verificacion (Vrfy) que toma la clave, el mensaje y el tag, y devuelve verdadero o falso.

### Definición formal de MAC

Un código de autenticación de mensajes (MAC) es un esquema $(\text{Gen}, \text{Mac}, \text{Vrfy})$ donde:

- Gen: Genera una clave aleatoria $k$.
- Mac: Toma una clave $k$ y un mensaje $m$ y produce un tag $t$.
- Vrfy: Toma una clave $k$, un mensaje $m$ y un tag $t$, y devuelve verdadero si $t$ es un tag válido para $m$ con la clave $k$, y falso en caso contrario.

### Seguridad de MACs

La seguridad de un MAC se define mediante un juego de falsificación entre un adversario y un verificador.

#### Definición del juego MAC-Forge(n)

1. El verificador invoca $\text{Gen}(1^n)$ para generar $k$
2. El adversario envía $m_0 \in \mathcal{M}$
3. El verificador responde $\text{Mac}_k(m_0)$
4. Los pasos 2 y 3 se repiten tantas veces como quiera el adversario
5. El adversario envía $(m,t)$, siendo $m$ un mensaje que no había enviado antes

#### ¿Cuándo gana el adversario?

El adversario gana si $\text{Vrfy}_k(t,m) = 1$

#### ¿Cuándo decimos que un esquema MAC es seguro?

Un esquema $(\text{Gen}, \text{Mac}, \text{Vrfy})$ es un **esquema de autenticación de mensajes seguro** si:

Todo adversario que juega en tiempo polinomial (en $n$) tiene una probabilidad despreciable (en $n$) de ganar el juego MAC-Forge(n).

En general se usa **seguro = unforgeable** (no falsificable).

## Chapter 7: Practical constructions of Symmetric-Key Primitives

## Confusion y diffusion

### Confusion

Confusion es la propiedad de que el resultado de una operación de cifrado debe depender de manera compleja de la clave y del mensaje.

Esto significa que cada bit del mensaje cifrado debe depender de muchos bits de la clave, y viceversa.

### Diffusion

La difusion consiste en distribuir la estructura estadistica del mensaje original en el mensaje cifrado. La informacion del mensaje original debe estar distribuida en el mensaje cifrado

Un cifrado con buena difusion debe hacer que un cambio en un bit del mensaje original cambie muchos bits del mensaje cifrado (más de la mitad).

## AES

Es un cifrado de bloque (toma bloques de un tamaño fijo y devolviendo bloques del mismo tamaño).

Es una red de sustitución y permutación (SPN) que utiliza una clave de 128 bits y bloques de 128 bits.

### Estructura de AES

AES consta de 10, 12 o 14 rondas, dependiendo del tamaño de la clave (128, 192 o 256 bits). Cada ronda consta de las siguientes operaciones:

1. **SubBytes**: Sustitución de bytes usando una tabla de sustitución (S-box).
2. **ShiftRows**: Desplazamiento de filas en la matriz de estado.
3. **MixColumns**: Mezcla de columnas en la matriz de estado.
4. **AddRoundKey**: Adición de la clave de ronda al estado.
5. **KeyExpansion**: Expansión de la clave para generar las claves de ronda.

#### ELI5

Antes que nada, convertimos el mensaje en un bloque de 128 bits (16 bytes). Luego, lo dividimos en una matriz de 4x4 bytes.

En cada ronda de AES, se realizan las siguientes operaciones:

1. Sustituimos cada letra del mensaje por otra letra usando una tabla de sustitución.
2. Desplazamos algunas entradas de la matriz dentro de su misma fila.
3. Mezclamos las columnas de la matriz para que los bytes se mezclen entre sí
4. Sumamos una clave de ronda a la matriz (esto es como sumar un número secreto al mensaje).
5. Generamos nuevas claves de ronda a partir de la clave original.

La gracia es que el paso 2 y 3 nos dan la difusion, y el paso 1 nos da la confusion.
