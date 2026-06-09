# Contenido base para workshop de 2 horas

## Taller: Git y GitHub — Ramas, Pull Requests y CI/CD

Este taller parte de una base previa: los estudiantes ya conocen Git de forma individual. Ya han trabajado con comandos como `git add`, `git commit`, `git push`, `git pull` y han subido proyectos propios a GitHub.

El objetivo de este segundo taller es avanzar hacia un flujo de trabajo más real, donde varios desarrolladores colaboran sobre un mismo proyecto, trabajan en ramas separadas, revisan cambios mediante Pull Requests, integran código de forma controlada y automatizan validaciones y despliegues usando CI/CD.

Durante la primera parte del taller se explicarán los conceptos principales: ramas, flujo de trabajo colaborativo, Pull Requests, merge, conflictos y CI/CD. Después, los estudiantes trabajarán en equipos de tres personas sobre un proyecto Astro ya preparado. Cada equipo hará un fork del repositorio base, seleccionará una o dos tareas de la tabla del README, creará ramas de trabajo, realizará cambios, abrirá Pull Requests, validará el CI y finalmente integrará los cambios hasta publicar el sitio con GitHub Pages.

---

# 1. Objetivo general del workshop

El objetivo es que los estudiantes comprendan y apliquen un flujo de trabajo colaborativo con Git y GitHub, simulando un entorno real de desarrollo.

Al finalizar, deberán poder:

Crear un fork de un repositorio base.

Trabajar en equipo dentro de un mismo fork.

Crear ramas desde una rama de integración.

Realizar cambios sin afectar directamente la rama principal.

Abrir Pull Requests para revisar e integrar código.

Resolver conflictos cuando dos ramas modifican el mismo archivo.

Comprender cuándo se ejecuta un pipeline de CI.

Comprender cuándo se ejecuta un pipeline de CD.

Desplegar automáticamente una aplicación web en GitHub Pages.

---

# 2. Contexto inicial para los estudiantes

Hasta ahora han trabajado principalmente de forma individual:

```bash
git add .
git commit -m "mensaje"
git push origin main
```

Ese flujo funciona cuando una sola persona trabaja sobre su propio repositorio. El problema aparece cuando varias personas editan el mismo proyecto.

En un equipo real no se recomienda que todos hagan `push` directamente a `main`, porque cualquier cambio incorrecto puede romper el proyecto completo. Para evitar eso, se usan ramas, Pull Requests, revisiones y validaciones automáticas.

El taller se basa en esta transición:

```txt
Trabajo individual:
main → commit → push

Trabajo colaborativo:
feature/* → Pull Request → develop → Pull Request → main → despliegue
```

---

# 3. ¿Qué es una rama en Git?

Una rama es una línea independiente de trabajo dentro del repositorio.

Permite modificar el proyecto sin afectar directamente la rama principal.

Ejemplo:

```bash
git checkout -b feature/about-section
```

Ese comando crea una nueva rama llamada:

```txt
feature/about-section
```

A partir de ese momento, los cambios se hacen en esa rama y no directamente en `main` ni en `develop`.

## Idea clave

Una rama permite trabajar de forma aislada.

```txt
main
 └── develop
      └── feature/about-section
```

Si algo sale mal en la rama `feature/about-section`, no se daña la versión principal del proyecto.

---

# 4. ¿Por qué no trabajar directamente en main?

La rama `main` representa la versión estable del proyecto.

En un flujo profesional, `main` debe contener únicamente código revisado, probado y listo para producción.

No se debe trabajar directamente en `main` porque:

Puede romperse el proyecto para todos.

Puede desplegarse código incompleto.

Puede mezclarse trabajo de varios integrantes sin revisión.

Es más difícil identificar quién introdujo un error.

No permite validar cambios antes de integrarlos.

Por eso se usa esta regla:

```txt
main = producción
develop = integración
feature/* = trabajo individual o por tarea
```

---

# 5. Ramas principales de un proyecto

## main

`main` es la rama estable del proyecto.

Representa la versión final o de producción.

En este workshop, cuando el código llega a `main`, se activa el despliegue automático en GitHub Pages.

Uso:

```txt
main → versión publicada
```

No se trabaja directamente sobre `main`.

---

## develop

`develop` es la rama de integración.

Aquí se integran los cambios de los equipos antes de pasar a producción.

Uso:

```txt
feature/* → develop
```

Sirve para probar el trabajo conjunto antes de enviarlo a `main`.

---

## feature/*

Las ramas `feature/*` se usan para nuevas funcionalidades, cambios visuales o secciones específicas.

Ejemplos:

```txt
feature/hero-section
feature/about-section
feature/cicd-section
feature/sponsors-section
```

Cada tarea del workshop debe hacerse en una rama `feature/*`.

---

## fix/*

Las ramas `fix/*` se usan para corregir errores detectados durante el desarrollo.

Ejemplo:

```txt
fix/header-responsive
fix/build-error
```

Normalmente se crean desde `develop`.

---

## hotfix/*

Las ramas `hotfix/*` se usan para corregir errores urgentes en producción.

Ejemplo:

```txt
hotfix/broken-deploy
hotfix/github-pages-path
```

Normalmente se crean desde `main`, porque corrigen algo que ya está publicado.

Para esta práctica, `hotfix/*` se explicará como concepto, pero el flujo principal será:

```txt
feature/* → develop → main
```

---

# 6. Flujo general de ramas

El flujo principal del taller será:

```txt
feature/* → Pull Request → develop → Pull Request → main → GitHub Pages
```

Interpretación:

Cada integrante crea una rama `feature/*`.

Trabaja en su sección asignada.

Sube la rama al fork del equipo.

Crea un Pull Request hacia `develop`.

Cuando todas las tareas están en `develop`, se crea otro Pull Request hacia `main`.

Cuando se fusiona en `main`, se activa el despliegue automático.

---

# 7. ¿Qué es un fork?

Un fork es una copia de un repositorio en otra cuenta de GitHub.

En este taller, el repositorio original será el repositorio base del docente. Cada equipo trabajará en un fork propio.

```txt
Repositorio original del docente
        ↓
Fork del equipo
        ↓
Trabajo colaborativo del equipo
```

Solo un integrante del equipo crea el fork.

Luego agrega a los demás como colaboradores.

Los demás integrantes no hacen forks individuales. Todos trabajan sobre el mismo fork del equipo.

---

# 8. ¿Por qué usamos fork en este workshop?

Usamos fork porque permite que cada equipo tenga su propia copia del proyecto sin modificar el repositorio original.

Ventajas:

El repositorio del docente no se daña.

Cada equipo tiene su propio espacio de trabajo.

Cada equipo puede activar su propio GitHub Pages.

Cada equipo practica Pull Requests, ramas y CI/CD.

Se simula un flujo parecido al trabajo con proyectos externos o de código abierto.

---

# 9. origin y upstream

Cuando se clona el fork, Git crea un remoto llamado `origin`.

```txt
origin = fork del equipo
```

Ejemplo:

```bash
git remote -v
```

Resultado esperado:

```txt
origin  https://github.com/usuario-equipo/fs-git-workshop-branchs-cicd.git
```

`origin` es donde el equipo hace `push`.

```bash
git push origin feature/about-section
```

## upstream

`upstream` es el repositorio original del docente.

```txt
upstream = repositorio original
```

Se usa solo si el docente actualiza el repositorio base y el equipo necesita sincronizar esos cambios.

No se hace push a `upstream`.

```txt
origin   → fork del equipo
upstream → repositorio original
```

Para este taller, `upstream` puede manejarse como concepto opcional o avanzado.

---

# 10. ¿Qué es un Pull Request?

Un Pull Request es una solicitud para integrar cambios de una rama hacia otra.

No integra el código automáticamente. Primero permite revisar qué cambió.

Ejemplo:

```txt
feature/about-section → develop
```

El Pull Request permite:

Ver los archivos modificados.

Revisar el código.

Comentar cambios.

Detectar conflictos.

Ejecutar el CI automáticamente.

Decidir si el cambio se integra o no.

---

# 11. ¿Dónde se crea el Pull Request?

En GitHub, dentro del fork del equipo.

Para este taller, el Pull Request no debe apuntar al repositorio original del docente.

Debe apuntar así:

```txt
base repository: fork del equipo
base: develop
compare: feature/nombre-de-la-tarea
```

Ejemplo:

```txt
feature/about-section → develop del fork
```

Cuando todas las tareas estén integradas en `develop`, se crea otro Pull Request:

```txt
develop del fork → main del fork
```

---

# 12. ¿Qué es merge?

Merge significa fusionar una rama con otra.

Cuando se hace merge, los commits de una rama se integran en otra.

Ejemplo:

```txt
feature/about-section → develop
```

Después del merge, `develop` contiene los cambios de `feature/about-section`.

## Merge desde consola

Ejemplo básico:

```bash
git checkout develop
git pull origin develop

git checkout feature/about-section
git merge develop
```

Este merge trae los cambios recientes de `develop` hacia la rama `feature/about-section`.

Esto se hace antes de completar un Pull Request para reducir conflictos.

---

# 13. Comandos principales para trabajar con ramas

## Ver ramas existentes

```bash
git branch
```

## Crear una rama

```bash
git checkout -b feature/about-section
```

## Cambiar de rama

```bash
git checkout develop
```

## Actualizar una rama

```bash
git pull origin develop
```

## Subir una rama al remoto

```bash
git push origin feature/about-section
```

## Traer cambios de develop a mi rama

```bash
git checkout develop
git pull origin develop

git checkout feature/about-section
git merge develop
```

---

# 14. ¿Qué es un conflicto?

Un conflicto ocurre cuando Git no puede decidir automáticamente qué cambio conservar.

Ejemplo:

Dos integrantes modifican el mismo bloque del archivo:

```txt
src/components/Header.astro
```

Git marca el conflicto así:

```txt
<<<<<<< HEAD
Contenido actual de mi rama
=======
Contenido que viene desde develop
>>>>>>> develop
```

El estudiante debe editar manualmente el archivo y dejar la versión final correcta.

Luego:

```bash
git add .
git commit -m "fix: resuelve conflicto con develop"
git push origin feature/header-conflict
```

---

# 15. Buenas prácticas de commits

Un commit debe representar un cambio concreto.

No se recomienda hacer un solo commit gigante al final.

Ejemplos correctos:

```bash
git commit -m "feat: actualiza hero del workshop"
git commit -m "style: mejora tarjetas de flujo git"
git commit -m "fix: corrige error de compilación"
git commit -m "docs: actualiza instrucciones de práctica"
```

Convenciones recomendadas:

```txt
feat: nueva funcionalidad
fix: corrección de error
style: cambios visuales
docs: documentación
refactor: reorganización sin cambiar comportamiento
ci: cambios en workflows
```

---

# 16. Flujo práctico del workshop

Antes de entrar a CI/CD, se debe explicar el flujo concreto que harán los estudiantes.

## Flujo del equipo

Un integrante crea el fork.

Agrega a los otros dos integrantes como colaboradores.

El equipo clona el fork.

Se crea la rama `develop`.

Cada integrante selecciona una tarea del README.

Cada integrante crea una rama `feature/*`.

Cada integrante modifica su sección.

Cada integrante hace commit y push.

Cada integrante abre un Pull Request hacia `develop`.

El equipo revisa los Pull Requests.

El CI valida automáticamente cada Pull Request.

Si el CI pasa, se hace merge hacia `develop`.

Cuando todas las tareas estén integradas, se crea Pull Request de `develop` hacia `main`.

Cuando se fusiona en `main`, se activa el despliegue automático.

El sitio queda publicado en GitHub Pages.

## Flujo resumido

```txt
fork → develop → feature/* → commit → push → PR → CI → merge a develop → PR a main → CD → GitHub Pages
```

---

# 17. Actividad práctica por equipos

Aunque el README tenga siete tareas, cada equipo de tres personas no debe hacer necesariamente las siete.

Cada equipo puede seleccionar una o dos tareas, según el tiempo y la organización.

Ejemplo:

```txt
Equipo A:
- feature/hero-section
- feature/header-conflict

Equipo B:
- feature/about-section
- feature/sponsors-section

Equipo C:
- feature/cicd-section
- feature/workshop-section
```

La idea no es completar todo el sitio, sino practicar correctamente el flujo colaborativo.

Cada tarea debe pasar por:

```txt
rama → commit → push → Pull Request → CI → merge
```

---

# 18. Tareas posibles del proyecto

## Tarea 1: HeroSection

Rama:

```txt
feature/hero-section
```

Archivo:

```txt
src/sections/HeroSection.astro
```

Qué hacer:

Actualizar el texto principal del Hero.

Mejorar el título, subtítulo o descripción.

Cambiar el botón o destacar el objetivo del taller.

Resultado esperado:

La sección inicial debe explicar claramente el taller de Git, GitHub, ramas y CI/CD.

---

## Tarea 2: AboutSection

Rama:

```txt
feature/about-section
```

Archivo:

```txt
src/sections/AboutSection.astro
```

Qué hacer:

Actualizar la explicación del workshop.

Agregar tarjetas sobre lo que se aprenderá.

Relacionar el taller con trabajo colaborativo real.

Resultado esperado:

La sección debe explicar por qué se usan ramas, Pull Requests y CI/CD.

---

## Tarea 3: GitFlowSection

Rama:

```txt
feature/gitflow-section
```

Archivo:

```txt
src/sections/GitFlowSection.astro
```

Qué hacer:

Mejorar la explicación visual del flujo de ramas.

Incluir `main`, `develop`, `feature/*`, `fix/*` y `hotfix/*`.

Resultado esperado:

La sección debe mostrar claramente cómo se integran los cambios.

---

## Tarea 4: CicdSection

Rama:

```txt
feature/cicd-section
```

Archivo:

```txt
src/sections/CicdSection.astro
```

Qué hacer:

Agregar explicación de CI/CD.

Explicar qué valida el CI y cuándo ocurre el deploy.

Resultado esperado:

La sección debe diferenciar integración continua y despliegue continuo.

---

## Tarea 5: WorkshopSection

Rama:

```txt
feature/workshop-section
```

Archivo:

```txt
src/sections/WorkshopSection.astro
```

Qué hacer:

Actualizar los pasos del workshop.

Resumir el flujo completo de trabajo.

Resultado esperado:

La sección debe servir como guía rápida de la práctica.

---

## Tarea 6: HeaderConflict

Rama:

```txt
feature/header-conflict
```

Archivo:

```txt
src/components/Header.astro
```

Qué hacer:

Modificar el menú de navegación.

Agregar, renombrar o reordenar enlaces.

Resultado esperado:

Provocar o resolver un conflicto controlado.

---

## Tarea 7: SponsorsSection

Rama:

```txt
feature/sponsors-section
```

Archivos:

```txt
src/sections/SponsorsSection.astro
src/pages/index.astro
```

Qué hacer:

Activar una sección que ya existe, pero no está importada.

Agregar el import:

```astro
import SponsorsSection from "../sections/SponsorsSection.astro";
```

Agregar el componente:

```astro
<SponsorsSection />
```

Resultado esperado:

La sección de aliados aparece en la página principal.

---

# 19. ¿Qué es CI?

CI significa Integración Continua.

Es un proceso automático que valida el proyecto cada vez que hay cambios importantes.

En este taller, el CI valida que el proyecto:

Instale dependencias correctamente.

No tenga errores de Astro o TypeScript.

Compile correctamente.

No llegue código roto a `develop` o `main`.

## Idea central

El CI responde esta pregunta:

```txt
¿El proyecto sigue funcionando después de este cambio?
```

---

# 20. ¿Para qué sirve CI?

Sirve para evitar integrar código roto.

Antes de que un Pull Request se fusione, GitHub Actions ejecuta comandos automáticos.

En nuestro caso:

```bash
pnpm install --frozen-lockfile
pnpm check
pnpm build
```

Si esos comandos fallan, el Pull Request no debe integrarse.

---

# 21. ¿Qué es CD?

CD significa Despliegue Continuo.

Es el proceso automático que publica el proyecto cuando los cambios llegan a una rama específica.

En este taller, el CD publica el sitio en GitHub Pages cuando hay un push a `main`.

## Idea central

El CD responde esta pregunta:

```txt
¿Ya llegó una versión estable a producción? Entonces se publica automáticamente.
```

---

# 22. Diferencia entre CI y CD

CI valida.

CD publica.

```txt
CI = revisar que el proyecto compila
CD = desplegar el proyecto en GitHub Pages
```

Ejemplo:

```txt
feature/* → CI
develop → CI
main → CI + CD
```

---

# 23. ¿Qué es GitHub Actions?

GitHub Actions es la herramienta de automatización de GitHub.

Permite ejecutar tareas automáticamente cuando ocurre un evento.

Ejemplos de eventos:

```txt
push a una rama
creación de Pull Request
merge hacia main
```

Las tareas se definen en archivos `.yml` dentro de:

```txt
.github/workflows/
```

En este proyecto existen dos workflows:

```txt
.github/workflows/ci.yml
.github/workflows/deploy-pages.yml
```

---

# 24. Explicación general del CI del proyecto

Archivo:

```txt
.github/workflows/ci.yml
```

Nombre:

```yml
name: CI - Validación del proyecto
```

Este workflow se ejecuta cuando hay push a:

```txt
main
develop
feature/**
fix/**
hotfix/**
```

También se ejecuta cuando hay Pull Request hacia:

```txt
main
develop
```

## ¿Por qué se ejecuta en feature/*?

Porque se quiere validar el trabajo de cada integrante desde su rama.

## ¿Por qué se ejecuta en Pull Request?

Porque antes de integrar cambios en `develop` o `main`, se necesita comprobar que el proyecto funciona.

---

# 25. Pasos del CI

El job principal se llama:

```yml
build:
    name: Validar y compilar Astro
```

Ejecuta estos pasos:

## 1. Descargar repositorio

```yml
uses: actions/checkout@v4
```

GitHub Actions descarga el código del repositorio para poder trabajar con él.

## 2. Configurar pnpm

```yml
uses: pnpm/action-setup@v4
with:
    version: 10
```

Instala `pnpm` en el runner.

## 3. Configurar Node.js

```yml
uses: actions/setup-node@v4
with:
    node-version: 22
    cache: pnpm
```

Configura Node.js 22 y activa caché para acelerar instalaciones.

## 4. Instalar dependencias

```yml
run: pnpm install --frozen-lockfile
```

Instala dependencias respetando exactamente el `pnpm-lock.yaml`.

## 5. Validar proyecto

```yml
run: pnpm check
```

Valida sintaxis, tipos y estructura del proyecto Astro.

## 6. Compilar proyecto

```yml
run: pnpm build
```

Genera el sitio estático en `dist`.

---

# 26. Explicación general del CD del proyecto

Archivo:

```txt
.github/workflows/deploy-pages.yml
```

Nombre:

```yml
name: CD - Deploy GitHub Pages
```

Este workflow se ejecuta solo cuando hay push a `main`.

```yml
on:
    push:
        branches:
            - main
```

## ¿Por qué solo en main?

Porque `main` representa producción.

No queremos desplegar ramas incompletas como:

```txt
feature/about-section
develop
fix/header
```

Solo se publica cuando el equipo decidió que el código ya está listo y lo fusionó en `main`.

---

# 27. Pasos del CD

El CD tiene dos jobs:

```txt
build
deploy
```

## Job 1: build

Construye el sitio.

Pasos:

Descargar repositorio.

Configurar pnpm.

Configurar Node.js.

Instalar dependencias.

Compilar Astro.

Subir el artefacto de GitHub Pages.

El artefacto es la carpeta:

```txt
dist/
```

En Astro, `dist` contiene los archivos finales del sitio:

```txt
HTML
CSS
JS
assets
```

---

## Job 2: deploy

Publica el sitio.

```yml
uses: actions/deploy-pages@v4
```

Este paso toma el artefacto generado por el job anterior y lo publica en GitHub Pages.

Resultado:

```txt
https://usuario.github.io/fs-git-workshop-branchs-cicd/
```

---

# 28. Relación entre ramas y CI/CD

## Cuando hago push a feature/*

Ejemplo:

```bash
git push origin feature/about-section
```

Se activa:

```txt
CI
```

No se activa CD.

Motivo:

La rama `feature/*` no es producción.

---

## Cuando creo Pull Request hacia develop

Ejemplo:

```txt
feature/about-section → develop
```

Se activa:

```txt
CI
```

No se activa CD.

Motivo:

Se está validando si el cambio puede integrarse a desarrollo.

---

## Cuando hago merge hacia develop

Ejemplo:

```txt
feature/about-section → develop
```

Se activa:

```txt
CI
```

No se activa CD.

Motivo:

`develop` es integración, todavía no es producción.

---

## Cuando creo Pull Request de develop hacia main

Ejemplo:

```txt
develop → main
```

Se activa:

```txt
CI
```

Motivo:

Antes de pasar a producción, se valida el proyecto completo.

---

## Cuando hago merge hacia main

Ejemplo:

```txt
develop → main
```

Se activa:

```txt
CI + CD
```

Motivo:

`main` representa producción. El proyecto se valida y se despliega automáticamente.

---

# 29. Mapa final de activación

```txt
push a feature/*       → CI
push a fix/*           → CI
push a hotfix/*        → CI
push a develop         → CI
Pull Request a develop → CI
Pull Request a main    → CI
push a main            → CI + CD
```

---

# 30. Flujo completo del workshop con CI/CD

```txt
1. Crear fork del repositorio base
2. Agregar colaboradores
3. Crear develop
4. Cada integrante crea feature/*
5. Cada integrante modifica su sección
6. Cada integrante hace commit
7. Cada integrante hace push
8. Se activa CI en la rama feature/*
9. Se crea Pull Request hacia develop
10. Se activa CI en el Pull Request
11. Se revisa y se hace merge hacia develop
12. Cuando todas las tareas están listas, se crea PR develop → main
13. Se activa CI
14. Se hace merge hacia main
15. Se activa CD
16. GitHub Pages publica el sitio
```

---

# 31. Comandos mínimos para la práctica

## Clonar fork

```bash
git clone <url-del-fork>
cd fs-git-workshop-branchs-cicd
```

## Crear develop

Solo lo hace un integrante:

```bash
git checkout main
git pull origin main
git checkout -b develop
git push origin develop
```

## Cambiar a develop

```bash
git checkout develop
git pull origin develop
```

## Crear rama feature

```bash
git checkout -b feature/about-section
```

## Validar proyecto

```bash
pnpm install
pnpm check
pnpm build
```

## Guardar cambios

```bash
git status
git add .
git commit -m "feat: actualiza sección about"
```

## Subir rama

```bash
git push origin feature/about-section
```

## Actualizar mi rama con develop

```bash
git checkout develop
git pull origin develop

git checkout feature/about-section
git merge develop
```

## Resolver conflicto

```bash
git add .
git commit -m "fix: resuelve conflicto con develop"
git push origin feature/about-section
```

---

# 32. Errores comunes que deben evitar

Trabajar directamente en `main`.

Trabajar directamente en `develop` sin autorización.

Crear Pull Request hacia el repositorio original en lugar del fork.

Hacer push a `upstream`.

No ejecutar `pnpm build` antes del Pull Request.

Borrar código de otro compañero para resolver un conflicto.

No revisar hacia qué rama apunta el Pull Request.

Subir cambios sin mensaje claro.

Modificar archivos no asignados.

---

# 33. Cierre conceptual para antes de iniciar la práctica

Antes de iniciar el workshop práctico, debe quedar claro este resumen:

Una rama permite trabajar sin afectar la versión estable.

`main` representa producción.

`develop` representa integración.

`feature/*` representa trabajo individual o por tarea.

Un Pull Request permite revisar antes de integrar.

Un merge fusiona cambios entre ramas.

Un conflicto ocurre cuando dos ramas modifican la misma parte de un archivo.

CI valida automáticamente el proyecto.

CD publica automáticamente el proyecto.

En este taller, el CI se ejecuta en ramas y Pull Requests.

En este taller, el CD se ejecuta únicamente cuando los cambios llegan a `main`.

---

# 34. Distribución sugerida de las 2 horas

## Bloque 1: Introducción y contexto — 10 minutos

Explicar el objetivo del taller.

Recordar el flujo individual que ya conocen.

Presentar el problema del trabajo colaborativo.

## Bloque 2: Ramas y flujo colaborativo — 25 minutos

Explicar `main`, `develop`, `feature/*`, `fix/*`, `hotfix/*`.

Explicar fork, origin y Pull Request.

Mostrar flujo `feature → develop → main`.

## Bloque 3: Pull Request, merge y conflictos — 20 minutos

Explicar PR.

Explicar merge.

Mostrar comandos.

Mostrar ejemplo de conflicto.

## Bloque 4: CI/CD — 25 minutos

Explicar CI.

Explicar CD.

Explicar GitHub Actions.

Analizar `ci.yml`.

Analizar `deploy-pages.yml`.

Explicar qué se activa en cada rama.

## Bloque 5: Práctica por equipos — 35 minutos

Crear fork.

Crear `develop`.

Seleccionar tarea.

Crear rama.

Modificar sección.

Commit y push.

Crear Pull Request.

Revisar CI.

## Bloque 6: Cierre — 5 minutos

Revisar resultados.

Identificar errores comunes.

Ver despliegue final en GitHub Pages.

---

# 35. Estructura sugerida para diapositivas

## Diapositiva 1: Título

Git y GitHub: Ramas, Pull Requests y CI/CD

Subtítulo:

Trabajo colaborativo y despliegue continuo con Astro, GitHub Actions y GitHub Pages.

---

## Diapositiva 2: Punto de partida

Ya conocen:

```txt
git add
git commit
git push
git pull
```

Ahora aprenderán:

```txt
ramas
Pull Requests
merge
conflictos
CI/CD
deploy automático
```

---

## Diapositiva 3: Problema

Trabajar todos sobre `main` es riesgoso.

```txt
main rota = proyecto roto
main incompleta = producción incompleta
main sin revisión = errores no detectados
```

---

## Diapositiva 4: Solución

Usar ramas.

```txt
main      → producción
develop   → integración
feature/* → trabajo por tarea
```

---

## Diapositiva 5: Flujo

```txt
feature/* → Pull Request → develop → Pull Request → main → GitHub Pages
```

---

## Diapositiva 6: Fork

Cada equipo tendrá su propio fork.

```txt
Repositorio base → fork del equipo → ramas → Pull Requests → deploy
```

---

## Diapositiva 7: Pull Request

Un PR permite revisar antes de integrar.

Debe responder:

```txt
¿Qué se cambió?
¿Por qué se cambió?
¿Compila?
¿El CI pasó?
```

---

## Diapositiva 8: Merge

Merge fusiona cambios entre ramas.

```txt
feature/about-section → develop
develop → main
```

---

## Diapositiva 9: Conflictos

Git no siempre puede decidir automáticamente.

```txt
<<<<<<< HEAD
mi cambio
=======
cambio de develop
>>>>>>> develop
```

---

## Diapositiva 10: CI

CI valida automáticamente.

```txt
pnpm install
pnpm check
pnpm build
```

---

## Diapositiva 11: CD

CD despliega automáticamente.

```txt
main → build → dist → GitHub Pages
```

---

## Diapositiva 12: Nuestro CI

Se ejecuta en:

```txt
main
develop
feature/**
fix/**
hotfix/**
Pull Requests a main y develop
```

---

## Diapositiva 13: Nuestro CD

Se ejecuta solo en:

```txt
main
```

Porque `main` representa producción.

---

## Diapositiva 14: Práctica

Cada equipo:

```txt
fork
develop
feature/*
commit
push
Pull Request
CI
merge
deploy
```

---

## Diapositiva 15: Resultado esperado

Al final:

```txt
fork configurado
ramas creadas
Pull Requests realizados
CI aprobado
sitio publicado
```

---

# 36. Mensaje final para estudiantes

El objetivo no es solo modificar una página web. El objetivo es practicar una forma profesional de colaborar en proyectos de software.

El código no debe llegar directamente a producción. Primero se trabaja en una rama, luego se revisa con Pull Request, después se valida con CI, se integra a `develop`, y finalmente se publica desde `main` mediante CD.

Ese flujo permite trabajar en equipo con menos errores, mayor control y una integración más segura.
