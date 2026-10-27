
# Taller de Introducción a CFD con OpenFOAM

**Universidad Mayor de San Andrés (UMSA) — Ingeniería Mecánica**

**Fechas:** 28 y 29 de octubre de 2026  
**Duración:** 2 sesiones de 90 minutos  
**Modalidad:** Teórico-práctica  
**Instructor:** Luis Andres Mollericon Titirico

---

## 1. Presentación

La dinámica de fluidos computacional (*Computational Fluid Dynamics*, CFD) permite estudiar el comportamiento de los fluidos mediante la resolución numérica de las ecuaciones que gobiernan su movimiento.

En este taller introduciremos los fundamentos físicos y numéricos de CFD utilizando **OpenFOAM**, una plataforma de código abierto ampliamente utilizada en investigación e ingeniería.

El taller está dirigido principalmente a estudiantes de Ingeniería Mecánica. No se requiere experiencia previa con OpenFOAM ni con sistemas Linux.

Durante las sesiones trabajaremos con ejemplos prácticos, desde la preparación de un caso hasta la interpretación de los resultados.

## 2. Objetivos de aprendizaje

Al finalizar el taller, los participantes podrán:

- Reconocer aplicaciones de CFD en ingeniería mecánica.
- Comprender el significado físico de las ecuaciones de conservación de masa y cantidad de movimiento.
- Identificar los conceptos fundamentales del método de volúmenes finitos.
- Utilizar comandos básicos de Linux.
- Comprender la estructura de un caso de OpenFOAM.
- Generar una malla y ejecutar una simulación.
- Visualizar e interpretar resultados mediante ParaView.
- Investigar el efecto del número de Reynolds sobre el flujo.

## 3. Programa

### Sesión 1 — 28 de octubre

**Fundamentos de CFD e instalación de OpenFOAM**

| Duración | Actividad |
|---|---|
| 15 min | Introducción a CFD y aplicaciones en ingeniería |
| 20 min | Conservación de masa, cantidad de movimiento y ecuaciones de Navier–Stokes |
| 15 min | Método de volúmenes finitos y generación de mallas |
| 10 min | Introducción a Linux y estructura de OpenFOAM |
| 30 min | Instalación guiada de OpenFOAM v2512 |

### Sesión 2 — 29 de octubre

**Simulación y análisis de resultados**

| Duración | Actividad |
|---|---|
| 10 min | Repaso y comprobación del entorno |
| 15 min | Estructura de un caso y condiciones de frontera |
| 20 min | Simulación de una cavidad con pared móvil |
| 20 min | Visualización de resultados en ParaView |
| 15 min | Modificación del número de Reynolds |
| 10 min | Interpretación y preguntas |

## 4. Preparación antes del taller

Cada participante deberá disponer de una computadora portátil con Windows y tener instalados:

1. **WSL2 con Ubuntu 22.04 LTS**, para ejecutar OpenFOAM.
2. **Visual Studio Code**, incluyendo la extensión WSL de Microsoft.
3. **ParaView 6.2**, para visualizar los resultados.

La guía detallada está disponible aquí:

**[Guía de preparación previa (PDF)](Guia_preparacion_previa_OpenFOAM_UMSA.pdf)**

> **Importante:** No es necesario instalar OpenFOAM antes del taller. Realizaremos su instalación juntos durante la primera sesión.

Se recomienda disponer de conexión a Internet, permisos de administrador y al menos 15–20 GB de espacio libre.

## 5. Material del taller

| Material | Enlace |
|---|---|
| Guía de preparación previa | [PDF](Guia_preparacion_previa_OpenFOAM_UMSA.pdf) |
| Instalación de OpenFOAM v2512 | [Guía 01](docs/01-instalacion-openfoam.md) |
| Introducción a Linux | [Guía 02](docs/02-introduccion-linux.md) |
| Primer caso de simulación | [Guía 03](docs/03-primer-caso-openfoam.md) |
| Visualización en ParaView | [Guía 04](docs/04-visualizacion-paraview.md) |
| Presentaciones | Próximamente |

Los documentos y ejercicios se incorporarán progresivamente a este repositorio.

## 6. Nuestro primer problema: cavidad con pared móvil

Estudiaremos el flujo incompresible dentro de una cavidad cuadrada cuya pared superior se desplaza horizontalmente.

Este problema, conocido como *lid-driven cavity*, es un caso clásico de CFD que permite estudiar la formación de vórtices, la influencia de la viscosidad y el efecto del número de Reynolds.

El número de Reynolds se define como:

$$
Re = \frac{UL}{\nu}
$$

donde:

- \(U\): velocidad característica del flujo.
- \(L\): longitud característica.
- \(µ\): viscosidad cinemática.

Comenzaremos con un caso de \(Re=100\) y posteriormente modificaremos sus parámetros.

Los archivos iniciales se encuentran en:

[`cases/cavity-Re100/`](cases/cavity-Re100/)

## 7. Cómo obtener los materiales

Los participantes pueden descargar el repositorio desde GitHub mediante **Code → Download ZIP**.

Quienes tengan Git instalado también pueden utilizar:

```bash
git clone https://github.com/LuisAndresMT/UMSA-OpenFOAM-Workshop.git
```

Después, desde Ubuntu:

```bash
cd UMSA-OpenFOAM-Workshop
```

Durante el taller aprenderemos a utilizar los archivos del repositorio para ejecutar nuestras propias simulaciones.

## 8. Software utilizado

- [OpenFOAM](https://www.openfoam.com/) — Simulación CFD.
- [Ubuntu](https://ubuntu.com/) — Sistema operativo Linux.
- [WSL](https://learn.microsoft.com/windows/wsl/) — Linux en Windows.
- [Visual Studio Code](https://code.visualstudio.com/) — Edición de archivos.
- [ParaView](https://www.paraview.org/) — Visualización científica.

---

**Instructor:** Luis Andres Mollericon Titirico  
**Universidad Mayor de San Andrés — Ingeniería Mecánica**  
**Octubre de 2026**
