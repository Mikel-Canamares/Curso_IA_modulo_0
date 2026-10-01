# Architecture — Gestor de tareas colaborativo

## 1. Objetivo

Este documento describe la arquitectura inicial del sistema y los límites entre sus principales componentes.

La aplicación se construirá como un producto web con frontend y backend separados, comunicados mediante HTTP.

La arquitectura inicial prioriza simplicidad, separación clara de responsabilidades, modularidad por dominio, facilidad de evolución, trazabilidad de reglas de negocio y contexto bien delimitado para trabajo asistido por IA.

## 2. Arquitectura general

```text
Browser / Frontend
        |
        | HTTP / API
        v
   TypeScript API
        |
        v
    PostgreSQL
```

### Frontend

Responsable de interfaz, navegación, formularios, representación visual, filtros, feedback visual y consumo de la API. No es la autoridad de negocio ni de autorización.

### API

Responsable de reglas de negocio, autenticación, autorización, validación de operaciones, transiciones de estado, gestión de usuarios, proyectos, membresías y tareas, y coordinación de persistencia.

### PostgreSQL

Responsable de persistencia, integridad referencial, constraints, relaciones, consultas y transacciones.

## 3. Regla de dependencia entre componentes

Permitido:

```text
Frontend -> API -> PostgreSQL
```

No permitido:

```text
Frontend -> PostgreSQL
```

## 4. Organización interna de la API

La API se implementará como monolito modular organizado por dominio:

```text
src/
├── auth/
├── users/
├── projects/
└── tasks/
```

Cada módulo mantiene:

```text
controller -> service -> repository
```

## 5. Reglas internas de capas

### Controller
Recibe HTTP, extrae entrada, delega y construye la respuesta. No contiene lógica de negocio relevante.

### Service
Posee reglas de negocio, autorización, coordinación, validación funcional e interacción con otros módulos mediante capacidades públicas.

### Repository
Gestiona acceso a persistencia, lectura, escritura y consultas. No contiene reglas de negocio.

## 6. Módulos de dominio

### users
Perfil, nombre visible, email, estado de cuenta y operaciones propias del usuario. No conoce proyectos ni tareas.

### auth
Login, logout, validación de credenciales, resolución del usuario autenticado y contexto de autenticación. Depende de `users`.

### projects
Creación y edición de proyectos, propietario, miembros, roles `OWNER`/`MEMBER`, altas/bajas de participantes y reglas de acceso del proyecto. Las membresías permanecen dentro de este módulo. Depende de `users`.

### tasks
Creación y edición de tareas, prioridad, estado, transiciones, asignación a usuario, asociación con uno o varios proyectos y autorización sobre tareas. Depende de `users` y `projects`.

## 7. Dependencias entre módulos

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

## 8. Comunicación entre módulos

Un módulo puede usar capacidades públicas de otro, pero no acceder directamente a su repositorio interno.

Permitido:

```text
TaskService
    ↓
ProjectService
    ↓
ProjectRepository
```

No permitido:

```text
TaskService
    ↓
ProjectRepository
```

## 9. Propiedad de reglas de negocio

```text
estado del usuario -> users
sesión y login     -> auth
OWNER / MEMBER     -> projects
estado de tarea    -> tasks
```

## 10. Modelo conceptual inicial

Entidades principales:

```text
User
Project
ProjectMembership
Task
TaskProject
```

Reglas principales:

- cada `Project` tiene exactamente un `OWNER`;
- puede tener cero o más `MEMBER`;
- una `Task` puede estar sin responsable o asignada a un único usuario;
- una `Task` debe asociarse al menos a un `Project`;
- puede asociarse a varios proyectos;
- mantiene un único estado aunque aparezca en varios proyectos.

El diseño físico definitivo de PostgreSQL se realizará en el módulo dedicado a datos/backend.

## 11. Principios arquitectónicos

- La interfaz no es una barrera de seguridad.
- La base de datos protege la integridad estructural.
- Las reglas viven en su dominio.
- Las dependencias son explícitas.
- Se evita complejidad prematura: sin microservicios, colas, event bus, CQRS ni distribución sin necesidad real.
- Los cambios arquitectónicos relevantes se revisan y, cuando corresponda, se registran mediante ADR.

## 12. Estado del documento

Estado: `v0`

Este documento describe la arquitectura inicial necesaria para comenzar P1 y debe evolucionar a partir de necesidades reales.
