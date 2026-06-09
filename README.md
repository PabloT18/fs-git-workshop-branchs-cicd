# Git & GitHub - Ramas, Pull Requests y CI/CD

[![Git](https://img.shields.io/badge/Git-2.37+-f14e32?style=for-the-badge\&logo=git\&logoColor=white\&labelColor=101010)](https://git-scm.com/)
[![GitHub](https://img.shields.io/badge/GitHub-Web-blue?style=for-the-badge\&logo=github\&logoColor=white\&labelColor=101010)](https://github.com/)
[![Astro](https://img.shields.io/badge/Astro-Web-ff5d01?style=for-the-badge\&logo=astro\&logoColor=white\&labelColor=101010)](https://astro.build/)
[![pnpm](https://img.shields.io/badge/pnpm-10+-f69220?style=for-the-badge\&logo=pnpm\&logoColor=white\&labelColor=101010)](https://pnpm.io/)
[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI/CD-2088ff?style=for-the-badge\&logo=githubactions\&logoColor=white\&labelColor=101010)](https://github.com/features/actions)

## Descripción

En el taller anterior se trabajaron conceptos fundamentales como commits, ramas, conflictos, `.gitignore`, revert y rebase. En esta nueva práctica se aplicará un flujo más cercano al trabajo real en equipos de desarrollo, usando ramas colaborativas, Pull Requests, revisión de cambios, integración continua y despliegue .

El proyecto base es una landing page desarrollada con Astro. Cada equipo deberá trabajar sobre una sección específica del sitio, crear una rama, realizar cambios, subirlos a GitHub y abrir un Pull Request hacia la rama de integración dentro del fork del equipo.

Además, el repositorio incluye workflows de GitHub Actions para validar automáticamente el proyecto y desplegarlo en GitHub Pages cuando los cambios lleguen a la rama `main` del fork del equipo. HOLA COMO TAI :3

---

## Objetivos de la práctica

Al finalizar la práctica, serán capaces de:

* Crear un fork de un repositorio y configurarlo correctamente para trabajo en equipo.
* Agregar colaboradores a un fork y gestionar el acceso.
* Configurar los remotos `origin` y `upstream` en Git.
* Entender la diferencia entre `origin` (fork del equipo) y `upstream` (repositorio original).
* Crear la rama `develop` dentro del fork cuando solo existe `main`.
* Trabajar con un flujo de ramas profesional dentro del fork del equipo.
* Diferenciar el propósito de `main`, `develop`, `feature/*`, `fix/*` y `hotfix/*`.
* Crear ramas desde `develop` del fork.
* Realizar commits claros y progresivos.
* Subir ramas al fork del equipo usando `git push origin`.
* Crear Pull Requests dentro del fork hacia `develop` del fork.
* Revisar el estado del CI antes de hacer merge.
* Resolver conflictos sin eliminar el trabajo de otros equipos.
* Comprender el funcionamiento básico de CI/CD con GitHub Actions.
* Desplegar automáticamente el sitio en GitHub Pages desde `main` del fork del equipo.

---

## Tecnologías usadas

| Tecnología     | Propósito                                           |
| -------------- | --------------------------------------------------- |
| Astro          | Framework para construir el sitio web estático      |
| pnpm           | Gestor de paquetes del proyecto                     |
| Git            | Sistema de control de versiones                     |
| GitHub         | Plataforma para repositorios, ramas y Pull Requests |
| GitHub Actions | Automatización de validación y despliegue           |
| GitHub Pages   | Publicación automática del sitio web                |

---

## Requisitos previos

Antes de iniciar, debe tener instalado:

* Git
* Node.js 22 o superior
* pnpm 10 o superior
* Visual Studio Code
* Cuenta de GitHub

Verificar versiones:

```bash
git --version
node --version
pnpm --version
```

Si no tienes `pnpm`, instalarlo con:

```bash
npm install -g pnpm
```

---

## Modalidad de trabajo con fork

Esta práctica se realizará por equipos.

Cada equipo trabajará sobre un **fork** del repositorio original. Solo **un integrante** del equipo debe crear el fork. Luego, ese integrante debe agregar a sus compañeros como colaboradores del fork para que todos puedan crear ramas, subir cambios y abrir Pull Requests dentro del mismo repositorio compartido del equipo.

El repositorio original se mantiene como base de referencia y **no se modifica** durante la práctica. Todo el trabajo colaborativo del equipo ocurre dentro del fork.

```txt
Repositorio original
        │
        │  fork (copia a la cuenta del equipo)
        ▼
Fork del equipo (en la cuenta de un integrante)
        │
        ├── main        → despliegue en GitHub Pages del equipo
        ├── develop     → integración del trabajo del equipo
        ├── feature/*   → ramas de trabajo individuales o por tarea
        ├── fix/*       → correcciones dentro del fork
        └── hotfix/*    → correcciones urgentes dentro del fork
```

### ¿Quién hace el fork?

**Solo un integrante** del equipo debe crear el fork. Si cada persona hace su propio fork, el equipo pierde el espacio de trabajo compartido y no puede colaborar en el mismo repositorio.

Para un equipo de tres personas:

```txt
Integrante 1:
  → Crea el fork en su cuenta de GitHub.
  → Agrega a Integrante 2 e Integrante 3 como colaboradores.

Integrante 2:
  → Acepta la invitación de colaboración.
  → Clona el fork del Integrante 1, no el repositorio original.
  → Trabaja y hace push directamente al fork del equipo.

Integrante 3:
  → Acepta la invitación de colaboración.
  → Clona el fork del Integrante 1, no el repositorio original.
  → Trabaja y hace push directamente al fork del equipo.
```

---

## Configuración inicial del fork

### Paso 1: Crear el fork (solo el integrante responsable)

El integrante responsable del equipo debe:

1. Ingresar al repositorio original en GitHub.
2. Hacer clic en el botón **Fork** (esquina superior derecha).
3. GitHub mostrará un formulario con las opciones del fork.
4. Revisar si aparece la opción **Copy the DEFAULT branch only**.
   * Si esa opción está **activada**, el fork copiará solo la rama por defecto del repositorio original (normalmente `main`). La rama `develop` no existirá en el fork y deberá crearse manualmente (ver Paso 5).
   * Si esa opción **no está activada**, GitHub copiará todas las ramas del repositorio original, incluyendo `develop`.
5. Para esta práctica se recomienda **dejar activada** la opción **Copy the DEFAULT branch only**. Así el fork queda limpio con solo `main`, y el equipo crea `develop` manualmente como parte del ejercicio.
6. Hacer clic en **Create fork**.

El fork quedará disponible en la cuenta del integrante con una URL como:

```txt
https://github.com/usuario-del-equipo/fs-git-workshop-branchs-cicd
```

### Paso 2: Agregar colaboradores al fork (solo el integrante que creó el fork)

Para que los demás integrantes del equipo puedan hacer push de ramas directamente al fork, deben ser agregados como colaboradores. Sin este paso, los demás podrán clonar y leer el fork, pero no podrán subir cambios.

Pasos:

1. Entrar al fork en GitHub (en la cuenta del integrante que lo creó).
2. Ir a **Settings**.
3. En el menú lateral, entrar a **Collaborators** (dentro de "Access").
4. Hacer clic en **Add people**.
5. Buscar el nombre de usuario de GitHub de cada integrante del equipo.
6. Confirmar la invitación.

Los demás integrantes recibirán un correo o una notificación en GitHub con la invitación. **Deben aceptarla** antes de poder hacer push al fork.


### Paso 3: Clonar el fork (todos los otros integrantes)


Una vez que el fork existe y los colaboradores fueron agregados, **todos los integrantes** deben clonar el fork del equipo. No se debe clonar el repositorio original.

```bash
git clone <url-del-fork-del-equipo>
cd fs-git-workshop-branchs-cicd
```

Ejemplo:

```bash
git clone https://github.com/usuario-del-equipo/fs-git-workshop-branchs-cicd.git
cd fs-git-workshop-branchs-cicd
```

Verificar que el remoto `origin` apunta al fork del equipo, no al repositorio original:

```bash
git remote -v
```

Resultado esperado:

```txt
origin  https://github.com/usuario-del-equipo/fs-git-workshop-branchs-cicd.git (fetch)
origin  https://github.com/usuario-del-equipo/fs-git-workshop-branchs-cicd.git (push)
```

Si `origin` apunta al repositorio original en lugar del fork, los pushes fallarán o irán al lugar incorrecto.


### Paso 4: Crear la rama develop en el fork

Si el fork solo copió `main`, es necesario crear la rama `develop` dentro del fork. Este paso lo ejecuta **un solo integrante** del equipo, y luego los demás la descargan.

El integrante que creó el fork ejecuta:

```bash
git checkout main
git pull origin main
git checkout -b develop
git push origin develop
```

Los demás integrantes descargan la rama `develop` del fork:

```bash
git fetch origin
git checkout develop
git pull origin develop
```

A partir de este punto, todo el trabajo del equipo parte desde `develop` del fork.

### Paso 5: Instalar dependencias y verificar el proyecto

```bash
pnpm install
pnpm dev
```

Abrir en el navegador:

```txt
http://localhost:4321
```

Detener el servidor con `Ctrl + C` y ejecutar las validaciones:

```bash
pnpm check
pnpm build
```

Si ambos comandos terminan sin errores, el proyecto está listo para trabajar.

---

## Comandos principales del proyecto

```bash
# Instalar dependencias
pnpm install

# Ejecutar servidor de desarrollo
pnpm dev

# Validar el proyecto (TypeScript y sintaxis Astro)
pnpm check

# Compilar para producción
pnpm build

# Previsualizar el build localmente
pnpm preview
```

Antes de abrir un Pull Request, se recomienda ejecutar siempre:

```bash
pnpm check
pnpm build
```

Si alguno de estos comandos falla localmente, el CI también fallará en GitHub Actions y el Pull Request no podrá ser integrado hasta que se corrija el error.

---

## Estructura del proyecto

```txt
fs-git-workshop-branchs-cicd/
├── .github/
│   └── workflows/
│       ├── ci.yml              # Validación automática en todas las ramas
│       └── deploy-pages.yml    # Despliegue automático a GitHub Pages (solo main)
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── Header.astro        # Barra de navegación sticky
│   │   ├── Footer.astro        # Pie de página institucional
│   │   ├── EventInfoCard.astro # Tarjeta de información del evento
│   │   ├── SectionHeading.astro # Encabezado reutilizable de sección
│   │   └── TechCard.astro      # Tarjeta técnica con efecto neón
│   ├── layouts/
│   │   └── MainLayout.astro    # Layout principal con HTML base
│   ├── sections/
│   │   ├── HeroSection.astro        # Sección principal tipo flyer
│   │   ├── AboutSection.astro       # Descripción del taller
│   │   ├── GitFlowSection.astro     # Diagrama de flujo de ramas
│   │   ├── CicdSection.astro        # Explicación de CI/CD
│   │   ├── WorkshopSection.astro    # Pasos del workshop
│   │   ├── PracticeSection.astro    # Tareas por equipo
│   │   ├── EventDetailsSection.astro # Datos del evento
│   │   └── SponsorsSection.astro    # Sección preparada para práctica (no importada aún)
│   ├── styles/
│   │   └── global.css          # Variables CSS, reset y clases globales
│   └── pages/
│       └── index.astro         # Página principal
├── astro.config.mjs
├── package.json
├── pnpm-lock.yaml
├── README.md
└── tsconfig.json
```

### Nota sobre `SponsorsSection.astro`

El archivo `SponsorsSection.astro` existe dentro del proyecto, pero no está importado en `src/pages/index.astro`. Esta sección está preparada para que el Equipo 7 la active durante la práctica mediante una rama `feature/sponsors-section` y un Pull Request hacia `develop` dentro del fork del equipo.

---

# Flujo de ramas de la práctica

Todo el trabajo del equipo ocurre dentro del fork. El repositorio original **no se modifica** durante la práctica.

```txt
Repositorio original
        │
        │  fork
        ▼
Fork del equipo
        │
        ├── feature/* ──────┐
        │                   ▼
        ├── fix/*    ──── develop del fork ──── Pull Request ──── main del fork ──── GitHub Pages del equipo
        │                   ▲
        └── hotfix/* ───────┘
```

Flujo completo:

```txt
feature/* → Pull Request (dentro del fork) → develop del fork → Pull Request (dentro del fork) → main del fork → GitHub Pages del equipo
```

## Descripción de ramas dentro del fork

| Rama        | Propósito dentro del fork                                                      |
| ----------- | ------------------------------------------------------------------------------ |
| `main`      | Producción del fork. Solo recibe cambios aprobados. Activa el despliegue.      |
| `develop`   | Integración del fork. Aquí se unen los trabajos de todos los integrantes.      |
| `feature/*` | Ramas para nuevas funcionalidades o modificaciones. Se crean desde `develop`.  |
| `fix/*`     | Correcciones dentro del fork. Se crean desde `develop`.                        |
| `hotfix/*`  | Correcciones urgentes. Se crean desde `main` del fork.                         |

---

# Reglas de trabajo

Durante la práctica se deben cumplir estas reglas:

* No trabajar directamente en `main` del fork.
* No trabajar directamente en `develop` del fork.
* Solo un integrante del equipo debe crear el fork.
* Los demás integrantes clonan el fork del equipo, **no el repositorio original**.
* Cada integrante debe agregar el repositorio original como remoto `upstream`.
* Cada equipo debe crear su rama desde `develop` del fork.
* Cada rama debe tener un nombre claro y descriptivo.
* Cada commit debe representar un cambio concreto y pequeño.
* Antes de subir cambios, ejecutar `pnpm build` para verificar que el proyecto compila.
* Antes de abrir un Pull Request, actualizar la rama con los cambios recientes de `develop` del fork.
* No modificar archivos asignados a otros equipos sin coordinación.
* No resolver conflictos eliminando el código de otros compañeros.
* Todo Pull Request debe tener título y descripción claros.
* El merge solo se realiza cuando el CI está aprobado (marca verde en GitHub).
* Los Pull Requests se crean dentro del fork del equipo, no hacia el repositorio original.
* El despliegue se realiza desde `main` del fork, no desde el repositorio original.

---

# Actividad práctica por tareas

Cada equipo trabaja sobre una sección del proyecto dentro de su fork.


| Equipo   | Rama sugerida              | Archivo principal                                              |
| -------- | -------------------------- | -------------------------------------------------------------- |
| Equipo 1 | `feature/hero-section`     | `src/sections/HeroSection.astro`                               |
| Equipo 2 | `feature/about-section`    | `src/sections/AboutSection.astro`                              |
| Equipo 3 | `feature/gitflow-section`  | `src/sections/GitFlowSection.astro`                            |
| Equipo 4 | `feature/cicd-section`     | `src/sections/CicdSection.astro`                               |
| Equipo 5 | `feature/workshop-section` | `src/sections/WorkshopSection.astro`                           |
| Equipo 6 | `feature/header-conflict`  | `src/components/Header.astro`                                  |
| Equipo 7 | `feature/sponsors-section` | `src/sections/SponsorsSection.astro` y `src/pages/index.astro` |

---

# Parte 1: Preparación inicial del fork

Explicadas anteriormente. Revisar pasos de la Parte 1 para configurar correctamente el fork del equipo antes de iniciar el trabajo colaborativo.


# Parte 2: Crear una rama de trabajo

Cada integrante parte desde `develop` del fork actualizado.

```bash
git checkout develop
git pull origin develop
```

Crear la rama asignada al equipo:

```bash
git checkout -b feature/nombre-de-la-tarea
```

Ejemplo para el Equipo 2:

```bash
git checkout -b feature/about-section
```

Verificar en qué rama se está trabajando:

```bash
git branch
```

La rama activa aparece marcada con un asterisco. Debe ser `feature/nombre-de-la-tarea`.

---

# Parte 3: Modificar el proyecto

Cada equipo modifica únicamente los archivos asignados en la tabla de actividades. No se deben tocar archivos de otros equipos sin coordinación.

Ejemplo para el Equipo 2:

```txt
src/sections/AboutSection.astro
```

Después de modificar el archivo, verificar el estado del repositorio:

```bash
git status
```

Ejecutar validaciones antes de guardar cambios:

```bash
pnpm check
pnpm build
```

Si hay errores, corregirlos antes de continuar. No se recomienda hacer commit de código que no compila.

---

# Parte 4: Crear commits

Agregar los archivos modificados al área de preparación:

```bash
git add .
```

Crear el commit con un mensaje descriptivo:

```bash
git commit -m "feat: actualiza sección about del workshop"
```

## Convención recomendada para commits

| Prefijo     | Uso                                                |
| ----------- | -------------------------------------------------- |
| `feat:`     | Nueva funcionalidad o sección                      |
| `fix:`      | Corrección de error                                |
| `style:`    | Cambios visuales o CSS                             |
| `docs:`     | Cambios en documentación                           |
| `refactor:` | Reorganización de código sin cambiar funcionalidad |
| `ci:`       | Cambios en workflows o automatización              |

Ejemplos:

```bash
git commit -m "feat: agrega tarjetas del flujo git"
git commit -m "style: mejora diseño responsive del hero"
git commit -m "docs: actualiza instrucciones de ramas"
git commit -m "ci: ajusta workflow de validación"
```

Se recomienda hacer commits pequeños y frecuentes. Cada commit debe representar un cambio concreto, no acumular todo el trabajo en un solo commit al final.

---

# Parte 5: Subir la rama al fork del equipo

El push **siempre se hace hacia `origin`**, que apunta al fork del equipo. Nunca se hace push hacia `upstream`.

```bash
git push origin feature/nombre-de-la-tarea
```

Ejemplo:

```bash
git push origin feature/about-section
```

Si es la primera vez que se sube esta rama, Git puede mostrar un comando sugerido con `--set-upstream`. También puede aparecer directamente un enlace para crear el Pull Request en GitHub.

---

# Parte 6: Crear Pull Request hacia `develop` del fork

El Pull Request se crea **dentro del fork del equipo**. El destino del PR es `develop` del fork, no el repositorio original.

En GitHub:

1. Ir al fork del equipo en GitHub.
2. Entrar en la pestaña **Pull Requests**.
3. Seleccionar **New Pull Request**.
4. Verificar los repositorios seleccionados:
   * En **base repository**: debe aparecer el fork del equipo, no el repositorio original.
   * En **base**: seleccionar `develop` del fork.
   * En **compare**: seleccionar `feature/nombre-de-la-tarea`.
5. Agregar un título claro.
6. Agregar una descripción de los cambios realizados.
7. Crear el Pull Request.

### Advertencia: GitHub puede sugerir el repositorio original como destino

Al crear el Pull Request desde el fork, GitHub puede proponer automáticamente el repositorio original como destino. Esto no es correcto para esta práctica. Si ocurre, hacer clic en **compare across forks** y seleccionar el fork del equipo como repositorio base.

Ejemplo de título:

```txt
feat: actualiza sección About del workshop
```

Ejemplo de descripción:

```md
## Cambios realizados

- Se actualizó el contenido de la sección About.
- Se agregaron tarjetas descriptivas sobre Git colaborativo.
- Se verificó el proyecto con pnpm build.

## Validación local

- [x] pnpm check
- [x] pnpm build
```

---

# Parte 7: Revisión del CI

Cuando se crea el Pull Request dentro del fork, GitHub Actions ejecuta automáticamente el workflow de CI, siempre que el fork tenga Actions habilitado.

### Habilitar GitHub Actions en el fork

La primera vez que se use el fork, GitHub puede solicitar confirmación para ejecutar Actions. Entrar a la pestaña **Actions** del fork y hacer clic en **I understand my workflows, go ahead and enable them**.

### Qué hace el CI

El CI realiza los siguientes pasos:

1. Descarga del repositorio del fork.
2. Configuración de `pnpm`.
3. Configuración de Node.js 22.
4. Instalación de dependencias con `pnpm install --frozen-lockfile`.
5. Validación con `pnpm check`.
6. Compilación con `pnpm build`.

Si el CI falla, el Pull Request no debe ser integrado hasta que se corrija.

Para corregir un fallo del CI:

```bash
# Corregir el error localmente
pnpm check
pnpm build

# Agregar cambios
git add .

# Crear nuevo commit
git commit -m "fix: corrige error detectado por CI"

# Subir cambios al fork
git push origin feature/nombre-de-la-tarea
```

El Pull Request se actualizará automáticamente y el CI volverá a ejecutarse.

---

# Parte 8: Actualizar la rama antes del merge

Antes de integrar el Pull Request, es necesario que la rama del equipo esté actualizada con los últimos cambios de `develop` del fork. Esto minimiza la posibilidad de conflictos durante el merge.

```bash
git checkout develop
git pull origin develop

git checkout feature/nombre-de-la-tarea
git merge develop
```

Si no hay conflictos:

```bash
git push origin feature/nombre-de-la-tarea
```

Si hay conflictos, continuar con la Parte 9.

---

# Parte 9: Resolver conflictos

Un conflicto ocurre cuando dos ramas del fork modificaron la misma parte de un archivo y Git no puede determinar automáticamente cuál versión conservar.

Git marcará el conflicto en el archivo así:

```txt
Contenido que viene desde develop del fork
```

El bloque entre `<<<<<<< HEAD` y `=======` representa lo que está en tu rama. El bloque entre `=======` y `>>>>>>> develop` representa lo que está en `develop` del fork.

Debes editar el archivo manualmente y dejar la versión final correcta, que puede ser una de las dos versiones, una combinación de ambas, o una versión completamente nueva que integre el trabajo de todos.

Ejemplo de conflicto en el menú de navegación:

```txt
<a href="#inicio">Inicio</a>
<a href="#evento">Evento</a>
```

Versión resuelta (conservando ambos cambios):

```astro
<a href="#inicio">Inicio</a>
<a href="#evento">Evento</a>
<a href="#aliados">Aliados</a>
```

Después de resolver todos los conflictos:

```bash
git add .
git commit -m "fix: resuelve conflictos con develop del fork"
git push origin feature/nombre-de-la-tarea
```

Regresar al Pull Request en GitHub y verificar que el CI vuelva a pasar.

---

# Parte 10: Merge hacia `develop` del fork

Cuando el Pull Request cumpla estas condiciones dentro del fork:

* Tiene descripción clara.
* No tiene conflictos.
* El CI está aprobado (marca verde).
* Fue revisado por el responsable del equipo o el .

Se puede hacer merge hacia `develop` del fork.

Después del merge, todos los integrantes del equipo actualizan su copia local:

```bash
git checkout develop
git pull origin develop
```

---

# Parte 11: Integración hacia `main` del fork

Cuando todas las tareas del equipo estén integradas en `develop` del fork, se crea un Pull Request dentro del fork:

```txt
develop del fork → main del fork
```

Este Pull Request representa el paso a producción dentro del fork del equipo.

Antes de hacer merge:

* Revisar que el sitio funcione correctamente.
* Revisar que el CI esté aprobado.
* Revisar que no existan conflictos.
* Revisar que todas las secciones asignadas estén completas.

Cuando se fusiona hacia `main` del fork, se activa automáticamente el despliegue en GitHub Pages del equipo.

---

# Parte 12: Despliegue automático con GitHub Pages desde el fork

El despliegue se activa automáticamente cuando hay cambios en `main` **del fork del equipo**.

Workflow responsable:

```txt
.github/workflows/deploy-pages.yml
```

### Configurar GitHub Pages en el fork (una sola vez)

El integrante que creó el fork debe configurar GitHub Pages:

1. Ir al fork en GitHub.
2. Entrar a **Settings → Pages**.
3. En **Build and deployment**, seleccionar **GitHub Actions**.
4. Guardar la configuración.

A partir de ese momento, cada push a `main` del fork activará el pipeline de despliegue automáticamente.

### Proceso de despliegue

1. GitHub Actions instala dependencias con `pnpm install`.
2. Compila el sitio con `pnpm build`.
3. Genera la carpeta `dist/`.
4. Sube el artefacto a GitHub Pages.
5. Publica el sitio en la URL del fork del equipo.

La URL del sitio publicado tendrá el formato:

```txt
https://usuario-del-equipo.github.io/fs-git-workshop-branchs-cicd/
```

### Flujo completo con fork

```txt
feature/* (en el fork)
    │
    │ Pull Request dentro del fork
    ▼
develop del fork
    │
    │ Pull Request dentro del fork
    ▼
main del fork
    │
    │ GitHub Actions (deploy-pages.yml)
    ▼
GitHub Pages del equipo
```

---

# CI/CD del proyecto

## CI: Integración continua

Archivo:

```txt
.github/workflows/ci.yml
```

Se ejecuta en el fork cuando hay:

* Push a `main`
* Push a `develop`
* Push a `feature/**`
* Push a `fix/**`
* Push a `hotfix/**`
* Pull Request hacia `main` del fork
* Pull Request hacia `develop` del fork

Comandos ejecutados automáticamente:

```bash
pnpm install --frozen-lockfile
pnpm check
pnpm build
```

Si alguno de estos comandos falla, el CI marca el Pull Request como fallido. No se puede hacer merge hasta que el problema sea corregido y el CI vuelva a pasar.

## CD: Despliegue continuo

Archivo:

```txt
.github/workflows/deploy-pages.yml
```

Se ejecuta solo cuando hay un push a `main` **del fork del equipo**.

Comando principal:

```bash
pnpm build
```

Salida publicada en GitHub Pages:

```txt
dist/
```

El sitio se publica automáticamente sin intervención manual.

---

# Práctica especial: Activar sección pendiente (Equipo 7)

El archivo `src/sections/SponsorsSection.astro` ya existe dentro del proyecto, pero no está importado en la página principal. El **Equipo 7** debe activarlo dentro del fork del equipo.

1. Crear la rama desde `develop` del fork:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/sponsors-section
```

2. Editar `src/pages/index.astro`.

3. Agregar el import junto con los demás imports al inicio del frontmatter:

```astro
import SponsorsSection from "../sections/SponsorsSection.astro";
```

4. Agregar el componente dentro del bloque `<main>`, antes del cierre de la etiqueta:

```astro
<SponsorsSection />
```

5. Validar que el proyecto compila correctamente:

```bash
pnpm check
pnpm build
```

6. Crear commit:

```bash
git add .
git commit -m "feat: activa sección de aliados del workshop"
```

7. Subir la rama al fork del equipo:

```bash
git push origin feature/sponsors-section
```

8. Crear el Pull Request dentro del fork hacia `develop` del fork.

---

# Práctica especial: Conflicto controlado en Header (Equipo 6)

El **Equipo 6** trabajará sobre `src/components/Header.astro`. El objetivo es generar un conflicto controlado con otro equipo que también modifique el mismo archivo, y aprender a resolverlo correctamente durante el proceso de merge dentro del fork.

El Equipo 6 puede hacer alguno de estos cambios en el menú de navegación:

* Agregar un enlace nuevo en el menú.
* Cambiar el texto de una opción existente.
* Reordenar los enlaces del menú.
* Agregar un enlace hacia `#aliados`.

Ejemplo de cambio:

```astro
<a href="#aliados">Aliados</a>
```

Cuando el Pull Request del Equipo 6 intente mergearse en `develop` del fork después de que otro equipo ya haya modificado el mismo bloque del Header, Git marcará conflicto. El equipo deberá:

1. Traer los últimos cambios de `develop` del fork a su rama.
2. Identificar el bloque en conflicto en el archivo.
3. Editar el archivo conservando los cambios válidos de ambas partes.
4. Hacer commit del resultado resuelto.
5. Verificar que el CI vuelve a pasar.
6. Completar el merge del Pull Request.

---

# Checklist antes de abrir un Pull Request

Antes de crear el Pull Request dentro del fork, verificar:

* [ ] Estoy en una rama `feature/*`, `fix/*` o `hotfix/*`.
* [ ] No trabajé directamente en `main` del fork.
* [ ] No trabajé directamente en `develop` del fork.
* [ ] Solo modifiqué los archivos asignados a mi equipo.
* [ ] Ejecuté `pnpm check` sin errores.
* [ ] Ejecuté `pnpm build` sin errores.
* [ ] Mis commits tienen mensajes claros y descriptivos.
* [ ] Subí mi rama al fork del equipo con `git push origin`.
* [ ] El Pull Request apunta al fork del equipo, no al repositorio original.
* [ ] El Pull Request apunta hacia `develop` del fork.

---

# Checklist para aprobar un Pull Request

Antes de hacer merge dentro del fork, verificar:

* [ ] El Pull Request apunta hacia `develop` del fork (no hacia el repositorio original).
* [ ] El repositorio base del Pull Request es el fork del equipo.
* [ ] Tiene título claro.
* [ ] Tiene descripción de los cambios realizados.
* [ ] El CI está aprobado (check verde en GitHub).
* [ ] No existen conflictos sin resolver.
* [ ] El cambio corresponde a la tarea asignada al equipo.
* [ ] No elimina trabajo de otros integrantes o equipos.
* [ ] El sitio compila correctamente con `pnpm build`.

---

# Información del evento

* **Evento:** Taller de Git y GitHub: Flujos de trabajo y despliegue continuo
* **Fecha:** Martes 9 de junio de 2026
* **Hora:** 11h00
* **Lugar:** Aula 1 Juan Bottasso
* **Animador:** Mgtr. Pablo Torres
* **Enfoque:** fork, ramas, Pull Requests, conflictos, CI/CD y despliegue continuo

---

# Resultado esperado

Al finalizar la práctica:

* Cada equipo tendrá su propio fork con `origin` y `upstream` configurados correctamente.
* El fork tendrá las ramas `main` y `develop` activas.
* Todas las ramas `feature/*` del equipo habrán sido integradas en `develop` del fork.
* El Pull Request `develop → main` del fork habrá sido aprobado.
* El sitio del equipo estará publicado en GitHub Pages desde `main` del fork.
* Se habrá aplicado el flujo completo: fork → ramas → Pull Requests → CI → merge → CD.
* El CI habrá validado el proyecto automáticamente en cada Pull Request.
* El CD habrá desplegado la versión final del equipo desde `main` del fork.

---


