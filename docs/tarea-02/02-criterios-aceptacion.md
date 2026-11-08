# Criterios de aceptación – Modernización de Attendize

**Proyecto:** TechFest  
**Módulo:** Proyecto Intermodular II – 2.º DAW  
**Tarea:** 2 – Propuesta y planificación  
**Alumno/a:** Jose Javier Garcia Tortola

## 1. Introducción

En este documento se definen los requisitos funcionales de la modernización de Attendize para el evento TechFest.

Cada requisito describe una necesidad del usuario, su prioridad, los datos afectados y su origen. También se establecen criterios de aceptación para comprobar que las funcionalidades cumplen los objetivos previstos.

## 2. Requisitos funcionales

### RF01 – Consultar listado de asistentes

- **Usuario:** Administrador y organizador.
- **Necesidad:** Consultar los asistentes registrados en el evento.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Identificador, nombre, apellidos, correo electrónico y tipo de entrada.
- **Origen:** Necesidad de facilitar la gestión y consulta de asistentes.

**Criterio de aceptación:**

Dado que existen asistentes registrados en TechFest, cuando el administrador accede al apartado de asistentes, entonces el sistema muestra un listado con sus datos principales.

### RF02 – Registrar nuevos asistentes

- **Usuario:** Administrador.
- **Necesidad:** Añadir nuevos asistentes al evento.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Nombre, apellidos, correo electrónico y tipo de entrada.
- **Origen:** Necesidad de centralizar el registro de asistentes.

**Criterio de aceptación:**

Dado que el administrador ha introducido todos los datos obligatorios correctamente, cuando pulsa el botón de registrar, entonces el sistema guarda el nuevo asistente y muestra un mensaje de confirmación.

### RF03 – Modificar datos de asistentes

- **Usuario:** Administrador.
- **Necesidad:** Corregir o actualizar la información de un asistente.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Nombre, apellidos, correo electrónico y tipo de entrada.
- **Origen:** Necesidad de mantener actualizada la información.

**Criterio de aceptación:**

Dado que existe un asistente registrado, cuando el administrador modifica sus datos y guarda los cambios, entonces el sistema actualiza la información y muestra los datos modificados.

### RF04 – Registrar entrada de asistentes

- **Usuario:** Personal de acceso y administrador.
- **Necesidad:** Registrar cuándo un asistente accede al evento.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Identificador del asistente, evento y fecha y hora de entrada.
- **Origen:** Necesidad de controlar el acceso a TechFest.

**Criterio de aceptación:**

Dado que existe un asistente registrado y todavía no ha accedido al evento, cuando el personal de acceso registra su entrada, entonces el sistema almacena la fecha y hora y muestra una confirmación.

### RF05 – Registrar salida de asistentes

- **Usuario:** Personal de acceso y administrador.
- **Necesidad:** Registrar cuándo un asistente abandona el evento.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Identificador del asistente, evento y fecha y hora de salida.
- **Origen:** Necesidad de controlar las entradas y salidas.

**Criterio de aceptación:**

Dado que un asistente tiene una entrada registrada y no tiene una salida registrada, cuando el personal de acceso registra su salida, entonces el sistema almacena la fecha y hora correspondiente y actualiza su estado.

### RF06 – Almacenar los registros en MySQL

- **Usuario:** Administrador y personal de acceso.
- **Necesidad:** Conservar la información de asistentes y movimientos de asistencia.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Asistentes, eventos y registros de entrada y salida.
- **Origen:** Necesidad de mantener la información de forma persistente.

**Criterio de aceptación:**

Dado que se registra un asistente o un movimiento de asistencia correctamente, cuando se guarda la operación, entonces la información queda almacenada en MySQL y puede consultarse posteriormente.

### RF07 – Validar los datos de los formularios

- **Usuario:** Administrador.
- **Necesidad:** Evitar registros incompletos o incorrectos.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Nombre, apellidos, correo electrónico y tipo de entrada.
- **Origen:** Necesidad de mantener la calidad de los datos.

**Criterio de aceptación:**

Dado que el administrador introduce datos obligatorios vacíos o un correo electrónico con formato incorrecto, cuando intenta guardar el formulario, entonces el sistema impide el registro y muestra mensajes indicando los errores.

### RF08 – Mostrar estadísticas básicas de asistencia

- **Usuario:** Administrador y organizador.
- **Necesidad:** Consultar información general sobre la asistencia al evento.
- **Prioridad:** Obligatoria.
- **Datos afectados:** Número de asistentes registrados, entradas realizadas y salidas realizadas.
- **Origen:** Necesidad de facilitar el seguimiento del evento.

**Criterio de aceptación:**

Dado que existen registros de asistencia, cuando el administrador accede al panel principal, entonces el sistema muestra los totales correspondientes a asistentes, entradas y salidas.

## 3. Comprobaciones adicionales

Además de los criterios anteriores, se comprobarán los siguientes casos:

| Código | Situación | Resultado esperado |
|---|---|---|
| RF02 | Se intenta registrar un asistente con campos obligatorios vacíos. | El sistema impide guardar el registro. |
| RF02 | Se intenta registrar un correo electrónico duplicado para el mismo evento. | El sistema informa del duplicado y evita crear el mismo registro dos veces. |
| RF04 | Se intenta registrar dos veces la entrada sin una salida intermedia. | El sistema impide duplicar la entrada. |
| RF05 | Se intenta registrar una salida sin una entrada previa. | El sistema impide la operación. |
| RF03 | Un usuario sin permisos intenta modificar un asistente. | El sistema deniega la operación. |
| RF04 | Un usuario sin permisos intenta registrar una entrada. | El sistema deniega la operación. |

## 4. Relación con la planificación

Los requisitos funcionales RF01 a RF08 se consideran obligatorios para la primera versión de la modernización.

Cada uno tendrá una tarea correspondiente en el documento `07-planificacion.md`, donde se indicarán las dependencias, estimaciones, orden de ejecución, responsable y condición de cierre.

## 5. Resultado esperado

La aplicación deberá permitir consultar y gestionar asistentes, registrar sus entradas y salidas, validar la información y mostrar estadísticas básicas.

Los criterios definidos servirán para comprobar el funcionamiento de cada requisito durante las siguientes fases de desarrollo y pruebas.