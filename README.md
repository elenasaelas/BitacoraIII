
# Retos de Investigación: Syslog y Log Management

## Reto de Investigación 1: Anatomía de Syslog

### El Estándar Syslog y la Clasificación de Eventos
El estándar **Syslog** constituye el sistema de mensajería fundamental en sistemas Linux para recolectar y organizar los registros de actividad del sistema. Siguiendo el Estándar de Jerarquía del Sistema de Ficheros (**FHS**), estos registros se almacenan centralizadamente en el directorio **/var/log**. El sistema clasifica cada evento basándose en dos variables principales:

*   **Facilidad (Facility):** Determina el origen o la categoría del proceso que genera el mensaje, con ejemplos como **auth** para seguridad, **cron** para tareas programadas y **daemon** para servicios en segundo plano.
*   **Prioridad/Severidad (Severity):** Define el nivel de importancia del evento en una escala que permite filtrar la información desde mensajes de depuración (**debug**) hasta estados críticos donde el sistema queda inusable (**emerg**).

### Seguridad en `/var/log/auth.log`
Permitir que usuarios no privilegiados tengan acceso de lectura al archivo `auth.log` representa una **negligencia grave** que compromete la tríada de seguridad **CID** (Confidencialidad, Integridad y Disponibilidad). Dado que este archivo registra todos los intentos de acceso, su exposición permite a un atacante realizar un reconocimiento interno del sistema. Esto rompe el pilar de la **confidencialidad**, ya que revela nombres de usuario válidos e incluso errores de escritura que podrían exponer contraseñas introducidas accidentalmente en el campo de usuario.

### Diferencias entre Conexión SSH y Acceso Local
Para realizar una auditoría correcta de los fallos de acceso, el administrador utiliza herramientas como `grep` para identificar metadatos específicos en las bitácoras:

1.  **Intento fallido de conexión remota SSH:** Se identifica principalmente por la presencia de una **dirección IP de origen** del host remoto y su asociación al proceso **sshd**.
2.  **Fallo de usuario local:** En lugar de una dirección de red, el registro muestra un identificador de terminal físico o virtual (**TTY**).
3.  **Metadatos comunes:** Ambos registros incluyen siempre el identificador de proceso (**PID**) y el nombre de usuario empleado en el intento.

---

## Reto de Investigación 2: Cumplimiento y Log Management

### Ventajas de la Gestión Centralizada de Registros
A nivel empresarial y legal, especialmente bajo el marco del **RGPD en España**, enviar y custodiar los logs en un **servidor externo seguro** ofrece ventajas vitales para la preservación de la integridad del sistema:

*   **Integridad y No Repudio:** En caso de que una máquina local sea vulnerada, el atacante suele intentar borrar sus huellas alterando los registros locales; un servidor externo garantiza que los logs permanezcan inalterados para auditorías forenses.
*   **Cumplimiento Legal (RGPD):** La normativa exige medidas proactivas para garantizar la integridad y vigilancia de los datos personales, y el *Log Management* centralizado permite demostrar una custodia adecuada de la información de acceso.
*   **Disponibilidad de la Evidencia:** Garantiza que las bitácoras estén accesibles para el diagnóstico incluso si el servidor de origen sufre un fallo crítico de hardware o una caída total del servicio.
*   **Eficiencia en la Auditoría:** Centralizar los registros facilita el diagnóstico de incidentes en infraestructuras distribuidas y complejas, permitiendo una visión global de la seguridad corporativa.

# Bibliografía

[1] Canonical Ltd., "Ubuntu Server documentation," *Ubuntu Documentation*, 2025. [En línea]. Disponible en: https://ubuntu.com/server/docs/explanation/intro-to/security/

[2] S. McClure, J. Scambray y G. Kurtz, *Hacking Exposed: Network Security Secrets & Solutions*, 3ª ed. Nueva York, NY, EE. UU.: McGraw-Hill Education, 2001. Disponible en: https://github.com/jwx0539/hackingLibrary/blob/master/McGraw.Hill.Hacking.Exposed.Network.Security.Secrets.And.Solutions.3rd.Edition.Sep.2001.ISBN.0072193816.pdf

Nota: Además de estos enlaces de búsqueda, también he utilizado el Notebook proporcionado por el docente donde aguarda todo el temario que llevamos dando hasta ahora.
