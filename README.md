# Gestión de instalaciones deportivas de Sueca

Proyecto académico de **2.º de DAM, módulo SGE semipresencial**, basado en **Odoo Community 16**. L1 reúne el diseño inicial y las comprobaciones realizadas en E1 y E2.

## Necesidad

El proyecto propone organizar la información y las solicitudes de uso de las instalaciones deportivas de Sueca. Las necesidades y el proceso definitivos se concretarán durante el análisis; no se presupone que los procesos actuales del Ayuntamiento tengan problemas.

## Personas destinatarias

- Personal responsable de administrar instalaciones y revisar solicitudes.
- Personas que quieran consultar o solicitar el uso de una instalación.

Los perfiles y permisos son una propuesta inicial pendiente de validación.

## Objetivo y funcionalidades previstas

El objetivo final es desarrollar en L2 un módulo propio para apoyar la gestión de instalaciones deportivas. En L1 se prepara el entorno, se define la necesidad y se comprueban funciones estándar de Odoo.

El flujo propuesto es:

1. Registrar instalaciones y su información básica.
2. Recibir solicitudes de uso para una instalación, fecha y horario.
3. Revisar cada solicitud y registrar su estado.

Este flujo aún no está implementado en un módulo propio. Quedan fuera del alcance inicial el uso en producción por el Ayuntamiento, los datos personales reales, los pagos y las integraciones con sistemas municipales.

El diseño conceptual de datos y permisos está en [`docs/diseno-inicial.md`](docs/diseno-inicial.md).

## Tecnologías y entorno

- Odoo Community 16.0 en Ubuntu Server 24.04.5, dentro de VirtualBox.
- PostgreSQL 16.15 guarda la base de datos de Odoo.
- Python 3.12.3 y un entorno virtual para Odoo.
- Git y GitHub para guardar el código y el seguimiento de cambios.
- MailHog 1.0.1 para comprobar el envío de correos ficticios.
- Docker Compose queda fuera de la ruta que voy a seguir; el profesor comentó que era opcional.

## Estado actual

- Odoo está configurado como servicio de Ubuntu y se comprobó activo tras reiniciar la máquina virtual.
- La base `gestion_instalaciones_sueca` se abre desde el navegador del portátil. El contacto ficticio `Prueba persistencia L1` siguió visible tras reiniciar Ubuntu.
- Contactos, CRM y Calendario se usan para pruebas con datos ficticios. En CRM se creó una solicitud y se asoció una reunión; esto simula parte del proceso, no sustituye al módulo propio.
- Se probó parcialmente la separación de acceso con los perfiles ficticios `prueba permisos` y `Personal gestor prueba`. La prueba usa CRM estándar y no demuestra todavía el acceso de solicitantes a sus propias solicitudes en un portal o módulo propio.
- MailHog recibió un correo ficticio enviado desde Odoo. La evidencia está en [`docs/prueba MailHog.png`](docs/prueba%20MailHog.png).
- El módulo propio no se ha iniciado; corresponde a L2. Los resultados y pendientes de L1 están en [`docs/L1-verificaciones.md`](docs/L1-verificaciones.md).

## Puesta en marcha y seguridad

Odoo, PostgreSQL, Python y MailHog ya están instalados y comprobados en esta VM. [`deploy/README.md`](deploy/README.md) recoge los pasos que seguimos para la instalación y las pruebas. La incidencia de la primera creación de la base de datos está en [`docs/incidencias.md`](docs/incidencias.md).

En el repositorio se usan datos ficticios. No se deben publicar contraseñas, tokens, archivos de configuración privados como `.odoorc` ni copias de la base de datos.

## Autoría y atribuciones

La autoría, el material docente y la ayuda utilizada se describen en [`docs/atribuciones.md`](docs/atribuciones.md). El PDF docente no se redistribuye aquí.

## Licencia

El proyecto se distribuye bajo la **GNU Lesser General Public License v3.0 (LGPL-3.0)**. El texto completo está en [`LICENSE`](LICENSE). El manifiesto del módulo propio indicará `LGPL-3` cuando se cree en L2.
