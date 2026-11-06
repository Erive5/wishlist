# RF-04 — Ocasiones especiales

**Producto:** WishList  
**Versión:** 0.2  
**Estado:** Aprobado funcionalmente, con decisiones pendientes identificadas  
**Prioridad:** MVP

## 1. Objetivo

Permitir a los integrantes de WishList consultar cumpleaños y organizar otras ocasiones especiales para anticipar la elección de regalos.

Las ocasiones deben estar relacionadas con las personas y grupos correspondientes, sin requerir listas de deseos específicas para cada celebración.

## 2. Requisitos funcionales

### RF-OCC-01 — Cumpleaños de usuarios

El sistema debe utilizar el día y mes de nacimiento registrados en cada perfil para identificar automáticamente los cumpleaños.

**Historia de usuario**

Como integrante de un grupo, quiero conocer los próximos cumpleaños para organizar regalos con anticipación.

**Criterios de aceptación**

- Todo usuario registrado tiene día y mes de cumpleaños.
- El año de nacimiento es opcional.
- El cumpleaños se reconoce como una ocasión anual recurrente.
- El sistema calcula la próxima fecha de celebración.
- Los cumpleaños se muestran únicamente a integrantes de grupos compartidos.
- No es necesario crear manualmente un evento de cumpleaños.
- El año de nacimiento y la edad no se muestran a otros integrantes por defecto.
- Los cumpleaños registrados el 29 de febrero se celebrarán el 28 de febrero durante los años no bisiestos.
- El ajuste afecta únicamente al cálculo de la celebración; la fecha de nacimiento original permanece registrada como 29 de febrero.

### RF-OCC-02 — Creación de ocasiones personalizadas

El sistema debe permitir crear ocasiones especiales adicionales.

**Historia de usuario**

Como integrante, quiero registrar una ocasión especial para organizar regalos más allá de los cumpleaños.

**Datos propuestos**

- Nombre de la ocasión: obligatorio.
- Fecha: obligatoria.
- Descripción: opcional.
- Grupo: obligatorio.
- Destinatario o destinatarios: uno o varios integrantes del grupo.
- Recurrencia: anual o evento único.

**Criterios de aceptación**

- Un integrante puede crear una ocasión dentro de un grupo activo.
- La ocasión debe tener un nombre y una fecha válida.
- Se pueden seleccionar uno o varios destinatarios.
- Los destinatarios deben pertenecer al grupo.
- El creador puede definir si la ocasión se repite anualmente.
- Los integrantes autorizados pueden consultar la ocasión.
- Las ocasiones de un grupo no son visibles para integrantes ajenos.

### RF-OCC-03 — Consulta de próximas ocasiones

El sistema debe permitir visualizar próximas celebraciones de los grupos donde participa el usuario.

**Historia de usuario**

Como usuario, quiero consultar las próximas ocasiones especiales para planificar regalos con anticipación.

**Criterios de aceptación**

- El usuario puede consultar próximas ocasiones de sus grupos.
- Las ocasiones aparecen ordenadas cronológicamente.
- Cada ocasión muestra su nombre, fecha y destinatarios.
- Los cumpleaños se incluyen automáticamente.
- Las ocasiones de grupos ajenos no son visibles.
- Una misma celebración puede presentarse una sola vez sin duplicar innecesariamente la información, sin revelar nombres ni membresías de grupos ajenos.

### RF-OCC-04 — Modificación y cancelación de ocasiones

El sistema debe permitir que el creador de una ocasión personalizada pueda modificarla o cancelarla.

**Historia de usuario**

Como creador de una ocasión, quiero actualizar o cancelar su información para mantener correcta la planificación de mi grupo.

**Criterios de aceptación**

- Solo el creador puede modificar o cancelar una ocasión personalizada.
- Los demás integrantes pueden comunicar sugerencias de modificación externamente.
- El creador puede modificar el nombre, descripción, fecha, destinatarios y recurrencia.
- Los cambios deben validarse antes de guardarse.
- La cancelación requiere confirmación.
- Los cumpleaños automáticos no pueden modificarse ni eliminarse desde la gestión de ocasiones.
- Si existen reservas o propuestas vinculadas, estas deben conservarse hasta resolver su estado.
- La cancelación no debe eliminar automáticamente registros históricos de regalos.
- Un creador que ya no pertenezca al grupo no podrá modificar sus ocasiones mientras no tenga una membresía activa.

**Decisión pendiente**

Definir qué sucede con una ocasión si su creador abandona el grupo o elimina su cuenta.

### RF-OCC-05 — Asociación de regalos a ocasiones

El sistema debe permitir relacionar una organización de regalo con una ocasión específica.

**Historia de usuario**

Como integrante, quiero identificar para qué celebración estoy organizando un regalo para mantener ordenados mis compromisos.

**Criterios de aceptación**

- Una reserva o propuesta puede asociarse a una ocasión.
- La ocasión debe ser accesible para las personas involucradas.
- Un mismo deseo puede considerarse para distintas ocasiones mientras no exista una reserva incompatible.
- Las asociaciones deben respetar la privacidad del destinatario.
- Las reglas detalladas de reservas se especificarán en RF-05.
- Los cambios o cancelaciones de ocasiones no cancelarán automáticamente las reservas asociadas; su tratamiento se definirá en RF-05.

### RF-OCC-06 — Recordatorios de ocasiones

El sistema debe mostrar avisos internos sobre ocasiones especiales próximas.

**Historia de usuario**

Como integrante de un grupo, quiero visualizar recordatorios de las próximas celebraciones para organizar regalos con anticipación.

**Criterios de aceptación**

- El sistema identifica automáticamente las próximas ocasiones.
- Se muestra un aviso cuando faltan 14 días para una ocasión.
- Se muestra un segundo aviso cuando faltan 7 días.
- Los avisos se muestran dentro de WishList.
- Los avisos identifican la ocasión, fecha y destinatarios.
- Los avisos son visibles únicamente para integrantes autorizados.
- Los avisos dirigidos al destinatario no deben revelar reservas, compras ni propuestas organizadas para él.
- Los avisos no requieren que el usuario tenga activadas notificaciones del dispositivo.
- Los recordatorios no se envían mediante correo, SMS ni notificaciones push en el MVP.
- Una misma ocasión no debe generar avisos duplicados para un mismo usuario y plazo.

## 3. Reglas de negocio

- **RN-OCC-01:** los cumpleaños se derivan del perfil del usuario.
- **RN-OCC-02:** el cumpleaños se repite anualmente.
- **RN-OCC-03:** el año de nacimiento no es necesario para calcular la próxima celebración.
- **RN-OCC-04:** las ocasiones personalizadas pueden ser únicas o recurrentes.
- **RN-OCC-05:** una ocasión personalizada pertenece a un grupo.
- **RN-OCC-06:** las ocasiones personalizadas pueden tener uno o varios destinatarios.
- **RN-OCC-07:** las ocasiones no requieren crear listas de deseos independientes.
- **RN-OCC-08:** los permisos para consultar ocasiones dependen de las membresías activas.
- **RN-OCC-09:** cancelar una ocasión no elimina automáticamente las reservas ni el historial.
- **RN-OCC-10:** los avisos dirigidos al destinatario no deben revelar la organización de sus regalos.
- **RN-OCC-11:** los cumpleaños del 29 de febrero se celebrarán el 28 de febrero en años no bisiestos, sin modificar la fecha de nacimiento registrada.
- **RN-OCC-12:** únicamente el creador de una ocasión personalizada podrá modificarla o cancelarla, siempre que mantenga una membresía activa en el grupo.
- **RN-OCC-13:** los integrantes pueden sugerir cambios mediante canales externos, sin que WishList gestione solicitudes formales de modificación.
- **RN-OCC-14:** el sistema mostrará recordatorios internos a 14 y 7 días de cada ocasión.
- **RN-OCC-15:** los recordatorios deberán respetar la privacidad del destinatario y las autorizaciones de acceso.
- **RN-OCC-16:** el sistema deberá evitar generar avisos duplicados para un mismo usuario, ocasión y plazo.

## 4. Fuera del alcance inicial

- Sincronización con Google Calendar u otros calendarios.
- Notificaciones push.
- Invitaciones a eventos sociales externos.
- Gestión de lugares o celebraciones presenciales.
- Recordatorios por correo electrónico o SMS.
- Planificación de compras y entregas físicas.
- Automatización de regalos mediante inteligencia artificial.
- Sistema interno de solicitudes de modificación de ocasiones.

## 5. Decisiones pendientes

### DP-OCC-01 — Cumpleaños del 29 de febrero

**Estado:** Resuelto.

En años no bisiestos, los cumpleaños del 29 de febrero se celebrarán el 28 de febrero.

La fecha de nacimiento registrada no se modificará.

### DP-OCC-02 — Responsabilidad de ocasiones personalizadas

**Estado:** Parcialmente resuelto.

Solo el creador puede modificar o cancelar una ocasión personalizada mientras mantenga una membresía activa.

Pendiente de definir:

- Qué ocurre cuando el creador abandona el grupo.
- Si otro integrante puede asumir la responsabilidad de una ocasión.
- Qué sucede cuando el creador elimina su cuenta.

### DP-OCC-03 — Recordatorios

**Estado:** Resuelto para el MVP.

Los recordatorios aparecerán dentro de WishList a 14 y 7 días de la celebración.

No se implementarán notificaciones push, correos ni SMS en la primera versión.

### DP-OCC-04 — Ocasiones con varios destinatarios

Definir cómo se presentan las organizaciones de regalos cuando una celebración involucra a más de una persona.

### DP-OCC-05 — Ocasiones compartidas entre grupos

Precisar cómo se muestran eventos relacionados con una misma persona en grupos diferentes, sin exponer información privada de otros grupos.

### DP-OCC-06 — Modificaciones y cancelaciones con reservas activas

Definir cómo se gestionan las reservas y propuestas de regalos cuando una ocasión cambia de fecha o es cancelada.

Esta decisión se resolverá en el módulo RF-05 — Reservas.

## 6. Consideraciones técnicas

- Las fechas de cumpleaños deben manejarse como día y mes, sin exigir año.
- Las ocasiones recurrentes deben calcular correctamente su siguiente ocurrencia.
- El cálculo de cumpleaños del 29 de febrero debe respetar la regla definida para años no bisiestos.
- Las fechas y los recordatorios deben respetar una zona horaria definida.
- Los recordatorios del MVP podrán calcularse a partir de las fechas de las ocasiones, sin requerir necesariamente un servicio de envío de notificaciones.
- Las consultas deben aplicar controles de autorización en el backend.
- Las ocasiones deben tener identificadores internos estables.
- Los cambios y cancelaciones de ocasiones no deben eliminar automáticamente las reservas ni los registros históricos asociados.
- Las consultas de próximas ocasiones deben evitar resultados duplicados innecesarios.

---

**Estado del documento:** aprobado funcionalmente, con decisiones pendientes identificadas que no impiden continuar con la especificación de otros módulos.