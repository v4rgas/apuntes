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

### HMAC

La implementacion ingenua de HMAC es:
$$\text{HMAC}_k(m) = h(m||k)$$

Sin embargo, no es segura porque se puede realizar un length extension attack.

Una implementación segura de HMAC es:
$$\text{HMAC}_k(m) = h(k_1 || h(k_2 || m))$$

donde $k_1$ y $k_2$ ocupan exactamente un bloque de longitud $n$

No se conocen ataques practicos contra HMAC-MD5.

### Key Derivation Functions (KDFs)

#### Password-Based Key Derivation Function

PBKDF1(hashfunction, password, salt, iterations, keylen)
Es una función de derivación de claves basada en contraseñas que toma una contraseña, un salt, un número de iteraciones y una longitud de clave deseada, y produce una clave derivada.

PBKDF2(HMAC, password, salt, iterations, keylen)

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

## Construir una funcion de hash

Podriamos pensar que podemos construir una función de hash a partir de una funcion de encriptación, pero esto no es tan sencillo. Necesitamos que la función de hash sea resistente a colisiones y resistente a preimágenes.

### Intento 1: $h(u||v) = \text{Enc}_u(v)$

No funciona porque existe un algoritmo eficiente para contruir preimagenes.Esto ocurre para cualquier esquema criptografico.

Dados $u,v \in \{0,1\}^n$, sea $x = h(u||v) = \text{Enc}_u(v)$.

Considere $u' \in \{0,1\}^n$ arbitrario, y defina $v' = \text{Dec}_{u'}(x)$.

Tenemos que:
$$h(u'||v') = \text{Enc}_{u'}(v') = \text{Enc}_{u'}(\text{Dec}_{u'}(x)) = x$$

Por lo tanto, $(u'||v')$ es una preimagen de $x$ para cualquier $u'$ elegido arbitrariamente. Esto demuestra que la función no es resistente a preimágenes, ya que dado cualquier valor hash $x$, podemos encontrar eficientemente una preimagen eligiendo cualquier $u'$ y calculando $v' = \text{Dec}_{u'}(x)$.

### Davies-Meyer construction

$$h(u||v) = \text{Enc}_u(v) \oplus v$$

Esta construccion es resistente a colisiones si la F es un cifrado ideal

### Merkle-Damgård construction

Digamos que tenemos una funcion de hash $h: M \rightarrow H$ para mensajes de longitud fija y queremos construir una funcion de hash $H: M^* \rightarrow H$ que tome mensajes de longitud variable.

podemos hacerlo de la siguiente manera:

1. Definimos un IV (vector de inicialización) aleatorio de longitud $n$.
2. Dividimos el mensaje en bloques de longitud fija $n$, haciendo padding si es necesario.
3. Para cada bloque $m_i$ del mensaje, aplicamos la función de hash de la siguiente manera:

   $$ H_i = h(H_{i-1} || m_i) $$

La intuición es que si h es resistente a colisiones, entonces en cada paso del proceso iterado no se pueden producir dos entradas distintas que generen la misma salida. Al procesar un mensaje largo en bloques encadenados, cualquier colisión en la función hash completa implicaría forzosamente una colisión en algún paso intermedio.

## Chapter 3: Private Key Encryption

### Modes of operation

Supongamos que tenemos un mensaje m de longitud variable, con padding dividido en l bloques de longitud fija n.

#### Electronic Codebook (ECB)

En el modo ECB, cada bloque de mensaje se cifra de manera independiente usando la misma clave. Esto significa que bloques idénticos en el mensaje original producirán bloques idénticos en el texto cifrado.

#### Cipher Block Chaining (CBC)

En CBC, se elige un IV aleatorio de longitud $n$ como bloque inicial $c_0$.

**Cifrado:** Para $i = 1$ a $\ell$: $c_i := F_k(c_{i-1} \oplus m_i)$

**Descifrado:** Para $i = 1$ a $\ell$: $m_i := F_k^{-1}(c_i) \oplus c_{i-1}$

El texto cifrado final es $c_0, c_1, \ldots, c_\ell$ (incluye el IV). Bloques idénticos producen cifrados diferentes según su posición.

#### CPA-secure encryption

La seguridad CPA (Chosen Plaintext Attack) se define mediante el siguiente juego:

**Juego CPA-Indistinguishability:**

1. El verificador toma $k$ en base a $\text{Gen}(1^n)$
2. El adversario puede preguntar lo que quiera a $\text{Enc}_k(\cdot)$
3. El adversario genera mensajes $m_0$ y $m_1$ y los envía al verificador
4. El verificador elige $b \in \{0,1\}$ y envía al adversario $c = \text{Enc}_k(m_b)$
5. El adversario puede preguntar lo que quiera a $\text{Enc}_k(\cdot)$
6. El adversario elige $b' \in \{0,1\}$ y gana si $b = b'$

Un esquema criptográfico es seguro frente a ataques de texto elegido (CPA-secure) si:

Para todo adversario $A$ de tiempo polinomial:

$$\Pr[A \text{ gane el juego}] - \frac{1}{2}$$

es una función despreciable en $n$.

Notemos que si F es una función pseudorandom, entonces el esquema CBC es CPA-secure.

## Chapter 12: Public-Key Encryption

Dos problemas principales de la criptografia simetrica son:

1. Dos usuarios deben reunirse para compartir una clave secreta.
2. El numero de claves crece proporcionalmente al numero de usuarios.

El cifrado asimetrico resuelve estos problemas

1. Cada usuario tiene una clave pública $P_a$ y una clave privada $S_a$.
2. Ambas claves estan relacionadasm $P_a$ se usa para cifrar y $S_a$ para descifrar.

## Propiedades básicas

Para que un esquema de cifrado asimétrico sea correcto, debe cumplir la propiedad fundamental de que el descifrado de un mensaje cifrado devuelva el mensaje original:

$$\text{Dec}_{S_a}(\text{Enc}_{P_a}(m)) = m$$

donde $P_a$ es la clave pública, $S_a$ es la clave privada correspondiente, y $m$ es el mensaje original.

## RSA

Las claves públicas y privadas se generan a partir de dos números primos, P y Q, de la siguiente manera:

1. Se eligen dos numeros primos grandes P y Q.
2. Se calcula $N = P \cdot Q$.
3. Se define $\phi(N) = (P-1)(Q-1)$.
4. Se genera un numero d tal que $MCD(d, \phi(N)) = 1$. (O sea, d es coprimo con $\phi(N)$).
5. Se calcula $e$ tal que $e \cdot d \equiv 1 \mod \phi(N)$.
6. $S_a =(d, N)$ es la clave privada, y $P_a = (e, N)$ es la clave pública.

Luego,

- $\text{Enc}_{P_a}(m) = m^e \mod N$
- $\text{Dec}_{S_a}(c) = c^d \mod N$

Notemos que para ejecutar de manera eficiente este algoritmo necesitamos:

- Generacion aleatoria de numeros primos grandes
- Operaciones basicas de aritmética modular
- MCD
- Generacion de un primo relativo
- Algoritmo extendido de Euclides (para inverso modular)
- Exponenciación rapida

## Chapter 11: Key Management and the Public-Key Revolution

### One way functions

Una función unidireccional es una función que es fácil de calcular en una dirección.

Un ejemplo de esto en RSA es:

$$f(x) = x^e \mod N$$

Para los subgrupos de $Z_p^*$, es dificil calcular el logaritmo discreto.

#### Logaritmo discreto

Dado $g \in G$ y el valor $y = g^x$, si $|\langle g \rangle|$ es grande, es difícil calculaur $x$.

Siendo $g \in G$ un elemento que genera un subgrupo grande, la función $f(x) = g^x$ es una one-way function.

### Diffie-Hellman Key Exchange

Protocolo para acordar una clave secreta en un canal inseguro.

- **Público**: primo \( p \), base \( g \)  
- **Secreto**: \( x \) (Alice), \( y \) (Bob)

1. Alice calcula y envía \( A = g^x \bmod p \) (**público**)  
2. Bob calcula y envía \( B = g^y \bmod p \) (**público**)  
3. Clave compartida:  
   - Alice obtiene \( K = B^x \bmod p \)  
   - Bob obtiene \( K = A^y \bmod p \)  
   - Ambos comparten \( K = g^{xy} \bmod p \) (**secreta**)

### ElGamal

Protocolo para cifrado asimétrico basado en el intercambio de claves Diffie-Hellman.

**Configuración inicial:**

- **Público**: grupo $\langle g \rangle$ de orden primo $q$
- **Clave privada de Alice**: $x \in \{1, ..., q\}$
- **Clave pública de Alice**: $g^x$

**Cifrado (Bob envía mensaje $m$ a Alice):**

1. Bob genera una clave efímera aleatoria $y \in \{1, ..., q\}$
2. Bob calcula $s = g^{xy}$ (secreto compartido)
3. Bob envía el par $(m \cdot s, g^y)$ a Alice

**Descifrado (Alice recibe $(m \cdot s, g^y)$):**

1. Alice calcula $s^{-1} = g^{y(q-x)}$
2. Alice recupera el mensaje: $m \cdot s \cdot s^{-1} = m$

# Chapter 13: Digital Signatures

La idea de las firmas digitales es demostrar que un mensaje fue enviado por una persona en particular y que no ha sido modificado.

## Firmas digitales con RSA

Supongamos que $P_a = (e, N)$ es la clave pública de Alice y $S_a = (d, N)$ es su clave privada.

Notemos que:

$$Enc_{P_a}((Dec_{S_a}(m)) = m$$

Entondces, si Alice quiere firmar un mensaje $m$, puede calcular su firma como:
$$s = Dec_{S_a}(m)$$

Tambien, para que sea mas rapido se puede firmar el hash del mensaje en vez del mensaje completo.

## Firmas de Schorr

Firmas basadas en el problema del logaritmo discreto.

Supongamos que dado un grupo finito $(G, *)$ y un elemento generador $g \in G$ tal que $|\langle g \rangle| = q$, donde:

- $G$, $g$ y $q$ son públicos

- $x \in \{1, ..., q\}$ es la clave privada de Alice
- $y=g^x$ es la clave pública de Alice

Alice firma m de la siguiente manera:

1. Elige un número aleatorio $k \in \{1, ..., q-1\}$
2. Calcula $c = h(g^r || m)$, donde $h$ es una función de hash
3. Calcula $s = r + c \cdot x$
4. Envía la firma $(c, s)$ a Bob

Bob verifica la firma de la siguiente manera:

1. Calcula $\alpha = g^s \cdot y^{q-c}$
2. Verifica si $c = h(\alpha || m)$

## Firmas de anillo

Permite a un grupo de personas firmar un mensaje sin revelar quién lo firmó.

El firmante es anonimo, pero se puede verificar que el mensaje fue firmado por alguien del grupo y los otros miembros grupo no necesitan consentir la firma.

- Linkable Rin Signatures (LRS): Permite verificar que dos firmas fueron hechas por la misma persona pero no revela quién es.
- Traceable Ring Signatures (TRS): Si dos firmas fueron hechas por la misma persona, se puede revelar quién es.

