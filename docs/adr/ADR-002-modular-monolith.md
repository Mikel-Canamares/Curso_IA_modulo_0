# ADR-002: Organización de la API como monolito modular por dominio

## Estado

Aceptado

## Contexto

La aplicación necesita una API mantenible para usuarios, autenticación, proyectos y tareas. Se quiere evitar una organización global únicamente por capas técnicas y tampoco existe una necesidad actual que justifique microservicios.

La arquitectura debe mantener responsabilidades separadas, localizar el contexto de cada funcionalidad, controlar dependencias entre dominios, conservar `controller -> service -> repository`, ser sencilla de desplegar y facilitar el trabajo de desarrolladores y agentes.

## Opciones consideradas

### 1. Organización global por capas

Ventajas: sencilla, conocida y familiar para desarrolladores Java/Spring.

Inconvenientes: una funcionalidad queda dispersa entre carpetas técnicas, menor cohesión y mayor riesgo de servicios transversales grandes.

### 2. Monolito modular organizado por dominio

Ventajas: mayor cohesión, contexto localizado, límites más explícitos, despliegue sencillo y mejor delimitación de contexto para agentes.

Inconvenientes: exige controlar dependencias y puede requerir interfaces públicas explícitas a medida que crece.

### 3. Microservicios por dominio

Ventajas: independencia de despliegue y aislamiento técnico.

Inconvenientes: red, fallos distribuidos, observabilidad y despliegue más complejos sin una necesidad actual que lo justifique.

## Decisión

La API se implementará como un **monolito modular organizado por dominio** con módulos `auth`, `users`, `projects` y `tasks`, manteniendo internamente `controller -> service -> repository`.

## Reglas de dependencia

Permitidas:

```text
AUTH     -> USERS
PROJECTS -> USERS
TASKS    -> USERS
TASKS    -> PROJECTS
```

No permitidas inicialmente:

```text
USERS    -> PROJECTS
USERS    -> TASKS
PROJECTS -> TASKS
```

Un módulo no accederá directamente al repositorio interno de otro.

## Consecuencias positivas

- Mayor cohesión por dominio.
- Menor dispersión del código de una funcionalidad.
- Límites de responsabilidad más explícitos.
- Contexto más acotado para desarrolladores y agentes.
- Se conserva `controller/service/repository`.
- Despliegue sencillo como una sola aplicación.

## Consecuencias negativas

- Será necesario vigilar dependencias entre módulos.
- Algunas operaciones requerirán colaboración entre servicios.
- Puede ser necesario introducir interfaces públicas específicas.
- Una mala definición de límites podría generar dependencias circulares.

## Criterio de revisión

Revisar esta decisión si aparecen límites de dominio claramente incorrectos, dependencias circulares difíciles de eliminar, necesidad demostrada de despliegue independiente o requisitos de escala/aislamiento incompatibles con el monolito.
