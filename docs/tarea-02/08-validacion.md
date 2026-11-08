# 08. Validación y pruebas – Attendize TechFest

## 1. Objetivo

El objetivo de este documento es definir las comprobaciones necesarias para verificar que la modernización de Attendize v2.8.0 cumple los requisitos establecidos para TechFest.

Las pruebas se realizarán cuando las funcionalidades estén implementadas. En esta fase se define el plan de validación, no se afirma que las pruebas ya se hayan ejecutado.

## 2. Tipos de pruebas

Se plantean los siguientes tipos de pruebas:

- **Pruebas funcionales:** comprobar que cada funcionalidad realiza correctamente su tarea.
- **Pruebas de validación:** verificar que los formularios rechazan datos incorrectos o incompletos.
- **Pruebas de seguridad:** comprobar que cada usuario solo puede realizar las operaciones permitidas.
- **Pruebas de usabilidad:** verificar que la interfaz sea clara y fácil de utilizar.
- **Pruebas de compatibilidad:** comprobar el funcionamiento en diferentes navegadores y tamaños de pantalla.
- **Pruebas de persistencia:** verificar que los datos se guardan y recuperan correctamente desde MySQL.

## 3. Casos de prueba

| ID | Prueba | Resultado esperado |
|---|---|---|
| P01 | Consultar el listado de asistentes. | Se muestran los asistentes registrados. |
| P02 | Registrar un nuevo asistente con datos válidos. | El asistente queda guardado correctamente. |
| P03 | Intentar registrar un asistente sin campos obligatorios. | El sistema muestra mensajes de validación. |
| P04 | Editar los datos de un asistente. | Los cambios se guardan correctamente. |
| P05 | Registrar la entrada de un asistente. | Se registra la entrada con fecha y hora. |
| P06 | Registrar la salida de un asistente. | Se registra la salida correctamente. |
| P07 | Intentar registrar una salida sin entrada previa. | El sistema impide la operación. |
| P08 | Intentar registrar una entrada duplicada. | El sistema aplica la regla de control de acceso definida. |
| P09 | Consultar las métricas de asistencia. | Los totales coinciden con los registros existentes. |
| P10 | Acceder con un usuario sin permisos suficientes. | El sistema deniega la operación. |
| P11 | Consultar la aplicación desde un móvil. | La interfaz se adapta al tamaño de pantalla. |
| P12 | Comprobar la persistencia de los datos. | La información continúa disponible después de volver a consultar la aplicación. |

## 4. Criterios de aceptación

Se considerará que la modernización cumple los objetivos cuando:

- Los asistentes puedan consultarse y gestionarse correctamente.
- Los datos se almacenen de forma persistente en MySQL.
- Las entradas y salidas respeten las reglas de acceso.
- Los formularios validen los datos introducidos.
- Los permisos de usuario estén correctamente aplicados.
- Las métricas básicas reflejen los datos reales.
- La interfaz pueda utilizarse desde ordenadores y dispositivos móviles.

## 5. Registro de resultados

Durante la ejecución de las pruebas se utilizará una tabla para registrar los resultados:

| ID | Estado | Observaciones |
|---|---|---|
| P01 | Pendiente | |
| P02 | Pendiente | |
| P03 | Pendiente | |
| P04 | Pendiente | |
| P05 | Pendiente | |
| P06 | Pendiente | |
| P07 | Pendiente | |
| P08 | Pendiente | |
| P09 | Pendiente | |
| P10 | Pendiente | |
| P11 | Pendiente | |
| P12 | Pendiente | |

Los estados posibles serán: Pendiente, Correcta o Incorrecta.

Si una prueba resulta incorrecta, se anotará el problema detectado y se revisará antes de repetirla.

## 6. Conclusión

El plan de validación permitirá comprobar que las mejoras propuestas para Attendize funcionan correctamente y cumplen los requisitos definidos. Las pruebas ayudarán a detectar errores, mejorar la seguridad y garantizar una experiencia de uso adecuada para el personal de TechFest.

---

**Proyecto:** TechFest  
**Aplicación:** Attendize v2.8.0  
**Actividad:** Tarea 2 – Propuesta y planificación