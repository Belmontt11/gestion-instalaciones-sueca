# Registro de incidencias

## 2026-11-07 · Primera inicialización de la base de datos Odoo

- **Entorno:** Ubuntu Server 24.04 en VirtualBox; Odoo Server 16.0 desde el entorno virtual; PostgreSQL local.
- **Síntoma:** la página de creación de base de datos permaneció cargando durante más de una hora.
- **Resultado esperado:** Odoo inicializa la base y redirige a la pantalla de inicio de sesión.
- **Diagnóstico:** el registro de Odoo llegó a `module base: creating or updating database tables`. En PostgreSQL, el estado del módulo `base` era `to install`; la inicialización no había terminado. Durante la revisión, la VM tenía 3,8 GiB de RAM total, 3,4 GiB disponibles y 3,3 GiB libres en disco. La causa original del bloqueo no quedó determinada.
- **Corrección:** se intentó interrumpir el proceso y apagar la VM mediante ACPI; la VM no respondió y se apagó desde VirtualBox. Tras reiniciar, se eliminó únicamente la base incompleta y se repitió la creación con registro detallado.
- **Verificación inicial:** Odoo mostró el inicio de sesión y la persona usuaria confirmó que pudo entrar.
- **Comprobaciones posteriores:** Odoo quedó como servicio `systemd`; PostgreSQL y Odoo aparecen activos, la base abre desde el navegador y el contacto ficticio `Prueba persistencia L1` siguió visible tras reiniciar Ubuntu. Las evidencias están enlazadas en [`docs/L1-verificaciones.md`](L1-verificaciones.md).
- **Nota:** no se determinó la causa exacta del primer bloqueo. No se presenta una hipótesis como causa demostrada.
- **Seguridad:** este registro no contiene contraseñas, correos reales ni capturas con campos de acceso.
