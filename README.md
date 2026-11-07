# Gestión de instalaciones deportivas de Sueca

Proyecto académico de **2.º de DAM, módulo SGE semipresencial**, desarrollado sobre **Odoo Community 16**. La primera entrega, L1, reúne evidencias de E1 y E2. La fecha límite indicada en Aules es el **15 de noviembre de 2026 a las 23:59**.

## Necesidad

El proyecto propone facilitar la organización de la información y la gestión de las instalaciones deportivas de Sueca. La situación actual y las necesidades concretas de la entidad se validarán durante el análisis; este README no presupone fallos de los procesos municipales existentes.

## Personas destinatarias

- Personal responsable de administrar las instalaciones y su disponibilidad.
- Personas que necesiten consultar o solicitar el uso de una instalación.

Los perfiles, permisos y procesos definitivos están pendientes de validar.

## Objetivo y alcance inicial

El objetivo final es desarrollar un módulo propio de Odoo Community 16 para apoyar un proceso de gestión de instalaciones deportivas. El módulo propio se desarrollará en L2; en L1 se prepara y verifica el entorno, se define la necesidad y se documentan los supuestos iniciales.

Flujo inicial propuesto, pendiente de validación:

1. Registrar una instalación y la información básica de disponibilidad.
2. Recibir una solicitud de uso para una instalación y una franja horaria.
3. Revisar la solicitud y comunicar o registrar su estado.

Quedan fuera del alcance inicial el uso en producción por el Ayuntamiento, los datos personales reales, los pagos y la integración con sistemas municipales. Se revisará el alcance al concretar los requisitos.

El diseño conceptual de datos y permisos está en [`docs/diseno-inicial.md`](docs/diseno-inicial.md). Es una propuesta, no describe modelos ya implementados.

## Tecnologías y entorno

- Odoo Community 16, instalado desde el código fuente en Ubuntu Server 24.04.
- Ubuntu Server 24.04 ejecutado en una máquina virtual de VirtualBox con red NAT.
- PostgreSQL para la base de datos de Odoo.
- Python y un entorno virtual para las dependencias de Odoo.
- Git y GitHub para el control de versiones y la publicación del proyecto.
- Docker Compose queda como prueba opcional, según la indicación verbal más reciente del profesor. MailHog continúa pendiente para las pruebas de correo de E2.
- Plantilla e instrucciones del servicio systemd en [`deploy/`](deploy/README.md), ajustadas a las rutas de esta VM y sin contraseñas.

## Estado actual

- La VM Ubuntu Server 24.04 está preparada y la red NAT reenvía Odoo `8069 → 8069` y SSH `2222 → 22` al portátil.
- PostgreSQL está instalado y activo.
- Odoo Server 16.0 se ha ejecutado desde el entorno virtual; la base inicial se creó y se ha iniciado sesión desde el navegador del portátil.
- Odoo está configurado como servicio `systemd`, habilitado para arrancar con Ubuntu y comprobado activo después de reiniciar. La página de Aplicaciones volvió a abrir desde el navegador del portátil.
- Después de reiniciar Ubuntu, Odoo volvió a abrir la base `gestion_instalaciones_sueca` y el contacto ficticio `Prueba persistencia L1` siguió visible en Contactos.
- Contactos, CRM y Calendario están disponibles para las pruebas estándar. Hay una solicitud ficticia en CRM en la etapa «Calificado» y una reunión de revisión asociada en Calendario; esto simula parte del proceso y no sustituye al módulo propio.
- Se creó el usuario ficticio `prueba permisos` con acceso a Ventas limitado a sus propios documentos y sin permisos de Administración. Desde esa cuenta no apareció la oportunidad de Administrator y sí pudo crear y ver `Solicitud perfil limitado`.
- Se creó el perfil ficticio `Personal gestor prueba` con acceso de Ventas a todos los documentos y sin permisos de Administración. Al quitar el filtro «Mi flujo», pudo ver las dos oportunidades y mover `Solicitud perfil limitado` de «Nuevo» a «Calificado». Esto prueba parcialmente la gestión separada con CRM estándar; no equivale a probar el futuro módulo ni un portal de solicitantes.
- Aún falta completar y documentar los dos supuestos de E2: gestión básica por un perfil interno y gestión separada entre administración, personal gestor y solicitantes. También están pendientes la decisión final del flujo, la validación de casos incorrectos y el correo de prueba con MailHog. Docker Compose queda como ampliación opcional según la indicación oral del profesor.
- No se ha iniciado el desarrollo del módulo propio; corresponde a L2.

El progreso comprobable de L1 se mantiene en [`docs/L1-verificaciones.md`](docs/L1-verificaciones.md). La incidencia real durante la primera creación de la base se registra en [`docs/incidencias.md`](docs/incidencias.md).

## Reproducibilidad, datos y seguridad

La guía de instalación desde una copia limpia, la configuración de ejemplo y los pasos para inicializar datos ficticios y ejecutar pruebas se completarán al documentar la configuración reproducible de E2 y el módulo de L2. En el repositorio solo se usarán datos ficticios. No se deben subir contraseñas, tokens, archivos `.env` reales, configuraciones privadas ni copias de bases de datos.

## Autoría, ayuda y atribuciones

La autoría y las atribuciones de código, bibliotecas y ayuda se completarán en [`docs/atribuciones.md`](docs/atribuciones.md). No se incluye aquí el PDF docente ni se copia su contenido de forma extensa.

## Licencia

El proyecto se distribuye bajo la **GNU Lesser General Public License v3.0 (LGPL-3.0)**. Consulta el texto completo en [`LICENSE`](LICENSE). Cuando exista el módulo propio, su manifiesto indicará `LGPL-3`.
