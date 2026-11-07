# Servicio systemd de Odoo

La plantilla `odoo.service` se ha probado en la VM Ubuntu Server 24.04 de este proyecto. Usa las rutas `/opt/odoo/venv/bin/python`, `/opt/odoo/odoo/odoo-bin` y el archivo de configuración privado `/opt/odoo/.odoorc`. No contiene contraseñas.

## Requisitos previos

- Usuario de sistema `odoo` con acceso al código y al entorno virtual.
- Odoo 16 instalado en `/opt/odoo/odoo` y entorno virtual en `/opt/odoo/venv`.
- Archivo `/opt/odoo/.odoorc` existente y protegido; no se debe copiar al repositorio.
- PostgreSQL instalado y activo.

## Instalar o actualizar la unidad

Desde la carpeta `deploy` de una copia del repositorio en Ubuntu:

```bash
sudo install -o root -g root -m 644 odoo.service /etc/systemd/system/odoo.service
sudo systemd-analyze verify /etc/systemd/system/odoo.service
sudo systemctl daemon-reload
sudo systemctl enable --now odoo
sudo systemctl status odoo --no-pager
```

La plantilla inicia Odoo escuchando en todas las interfaces de la VM para permitir el acceso por el reenvío NAT de VirtualBox. En esta práctica, el portátil accede a través del puerto reenviado `8069`. No expongas el puerto a redes públicas ni compartas el archivo `.odoorc`.

## Evidencia obtenida en esta VM

El servicio quedó `enabled` y `active (running)` después de reiniciar Ubuntu. La base `gestion_instalaciones_sueca` se cargó, la página de Aplicaciones volvió a abrir desde el navegador del portátil y el contacto ficticio `Prueba persistencia L1` siguió visible en Contactos tras el reinicio.
