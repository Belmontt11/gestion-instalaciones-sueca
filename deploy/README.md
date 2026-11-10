# Guía breve: servicio Odoo en la VM preparada

Odoo, PostgreSQL, Python y MailHog ya están instalados y comprobados en la VM de este proyecto. Esta guía explica cómo instalar o revisar el servicio Odoo y comprobar el correo de prueba. No cubre la instalación de esos componentes desde cero; los pasos completos para repetir el montaje en una VM limpia aún no se han documentado ni probado.

## Requisitos previos

- VM con Ubuntu Server 24.04.5 y PostgreSQL instalado.
- Código de Odoo 16 en `/opt/odoo/odoo`.
- Entorno virtual de Python en `/opt/odoo/venv`.
- Usuario del sistema `odoo` y archivo privado `/opt/odoo/.odoorc` ya configurados.
- Adaptador NAT de VirtualBox con SSH del portátil al puerto invitado 22 reenviado como puerto anfitrión 2222.

El archivo `.odoorc` puede contener datos privados: no se copia al repositorio ni se comparte.

## 1. Copiar la unidad desde Windows a la VM

Abre PowerShell en el portátil. Si el repositorio está en `Documentos`, ejecuta:

~~~powershell
scp -P 2222 "$HOME\Documents\gestion-instalaciones-sueca\deploy\odoo.service" estudiant@127.0.0.1:/tmp/odoo.service
~~~

Introduce la contraseña de Ubuntu cuando SSH la solicite. No la guardes en el repositorio. Si guardaste el proyecto en otra carpeta, adapta la ruta del archivo.

## 2. Instalar e iniciar el servicio en Ubuntu

En la terminal de Ubuntu, ejecuta:

~~~bash
sudo install -o root -g root -m 644 /tmp/odoo.service /etc/systemd/system/odoo.service
sudo systemd-analyze verify /etc/systemd/system/odoo.service
sudo systemctl daemon-reload
sudo systemctl enable --now odoo
systemctl is-enabled odoo
systemctl is-active odoo
~~~

Los dos últimos comandos deben responder `enabled` y `active`. `enable --now` deja Odoo iniciado ahora y preparado para arrancar junto a Ubuntu.

## 3. Comprobar la base, el puerto y el acceso

En Ubuntu:

~~~bash
sudo -u postgres psql -Atc "SELECT datname FROM pg_database WHERE datname='gestion_instalaciones_sueca';"
sudo ss -ltnp 'sport = :8069'
~~~

La primera comprobación debe mostrar `gestion_instalaciones_sueca`; la segunda, el puerto `8069` escuchando en la VM. Desde el navegador del portátil, abre `http://127.0.0.1:8069` (la regla NAT de VirtualBox reenvía el puerto 8069).

Para revisar el servicio después de reiniciar Ubuntu:

~~~bash
sudo systemctl status odoo --no-pager
~~~

Debe aparecer `active (running)`. El servicio de esta guía fue comprobado en esta VM; repetir una instalación desde cero requiere documentar y verificar también la instalación inicial de Odoo y PostgreSQL.

## 4. Probar un correo con MailHog

En el servidor saliente de Odoo se usó `127.0.0.1`, puerto `1025`, sin cifrado ni autenticación. Se envió un mensaje ficticio a `prueba@example.com` y se comprobó su recepción en MailHog. La captura está en [`../docs/prueba MailHog.png`](../docs/prueba%20MailHog.png).

Para abrir la bandeja MailHog desde el portátil, crea un túnel SSH y deja esa terminal abierta:

~~~powershell
ssh -L 8025:127.0.0.1:8025 -p 2222 estudiant@127.0.0.1
~~~

Después abre `http://127.0.0.1:8025` en el navegador del portátil. MailHog y Odoo están dentro de la VM; el túnel permite ver la bandeja desde el portátil.

Esta prueba confirma el correo local de laboratorio, no el envío a destinatarios reales. No introduzcas datos personales ni envíes mensajes reales desde esta configuración.

## Archivo del servicio

`odoo.service` es la plantilla de la unidad utilizada en esta VM. Define el usuario del sistema, la carpeta de Odoo, el entorno Python, el archivo de configuración privado y el reinicio automático si Odoo se detiene.
