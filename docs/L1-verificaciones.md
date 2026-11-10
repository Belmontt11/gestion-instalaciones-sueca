# L1 · Verificaciones y evidencias

**Entrega:** L1 junto con E1. 

**Nota sobre Docker:** Docker es opcional, así que en esta entrega voy a usar Odoo como servicio en la VM y dejar Docker fuera.

## Tabla de comprobaciones

| Criterio o comprobación | Resultado esperado | Resultado obtenido hasta ahora | Ubicación de la prueba | Pendiente |
|---|---|---|---|---|
| Necesidad, personas destinatarias, tres acciones y fuera de alcance (E1) | README con alcance inicial comprensible | Borrador preparado; proceso y perfiles marcados como provisionales | `README.md`, `docs/diseno-inicial.md` | Validar el proceso y el alcance |
| Relaciones entre datos y permisos (E1) | Diagrama y permisos justificados | Diagrama conceptual y tabla propuestos; | `docs/diseno-inicial.md` | Confirmar modelos y reglas |
| RA1.f · Sistema, PostgreSQL, Odoo y acceso HTTP | Servicio operativo; conexión a PostgreSQL y acceso web comprobados | Ubuntu Server 24.04.5 LTS; PostgreSQL Server 16.15 activo; la base `gestion_instalaciones_sueca` existe; Python 3.12.3 en el entorno virtual; Odoo Server 16.0 activo y escuchando en `0.0.0.0:8069`; Odoo abre la base y sus contactos desde el navegador | [Captura del servicio y versiones](Prueba%20de%20persistencia%20tras%20reinicio.png); [configuración del servicio](Configuracion%20del%20servicio.png); `deploy/odoo.service`; salidas de `systemctl`, `ss` y `psql`; navegador en `127.0.0.1:8069`; explicación en la sección RA1.f de este documento | — |
| RA1.h · Diagnóstico de incidencia | Síntoma, diagnóstico, corrección y verificación final | La primera creación se atascó; el registro llegó a crear tablas y el módulo `base` quedó `to install`. Se revisaron memoria y espacio en disco, sin detectar falta de recursos. Se quitó la base incompleta, se creó otra y se pudo iniciar sesión. La causa exacta no se pudo determinar. | `docs/incidencias.md` | — (la causa desconocida está indicada en el registro) |
| RA2.a · Módulos y dependencias | Relacionar módulos con procesos del proyecto | Contactos para solicitantes; CRM para registrar y seguir solicitudes; Calendario para reuniones vinculadas | README, módulos instalados y este registro | Documentar dependencias |
| RA2.b · Servicio VM y despliegue alternativo | E2 describe Odoo como servicio; | Servicio `systemd` configurado, habilitado y activo después del reinicio; acceso web confirmado; Docker no previsto | `deploy/odoo.service`; resultado de `systemctl status`; navegador en `127.0.0.1:8069` | 
| RA2.c · Empresa, módulos, datos y correo | Configuración ficticia y correo de prueba verificado | Empresa «Gestión deportiva de Sueca»; Contactos, CRM y Calendario disponibles; MailHog 1.0.1 activo como servicio; servidor saliente «MailHog pruebas» en `127.0.0.1:1025`, sin cifrado ni autenticación; correo ficticio enviado a `prueba@example.com` y capturado en MailHog | [Captura de MailHog](prueba%20MailHog.png); base de Odoo y bandeja de MailHog | — |
| RA2.d · Dos supuestos organizativos | Gestión básica por un perfil interno y gestión separada entre administración, personal gestor y solicitantes | El usuario ficticio `prueba permisos` (Ventas: solo documentos propios; sin Administración) no vio la oportunidad de Administrator y creó/consultó `Solicitud perfil limitado`. `Personal gestor prueba` (Ventas: todos los documentos; sin Administración) vio ambas oportunidades al quitar la etiqueta «Mi flujo» y movió la del solicitante de «Nuevo» a «Calificado». Prueba con CRM estándar; no equivale al módulo propio | Usuarios ficticios y oportunidades CRM; `docs/diseno-inicial.md` | Completar/documentar la gestión básica y definir cómo se comprobará el acceso propio en el portal/módulo de L2 |
| RA2.e · Acceso, persistencia y flujo | Acceso; dato que persiste al reiniciar; flujo funcional | Tras reiniciar Ubuntu, Odoo arrancó y el contacto ficticio persistió. Además, se ejecutó `sudo systemctl restart odoo`, el servicio quedó `active` y el contacto de persistencia junto con los demás contactos ficticios siguieron visibles en Contactos. En CRM se creó una solicitud ficticia, se vinculó una reunión en Calendario y se avanzó a «Calificado»; también se comprobó el avance por el perfil gestor. Al intentar crear una oportunidad sin título, Odoo no permitió guardarla. Esta prueba solo confirma el campo obligatorio estándar de CRM; no valida reglas propias del proyecto. Decisión final del flujo pendiente | Servicio `systemd`; Contactos; [captura del servicio y reinicio](Prueba%20de%20persistencia%20tras%20reinicio.png); [captura del contacto tras el reinicio](Prueba%20persistencia%20contactos.png); salida de `systemctl is-active odoo`; oportunidades CRM y reunión de Calendario; formulario de creación CRM | Definir el flujo y sus reglas; probar y documentar un caso incorrecto propio del proyecto |
| RA2.f · Reproducción, configuración e incidencias | Instrucciones reproducibles y configuración | `.gitignore`, registro de incidencia y plantilla systemd sin credenciales; Odoo, PostgreSQL y MailHog instalados y comprobados en la VM. La guía recoge los pasos seguidos para repetir esta instalación | `.gitignore`, `deploy/odoo.service`, `deploy/README.md`, `docs/incidencias.md` | — |
| Repositorio público, atribuciones y licencia | URL accesible sin sesión; licencia y créditos correctos | Repositorio público con README, licencia LGPL-3.0, atribuciones y commits iniciales; etiqueta L1 | `README.md`, `docs/atribuciones.md`, `LICENSE` | Verificar URL sin sesión; crear etiqueta L1 al cerrar las pruebas |

## RA1.f · Cómo arranca Odoo y dónde guarda los datos

Al encender Ubuntu, **systemd** inicia Odoo automáticamente. La configuración que hace esto está en `/etc/systemd/system/odoo.service`. Odoo funciona con una cuenta propia del sistema llamada `odoo`; es distinta de la cuenta `Administrator` con la que se entra a la aplicación web. El sistema inicia PostgreSQL antes que Odoo.

Odoo utiliza el Python preparado para él en `/opt/odoo/venv/`. Así sus componentes quedan separados de los programas generales de Ubuntu.

Los ajustes de esta instalación están en `/opt/odoo/.odoorc`. La guía del curso llama a este archivo `odoo.conf`. Odoo lo consulta para saber cómo funcionar y conectarse con PostgreSQL.

**PostgreSQL guarda la información** de Odoo, como contactos y solicitudes. Comprobamos que existe la base `gestion_instalaciones_sueca` y que Odoo puede abrirla y mostrar sus contactos.

Odoo atiende las conexiones web por el puerto `8069` dentro de la máquina virtual. VirtualBox reenvía ese puerto al portátil, por eso se abre la aplicación en `http://127.0.0.1:8069`.
## Preparación de la entrega en Aules

La ficha `lliurament.md` o PDF se prepara para Aules al cerrar L1. Debe incluir URL pública, etiqueta `L1`, SHA completo del commit, resumen de lo que funciona, criterios trabajados y ubicación de las pruebas. No se debe inventar el SHA ni mover la etiqueta después de entregar; las revisiones requieren una etiqueta y un SHA nuevos.

