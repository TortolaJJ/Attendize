1.Puesta en marcha de Attendize 

-1.1 Instalación App Attendize Despiligue en Local:
	1.Repositorio Oficial de Attendize:
		https://github.com/Attendize/Attendize 
	2.Versión v2.8.0. 
	3.Requisitos Previos:
		 Docker Desktop 
		 Git y terminal Git Bash. 
		 Sistema operativo anfitrión: Windows / Linux / macOS.

-1.2 Arquitectura y Servicios El entorno se compone de cuatro contenedores interconectados: 

    • Web (Nginx + PHP): Servidor y motor de la aplicación Laravel. 
    • MySQL: Base de datos relacional para guardar organizadores, eventos y usuarios con persistencia.
    • Redis: Caché en memoria para optimizar el rendimiento y colas de trabajo.
    • Maildev: Servidor SMTP local para capturar y probar los correos salientes.

  2.Instrucciones 

-2.1 GitHub en el repositorio oficial y pulsar Fork, cread una copia en vuestro gitHub individual. Acceso como Public para poder acceder al repositorio desde el exterior.
-2.2 Clonar copia sustituyendo el usuario por el nombre de la cuenta que contiene el fork, utilizar estos comandos en Git Bah:
    • git clone https://github.com/USUARIO/Attendize.git 
    • cd Attendize git remote add upstream https://github.com/Attendize/Attendize.git 
    • git fetch upstream --tags 
-2.3 Observar la versión de partida y partir de la etiqueta v2.8.0 y cread una rama, utilizar estos comandos en Git Bash:
    • git checkout v2.8.0 
    • git switch -c tarea-01-despliegue 
    • git tag tarea-01-inicio 
    • git push -u origin tarea-01-despliegue 
    • git push origin tarea-01-inicio  
-2.4 Preparar la configuración según el sistema operativo con el que se trabaje, se comprobara que Docker está iniciado. Se ha generado el archivo .env a partir de .env.example, configurando DB_HOST=db para conectar correctamente con el contenedor de MySQL dentro de la red interna de Docker. No se han incluido credenciales reales en el control de versiones. Comandos Git Bash:
    • Copy-Item .env.example .env # PowerShell 
    • cp .env.example .env # Linux, macOS o Git Bash 
    • docker version 
    • docker compose version 
-2.5 Construir las imagenes  en dockers, se ejecutara desde la raíz del repositorio las mismas construcciones que declara Makelife. Utilizar estos comandos en Git Bash:
    • docker build --tag attendize_base --target base . 
    • docker build --tag attendize_worker --target worker --cache-from attendize_base:latest . 
    • docker build --tag attendize_web --target web --cache-from attendize_worker:latest . 
-2.6 Iniciar los servicios en dockers y compobad el estado de los contenedores. Utilizar estos comandos:
    • docker compose up -d 
    • docker compose ps 
-2.7 Preparar Laravel y MySQL, esperar a que la base de datos acepte las conexiones y general la clave y ejecutad la migración desde el contenedor web. Utilizar este comando en Git Bash:
    • docker compose exec web sh -c "wait-for-it db:3306 -t 180 && php artisan key:generate && php artisan migrate" 
-2.8 Completar la instalación en local abriendo con https://localhost:8081/install, aceptar los certificado en local y completar el asistente con datos.
-2.9 Comprobar la persistencia de la App Attendize, creando un organizador y evento. Detened los servicios e iniciarlos de nuevo y verificar que los datos continúan disponibles. Utilizando estos comandos en Git Bash:
    • docker compose stop 
    • docker compose start 
    • docker compose ps 
-2.10 Registrar la versión entregada en un Readme, guardar cambios y cread etiquetas finales, utilizando estos comandos en Git Bash.
    • git status 
    • git add . 
    • git commit -m "Tarea 1: despliegue local reproducible de Attendize" 
    • git push origin tarea-01-despliegue 
    • git tag tarea-01-entrega 
    • git push origin tarea-01-entrega 
-2.11 Comprobaciones obligatorias para verificar el funcionamiento de Attendize:
    • Contenedores: Los servicios necesarios aparecen iniciados en docker compose ps y no existe un reinicio continuo. 
    • Navegador: Attendize abre mediante HTTPS en el puerto indicado y permite iniciar sesión. 
    • MySQL: La aplicación accede a la base de datos y permite guardar un evento ficticio. 
    • Persistencia: El evento sigue disponible después de docker compose stop y docker compose start. 
    • Reproducibilidad: Otra persona puede clonar la etiqueta de entrega y seguir el README. 
    • Seguridad: El repositorio no contiene .env, contraseñas, claves privadas ni datos personales reales. 

	





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