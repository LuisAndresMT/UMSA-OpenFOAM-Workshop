
# Guía 04 — Visualización y análisis de resultados con ParaView

**Taller de Introducción a CFD con OpenFOAM**  
Universidad Mayor de San Andrés (UMSA) — Ingeniería Mecánica

**Instructor:** Luis Andres Mollericon Titirico  
**Sesión:** 29 de octubre de 2026

---

## 1. Objetivo

En esta práctica utilizaremos **ParaView 6.2** para visualizar e interpretar los resultados de la simulación de una cavidad con pared móvil (*lid-driven cavity*).

Aprenderemos a:

- Abrir resultados de OpenFOAM en ParaView.
- Visualizar la magnitud del campo de velocidades.
- Identificar las regiones de mayor y menor velocidad.
- Representar vectores de velocidad.
- Examinar el movimiento de recirculación.
- Extraer perfiles de velocidad para analizar el flujo.

## 2. Antes de comenzar

Necesitamos:

- OpenFOAM v2512 funcionando en Ubuntu 22.04.
- ParaView 6.2 instalado en Windows.
- El caso `cavity-Re100` ejecutado correctamente.

Si todavía no has realizado la simulación, sigue la [Guía 03 — Primera simulación CFD](03-primer-caso-openfoam.md).

## 3. Preparar el caso para ParaView

Abre Ubuntu y entra en la carpeta del caso:

```bash
cd ~/UMSA-OpenFOAM-Workshop/cases/cavity-Re100
```

Comprueba que existen resultados de la simulación:

```bash
ls
```

Además de las carpetas `0`, `constant` y `system`, deberían aparecer carpetas de tiempo, por ejemplo:

```text
0
0.1
0.2
...
5
constant
system
```

Estas carpetas contienen los campos calculados en los tiempos de escritura configurados en `controlDict`.

### Crear el archivo de lectura

Ejecuta:

```bash
touch cavity.foam
```

Este archivo vacío permite que ParaView reconozca el directorio como un caso de OpenFOAM.

**Importante:** El archivo `cavity.foam` no contiene los resultados. ParaView utiliza este archivo para localizar y leer los datos del caso.

## 4. Abrir el caso desde Windows

Como ParaView está instalado en Windows y la simulación se encuentra en Ubuntu, utilizaremos el acceso de Windows a los archivos de WSL.

Desde Ubuntu, dentro del directorio del caso, ejecuta:

```bash
explorer.exe .
```

Se abrirá el Explorador de archivos de Windows directamente en la carpeta del caso.

Localiza el archivo:

```text
cavity.foam
```

Abre ParaView y selecciona:

**File → Open**

Busca `cavity.foam`, selecciónalo y pulsa **OK**.

En el panel **Properties**, pulsa **Apply**.

Si ParaView ofrece varias opciones de lectura, utiliza el lector de OpenFOAM disponible para el archivo `.foam`.

### Comprobar los campos

En las propiedades del lector, comprueba que los campos `U` y `p` estén seleccionados.

Si cambias la selección, pulsa nuevamente **Apply**.

## 5. Seleccionar el tiempo de simulación

En la barra superior de ParaView encontrarás los controles temporales.

Selecciona el último tiempo disponible:

$$
t = 5\ \mathrm{s}
$$

También puedes utilizar el botón **Last Frame** para avanzar hasta el último resultado.

**Importante:** Este tiempo corresponde al tiempo físico simulado, no al tiempo que tardó la computadora en ejecutar el cálculo.

## 6. Visualizar la magnitud de velocidad

Selecciona el caso `cavity.foam` en **Pipeline Browser**.

En la barra de herramientas de coloreado:

1. Selecciona el campo `U`.
2. Cambia el componente de `Magnitude` si es necesario.
3. Activa la barra de colores mediante **Show Color Legend**.
4. Ajusta la cámara con **Reset Camera**.

Si aparece una geometría tridimensional muy delgada, utiliza una vista perpendicular al plano de la cavidad.

### ¿Qué estamos observando?

La magnitud de velocidad se define como:

$$
|\mathbf{U}| = \sqrt{U_x^2 + U_y^2 + U_z^2}
$$

En nuestro problema bidimensional:

$$
U_z = 0
$$

Por tanto:

$$
|\mathbf{U}| = \sqrt{U_x^2 + U_y^2}
$$

Deberíamos observar velocidades mayores cerca de la pared superior móvil y regiones de menor velocidad en otras partes de la cavidad.

Los colores representan la magnitud del campo, no necesariamente la dirección del movimiento.

## 7. Diferencia entre magnitud y componentes

ParaView permite visualizar:

- `U → Magnitude`: magnitud total de la velocidad.
- `U → X`: componente horizontal.
- `U → Y`: componente vertical.

### Componente horizontal

Selecciona `U` y después `X`.

Un valor positivo indica movimiento hacia la derecha; uno negativo indica movimiento hacia la izquierda.

### Componente vertical

Selecciona `U` y después `Y`.

Un valor positivo indica movimiento hacia arriba; uno negativo indica movimiento hacia abajo.

**Pregunta:** ¿Por qué pueden aparecer velocidades horizontales negativas si la pared superior se mueve hacia la derecha?

La respuesta está relacionada con la recirculación del fluido.

## 8. Visualizar vectores de velocidad

Los vectores permiten representar tanto la dirección como la magnitud del movimiento.

Selecciona `cavity.foam` en **Pipeline Browser**.

Aplica:

**Filters → Alphabetical → Glyph**

En las propiedades del filtro:

1. Selecciona `Arrow` como tipo de símbolo.
2. Selecciona `U` como campo de orientación.
3. Configura la orientación según los vectores de velocidad.
4. Utiliza una distribución de puntos reducida, si está disponible, para evitar demasiadas flechas.
5. Ajusta el factor de escala para que las flechas sean visibles.
6. Pulsa **Apply**.

Dependiendo de la configuración, los nombres de algunas propiedades pueden variar.

### Interpretación

Observa cómo cambia la dirección del flujo dentro de la cavidad.

¿Puedes identificar el movimiento de recirculación principal?

**Nota:** El tamaño de las flechas depende también de los parámetros de visualización. No debe interpretarse como una medida absoluta sin comprobar la escala.

## 9. Visualizar líneas de corriente

Las líneas de corriente son curvas tangentes al campo de velocidades en un instante dado.

Para generarlas:

1. Selecciona el caso en **Pipeline Browser**.
2. Aplica **Filters → Alphabetical → Stream Tracer**.
3. Selecciona `U` como campo vectorial.
4. Configura una fuente de semillas apropiada dentro de la cavidad.
5. Ajusta los parámetros de integración si es necesario.
6. Pulsa **Apply**.

Puedes comenzar con una línea de semillas situada dentro del dominio, evitando colocar sus extremos sobre las paredes.

Si no aparecen líneas, revisa la posición de las semillas y la longitud máxima de integración.

### Interpretación física

En un campo estacionario, las líneas de corriente describen la dirección local del flujo.

En un flujo transitorio, representan la estructura instantánea del campo en el tiempo seleccionado.

**Atención:** Las líneas de corriente no son necesariamente las trayectorias recorridas por partículas individuales en un flujo transitorio.

## 10. Extraer un perfil de velocidad

Una forma habitual de estudiar el flujo en la cavidad consiste en representar la componente horizontal de velocidad sobre una línea vertical que pasa por el centro.

Utilizaremos:

$$
x = 0.05\ \mathrm{m}
$$

La línea irá desde:

$$
(x,y,z) = (0.05,0,0.005)
$$

hasta:

$$
(x,y,z) = (0.05,0.1,0.005)
$$

La coordenada \(z=0.005\) corresponde al plano medio de nuestro caso, cuyo espesor es de 0.01 m.

### Procedimiento

Selecciona `cavity.foam` y aplica:

**Filters → Data Analysis → Plot Over Line**

En las propiedades del filtro, configura:

**Point 1:**

```text
0.05  0  0.005
```

**Point 2:**

```text
0.05  0.1  0.005
```

Selecciona una resolución apropiada, por ejemplo, 100 intervalos.

Pulsa **Apply**.

ParaView generará una gráfica de los valores muestreados a lo largo de la línea.

### Configurar la gráfica

En las propiedades de visualización del gráfico:

1. Selecciona la componente horizontal `U_X`.
2. Desactiva las demás series si aparecen.
3. Utiliza la distancia a lo largo de la línea como eje horizontal.

La gráfica resultante mostrará cómo cambia la velocidad horizontal desde la pared inferior hasta la superior.

### Interpretación

Deberíamos observar:

- Velocidad horizontal aproximadamente nula en la pared inferior.
- Valores negativos de velocidad horizontal en parte del interior.
- Velocidad horizontal próxima a 1 m/s en la pared superior.

La presencia de valores negativos está relacionada con el movimiento de recirculación.

## 11. Comparar distintos números de Reynolds

Si dispones de simulaciones para diferentes valores de Reynolds, puedes comparar sus campos de velocidad.

Recordemos:

$$
Re = \frac{UL}{\nu}
$$

| Caso | Reynolds | Viscosidad cinemática |
|---|---:|---:|
| A | 100 | 0.001 m²/s |
| B | 400 | 0.00025 m²/s |
| C | 1000 | 0.0001 m²/s |

Al aumentar Reynolds, los efectos inerciales adquieren mayor importancia relativa respecto a los viscosos.

Para comparar correctamente las simulaciones:

- Utiliza la misma orientación de cámara.
- Mantén una escala de colores común.
- Compara campos correspondientes a estados físicos comparables.
- Comprueba la convergencia temporal y la resolución de la malla.

No debemos interpretar diferencias debidas a un tiempo de simulación insuficiente como si fueran únicamente efectos del número de Reynolds.

## 12. Guardar imágenes de los resultados

Para guardar una imagen de la visualización:

**File → Save Screenshot**

Selecciona la ubicación, el nombre y la resolución deseada.

Puedes guardar, por ejemplo:

```text
cavity-Re100-velocity.png
```

Para guardar la configuración de visualización y continuar más tarde:

**File → Save State**

ParaView guardará un archivo de estado `.pvsm`.

Este archivo conserva la configuración de la sesión, pero no sustituye los datos originales de la simulación.

## 13. Preguntas para discutir

1. ¿Dónde se encuentran las velocidades más elevadas?
2. ¿Por qué aparecen velocidades horizontales negativas?
3. ¿Qué información proporciona el campo de magnitud que no muestran las componentes individuales?
4. ¿Cómo se identifica el vórtice principal?
5. ¿Qué diferencias esperarías al aumentar Reynolds?
6. ¿Por qué es importante utilizar una escala de colores común al comparar dos simulaciones?
7. ¿Cómo podríamos comprobar que nuestros resultados son independientes de la malla?

## 14. Resultado esperado

Al finalizar esta práctica, cada participante debería poder:

- Abrir un caso de OpenFOAM en ParaView.
- Seleccionar un tiempo de simulación.
- Representar la magnitud de velocidad.
- Examinar las componentes horizontal y vertical.
- Visualizar vectores y líneas de corriente.
- Obtener un perfil de velocidad sobre una línea definida.

---

**Instructor:** Luis Andres Mollericon Titirico  
**Universidad Mayor de San Andrés — Ingeniería Mecánica**  
**Octubre de 2026**
