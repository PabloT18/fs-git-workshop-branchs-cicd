# Contenido base para workshop de 2 horas

## Taller: Git y GitHub — Ramas, Pull Requests y CI/CD



Este material sirve como guía auxiliar para el workshop. Aquí se resumen los conceptos principales que se aplicarán durante la práctica: ramas, flujo de trabajo colaborativo, Pull Requests, merge, conflictos, CI y CD.

Después de revisar los conceptos, el trabajo se realizará en equipos de tres personas sobre un proyecto Astro ya preparado. Cada equipo hará un fork del repositorio base, seleccionará una o dos tareas de la tabla del README, creará ramas de trabajo, realizará cambios, abrirá Pull Requests, validará el CI e integrará los cambios hasta publicar el sitio con GitHub Pages.

---

## Índice

- [A. ¿Qué es una rama en Git?](#a-qué-es-una-rama-en-git)
- [B. ¿Por qué no trabajar directamente en main?](#b-por-qué-no-trabajar-directamente-en-main)
- [C. Ramas principales de un proyecto](#c-ramas-principales-de-un-proyecto)
- [D. Flujo general de ramas](#d-flujo-general-de-ramas)
- [E. ¿Qué es un fork?](#e-qué-es-un-fork)
- [F. ¿Qué es un Pull Request?](#f-qué-es-un-pull-request)
- [G. ¿Dónde se crea el Pull Request?](#g-dónde-se-crea-el-pull-request)
- [H. ¿Qué es merge?](#h-qué-es-merge)
- [I. Comandos principales para trabajar con ramas](#i-comandos-principales-para-trabajar-con-ramas)
- [J. Flujo práctico del workshop](#j-flujo-práctico-del-workshop)
- [K. ¿Qué es CI?](#k-qué-es-ci)
- [L. ¿Qué es CD?](#l-qué-es-cd)
- [M. Diferencia entre CI y CD](#m-diferencia-entre-ci-y-cd)
- [N. ¿Qué es GitHub Actions?](#n-qué-es-github-actions)
- [O. Explicación general del CI del proyecto](#o-explicación-general-del-ci-del-proyecto)
- [P. Explicación general del CD del proyecto](#p-explicación-general-del-cd-del-proyecto)
- [Q. Relación entre ramas y CI/CD](#q-relación-entre-ramas-y-cicd)
- [R. Mapa final de activación](#r-mapa-final-de-activación)

---


# A. ¿Qué es una rama en Git?

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

# B. ¿Por qué no trabajar directamente en main?

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

# C. Ramas principales de un proyecto

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

## develop / dev

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

# D. Flujo general de ramas

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

# E. ¿Qué es un fork?

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

## ¿Por qué usamos fork en este workshop?

Usamos fork porque permite que cada equipo tenga su propia copia del proyecto sin modificar el repositorio original.

Ventajas:

El repositorio del docente no se daña.

Cada equipo tiene su propio espacio de trabajo.

Cada equipo puede activar su propio GitHub Pages.

Cada equipo practica Pull Requests, ramas y CI/CD.

Se simula un flujo parecido al trabajo con proyectos externos o de código abierto.

---

# F. ¿Qué es un Pull Request?

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

# G. ¿Dónde se crea el Pull Request?

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

# H. ¿Qué es merge?

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

# I. Comandos principales para trabajar con ramas

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

## ¿Qué es un conflicto?

Un conflicto ocurre cuando Git no puede decidir automáticamente qué cambio conservar.

Ejemplo:

Dos integrantes modifican el mismo bloque del archivo:

```txt
src/components/Header.astro
```

Git marca el conflicto así:

![alt text](assets/conte-j.png)

```txt
<<<<<<< HEAD
Contenido actual de mi rama
=======
Contenido que viene desde develop
>>>>>>> develop
```

Se debe editar manualmente el archivo y dejar la versión final correcta.

Luego:

```bash
git add .
git commit -m "fix: resuelve conflicto con develop"
git push origin feature/header-conflict
```

---

# J. Flujo práctico del workshop

Antes de trabajar con CI/CD, revisa el flujo completo que se aplicará durante la práctica.

El objetivo no es solamente modificar una página web. El objetivo es aplicar un flujo colaborativo real usando Git, GitHub, ramas, Pull Requests, validación automática y despliegue.

Durante el workshop se trabajará con un proyecto Astro ya configurado. Cada equipo tendrá su propio fork del repositorio base y realizará cambios en una o dos secciones del proyecto.

## Flujo del equipo

El flujo de trabajo será el siguiente:

1. Crear un fork del repositorio base.
2. Agregar a los integrantes del equipo como colaboradores del fork.
3. Clonar el fork del equipo.
4. Crear la rama `develop` dentro del fork.
5. Seleccionar una o dos tareas de la tabla del README.
6. Crear una rama `feature/*` para cada tarea.
7. Modificar únicamente los archivos asignados.
8. Ejecutar las validaciones locales.
9. Crear commits claros.
10. Subir la rama al fork.
11. Crear un Pull Request hacia `develop`.
12. Revisar que el CI pase correctamente.
13. Resolver conflictos si aparecen.
14. Hacer merge hacia `develop`.
15. Crear un Pull Request de `develop` hacia `main`.
16. Hacer merge hacia `main`.
17. Activar el despliegue automático en GitHub Pages.

## Flujo resumido

```txt
fork → develop → feature/* → commit → push → Pull Request → CI → merge a develop → PR a main → CD → GitHub Pages
```

Este flujo permite trabajar de forma ordenada sin modificar directamente la rama principal del proyecto.

---

# K. ¿Qué es CI?

CI significa Integración Continua.

Es un proceso automático que valida el proyecto cada vez que se realizan cambios importantes en el repositorio.

En este workshop, el CI revisa que el proyecto:

* Instale dependencias correctamente.
* No tenga errores de Astro o TypeScript.
* Compile sin errores.
* No integre código roto en `develop` o `main`.

## Idea central

El CI responde esta pregunta:

```txt
¿El proyecto sigue funcionando después de este cambio?
```

---

## ¿Para qué sirve CI?

CI sirve para evitar que se integren cambios que rompen el proyecto.

Antes de fusionar un Pull Request, GitHub Actions ejecuta comandos automáticos.

En este proyecto se ejecutan:

```bash
pnpm install --frozen-lockfile
pnpm check
pnpm build
```

Si alguno de estos comandos falla, el Pull Request no debe integrarse hasta corregir el problema.

---

# L. ¿Qué es CD?

CD significa Despliegue Continuo.

Es el proceso automático que publica el proyecto cuando los cambios llegan a una rama específica.

En este workshop, el CD publica el sitio en GitHub Pages cuando hay cambios en `main`.

## Idea central

El CD responde esta pregunta:

```txt
¿La versión estable llegó a producción? Entonces se publica automáticamente.
```

---

# M. Diferencia entre CI y CD

CI valida.

CD publica.

```txt
CI = revisar que el proyecto compila
CD = desplegar el proyecto en GitHub Pages
```

En este proyecto:

```txt
feature/* → CI
develop → CI
main → CI + CD
```

---

# N. ¿Qué es GitHub Actions?

GitHub Actions es la herramienta de automatización de GitHub.

Permite ejecutar tareas automáticamente cuando ocurre un evento dentro del repositorio.

Eventos comunes:

```txt
push a una rama
creación de Pull Request
merge hacia main
```

Las automatizaciones se definen en archivos `.yml` dentro de:

```txt
.github/workflows/
```

En este proyecto existen dos workflows:

```txt
.github/workflows/ci.yml
.github/workflows/deploy-pages.yml
```

---

# O. Explicación general del CI del proyecto

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

Porque cada rama de trabajo necesita validarse antes de integrarse con el resto del proyecto.

## ¿Por qué se ejecuta en Pull Request?

Porque antes de integrar cambios en `develop` o `main`, se necesita comprobar que el proyecto sigue funcionando correctamente.

---

## Pasos del CI

El job principal se llama:

```yml
build:
    name: Validar y compilar Astro
```

Este job ejecuta los siguientes pasos:

## 1. Descargar repositorio

```yml
uses: actions/checkout@v4
```

GitHub Actions descarga el código del repositorio para poder ejecutar los comandos del proyecto.

## 2. Configurar pnpm

```yml
uses: pnpm/action-setup@v4
with:
    version: 10
```

Instala `pnpm` en el entorno de ejecución.

## 3. Configurar Node.js

```yml
uses: actions/setup-node@v4
with:
    node-version: 22
    cache: pnpm
```

Configura Node.js 22 y activa caché para acelerar la instalación de dependencias.

## 4. Instalar dependencias

```yml
run: pnpm install --frozen-lockfile
```

Instala dependencias respetando exactamente el archivo `pnpm-lock.yaml`.

## 5. Validar proyecto

```yml
run: pnpm check
```

Valida sintaxis, tipos y estructura del proyecto Astro.

## 6. Compilar proyecto

```yml
run: pnpm build
```

Genera el sitio estático en la carpeta `dist`.

---

# P. Explicación general del CD del proyecto

Archivo:

```txt
.github/workflows/deploy-pages.yml
```

Nombre:

```yml
name: CD - Deploy GitHub Pages
```

Este workflow se ejecuta únicamente cuando hay push a `main`.

```yml
on:
    push:
        branches:
            - main
```

## ¿Por qué solo en main?

Porque `main` representa producción.

No se despliegan ramas incompletas como:

```txt
feature/about-section
develop
fix/header
```

Solo se publica cuando el código fue revisado, validado e integrado en `main`.

---

## Pasos del CD

El CD tiene dos jobs:

```txt
build
deploy
```

## Job 1: build

Construye el sitio.

Pasos principales:

1. Descargar el repositorio.
2. Configurar pnpm.
3. Configurar Node.js.
4. Instalar dependencias.
5. Compilar Astro.
6. Subir el artefacto para GitHub Pages.

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

## Job 2: deploy

Publica el sitio.

```yml
uses: actions/deploy-pages@v4
```

Este paso toma el artefacto generado en el job anterior y lo publica en GitHub Pages.

Resultado:

```txt
https://usuario.github.io/fs-git-workshop-branchs-cicd/
```

---

# Q. Relación entre ramas y CI/CD

## Cuando se hace push a feature/*

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

La rama `feature/*` no representa producción.

---

## Cuando se crea Pull Request hacia develop

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

## Cuando se hace merge hacia develop

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

## Cuando se crea Pull Request de develop hacia main

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

## Cuando se hace merge hacia main

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

# R. Mapa final de activación

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

