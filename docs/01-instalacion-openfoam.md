
# Guía 01 — Instalación de OpenFOAM v2512

**Taller de Introducción a CFD con OpenFOAM**  
Universidad Mayor de San Andrés (UMSA) — Ingeniería Mecánica

**Instructor:** Luis Andres Mollericon Titirico  
**Sesión:** 28 de octubre de 2026  
**Tiempo estimado:** 30 minutos

---

## 1. Objetivo

En esta práctica instalaremos OpenFOAM v2512 en Ubuntu 22.04, utilizando Windows Subsystem for Linux (WSL2).

Al finalizar, podremos ejecutar los programas necesarios para realizar nuestras primeras simulaciones CFD.

**Importante:** Todos los comandos de esta guía se ejecutan en la terminal de **Ubuntu**, no en PowerShell.

## 2. Verificar Ubuntu

Abre Ubuntu desde el menú Inicio de Windows.

Ejecuta:

```bash
lsb_release -a
```

Comprueba que aparezca:

```text
Description: Ubuntu 22.04 LTS
```

Si utilizas una versión diferente, comunícalo al instructor antes de continuar.

## 3. Actualizar la lista de paquetes

Ejecuta:

```bash
sudo apt update
```

**¿Qué significa este comando?**

- `sudo`: permite ejecutar un comando con privilegios de administrador.
- `apt`: gestor de paquetes de Ubuntu.
- `update`: actualiza la información sobre los paquetes disponibles.

Ubuntu puede solicitar tu contraseña de Linux. Al escribirla, no aparecerán caracteres en pantalla. Es normal.

## 4. Configurar el repositorio de OpenFOAM

OpenFOAM v2512 se distribuye mediante el repositorio de paquetes de OpenCFD.

Primero, instala las herramientas necesarias:

```bash
sudo apt install -y curl ca-certificates
```

Después, descarga el script oficial de configuración del repositorio:

```bash
curl -fsSLo /tmp/add-openfoam-repo.sh https://dl.openfoam.com/add-debian-repo.sh
```

Ejecuta el script:

```bash
sudo bash /tmp/add-openfoam-repo.sh
```

Este paso configura el repositorio y su clave de verificación para que Ubuntu pueda encontrar los paquetes de OpenFOAM.

## 5. Instalar OpenFOAM v2512

Actualiza nuevamente la información de paquetes:

```bash
sudo apt update
```

Comprueba que el paquete esté disponible:

```bash
apt-cache policy openfoam2512
```

Debería aparecer una versión candidata (*Candidate*) distinta de `(none)`.

Ahora instala OpenFOAM:

```bash
sudo apt install -y openfoam2512
```

La descarga e instalación pueden tardar varios minutos, dependiendo de la conexión a Internet.

No cierres la terminal mientras se instala.

## 6. Activar OpenFOAM

Después de instalar, ejecuta:

```bash
source /usr/lib/openfoam/openfoam2512/etc/bashrc
```

Este comando carga las variables de entorno necesarias para utilizar los programas de OpenFOAM.

Comprueba la versión:

```bash
foamVersion
```

El resultado esperado es:

```text
OpenFOAM-v2512
```

## 7. Activación automática

Para evitar repetir el comando anterior cada vez que abramos Ubuntu, añadiremos la configuración a `~/.bashrc`.

Ejecuta:

```bash
nano ~/.bashrc
```

Ve al final del archivo y añade:

```bash
# OpenFOAM v2512
source /usr/lib/openfoam/openfoam2512/etc/bashrc
```

Guarda los cambios:

1. `Ctrl + O` para guardar.
2. `Enter` para confirmar.
3. `Ctrl + X` para salir.

Recarga la configuración:

```bash
source ~/.bashrc
```

Ahora OpenFOAM estará disponible automáticamente en las nuevas terminales Bash de Ubuntu.

## 8. Verificar la instalación

Ejecuta estos tres comandos:

```bash
foamVersion
which blockMesh
which icoFoam
```

Los resultados deberían ser similares a:

```text
OpenFOAM-v2512
/usr/lib/openfoam/openfoam2512/platforms/.../bin/blockMesh
/usr/lib/openfoam/openfoam2512/platforms/.../bin/icoFoam
```

Las rutas pueden variar ligeramente entre computadoras.

### ¿Qué hacen estos programas?

| Programa | Función |
|---|---|
| `foamVersion` | Muestra la versión activa de OpenFOAM |
| `blockMesh` | Genera mallas estructuradas a partir de bloques |
| `icoFoam` | Resuelve flujos laminares, transitorios e incompresibles |

## 9. Primera prueba

Para comprobar que OpenFOAM funciona, ejecuta:

```bash
blockMesh -help
```

Si aparece la ayuda del programa, significa que el ejecutable está disponible.

No es necesario generar una malla todavía. Lo haremos durante la segunda sesión.

## 10. Problemas frecuentes

### `foamVersion: command not found`

El entorno de OpenFOAM no está activado.

Ejecuta:

```bash
source /usr/lib/openfoam/openfoam2512/etc/bashrc
```

Y vuelve a comprobar:

```bash
foamVersion
```

### `Unable to locate package openfoam2512`

Ubuntu no encuentra el paquete.

Comprueba que hayas completado el paso 4 y ejecuta:

```bash
sudo apt update
apt-cache policy openfoam2512
```

Si sigue sin aparecer, solicita ayuda al instructor.

### La contraseña no aparece al escribir

Es el comportamiento normal de `sudo` en Linux. Escribe tu contraseña y pulsa `Enter`.

### `Permission denied`

No utilices `sudo` indiscriminadamente. Algunos comandos requieren permisos de administrador y otros deben ejecutarse con tu usuario habitual.

Solicita ayuda antes de modificar permisos o carpetas del sistema.

## 11. Lista de comprobación

- [ ] Ubuntu 22.04 funciona correctamente.
- [ ] El repositorio de OpenFOAM está configurado.
- [ ] OpenFOAM v2512 está instalado.
- [ ] `foamVersion` muestra `OpenFOAM-v2512`.
- [ ] `blockMesh -help` funciona.
- [ ] `which icoFoam` muestra la ruta del solver.

**¡Entorno preparado!**

En la segunda sesión utilizaremos estos programas para simular el flujo dentro de una cavidad con pared móvil.

---

**Instructor:** Luis Andres Mollericon Titirico  
**Universidad Mayor de San Andrés — Ingeniería Mecánica**  
**Octubre de 2026**
