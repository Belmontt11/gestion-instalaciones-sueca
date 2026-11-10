# Guía de instalación y comprobación de Odoo 16

Estos pasos describen el montaje manual usado para el proyecto: Ubuntu Server 24.04, PostgreSQL, Odoo Community 16 como servicio y MailHog para el correo de prueba. Se parte de una VM recién instalada con acceso a Internet.

> **Estado:** estos son los pasos que seguimos para instalar y configurar Odoo y MailHog en la VM del proyecto. Comprobamos el servicio, el acceso web, la persistencia del contacto ficticio y la recepción del correo de prueba.

## 1. Preparar VirtualBox y Ubuntu

En VirtualBox configura el adaptador de red como **NAT** y estas redirecciones: SSH `2222 → 22` y Odoo `8069 → 8069`. Instala Ubuntu Server 24.04 y entra con el usuario que tenga permisos de `sudo`.

Actualiza Ubuntu e instala los paquetes base:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git postgresql postgresql-contrib python3 python3-venv python3-pip python3-dev build-essential libldap2-dev libpq-dev libsasl2-dev curl
sudo systemctl enable --now postgresql
```

## 2. Crear las cuentas necesarias

Crea el usuario de PostgreSQL que usará Odoo y el usuario de sistema que ejecutará el servicio:

```bash
sudo -u postgres createuser --createdb --no-createrole --no-superuser odoo
sudo adduser --system --home=/opt/odoo --group odoo
```

El usuario de sistema `odoo` ejecuta el proceso; la cuenta `Administrator` es la que se usa después para entrar en la aplicación web.

## 3. Descargar Odoo e instalar sus dependencias

Descarga la rama 16.0 del repositorio oficial de Odoo. Después instala las dependencias de Ubuntu y prepara el entorno virtual:

```bash
sudo -u odoo git clone --depth 1 --branch 16.0 https://github.com/odoo/odoo.git /opt/odoo/odoo
sudo -u odoo python3 -m venv /opt/odoo/venv
sudo -u odoo /opt/odoo/venv/bin/pip install --upgrade pip wheel
sudo -u odoo /opt/odoo/venv/bin/pip install -r /opt/odoo/odoo/requirements.txt
```

Las dependencias de Python se instalaron dentro del entorno virtual con el archivo `requirements.txt` de la rama 16.0.

## 4. Crear la configuración privada

Crea `/opt/odoo/.odoorc` con los valores locales. Sustituye el texto de ejemplo por una contraseña maestra propia; no la publiques ni añadas este archivo al repositorio:

```ini
[options]
admin_passwd = CAMBIAR_POR_UNA_CLAVE_PROPIA
db_user = odoo
addons_path = /opt/odoo/odoo/addons
```

```bash
sudo chown odoo:odoo /opt/odoo/.odoorc
sudo chmod 600 /opt/odoo/.odoorc
```

La contraseña maestra protege las operaciones con bases de datos; es distinta de la contraseña de acceso a Odoo.

## 5. Instalar e iniciar el servicio Odoo

Clona este repositorio en la VM y copia la plantilla del servicio:

```bash
git clone https://github.com/Belmontt11/gestion-instalaciones-sueca.git ~/gestion-instalaciones-sueca
sudo install -o root -g root -m 644 ~/gestion-instalaciones-sueca/deploy/odoo.service /etc/systemd/system/odoo.service
sudo systemd-analyze verify /etc/systemd/system/odoo.service
sudo systemctl daemon-reload
sudo systemctl enable --now odoo
systemctl is-enabled odoo
systemctl is-active odoo
```

Los dos últimos comandos deben responder `enabled` y `active`. La plantilla usa `/opt/odoo/venv`, `/opt/odoo/odoo/odoo-bin` y el archivo privado `/opt/odoo/.odoorc`.

## 6. Crear la base y comprobar Odoo

Desde el navegador del portátil abre `http://127.0.0.1:8069`. Crea la base `gestion_instalaciones_sueca`, usando la contraseña maestra configurada en `.odoorc`, un correo ficticio y una contraseña propia para `Administrator`. No marques datos de demostración si quieres mantener la base de prueba vacía.

En Ubuntu, comprueba PostgreSQL, la base, el puerto y el servicio:

```bash
sudo -u postgres psql -Atc "SHOW server_version;"
sudo -u postgres psql -Atc "SELECT datname FROM pg_database WHERE datname='gestion_instalaciones_sueca';"
sudo ss -ltnp 'sport = :8069'
sudo systemctl status odoo --no-pager
```

Abre la base desde el navegador para comprobar que Odoo se conecta a PostgreSQL. Crea un contacto ficticio y reinicia Ubuntu; vuelve a entrar y confirma que el contacto sigue ahí. Guarda las salidas y capturas en `docs/L1-verificaciones.md`.

## 7. Instalar MailHog y probar el correo

Descarga MailHog 1.0.1 desde su [página oficial de versiones](https://github.com/mailhog/MailHog/releases/tag/v1.0.1) y crea un servicio local que escuche solo dentro de la VM:

```bash
sudo curl -fL https://github.com/mailhog/MailHog/releases/download/v1.0.1/MailHog_linux_amd64 -o /usr/local/bin/mailhog
sudo chmod 755 /usr/local/bin/mailhog
sudo useradd --system --no-create-home --shell /usr/sbin/nologin mailhog
sudo nano /etc/systemd/system/mailhog.service
```

Escribe este contenido en `mailhog.service`:

```ini
[Unit]
Description=MailHog para pruebas de correo
After=network.target

[Service]
Type=simple
User=mailhog
Group=mailhog
ExecStart=/usr/local/bin/mailhog -smtp-bind-addr 127.0.0.1:1025 -ui-bind-addr 127.0.0.1:8025 -api-bind-addr 127.0.0.1:8025
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Guarda el archivo y activa el servicio:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now mailhog
systemctl is-active mailhog
```

Debe responder `active`. MailHog usa SMTP `1025` y la interfaz web `8025`; ambos quedan limitados a la VM.

En Odoo configura el servidor saliente de pruebas con servidor `127.0.0.1`, puerto `1025`, sin cifrado ni autenticación. Envía un correo a `prueba@example.com` y comprueba que aparece en la bandeja de MailHog. Para abrir su interfaz desde el portátil, deja activo este túnel SSH:

```bash
ssh -L 8025:127.0.0.1:8025 -p 2222 estudiant@127.0.0.1
```

Después abre `http://127.0.0.1:8025` en el navegador del portátil. Este correo queda en el entorno de pruebas; no se envía a una dirección real.

## 8. Incidencias y datos

Anota en `docs/incidencias.md` el síntoma, lo que se comprobó, la corrección y el resultado final. No inventes la causa si no se pudo determinar. Usa solo datos ficticios y no publiques contraseñas, tokens, `.odoorc` ni copias de la base de datos.

## Resultado comprobado

En esta VM, PostgreSQL y Odoo quedan activos, Odoo abre la base desde el navegador, el contacto ficticio sigue tras reiniciar y el correo de prueba aparece en MailHog. Las capturas y el detalle de las comprobaciones están en [`docs/L1-verificaciones.md`](../docs/L1-verificaciones.md).
