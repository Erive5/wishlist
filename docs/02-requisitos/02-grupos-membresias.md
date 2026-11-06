# RF-02 — Grupos y membresías

**Producto:** WishList  
**Versión:** 0.1  
**Estado:** Propuesta para revisión  
**Prioridad:** MVP

## 1. Objetivo

Permitir que los usuarios de WishList creen y administren grupos privados para organizar regalos, compartir deseos y coordinar ocasiones especiales.

La gestión de membresías debe favorecer la participación democrática de sus integrantes, sin concentrar las decisiones en un administrador único.

## 2. Requisitos funcionales

### RF-GRP-01 — Creación de grupos

El sistema debe permitir que un usuario autenticado y con correo electrónico verificado cree un grupo privado.

**Historia de usuario**

Como usuario, quiero crear un grupo para organizar regalos con familiares, amigos u otras personas cercanas.

**Criterios de aceptación**

- El usuario puede crear un grupo indicando un nombre obligatorio.
- El nombre del grupo no necesita ser único.
- El grupo recibe un identificador interno único.
- El creador se incorpora automáticamente como primer integrante.
- Un usuario puede crear y pertenecer a múltiples grupos.
- Cada grupo mantiene sus propias membresías y decisiones.
- El grupo es privado y no aparece en búsquedas públicas.

### RF-GRP-02 — Invitaciones a grupos

El sistema debe permitir compartir un mecanismo de invitación para solicitar la incorporación de nuevos integrantes.

**Historia de usuario**

Como integrante, quiero invitar a personas conocidas para que puedan solicitar unirse a nuestro grupo de regalos.

**Criterios de aceptación**

- Cada grupo dispone de un código o enlace de invitación.
- Los integrantes pueden compartir la invitación.
- La invitación no concede acceso inmediato al grupo.
- El usuario invitado debe tener una cuenta verificada.
- El sistema debe impedir solicitudes duplicadas que permanezcan pendientes.
- Los códigos de invitación deben ser difíciles de adivinar.
- Debe existir un mecanismo para invalidar y renovar el código cuando sea necesario.

**Decisión pendiente**

Determinar si se mantendrá una contraseña adicional de grupo o si el código de invitación y la aprobación por votación proporcionan suficiente control de acceso.

### RF-GRP-03 — Solicitud de incorporación

El sistema debe permitir que un usuario solicite incorporarse a un grupo utilizando una invitación válida.

**Historia de usuario**

Como usuario invitado, quiero solicitar mi incorporación a un grupo para compartir deseos y participar en la organización de regalos.

**Criterios de aceptación**

- El usuario debe estar autenticado y tener su correo verificado.
- Una invitación inválida o revocada no permite solicitar ingreso.
- La solicitud se registra con estado pendiente.
- El solicitante puede consultar su estado.
- El solicitante no puede acceder a información privada del grupo mientras su solicitud esté pendiente.
- El solicitante puede retirar una solicitud pendiente.

### RF-GRP-04 — Votación de solicitudes

El sistema debe permitir que los integrantes activos voten para aprobar o rechazar solicitudes de incorporación.

**Historia de usuario**

Como integrante de un grupo, quiero participar en la decisión de aceptar nuevos miembros para mantener el control compartido sobre quién accede a nuestra información.

**Criterios de aceptación propuestos**

- Todos los integrantes activos tienen derecho a un voto por solicitud.
- Cada integrante puede votar a favor o en contra.
- Los votos deben registrarse una sola vez por integrante.
- El solicitante no forma parte del conjunto de votantes ni puede participar en su propia votación.
- La solicitud se aprueba al alcanzar la mayoría absoluta de todos los integrantes activos habilitados para votar: `votos_favorables_requeridos = floor(integrantes_habilitados / 2) + 1`.
- La ausencia de voto no equivale a un voto en contra ni reduce el umbral necesario.
- La solicitud se rechaza cuando los votos emitidos hacen imposible alcanzar el umbral de votos favorables requerido.
- Si todavía no existe una decisión, permanece pendiente.
- El resultado debe calcularse en el backend.
- Las operaciones de votación deben ser seguras frente a solicitudes simultáneas.

**Excepción propuesta**

Cuando el grupo tiene un único integrante, este puede aprobar la incorporación del segundo usuario.

**Decisión pendiente**

Establecer qué sucede con solicitudes que permanecen sin resolver y si los votos pueden modificarse antes de la decisión final.

### RF-GRP-05 — Consulta de integrantes

El sistema debe permitir consultar los integrantes actuales de un grupo.

**Historia de usuario**

Como integrante, quiero saber quiénes pertenecen a mi grupo para organizar regalos y reconocer a las personas participantes.

**Criterios de aceptación**

- Solo los integrantes activos pueden consultar la lista de miembros.
- Cada integrante aparece con su nombre o apodo, según las reglas del perfil.
- Los nombres coincidentes no deben afectar la identificación interna.
- La lista debe actualizarse cuando cambien las membresías.
- El listado no debe revelar información personal restringida.

### RF-GRP-06 — Salida voluntaria

El sistema debe permitir que un integrante abandone voluntariamente un grupo.

**Historia de usuario**

Como integrante, quiero poder abandonar un grupo cuando ya no desee participar en sus actividades.

**Criterios de aceptación**

- Cualquier integrante puede solicitar su salida.
- El sistema solicita confirmación antes de ejecutar la operación.
- Al abandonar el grupo, pierde acceso a su información privada, lo que se informa al usuario antes de salir.
- La salida no elimina la cuenta de WishList.
- El usuario conserva sus deseos personales.
- Los deseos que compartía con el grupo dejan de estar disponibles para sus integrantes, salvo que exista otra relación autorizada.
- Las reservas y propuestas pendientes deben resolverse según reglas específicas para evitar inconsistencias.

**Decisión pendiente**

Definir cómo se gestionarán las reservas y propuestas colaborativas de una persona que abandona el grupo.

### RF-GRP-07 — Retirada de integrantes

El sistema debe permitir iniciar una solicitud para retirar a un integrante del grupo mediante una decisión colectiva.

**Historia de usuario**

Como integrante, quiero que el grupo pueda decidir colectivamente la retirada de una persona cuando su permanencia ya no sea adecuada.

**Criterios de aceptación propuestos**

- Un integrante puede proponer la retirada de otro.
- La persona afectada no puede votar sobre su propia retirada.
- Los demás integrantes habilitados pueden votar.
- La retirada requiere alcanzar el umbral de aprobación definido.
- La operación no debe ejecutarse sin una decisión válida.
- Al retirarse la membresía, se revocan los permisos asociados al grupo.
- La acción debe registrarse para mantener trazabilidad.

**Decisión pendiente**

Definir el umbral de votación y las protecciones contra el uso abusivo de esta funcionalidad.

### RF-GRP-08 — Consulta de grupos

El sistema debe permitir que cada usuario consulte los grupos a los que pertenece.

**Historia de usuario**

Como usuario, quiero consultar mis grupos para acceder rápidamente a sus integrantes, deseos y ocasiones especiales.

**Criterios de aceptación**

- El usuario puede consultar sus grupos activos.
- No puede consultar los datos privados de grupos ajenos.
- Cada grupo presenta su nombre y cantidad de integrantes.
- El usuario puede acceder a un grupo desde su listado.
- Los grupos abandonados o de los que fue retirado dejan de aparecer como membresías activas.

## 3. Reglas de negocio

- **RN-GRP-01:** cada grupo tiene un identificador interno único.
- **RN-GRP-02:** los nombres de grupos pueden repetirse, el enlace de invitación o código no.
- **RN-GRP-03:** solo usuarios autenticados y verificados pueden participar.
- **RN-GRP-04:** una cuenta puede pertenecer a múltiples grupos.
- **RN-GRP-05:** los integrantes activos tienen los mismos derechos de participación en las decisiones colectivas, salvo excepciones expresamente definidas.
- **RN-GRP-06:** crear un grupo no concede privilegios administrativos permanentes.
- **RN-GRP-07:** las invitaciones no otorgan acceso automáticamente.
- **RN-GRP-08:** las nuevas membresías requieren aprobación según las reglas de votación.
- **RN-GRP-09:** una persona no puede tener más de una membresía activa en el mismo grupo.
- **RN-GRP-10:** cada persona habilitada dispone de un voto por decisión.
- **RN-GRP-11:** las restricciones de acceso se validan en el backend.
- **RN-GRP-12:** las decisiones y cambios de membresía no deben revelar información privada de regalos destinados a quien recibe la sorpresa.

## 4. Fuera del alcance inicial

- Grupos públicos.
- Búsqueda pública de grupos.
- Roles administrativos jerárquicos.
- Subgrupos.
- Grupos con membresías anónimas.
- Sistemas de reputación de integrantes.
- Chat interno.
- Moderación automatizada mediante inteligencia artificial.

## 5. Decisiones pendientes

- **DP-GRP-01:** uso de contraseña adicional de grupo.
- **DP-GRP-02:** vencimiento y modificación de votaciones pendientes.
- **DP-GRP-03:** umbral y condiciones de retirada de integrantes.
- **DP-GRP-04:** tratamiento de reservas y propuestas cuando alguien abandona un grupo. **Parcialmente resuelto en RF-05:** las reservas individuales en estado *Reservada* se liberan automáticamente si el responsable pierde todos sus accesos grupales al deseo; las *Compradas* o *Entregadas* se conservan para resolución posterior; si mantiene acceso por otro grupo autorizado, la reserva permanece. El tratamiento de los participantes de regalos colaborativos continúa pendiente según RF-06 (DP-COL-01).
- **DP-GRP-05:** comportamiento de un grupo cuando queda sin integrantes.
- **DP-GRP-06:** mecanismo de recuperación frente a invitaciones comprometidas o conflictos de gobernanza.

## 6. Consideraciones técnicas

- Las membresías deben relacionarse mediante identificadores internos de usuario y grupo.
- La autorización debe comprobar el estado actual de la membresía.
- Las votaciones y sus resultados deben procesarse de forma consistente, incluso ante operaciones simultáneas.
- Las invitaciones deben ser revocables y no contener secretos sensibles en texto legible.
- Los cambios de membresía deben invalidar los permisos correspondientes.
- Las reglas de privacidad de deseos y regalos se implementarán independientemente de la gobernanza del grupo.

---

**Estado:** propuesta funcional pendiente de aprobación de las reglas de ingreso, votación y retirada.