# Introducción al Procesamiento Paralelo de Datos

## Arquitecturas y Procesadores Modernos

- **Paralelismo de Instrucciones:** Varias instrucciones se ejecutan al mismo tiempo.
- **Paralelismo de Datos:** Los datos de entrada se subdividen para su procesamiento en paralelo.

Para mejorar la ejecución de instrucciones más rápidamente, se pueden emplear técnicas como:

## Instruction Level Paralelism (ILP)

- **Out of Order:** Reordenamiento de instrucciones para reducir la latencia.
- **Pipelining:** División del proceso de ejecución en etapas para ejecutar múltiples instrucciones en paralelo.
- **Branch Prediction:** Predicción de ramificaciones para evitar retrasos por decisiones condicionales.
- **Prefetching:** Adelanto de datos a la caché antes de su necesidad para reducir latencia.

## Unidad de Punto Flotante (FPU)

En cómputo científico, trabajar con números de punto flotante es crucial.

En una FPU tradicional, el tiempo de ejecución se calcula como:
- Sin Pipelining: T(n) = n * l * t (n: resultados, l: etapas, t: tiempo de ciclo)
- Con Pipelining: T(n) = (s + l + n - 1) * t (s: inicialización)

## Rendimiento Peak y Rendimiento Real

- **Rendimiento Peak:** Se calcula multiplicando la velocidad del reloj por la cantidad de FPUs.
- **Rendimiento Real:** Medido con benchmarks como Linpack y HPCG, afectado por la latencia.

## Jerarquía de Memoria en CPU

1. Registros (rápidos pero limitados y programables).
2. L1 Caché.
3. L2 Caché (compartido, gestiona coherencia entre núcleos).
4. L3 Caché (compartido, mayor capacidad).
5. Memoria Principal.

## Caché

- **Working Set:** Conjunto de páginas usadas por un programa.
- Programar considerando el funcionamiento de la caché para maximizar la eficiencia.

### Problemas de Coherencia en Caché

- Valid: Datos en caché con copia coherente en memoria.
- Reserved: Datos en caché válidos con única copia.
- Dirty: Datos modificados en caché.
- Invalid: Datos inválidos debido a modificaciones en otro caché.

### Fallos en Caché (Miss)

- Compulsory: Primer acceso a un dato.
- Capacity: Caché sin capacidad para los datos.
- Conflict: Dos datos intentan ocupar la misma ubicación en caché.
- Invalidation: Caché se invalida por cambios en otros cachés.

### Coherencia de Caché

- Problemas al acceder a un mismo dato modificado concurrentemente en diferentes cachés.

## Tipos de Arquitecturas de RAM

- **UMA (Uniform Memory Access):** Todos los nodos comparten una única memoria RAM.
- **NUMA (Non-Uniform Memory Access):** Cada nodo tiene conexión directa a un segmento de RAM, requiere comunicación para acceder a datos fuera de su segmento.

## Metricas computación concurrente
- **Tiempo de ejecución**: Ncpu
- **Speedup** : t1/tN
- **Eficiencia** : speedup/N

# Parallel Computing
## Data-Paralelism

## Task-Paralelism
Cuando se tiene una 

## Function Parallelism

## Speedup y eficiencia

### Speedup
- Tp: Tiempre de ejecucion en p procesadores
- T1: Tiempo base
El speedup corresponde a T1/Tp 

#### Speedup superlineal
Usualmente ocurre por temas de cache, corresponde a cuando el speedup es mejor al ideal

### Eficiencia
Speedup/P

## Ley de Amdahl
El codigo inheremente secuencial limita la eficiencia del paralelismo

Si Fs es el la fraccion de tiempo secuencial y Fp es la fraccion paralelizable
Tp = T1(Fs + Fp/P)

Entonces el speedup esta limitado a 
Sp = 1/Fs

## Ley de Gustafson
En un mejor computadorpuedo hacer mas trabajos el mismo tiempo
Tp = FsTp + pFpTp
Sp = p-(p-1)*Fs

## Concurrencia
- Computo paralelo: Actividades se pueden ejectuar de manera no secuencial
- Computo concurrente: Actividades se pueden ejectuar al mismo tiempo
- Computo distribuido: Actividades en diversos procesadores usualmente con memorias separadas
- Computo asincrono: Permite ejecutar actividades en distinto orden

## Arquitecturas
- SISD: single instruction single data
    - CPU tradicional, una instruccion ejecuta sobre un dato
- SIMD: single instruction multiple data
    - Computaciones vectoriales, GPU (SIMT)
- MISD: multiple instruction single data
    - Computacion redundante
- MIMD: multiple instruction multiple data
    - Computacion paralela

## Arquitecturaras de memoria
- UMA
    - Todos los procesadores comparten memoria
- NUMA
    - Cada procesador tiene un trozo de la RAM asignado, pero tambien pueden acceder al de los otros
    - ccNUMA, cache coherent
    - Affinity: Un proceso se ejecuta en el procesador que tiene sus datos

# Parallel Programming
## Fork Join
Se define un thread principal que crea más threads los cuales luego hacen join al thread principal

## Modelos de memoria y consistencia secuencia
### Consistencia secuencial
- Lo que se lee es lo ultimo que se escribio
- Las operaciones de memoria ocurren en el orden del programa

## Affinity
Permite vincular y desvicular threads o procesos a un cierto CPU o rango de CPUs

Es bueno que threads que comparten datos esten dentro del mismo cache

