# Attendize — Despliegue local v2.8.0

Despliegue y puesta en marcha de **Attendize v2.8.0** en un entorno local utilizando Docker.

El objetivo de esta tarea es disponer de una instalación reproducible de Attendize, con persistencia de datos y todos los servicios necesarios ejecutándose mediante contenedores Docker.

---

## 1. Puesta en marcha de Attendize

### 1.1 Instalación de Attendize en local

Para realizar el despliegue se utiliza el repositorio oficial de Attendize:

- **Repositorio:** https://github.com/Attendize/Attendize
- **Versión:** `v2.8.0`

### Requisitos previos

Antes de comenzar es necesario disponer de:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Git
- Git Bash
- Sistema operativo anfitrión:
  - Windows
  - Linux
  - macOS

---

## 1.2 Arquitectura y servicios

El entorno de Attendize está compuesto por **cuatro contenedores interconectados**:

| Servicio | Descripción |
|---|---|
| **Web** | Nginx + PHP. Servidor y motor de la aplicación Laravel. |
| **MySQL** | Base de datos relacional para almacenar organizadores, eventos y usuarios. |
| **Redis** | Sistema de caché en memoria utilizado para mejorar el rendimiento y gestionar colas de trabajo. |
| **Maildev** | Servidor SMTP local para capturar y probar los correos electrónicos enviados por la aplicación. |

Todos los servicios se ejecutan dentro de la red interna de Docker.

---

# 2. Instrucciones de despliegue

## 2.1 Crear un Fork del repositorio

Acceder al repositorio oficial de Attendize:

https://github.com/Attendize/Attendize

Pulsar sobre **Fork** para crear una copia del repositorio en la cuenta personal de GitHub.

El repositorio deberá configurarse como **Public**, de forma que pueda ser accesible desde el exterior.

---

## 2.2 Clonar el repositorio

Clonar el Fork utilizando Git Bash.

Sustituir `USUARIO` por el nombre de usuario de GitHub que contiene el Fork:

```bash
git clone https://github.com/USUARIO/Attendize.git
cd Attendize
```

Añadir el repositorio oficial como `upstream`:

```bash
git remote add upstream https://github.com/Attendize/Attendize.git
git fetch upstream --tags
```

De esta forma se mantiene la posibilidad de consultar las versiones y cambios del repositorio original.

---

## 2.3 Seleccionar la versión v2.8.0 y crear la rama de trabajo

Partimos de la etiqueta oficial `v2.8.0`:

```bash
git checkout v2.8.0
```

Crear la rama correspondiente a la tarea:

```bash
git switch -c tarea-01-despliegue
```

Crear una etiqueta para identificar el estado inicial:

```bash
git tag tarea-01-inicio
```

Subir la rama y la etiqueta al repositorio personal:

```bash
git push -u origin tarea-01-despliegue
git push origin tarea-01-inicio
```

---

## 2.4 Preparar la configuración

Antes de iniciar los contenedores es necesario comprobar que Docker está correctamente instalado y funcionando.

### Crear el archivo `.env`

El archivo `.env` se genera a partir de `.env.example`.

### Windows / PowerShell

```powershell
Copy-Item .env.example .env
```

### Linux, macOS o Git Bash

```bash
cp .env.example .env
```

La configuración de la aplicación debe utilizar:

```env
DB_HOST=db
```

El valor `db` permite que Laravel se conecte al contenedor de MySQL mediante la red interna de Docker.

> **Importante:** el archivo `.env` contiene configuración sensible y **no debe subirse al repositorio**.

Comprobar que Docker está correctamente instalado:

```bash
docker version
docker compose version
```

---

## 2.5 Construir las imágenes Docker

Desde la raíz del repositorio se construyen las imágenes siguiendo las etapas definidas en el `Dockerfile`.

### Imagen base

```bash
docker build --tag attendize_base --target base .
```

### Imagen worker

```bash
docker build --tag attendize_worker --target worker --cache-from attendize_base:latest .
```

### Imagen web

```bash
docker build --tag attendize_web --target web --cache-from attendize_worker:latest .
```

Una vez finalizado el proceso, las imágenes necesarias estarán disponibles localmente.

---

## 2.6 Iniciar los servicios

Iniciar todos los servicios mediante Docker Compose:

```bash
docker compose up -d
```

Comprobar el estado de los contenedores:

```bash
docker compose ps
```

Los servicios necesarios deberán aparecer correctamente iniciados y no deberá existir ningún contenedor reiniciándose continuamente.

---

## 2.7 Preparar Laravel y MySQL

Es necesario esperar a que MySQL esté preparado para aceptar conexiones.

Desde el contenedor `web`, ejecutar:

```bash
docker compose exec web sh -c "wait-for-it db:3306 -t 180 && php artisan key:generate && php artisan migrate"
```

Este comando realiza las siguientes operaciones:

1. Espera a que MySQL esté disponible.
2. Genera la clave de aplicación de Laravel.
3. Ejecuta las migraciones de la base de datos.

---

## 2.8 Completar la instalación de Attendize

Una vez iniciados los servicios y preparada la base de datos, abrir el navegador y acceder a:

```text
https://localhost:8081/install
```

> **Nota:** al utilizar HTTPS en local, el navegador puede mostrar un aviso relacionado con el certificado. Es necesario aceptar el certificado local para continuar.

Completar el asistente de instalación proporcionando los datos solicitados.

Al finalizar, la aplicación deberá estar disponible y permitir iniciar sesión.

---

# 3. Comprobación de persistencia

Para comprobar que los datos de Attendize se almacenan correctamente:

1. Acceder a la aplicación.
2. Crear un **organizador**.
3. Crear un **evento ficticio** asociado al organizador.
4. Comprobar que el evento aparece correctamente.

A continuación, detener los servicios:

```bash
docker compose stop
```

Volver a iniciarlos:

```bash
docker compose start
```

Comprobar nuevamente el estado:

```bash
docker compose ps
```

Volver a acceder a Attendize y comprobar que el organizador y el evento siguen disponibles.

Esto permite verificar que los datos de MySQL mantienen su persistencia aunque los contenedores sean detenidos y posteriormente iniciados.

---

# 4. Entrega y versionado

Una vez realizadas todas las comprobaciones, revisar los cambios:

```bash
git status
```

Añadir los archivos modificados:

```bash
git add .
```

Crear el commit de la tarea:

```bash
git commit -m "Tarea 1: despliegue local reproducible de Attendize"
```

Subir la rama al repositorio:

```bash
git push origin tarea-01-despliegue
```

Crear la etiqueta final de entrega:

```bash
git tag tarea-01-entrega
```

Subir la etiqueta:

```bash
git push origin tarea-01-entrega
```

---

# 5. Comprobaciones obligatorias

Antes de dar por finalizada la tarea, comprobar los siguientes puntos:

### 🐳 Contenedores

- [ ] Los servicios necesarios aparecen iniciados en `docker compose ps`.
- [ ] No existe ningún contenedor en un ciclo continuo de reinicios.
- [ ] Los cuatro servicios están correctamente conectados.

### 🌐 Navegador

- [ ] Attendize es accesible mediante HTTPS.
- [ ] La aplicación responde en el puerto `8081`.
- [ ] Se puede acceder al sistema e iniciar sesión.

### 🗄️ MySQL

- [ ] Laravel puede conectarse correctamente a MySQL.
- [ ] Las migraciones se ejecutan correctamente.
- [ ] Se puede crear un organizador.
- [ ] Se puede crear y guardar un evento ficticio.

### 💾 Persistencia

- [ ] Se ha creado correctamente un evento.
- [ ] Se han detenido los servicios con `docker compose stop`.
- [ ] Se han iniciado nuevamente con `docker compose start`.
- [ ] El evento continúa disponible después del reinicio.

### 🔄 Reproducibilidad

- [ ] El repositorio contiene las instrucciones necesarias para realizar el despliegue.
- [ ] La etiqueta `tarea-01-entrega` identifica la versión final.
- [ ] Otra persona puede clonar el repositorio y seguir las instrucciones del README para reproducir el entorno.

### 🔐 Seguridad

- [ ] El archivo `.env` no está incluido en el repositorio.
- [ ] No se han subido contraseñas reales.
- [ ] No se han subido claves privadas.
- [ ] No se han incluido datos personales reales.
- [ ] La configuración sensible se mantiene fuera del control de versiones.

---

# 6. Estructura de ramas y etiquetas

Durante el desarrollo se utilizan las siguientes referencias:

| Referencia | Descripción |
|---|---|
| `v2.8.0` | Versión oficial de partida de Attendize. |
| `tarea-01-despliegue` | Rama utilizada para realizar la tarea. |
| `tarea-01-inicio` | Estado inicial de la tarea a partir de `v2.8.0`. |
| `tarea-01-entrega` | Versión final entregada. |

---

# 7. Resumen del proceso

El proceso completo de despliegue es:

```text
Repositorio oficial
       │
       ▼
     Fork
       │
       ▼
    Clone
       │
       ▼
  v2.8.0
       │
       ▼
tarea-01-despliegue
       │
       ▼
Configuración .env
       │
       ▼
Construcción Docker
       │
       ▼
Docker Compose
       │
       ├── Web (Nginx + PHP)
       ├── MySQL
       ├── Redis
       └── Maildev
       │
       ▼
Migraciones Laravel
       │
       ▼
Instalación Attendize
       │
       ▼
Creación de organizador y evento
       │
       ▼
Prueba de persistencia
       │
       ▼
tarea-01-entrega
```

---

# 8. Resultado esperado

Al finalizar la tarea se debe disponer de una instalación local y reproducible de **Attendize v2.8.0**, ejecutándose mediante Docker y con:

- Aplicación web funcionando mediante HTTPS.
- Laravel correctamente configurado.
- MySQL operativo y persistente.
- Redis disponible para caché y colas.
- Maildev disponible para pruebas de correo.
- Datos persistentes después de detener y volver a iniciar los contenedores.
- Rama `tarea-01-despliegue` subida a GitHub.
- Etiqueta `tarea-01-inicio` correspondiente al estado inicial.
- Etiqueta `tarea-01-entrega` correspondiente a la versión final.
- Sin credenciales, claves privadas ni datos personales reales en el repositorio.

**Versión de Attendize:** `v2.8.0`  
**Tarea:** `01 - Despliegue local reproducible`
	





*Attendize* is an open-source ticketing and event management application built using the Laravel PHP framework. Attendize allows event organisers to sell tickets to their events and manage attendees without paying service fees to third party ticketing companies.

<p align="center">
  <img src="/public/assets/images/logo-dark.png" alt="Attendize"/>
  <img style='border: 1px solid #444;' src="https://www.attendize.com/images/screenshots/screen1.PNG" alt="Attendize"/>
</p>

# Attendize
Open-source ticket selling and event management platform

> Please report bugs here: https://github.com/Attendize/Attendize/issues. Detailed bug reports are more likely to be looked at. Simple creating an issue and saying "it doesn't work" is not useful. Providing some steps to reproduce your problem as well as details about your operating system, PHP version etc can help.

> Take a look https://www.attendize.com/troubleshooting.html and follow the https://www.attendize.com/getting_started.html guide to make sure you have configured attendize correctly.  

Documentation Website: https://www.attendize.com<br />

## Current Features (v2.X.X)
 - Beautiful mobile friendly event pages
 - Easy attendee management - Refunds, Messaging etc.
 - Data export - attendees list to XLS, CSV etc.
 - Generate print friendly attendee list
 - Ability to manage unlimited organisers / events
 - Manage multiple organisers 
 - Real-time event statistics
 - Customizable event pages
 - Multiple currency support
 - Quick and easy checkout process
 - Customizable tickets - with QR codes, organiser logos etc.
 - Fully brandable - Have your own logos on tickets etc.
 - Affiliate tracking
    - track sales volume / number of visits generated etc.
 - Widget support - embed ticket selling widget into existing websites / WordPress blogs
 - Social sharing 
 - Support multiple payment gateways - Stripe, PayPal & Coinbase so far, with more being added
 - Support for offline payments
 - Refund payments - partial refund & full refunds
 - Ability to add service charge to tickets
 - Messaging - eg. Email all attendees with X ticket
 - Public event listings page for organisers
 - Ability to ask custom questions during checkout
 - Browser based QR code scanner for door management
 - Elegant dashboard for easy management.

## Minimum Requirements

Attendize should run on most pre-configured LAMP or LEMP environments as long as certain requirements are adhered to. Attendize is based on the [Laravel Framework](https://laravel.com/)

**PHP Requirements**
1. PHP >= 7.1.3
2. OpenSSL PHP Extension 
3. PDO PHP Extension 
4. Mbstring PHP Extension 
5. Tokenizer PHP Extension 
6. Fileinfo PHP Extension 
7. GD PHP Extension

**MySQL Requirements**
1. MySQL version 5.7 or higher required

## Contributing
Feel free to fork and contribute. If you are unsure about adding a feature, create a Github issue to ask for Feedback. Read the [contribution guidelines](CONTRIBUTING.md)

## Submitting an issue
If you encounter a bug in Attendize, please first search the list of current open [Issues on the GitHub repository](https://github.com/Attendize/Attendize/issues). You may add additional feedback on an existing bug report. If the issue you're having has not yet been reported, please open a new issue. There is a template available for new issues. Please fill out all information requested in the template so we can help you more easily.

Please note: support is not offered from the project maintainers through GitHub. Paid support is available by [purchasing a license](https://www.attendize.com/license.html).

## Installation
To get developing straight away use the [Pre-configured Docker Environment](https://www.attendize.com/getting_started.html#running-attendize-in-docker-for-development)<br />
To do a manual installation use the [Manual Installation Steps](https://www.attendize.com/getting_started.html#manual-installation)

## Upgrade
To upgrade to a new version:

- we recommend putting your site into [Maintenance Mode](https://laravel.com/docs/6.x/configuration#maintenance-mode) by running `php artisan down` from the root of your installation. This isn't required but can help prevent problems if users are on your site during the upgrade.
- download the latest version of the code from GitHub and copy the code to your server however you normally do so - via FTP, git pull, continuous integration deployment, etc.
- navigate to https://<your-site.com>/upgrade and follow the instructions on screen
- the upgrade page will run database migrations, update configuration settings, etc as needed for the new code version.

## Testing
To run the application tests, you can run the following from your project root:

```sh
# If the testing db does not exist yet, please create it
touch database/database.sqlite
# Run the test suite
./vendor/bin/phpunit
```

This will run the feature tests that hits the database using the `sqlite` database connection.

## Troubleshooting
If you are having problems please read the [troubleshooting guide](https://www.attendize.com/troubleshooting.html) 

## License
Attendize is open-sourced software licensed under the Attribution Assurance License. See [https://www.attendize.com/license.html](https://www.attendize.com/license.html) for further details. We also have white-label license options available.

## Code of Conduct
The Attendize community operates a [Code of Conduct](CODE_OF_CONDUCT.md) to ensure everyone is able to participate comfortably, equally and safely.