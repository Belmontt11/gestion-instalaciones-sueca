# Diseño inicial de la necesidad

Este documento satisface el diseño previo de E1 de forma **provisional**. Hay que validar el proceso con el alcance acordado y revisar este diseño antes de convertirlo en modelos o permisos reales.

## Proceso inicial propuesto

1. Una persona responsable registra una instalación y sus datos básicos.
2. Una persona solicitante pide el uso de una instalación en una fecha y franja horaria.
3. El personal responsable revisa la solicitud y registra si queda aceptada, rechazada o pendiente.

## Relaciones conceptuales de datos

```mermaid
erDiagram
    CONTACTO ||--o{ SOLICITUD_USO : presenta
    INSTALACION ||--o{ SOLICITUD_USO : recibe
    USUARIO_INTERNO ||--o{ SOLICITUD_USO : gestiona

    CONTACTO {
        string identificador_ficticio
        string nombre_ficticio
    }
    INSTALACION {
        string codigo
        string nombre
        string estado
    }
    SOLICITUD_USO {
        string referencia
        datetime inicio
        datetime fin
        string estado
    }
    USUARIO_INTERNO {
        string rol
    }
```

Las entidades son conceptos, no modelos Odoo implementados. La disponibilidad, las reglas contra solapamientos y los campos definitivos se concretarán antes del módulo de L2.

## Permisos conceptuales

| Perfil propuesto | Acciones iniciales | Estado |
|---|---|---|
| Administración | Configurar instalaciones, usuarios y parámetros | Por validar |
| Personal gestor | Consultar y resolver solicitudes; mantener disponibilidad | Por validar |
| Persona solicitante | Crear solicitudes y consultar las propias | Por validar; portal aún no configurado |

## Dos supuestos organizativos que se probarán en E2

1. **Gestión básica:** un perfil interno gestiona instalaciones y solicitudes; las personas solicitantes solo consultan o crean sus solicitudes de prueba.
2. **Gestión separada por roles:** administración mantiene la configuración, el personal gestor procesa solicitudes y las personas solicitantes acceden solo a sus propias solicitudes.

Ambos supuestos son escenarios de prueba, no una afirmación sobre la organización real del Ayuntamiento de Sueca.
