# 05. Análisis de riesgos – Attendize TechFest

## 1. Objetivo

Este documento identifica los principales riesgos que pueden afectar al proyecto de modernización de Attendize v2.8.0 para TechFest. También establece medidas preventivas y soluciones para reducir su impacto.

## 2. Identificación de riesgos

| ID | Riesgo | Probabilidad | Impacto |
|---|---|---|---|
| R01 | Incompatibilidad entre las tecnologías actuales y las propuestas. | Media | Alto |
| R02 | Pérdida o corrupción de datos durante los cambios. | Baja | Muy alto |
| R03 | Errores en el registro de entradas y salidas. | Media | Alto |
| R04 | Acceso a información por usuarios sin permisos. | Media | Alto |
| R05 | Retrasos en la planificación del proyecto. | Media | Medio |
| R06 | Problemas de adaptación de la interfaz a dispositivos móviles. | Media | Medio |
| R07 | Dificultades para comprender o modificar el código existente. | Alta | Medio |
| R08 | Fallos en las validaciones de los formularios. | Media | Medio |

## 3. Medidas preventivas y soluciones

### R01. Incompatibilidad tecnológica
- Revisar las versiones de Laravel, PHP y las tecnologías frontend utilizadas.
- Comprobar la compatibilidad antes de incorporar nuevas dependencias.
- Mantener las tecnologías actuales cuando sea posible.

### R02. Pérdida o corrupción de datos
- Realizar copias de seguridad antes de cualquier modificación.
- Probar los cambios en un entorno de desarrollo.
- Verificar la integridad de los datos después de las operaciones.

### R03. Errores en entradas y salidas
- Definir reglas claras para registrar entradas y salidas.
- Evitar entradas duplicadas y salidas sin una entrada previa.
- Realizar pruebas con diferentes situaciones de acceso.

### R04. Accesos no autorizados
- Utilizar el sistema de autenticación existente.
- Comprobar los permisos antes de ejecutar operaciones.
- Evitar mostrar información personal a usuarios que no la necesiten.

### R05. Retrasos en la planificación
- Dividir el trabajo en tareas pequeñas.
- Establecer prioridades.
- Revisar periódicamente el progreso.

### R06. Problemas de adaptación móvil
- Diseñar interfaces adaptables a distintos tamaños de pantalla.
- Probar las pantallas en dispositivos móviles y ordenadores.
- Evitar elementos demasiado pequeños o difíciles de utilizar.

### R07. Dificultades con el código existente
- Analizar la estructura del proyecto antes de modificarlo.
- Consultar la documentación de Attendize y Laravel.
- Documentar los cambios realizados.

### R08. Errores de validación
- Definir los campos obligatorios.
- Validar los datos tanto en el cliente como en el servidor.
- Mostrar mensajes de error claros y comprensibles.

## 4. Seguimiento de riesgos

Los riesgos se revisarán durante las distintas fases del proyecto. Si aparece un problema nuevo, se incorporará a este documento indicando su probabilidad, impacto y medidas correctoras.

Los riesgos de mayor prioridad serán los relacionados con la pérdida de datos, la seguridad y el control de entradas y salidas.

## 5. Conclusión

La identificación anticipada de riesgos permite reducir los problemas durante la modernización de Attendize. Aplicando medidas preventivas, realizando pruebas y manteniendo copias de seguridad, se pretende conseguir una aplicación más segura, estable y fácil de utilizar.

---

**Proyecto:** TechFest  
**Aplicación:** Attendize v2.8.0  
**Actividad:** Tarea 2 – Propuesta y planificación
