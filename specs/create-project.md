# Feature: Crear proyecto

## Objetivo

Permitir que un usuario autenticado cree un proyecto para organizar tareas y colaborar posteriormente con otros usuarios.

## Actor

Usuario autenticado.

## Precondiciones

- El usuario debe estar autenticado.

## Criterios de aceptación

1. El usuario puede crear un proyecto indicando un nombre.
2. Puede incluir opcionalmente una descripción.
3. El proyecto se crea inicialmente con estado `ACTIVO`.
4. El usuario que crea el proyecto se convierte en su único `OWNER`.
5. El proyecto aparece entre los proyectos accesibles por el usuario.

## Casos rechazados

1. No se permite crear un proyecto sin nombre.
2. Un usuario no autenticado no puede crear proyectos.

## Fuera de alcance

- Añadir `MEMBER`.
- Archivar proyectos.
- Eliminar proyectos.
- Crear tareas.
