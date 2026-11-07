# Registro de incidencias

## 2026-11-07 · Primera inicialización de la base de datos Odoo

- **Entorno:** Ubuntu Server 24.04 en VirtualBox; Odoo Server 16.0 desde el entorno virtual; PostgreSQL local.
- **Síntoma:** la página de creación de base de datos permaneció cargando durante más de una hora.
- **Resultado esperado:** Odoo inicializa la base y redirige a la pantalla de inicio de sesión.
- **Diagnóstico:** el registro de Odoo llegó a `module base: creating or updating database tables`. En PostgreSQL, el estado del módulo `base` era `to install`; la inicialización no había terminado. Durante la revisión, la VM tenía 3,8 GiB de RAM total (3,4 GiB disponibles) y 3,3 GiB disponibles en disco. La causa original del bloqueo no quedó determinada.
- **Corrección:** se intentó interrumpir el proceso y apagar la VM mediante ACPI; la VM no respondió y se apagó desde VirtualBox. Tras reiniciar, se eliminó únicamente la base incompleta y se repitió la creación con registro detallado.
- **Verificación final:** Odoo mostró el inicio de sesión y la persona usuaria confirmó que pudo entrar.
- **Pendiente:** repetir la instalación como servicio `systemd` y comprobar PostgreSQL, puerto y persistencia en pruebas separadas.
- **Seguridad:** este registro no contiene contraseñas, correos ni capturas con campos de acceso.
