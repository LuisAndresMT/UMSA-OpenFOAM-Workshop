
# Guía 02 — Introducción a Linux y comandos básicos

**Taller de Introducción a CFD con OpenFOAM**  
Universidad Mayor de San Andrés (UMSA) — Ingeniería Mecánica

**Instructor:** Luis Andres Mollericon Titirico  
**Sesión:** 28 de octubre de 2026  
**Duración en clase:** 10 minutos

---

## 1. ¿Por qué utilizamos Linux?

OpenFOAM es una plataforma de simulación CFD que se utiliza principalmente mediante comandos ejecutados en una terminal.

Linux nos permite:

- Navegar entre carpetas y archivos.
- Editar los parámetros de una simulación.
- Generar mallas.
- Ejecutar solvers.
- Consultar resultados y detectar errores.

En este taller utilizaremos **Ubuntu 22.04 mediante WSL2**, que permite ejecutar Linux dentro de Windows.

## 2. La terminal

Abre Ubuntu desde el menú Inicio de Windows.

Verás una línea similar a:

```text
estudiante@computadora:~$
```

Sus componentes son:

- `estudiante`: nombre del usuario.
- `computadora`: nombre del equipo.
- `~`: carpeta personal del usuario.
- `$`: indica que la terminal está esperando un comando.

Para ejecutar un comando, escríbelo y pulsa `Enter`.

## 3. Comandos fundamentales

| Comando | Descripción |
|---|---|
| `pwd` | Muestra la carpeta actual |
| `ls` | Muestra archivos y carpetas |
| `ls -l` | Muestra información detallada |
| `cd carpeta` | Entra en una carpeta |
| `cd ..` | Regresa a la carpeta superior |
| `cd ~` | Regresa a la carpeta personal |
| `mkdir nombre` | Crea una carpeta |
| `cp origen destino` | Copia un archivo |
| `cp -r origen destino` | Copia una carpeta |
| `mv origen destino` | Mueve o renombra archivos |
| `cat archivo` | Muestra el contenido de un archivo |
| `nano archivo` | Abre un editor de texto |
| `clear` | Limpia la pantalla |

**Nota:** Linux distingue entre mayúsculas y minúsculas. `Caso` y `caso` son nombres diferentes.

## 4. Primer ejercicio: navegar entre carpetas

Comprueba tu ubicación actual:

```bash
pwd
```

Regresa a tu carpeta personal:

```bash
cd ~
```

Crea una carpeta para los ejercicios:

```bash
mkdir -p OpenFOAM-Workshop
```

Entra en ella:

```bash
cd OpenFOAM-Workshop
```

Comprueba la nueva ubicación:

```bash
pwd
```

Deberías obtener una ruta similar a:

```text
/home/estudiante/OpenFOAM-Workshop
```

## 5. Trabajar con archivos

Crea un archivo de texto:

```bash
nano notas.txt
```

Escribe:

```text
Mi primer taller de CFD con OpenFOAM
```

Guarda con `Ctrl + O`, confirma con `Enter` y sal con `Ctrl + X`.

Consulta el contenido:

```bash
cat notas.txt
```

Crea una copia:

```bash
cp notas.txt copia-notas.txt
```

Muestra los archivos:

```bash
ls
```

## 6. Rutas absolutas y relativas

Una **ruta absoluta** comienza desde la raíz del sistema `/`.

Ejemplo:

```text
/home/estudiante/OpenFOAM-Workshop/notas.txt
```

Una **ruta relativa** se interpreta desde la carpeta actual.

Ejemplo:

```text
notas.txt
```

Otros símbolos útiles:

| Símbolo | Significado |
|---|---|
| `/` | Raíz del sistema |
| `~` | Carpeta personal |
| `.` | Carpeta actual |
| `..` | Carpeta superior |

## 7. Linux y Windows mediante WSL2

WSL2 permite acceder a los archivos de Windows desde Ubuntu.

Por ejemplo, la unidad `C:` de Windows se encuentra en:

```text
/mnt/c/
```

Sin embargo, para ejecutar simulaciones de OpenFOAM, recomendamos trabajar en el sistema de archivos de Linux, dentro de `/home/usuario/`, para obtener un mejor rendimiento.

Podemos editar los archivos desde Visual Studio Code utilizando la extensión WSL.

Desde la terminal de Ubuntu, abre la carpeta actual con:

```bash
code .
```

El punto `.` significa «carpeta actual».

## 8. Primer contacto con OpenFOAM

Si OpenFOAM v2512 está instalado y activado, ejecuta:

```bash
foamVersion
```

Después:

```bash
which blockMesh
```

Y:

```bash
which icoFoam
```

Estos comandos permiten comprobar que OpenFOAM está disponible.

### Comandos que utilizaremos en la segunda sesión

| Comando | Función |
|---|---|
| `blockMesh` | Generar la malla |
| `checkMesh` | Comprobar la calidad y estructura de la malla |
| `icoFoam` | Resolver el problema de flujo |
| `foamVersion` | Consultar la versión de OpenFOAM |

## 9. Consejos prácticos

- Pulsa `Tab` para completar nombres de archivos y comandos.
- Utiliza las flechas ↑ y ↓ para recuperar comandos anteriores.
- Pulsa `Ctrl + C` para interrumpir un proceso en ejecución.
- Evita utilizar `sudo` para ejecutar simulaciones.
- No utilices `rm -rf` si no comprendes exactamente qué archivos eliminará.

## 10. Ejercicio de repaso

Sin consultar los ejemplos anteriores, intenta:

1. Regresar a tu carpeta personal.
2. Crear una carpeta llamada `CFD`.
3. Entrar en ella.
4. Crear un archivo llamado `ejercicio.txt`.
5. Mostrar su contenido desde la terminal.
6. Abrir la carpeta con Visual Studio Code.

---

**Instructor:** Luis Andres Mollericon Titirico  
**Universidad Mayor de San Andrés — Ingeniería Mecánica**  
**Octubre de 2026**
