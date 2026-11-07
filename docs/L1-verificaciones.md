# L1 · Verificaciones y evidencias

**Entrega:** L1 junto con E1. **Fecha indicada en Aules:** 15/11/2026.

La evidencia debe conservarse en esta versión del repositorio. No se marcan como superadas las pruebas que aún no se han ejecutado.

**Nota sobre Docker:** el texto escrito de E2 compartido para este trabajo describe Docker Compose y la comparación de dos despliegues; el profesor dice que Docker es opcional.

## Tabla de comprobaciones

| Criterio o comprobación | Resultado esperado | Resultado obtenido hasta ahora | Ubicación de la prueba | Pendiente |
|---|---|---|---|---|
| Necesidad, personas destinatarias, tres acciones y fuera de alcance (E1) | README con alcance inicial comprensible | Borrador preparado; proceso y perfiles marcados como provisionales | `README.md`, `docs/diseno-inicial.md` | Validar el proceso y el alcance |
| Relaciones entre datos y permisos (E1) | Diagrama y permisos justificados | Diagrama conceptual y tabla propuestos; no implementados | `docs/diseno-inicial.md` | Confirmar modelos y reglas |
| RA1.f · Sistema, PostgreSQL, Odoo y acceso HTTP | Servicio operativo; conexión a PostgreSQL y acceso web comprobados | Ubuntu 24.04, PostgreSQL activo, Odoo 16.0 ejecutado manualmente, base inicial creada y acceso web confirmado | Evidencia por recopilar sin secretos | Instalar/validar el servicio `systemd`, registrar versiones y conservar comprobaciones |
| RA1.h · Diagnóstico de incidencia | Síntoma, diagnóstico, corrección y verificación final | Primera creación de base bloqueada; módulo `base` quedó `to install`; base incompleta eliminada y nueva base creada con acceso correcto | `docs/incidencias.md` | Añadir evidencia segura y explicar que la causa inicial quedó sin determinar |
| RA2.a · Módulos y dependencias | Relacionar módulos con procesos del proyecto | Pendiente; aún no se han seleccionado módulos funcionales | Pendiente | Elegir y justificar módulos para los dos supuestos |
| RA2.b · Servicio VM y despliegue alternativo | La guía escrita de E2 describe la comparación de servicio y contenedores; el profesor indicó verbalmente que Docker es opcional | Instalación fuente y ejecución manual en VM; falta validar el servicio `systemd`; Docker no previsto en la ruta mínima | Esta nota y las comprobaciones del servicio | Completar servicio. Añadir Docker solo si se confirma que RA2.b se exige pese a la indicación verbal |
| RA2.c · Empresa, módulos, datos y correo | Configuración ficticia y correo de prueba verificado | Base creada; falta configurar la entidad, módulos y MailHog | Pendiente | Usar datos ficticios y verificar mensaje en MailHog |
| RA2.d · Dos supuestos organizativos | Configuraciones básica y separada por roles probadas | Escenarios descritos como propuesta, aún sin configurar | `docs/diseno-inicial.md` | Aplicar y documentar ambas configuraciones |
| RA2.e · Acceso, persistencia y flujo | Acceso; dato que persiste al reiniciar; flujo funcional | Acceso web confirmado; persistencia tras reinicio y flujo completo no comprobados | Pendiente | Crear registro ficticio, reiniciar y volver a comprobarlo |
| RA2.f · Reproducción, configuración e incidencias | Instrucciones reproducibles y configuración sin secretos | `.gitignore` inicial y registro de incidencia preparados; configuración reproducible pendiente | `.gitignore`, `docs/incidencias.md` | Añadir ejemplo sin credenciales válidas y pasos limpios |
| Repositorio público, atribuciones y licencia | URL accesible sin sesión; licencia y créditos correctos | Atribuciones preparadas y `LICENSE` LGPL-3.0 añadido; primer commit y etiqueta L1 pendientes | `docs/atribuciones.md`, `LICENSE` | Verificar URL; publicar commit; crear etiqueta L1 al cerrar las pruebas |

## Preparación de la entrega en Aules

La ficha `lliurament.md` o PDF se prepara para Aules al cerrar L1. Debe incluir URL pública, etiqueta `L1`, SHA completo del commit, resumen de lo que funciona, criterios trabajados y ubicación de las pruebas. No se debe inventar el SHA ni mover la etiqueta después de entregar; las revisiones requieren una etiqueta y un SHA nuevos.

## Lista final de L1

- [ ] La URL pública se abre sin iniciar sesión y apunta a la versión entregada.
- [ ] Las instrucciones permiten reproducir la instalación desde una copia limpia.
- [ ] Se han comprobado casos correctos e incorrectos y anotado los pendientes.
- [ ] El repositorio contiene configuración sin secretos, datos ficticios, pruebas, LICENSE y atribuciones.
- [ ] Se puede ejecutar y explicar el trabajo en una demostración individual.
