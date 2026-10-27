
# Guía 03 — Primera simulación CFD con OpenFOAM

**Taller de Introducción a CFD con OpenFOAM**  
Universidad Mayor de San Andrés (UMSA) — Ingeniería Mecánica

**Instructor:** Luis Andres Mollericon Titirico  
**Sesión:** 29 de octubre de 2026

---

## 1. Objetivo

En esta práctica simularemos un flujo viscoso e incompresible dentro de una cavidad cuadrada cuya pared superior se mueve horizontalmente.

Aprenderemos a:

- Identificar los archivos de un caso de OpenFOAM.
- Interpretar las condiciones iniciales y de frontera.
- Generar y verificar una malla.
- Ejecutar un solver.
- Examinar la convergencia numérica.
- Comprender el efecto del número de Reynolds.

## 2. Descripción del problema físico

Consideremos una cavidad cuadrada de lado \(L\), llena de un fluido newtoniano e incompresible.

Tres paredes permanecen estacionarias y la pared superior se mueve hacia la derecha con velocidad constante \(U\).

Este problema se conoce como *lid-driven cavity*.

La pared superior transmite cantidad de movimiento al fluido mediante esfuerzos viscosos, generando un movimiento de recirculación dentro de la cavidad.

### Parámetros de nuestro caso

| Parámetro | Símbolo | Valor |
|---|---|---|
| Longitud de la cavidad | \(L\) | 0.1 m |
| Velocidad de la pared superior | \(U\) | 1 m/s |
| Viscosidad cinemática | \(\nu\) | 0.001 m²/s |
| Número de Reynolds | \(Re\) | 100 |
| Malla | — | 40 × 40 × 1 celdas |
| Tiempo final de simulación | \(t_f\) | 5 s |
| Paso temporal | \(\Delta t\) | 0.001 s |

El número de Reynolds se calcula mediante:

$$
Re = \frac{UL}{\nu}
$$

Sustituyendo:

$$
Re = \frac{1 \times 0.1}{0.001} = 100
$$

El número de Reynolds representa la relación entre los efectos inerciales y viscosos del flujo.

## 3. Ecuaciones gobernantes

Para un fluido newtoniano, incompresible y de viscosidad constante, sin fuerzas volumétricas, las ecuaciones utilizadas son:

### Conservación de masa

$$
\nabla \cdot \mathbf{U} = 0
$$

Esta ecuación expresa que el campo de velocidades es solenoidal.

### Conservación de cantidad de movimiento

$$
\frac{\partial \mathbf{U}}{\partial t}
+
(\mathbf{U}\cdot\nabla)\mathbf{U}
=
-\nabla p
+
\nu\nabla^2\mathbf{U}
$$

En esta formulación de OpenFOAM, \(p\) representa la presión cinemática, es decir, la presión física dividida por la densidad constante.

El solver `icoFoam` resuelve estas ecuaciones para un flujo laminar, transitorio e incompresible.

## 4. Obtener el caso de simulación

Si todavía no has descargado el repositorio, abre Ubuntu y ejecuta:

```bash
cd ~
git clone https://github.com/LuisAndresMT/UMSA-OpenFOAM-Workshop.git
```

Entra en el directorio del caso:

```bash
cd ~/UMSA-OpenFOAM-Workshop/cases/cavity-Re100
```

Si ya descargaste el repositorio, utiliza directamente el segundo comando.

Comprueba su contenido:

```bash
ls
```

Encontrarás las carpetas:

```text
0
constant
system
```

Estas tres carpetas constituyen la estructura fundamental de un caso de OpenFOAM.

## 5. Estructura de un caso de OpenFOAM

### Carpeta `0/`

Contiene las condiciones iniciales y de frontera de las variables físicas.

En nuestro caso:

```text
0/
├── U
└── p
```

- `U`: campo de velocidades.
- `p`: campo de presión cinemática.

### Carpeta `constant/`

Contiene las propiedades físicas del fluido y, después de generar la malla, la geometría discretizada.

En nuestro caso:

```text
constant/
└── transportProperties
```

El archivo `transportProperties` contiene la viscosidad cinemática.

### Carpeta `system/`

Contiene los parámetros numéricos y de control.

Entre los archivos más importantes:

| Archivo | Función |
|---|---|
| `blockMeshDict` | Define la geometría y la malla |
| `controlDict` | Define el solver y el control temporal |
| `fvSchemes` | Define los esquemas de discretización |
| `fvSolution` | Define los algoritmos y solvers algebraicos |

El caso también puede contener otros archivos auxiliares que no utilizaremos directamente en esta práctica.

## 6. Examinar las condiciones de frontera

Abre el archivo de velocidad:

```bash
code 0/U
```

Identifica la sección `boundaryField`.

Las condiciones físicas son:

| Región | Condición | Interpretación |
|---|---|---|
| `movingWall` | `fixedValue` | Pared superior con velocidad impuesta |
| `fixedWalls` | `noSlip` | Paredes estacionarias |
| `frontAndBack` | `empty` | Tratamiento bidimensional |

La velocidad de la pared superior es:

```text
(1 0 0)
```

Esto significa que se desplaza a 1 m/s en la dirección positiva del eje \(x\).

Las paredes estacionarias tienen velocidad nula.

### ¿Por qué `empty`?

OpenFOAM utiliza una malla tridimensional con una sola celda de espesor para representar este problema bidimensional.

La condición `empty` en las caras frontal y posterior elimina la dependencia de los campos en esa dirección.

**Importante:** `empty` no significa que esas caras estén abiertas al flujo.

Ahora examina la presión:

```bash
code 0/p
```

Observa que las condiciones de presión son diferentes de las condiciones de velocidad.

## 7. Examinar las propiedades del fluido

Abre:

```bash
code constant/transportProperties
```

Identifica el valor de la viscosidad cinemática:

```text
nu = 0.001 m²/s
```

En el archivo de OpenFOAM, este valor aparece acompañado de sus dimensiones físicas.

La viscosidad cinemática está relacionada con la viscosidad dinámica mediante:

$$
\nu = \frac{\mu}{\rho}
$$

donde:

- \(\mu\): viscosidad dinámica.
- \(\rho\): densidad.
- \(\nu\): viscosidad cinemática.

## 8. Examinar la malla

Abre:

```bash
code system/blockMeshDict
```

La cavidad tiene dimensiones:

$$
L_x = L_y = 0.1 \text{ m}
$$

La malla contiene:

$$
N_x = 40,\quad N_y = 40,\quad N_z = 1
$$

Por tanto, el número total de celdas es:

$$
N = 40 \times 40 \times 1 = 1600
$$

La dimensión de cada celda en el plano es:

$$
\Delta x = \Delta y = \frac{0.1}{40} = 0.0025 \text{ m}
$$

## 9. Generar la malla

Asegúrate de estar dentro del directorio `cavity-Re100`.

Ejecuta:

```bash
blockMesh
```

Este programa lee `system/blockMeshDict` y genera la malla.

Los archivos resultantes se almacenan en:

```text
constant/polyMesh/
```

Comprueba la malla:

```bash
checkMesh
```

Al finalizar, busca el mensaje:

```text
Mesh OK.
```

Este mensaje indica que la malla supera las comprobaciones realizadas por `checkMesh`, pero no garantiza por sí solo la precisión de los resultados.

## 10. Examinar el control de la simulación

Abre:

```bash
code system/controlDict
```

Identifica los siguientes parámetros:

| Parámetro | Valor | Significado |
|---|---|---|
| `application` | `icoFoam` | Solver utilizado |
| `startTime` | 0 | Tiempo inicial |
| `endTime` | 5 | Tiempo final |
| `deltaT` | 0.001 | Paso temporal |
| `writeControl` | `runTime` | Escritura según tiempo de simulación |
| `writeInterval` | 0.1 | Intervalo de escritura en segundos simulados |

Con un paso temporal constante, el número de pasos es:

$$
N_t = \frac{5}{0.001} = 5000
$$

OpenFOAM resolverá la evolución temporal del flujo desde \(t=0\) hasta \(t=5\) s.

## 11. Ejecutar la simulación

Ejecuta:

```bash
icoFoam
```

Observarás información sobre:

- Tiempo de simulación.
- Número de Courant.
- Resolución de los sistemas de ecuaciones.
- Residuos numéricos.
- Conservación de masa.
- Tiempo de ejecución.

Para guardar el registro en un archivo y mostrarlo simultáneamente en pantalla, también puedes utilizar:

```bash
icoFoam | tee log.icoFoam
```

Utiliza esta segunda opción solamente si todavía no has ejecutado el solver o si has preparado nuevamente el caso para comenzar desde el tiempo inicial.

## 12. Interpretar el número de Courant

El número de Courant relaciona la distancia recorrida por el flujo durante un paso temporal con el tamaño de las celdas.

De manera simplificada:

$$
Co \approx \frac{U\Delta t}{\Delta x}
$$

Para nuestros parámetros característicos:

$$
Co \approx \frac{1 \times 0.001}{0.0025} = 0.4
$$

Este valor es una estimación basada en la velocidad característica y el tamaño de celda.

OpenFOAM calcula los valores reales del número de Courant a partir del flujo numérico en la malla.

El número de Courant es importante para el comportamiento y la estabilidad de los esquemas temporales.

## 13. ¿Qué significan los residuos?

Durante cada paso temporal, OpenFOAM resuelve sistemas de ecuaciones algebraicas.

Los residuos indican qué tan bien se satisfacen esos sistemas dentro de las tolerancias establecidas.

Un residuo pequeño suele indicar que el sistema algebraico correspondiente ha sido resuelto adecuadamente.

Sin embargo:

**Residuos pequeños no garantizan por sí solos que la solución física sea correcta ni que se haya alcanzado un régimen estacionario.**

También es necesario comprobar la conservación de masa, la estabilidad temporal y, cuando corresponda, comparar los resultados con datos de referencia.

## 14. Interpretación física esperada

Al visualizar el campo de velocidades deberíamos observar:

1. Movimiento del fluido próximo a la pared superior hacia la derecha.
2. Transferencia de cantidad de movimiento hacia el interior por efectos viscosos.
3. Formación de una recirculación principal dentro de la cavidad.
4. Velocidades reducidas cerca de las paredes estacionarias.

La posición e intensidad de los vórtices dependen del número de Reynolds.

## 15. Experimento: modificar el número de Reynolds

Mantendremos constantes:

$$
U = 1 \text{ m/s}, \qquad L = 0.1 \text{ m}
$$

Modificaremos únicamente la viscosidad cinemática.

| Caso | Reynolds | Viscosidad cinemática |
|---|---:|---:|
| A | 100 | 0.001 m²/s |
| B | 400 | 0.00025 m²/s |
| C | 1000 | 0.0001 m²/s |

Para cada caso:

1. Crea una copia independiente del caso inicial.
2. Modifica `nu` en `constant/transportProperties`.
3. Genera la malla con `blockMesh`.
4. Revisa la configuración temporal y el número de Courant.
5. Ejecuta `icoFoam`.
6. Compara los campos de velocidad en ParaView.

**Precaución:** Los casos con Reynolds más altos pueden necesitar un paso temporal menor, un tiempo de simulación mayor y una malla más refinada. No debemos asumir que la configuración de Re=100 será suficiente para obtener resultados convergidos en Re=400 o Re=1000.

## 16. Preguntas de reflexión

1. ¿Por qué se genera una recirculación dentro de la cavidad?
2. ¿Qué representa físicamente el número de Reynolds?
3. ¿Qué ocurriría si duplicáramos la velocidad de la pared superior?
4. ¿Por qué necesitamos discretizar el dominio?
5. ¿Cuál es la diferencia entre convergencia algebraica y convergencia temporal?
6. ¿Cómo podríamos evaluar si nuestra malla es suficientemente fina?

---

**Instructor:** Luis Andres Mollericon Titirico  
**Universidad Mayor de San Andrés — Ingeniería Mecánica**  
**Octubre de 2026**
