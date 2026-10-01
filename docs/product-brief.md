# Product Brief — Gestor de tareas colaborativo

## 1. Problema

Se necesita una aplicación sencilla que permita registrar, organizar y seguir tareas personales o compartidas, agrupándolas en proyectos cuando sea necesario.

La aplicación debe facilitar la colaboración de un pequeño número de usuarios sin introducir la complejidad propia de herramientas avanzadas de gestión de proyectos.

## 2. Usuario objetivo

La aplicación está dirigida inicialmente a usuarios particulares o pequeños grupos de personas que desean organizar tareas y proyectos de forma sencilla.

No está orientada inicialmente a organizaciones grandes, gestión empresarial avanzada ni metodologías complejas de gestión de proyectos.

## 3. Propuesta de valor

Permitir que varios usuarios organicen proyectos y tareas en un entorno sencillo, con proyectos compartidos, tareas asociadas a uno o varios proyectos, seguimiento de estado, prioridades y revisión de tareas completadas.

## 4. Conceptos principales

### Usuario

Persona registrada en la aplicación. Un usuario puede participar en varios proyectos.

### Proyecto

Agrupación lógica de tareas. Un proyecto puede tener varios usuarios participantes.

Atributos iniciales:

- nombre;
- descripción;
- estado: `ACTIVO` o `ARCHIVADO`.

Cada proyecto tiene exactamente un `OWNER` y puede tener cero o más `MEMBER`.

### Tarea

Unidad de trabajo cuyo progreso puede seguirse.

Atributos iniciales:

- título;
- descripción;
- notas;
- estado;
- prioridad;
- responsable opcional.

Una tarea debe pertenecer al menos a un proyecto, puede pertenecer a varios, mantiene una única identidad y un único estado, y puede estar sin asignar o asignada a un único usuario.

## 5. Estados de una tarea

1. `PENDIENTE`
2. `EN_PROGRESO`
3. `COMPLETADA_PENDIENTE_REVISION`
4. `REVISADA`

```text
PENDIENTE
   ↓
EN_PROGRESO
   ↓
COMPLETADA_PENDIENTE_REVISION
   ↓
REVISADA
```

Las reglas exactas de transición se definirán mediante specs de comportamiento. Cualquier participante autorizado puede revisar una tarea.

## 6. Prioridad

- `BAJA`
- `MEDIA`
- `ALTA`

## 7. Alcance inicial del producto

El MVP permitirá registrar usuarios, iniciar y cerrar sesión, crear proyectos, gestionar participantes y roles `OWNER`/`MEMBER`, crear y editar tareas, asociarlas a uno o varios proyectos, establecer prioridad y estado, asignarlas opcionalmente a un usuario, consultar proyectos y tareas y filtrar por estado y prioridad.

## 8. Fuera de alcance inicial

No forman parte del MVP: chat, comentarios, archivos adjuntos, notificaciones push, correo electrónico, calendario externo, integraciones de terceros, Kanban avanzado, drag & drop, automatizaciones, IA, pagos, facturación, gestión empresarial avanzada, jerarquías de equipos u organizaciones y reporting avanzado.

## 9. Principios del producto

- Simplicidad.
- Colaboración controlada.
- Estado explícito.
- Alcance reducido y guiado por necesidades reales.

## 10. Criterio de éxito inicial

El producto será útil cuando un pequeño grupo de usuarios pueda crear proyectos, colaborar dentro de ellos, registrar tareas, seguir su progreso, completar y revisar tareas y consultar con claridad el trabajo pendiente.
