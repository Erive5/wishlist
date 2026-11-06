# RF-05 — Reservas de regalos

**Producto:** WishList  
**Versión:** 0.2  
**Estado:** Consolidado para revisión; reglas confirmadas y pendientes diferenciadas  
**Prioridad:** MVP

## 1. Objetivo y alcance

Permitir que integrantes autorizados reserven deseos ajenos, coordinen su entrega y eviten duplicaciones, incluso cuando una misma lista esté compartida entre varios grupos. El destinatario no debe descubrir la organización del regalo mediante la aplicación.

WishList es una herramienta **organizacional**: no procesa pagos, verifica compras ni coordina entregas físicas. Los estados de compra y entrega reflejan declaraciones de usuarios, no hechos verificados externamente.

Este módulo regula **reservas individuales**. Las propuestas con múltiples participantes se detallarán en **RF-06 — Regalos colaborativos**, manteniendo la misma garantía de disponibilidad global.

## 2. Conceptos y estados

- **Deseo:** elemento de una lista cuyo propietario es el destinatario potencial.
- **Reserva:** compromiso de un usuario de regalar un deseo. Se vincula al **deseo original**, no a una copia por grupo.
- **Responsable:** usuario que crea y gestiona una reserva individual.
- **Acceso autorizado:** se deriva de una membresía activa en al menos un grupo con el que esté compartida la lista.

### 2.1. Estado del deseo

| Estado | Significado |
|---|---|
| Pendiente | El propietario aún desea recibirlo. |
| Cumplido | El propietario confirmó que lo recibió. |
| Archivado | El propietario lo retiró de los deseos activos sin borrar sus datos. |

**Disponible** no es un estado del deseo: es una condición calculada. Un deseo pendiente puede reservarse únicamente si no hay una organización incompatible activa y se cumplen las reglas de autorización y visibilidad.

### 2.2. Estado de una reserva individual

| Estado | Significado |
|---|---|
| Reservada | Existe compromiso, sin compra declarada. |
| Comprada | El responsable declaró una compra externa. |
| Entregada | El responsable declaró una entrega. |
| Cancelada | Se cerró sin atribuir entrega; no constituye un regalo realizado. |
| Completada | Se cerró con entrega confirmada en la conciliación correspondiente. |

La **recepción** es una confirmación del destinatario que puede convertir el deseo en *cumplido*. No es un estado adicional de la reserva.

La **conciliación** se gestiona aparte, como *no requerida*, *pendiente* o *resuelta*. Por ejemplo, una reserva puede conservar su último estado declarado «Comprada» mientras tiene una conciliación pendiente. Los cambios deben conservar trazabilidad, sin sobrescribir las declaraciones previas.

**Flujo habitual propuesto:** Reservada → Comprada → Entregada → Completada. Se admite confirmar una entrega sin registrar antes una compra. También es posible cerrar sin entrega mediante un flujo controlado. Las transiciones exactas y correcciones se validarán en el diseño técnico y en las reglas aún pendientes.

## 3. Requisitos funcionales

### RF-RES-01 — Crear una reserva

**Historia de usuario:** Como integrante, quiero reservar el deseo de otra persona para evitar que se organice el mismo regalo por duplicado.

**Criterios de aceptación**

- Solo un usuario autenticado y con acceso autorizado al deseo puede reservarlo.
- Nadie puede reservar sus propios deseos.
- Solo pueden reservarse deseos pendientes, no archivados, que estén disponibles.
- La reserva identifica al deseo original, responsable y fecha de creación.
- Antes de confirmar, el backend verifica disponibilidad y autorización vigentes.
- Si ya existe una reserva u organización incompatible, la operación se rechaza sin modificar los datos.
- Dos solicitudes simultáneas no pueden producir compromisos incompatibles sobre el mismo deseo.
- Una reserva confirmada bloquea el deseo para todos los grupos donde se comparte.

### RF-RES-02 — Disponibilidad global

**Historia de usuario:** Como integrante, quiero ver que un deseo está ocupado aunque alguien lo haya reservado desde otro grupo, para no regalar lo mismo.

**Criterios de aceptación**

- El deseo y su disponibilidad son únicos entre los grupos que comparten la lista.
- Una reserva activa incompatible impide crear otra reserva individual.
- Los integrantes autorizados ven la disponibilidad actualizada.
- No se crean reservas independientes por grupo.
- La disponibilidad vuelve a calcularse cuando una reserva se libera o cierra, considerando también el estado del deseo.
- Un deseo cumplido no vuelve automáticamente a estar disponible.

### RF-RES-03 — Consultar reservas y proteger la sorpresa

**Historia de usuario:** Como posible regalador, quiero consultar el estado de los deseos y conocer al responsable cuando corresponda, sin que el destinatario descubra la sorpresa.

**Criterios de aceptación**

- Los integrantes autorizados distintos del destinatario pueden consultar disponibilidad y estado organizacional.
- Todos los grupos autorizados conocen la disponibilidad global del deseo: ven que está reservado o no disponible.
- Un integrante puede conocer la identidad del responsable únicamente si comparte al menos un grupo con él; en caso contrario, solo ve que el regalo está reservado o no disponible.
- Nunca se revelan nombres ni membresías de grupos ajenos para justificar la reserva.
- El destinatario ve sus deseos y su estado personal, pero no reservas, compras, propuestas, responsables ni conciliaciones destinadas a él.
- Ninguna respuesta de API, URL directa, aviso o resumen debe revelar estos datos al destinatario.
- Consultar una lista desde un grupo no debe exponer información privada de otros grupos innecesariamente.
- Las autorizaciones se comprueban en el backend en cada operación.

### RF-RES-04 — Liberar una reserva

**Historia de usuario:** Como responsable, quiero liberar una reserva si cambio de planes para que otra persona pueda organizar el regalo.

**Criterios de aceptación**

- El responsable puede liberar manualmente una reserva en estado **Reservada**, con confirmación previa.
- Otros integrantes no pueden liberar reservas ajenas mediante el flujo habitual.
- Si el responsable abandona o es retirado de grupos y pierde **todos sus accesos grupales** al deseo, una reserva en estado *Reservada* se libera automáticamente.
- Si conserva acceso por otro grupo autorizado, su reserva continúa activa.
- Las reservas *Compradas* o *Entregadas* no se liberan automáticamente por esa pérdida de acceso.
- La liberación actualiza la disponibilidad global del deseo si este sigue pendiente y no existen otros bloqueos.
- Se conserva un registro de liberaciones manuales y automáticas.
- La liberación automática no revela al destinatario la organización anterior.

**Pendiente:** resolución de reservas compradas o entregadas cuyos responsables pierdan acceso; aplicación de reglas cuando el propietario **deja de compartir** una lista, que es un evento distinto a abandonar un grupo.

### RF-RES-05 — Registrar compra o cierre sin entrega

**Historia de usuario:** Como responsable, quiero informar si compré un regalo y poder indicar que finalmente no lo entregaré, para mantener actualizada la organización.

**Criterios de aceptación**

- El responsable autorizado puede marcar una reserva como *Comprada*.
- No se requiere comprobante ni se realizan pagos en WishList.
- Una reserva *Comprada* mantiene bloqueado el deseo.
- El responsable autorizado puede declarar que no entregará el regalo, incluso si anteriormente lo marcó como comprado.
- Al cerrarse sin entrega, la reserva queda *Cancelada*, conservando la declaración de compra previa en la trazabilidad.
- Si el deseo sigue pendiente y no existen otros bloqueos, vuelve a estar disponible.
- Una compra sin entrega confirmada **no** se registra en el historial de regalos realizados.
- El destinatario no ve estas declaraciones antes ni durante la organización.

**Pendiente:** mecanismo de corrección si una reserva ya figura como *Entregada* y se necesita rectificar la declaración.

### RF-RES-06 — Entrega, recepción y conciliación

**Historia de usuario:** Como responsable, quiero informar que entregué el regalo; como destinatario, quiero marcar un deseo como recibido sin tener que conocer los acuerdos privados.

**Criterios de aceptación**

- El responsable autorizado puede declarar la entrega, incluso si no registró previamente una compra.
- El propietario puede marcar su deseo como *Cumplido* al confirmar que lo recibió, aunque no exista reserva.
- La recepción actualiza inmediatamente el estado personal del deseo.
- Si hay una reserva activa, se abre una **conciliación privada** con el responsable y se conserva su estado y trazabilidad anteriores.
- El sistema solicita privadamente al responsable indicar si fue él quien entregó el regalo.
- Si confirma su entrega, la organización se cierra como *Completada* y puede atribuirse a su historial de regalos realizados.
- Si declara que no lo entregó, se cierra como *Cancelada* sin atribuirle el regalo.
- La conciliación no revierte la recepción confirmada, no elimina compras declaradas previamente y no permite reservar otra vez un deseo cumplido.
- Mientras la conciliación esté pendiente, no se atribuye el regalo a un responsable en su historial de regalos realizados.
- La conciliación no vence automáticamente y sus recordatorios son privados.
- El destinatario no puede consultar la identidad del responsable ni los detalles de la conciliación por esta vía.

**Aclaración:** el historial de regalos **recibidos** puede registrar el deseo cumplido sin conocer al regalador; el historial de regalos **realizados** requiere confirmación válida de participación en la entrega. Si nadie confirma la entrega, el deseo cumplido queda registrado en recibidos sin regalador atribuido. El módulo RF-07 definirá los campos y consultas definitivos.

### RF-RES-07 — Recordatorios de revisión

**Historia de usuario:** Como responsable, quiero recordar mis reservas pendientes sin que se cancelen automáticamente.

**Criterios de aceptación**

- Las reservas no caducan por el mero transcurso del tiempo.
- WishList muestra avisos internos de revisión a responsables autorizados cuando corresponda.
- Los recordatorios no modifican ni liberan reservas.
- No revelan información organizacional al destinatario.
- Su frecuencia y activadores quedan pendientes de definición; no deben confundirse con los avisos de ocasiones a 14 y 7 días de RF-04.

### RF-RES-08 — Cambios en listas, grupos y ocasiones

**Historia de usuario:** Como responsable, quiero que mis compromisos se gestionen consistentemente cuando cambien los accesos o la fecha del evento.

**Criterios de aceptación**

- Archivar una lista no cancela reservas existentes ni permite crear otras nuevas.
- Las organizaciones activas continúan accesibles a sus participantes mientras mantengan autorización, sin revelar la sorpresa.
- No se permite eliminar definitivamente una lista con reservas activas.
- Salir de un grupo o ser retirado exige comprobar si el responsable conserva acceso al deseo por otros grupos.
- Si pierde todos esos accesos, sus reservas *Reservadas* se liberan automáticamente y se registra la operación.
- Las reservas *Compradas* o *Entregadas* se conservan hasta resolver el caso por un mecanismo pendiente de definir.
- El cambio o cancelación de una ocasión no cancela automáticamente las reservas relacionadas.
- Dejar de compartir una lista con un grupo tampoco elimina por sí solo sus registros de reserva; el efecto sobre reservas sin acceso queda **pendiente**.
- Los cambios no borran el historial ni conceden acceso indebido a grupos abandonados.

## 4. Reglas de negocio confirmadas

- **RN-RES-01:** una reserva representa un compromiso organizacional; no es una transacción de compra.
- **RN-RES-02:** cada reserva se vincula al deseo original, no a una copia por grupo.
- **RN-RES-03:** un deseo no puede mantener dos reservas individuales incompatibles activas.
- **RN-RES-04:** la disponibilidad del deseo es global para todos los grupos donde se comparte su lista.
- **RN-RES-05:** el destinatario no puede consultar información que revele reservas, compras o propuestas para él.
- **RN-RES-06:** todos los grupos autorizados conocen la disponibilidad global del deseo. La identidad del responsable solo es visible para quienes comparten al menos un grupo con él; el resto ve únicamente que el regalo está reservado o no disponible, sin exponer nombres ni membresías de grupos ajenos.
- **RN-RES-07:** las reservas no vencen automáticamente por tiempo transcurrido.
- **RN-RES-08:** se contemplan recordatorios de revisión que no modifican la reserva.
- **RN-RES-09:** solo el responsable libera una reserva mediante el flujo habitual.
- **RN-RES-10:** se requieren excepciones controladas para cuentas eliminadas o inactivas, aún por especificar.
- **RN-RES-11:** archivar una lista no cancela reservas existentes ni habilita reservas nuevas.
- **RN-RES-12:** el propietario puede marcar su deseo como recibido sin conocer la organización privada.
- **RN-RES-13:** la autorización y disponibilidad deben comprobarse en el backend.
- **RN-RES-14:** deben impedirse reservas incompatibles incluso ante operaciones concurrentes.
- **RN-RES-15:** los pagos, compras y entregas físicas ocurren fuera de WishList.
- **RN-RES-16:** la confirmación de recepción convierte inmediatamente el deseo en *Cumplido*.
- **RN-RES-17:** si existe una reserva activa al confirmar recepción, comienza una conciliación privada.
- **RN-RES-18:** la recepción no demuestra que el responsable de la reserva haya efectuado la entrega.
- **RN-RES-19:** un regalo solo se atribuye al historial de regalos realizados tras una declaración válida que confirme participación en la entrega.
- **RN-RES-20:** la conciliación conserva los estados y declaraciones anteriores, incluida la compra declarada.
- **RN-RES-21:** una conciliación pendiente no revierte el deseo cumplido ni habilita nuevas reservas sobre él.
- **RN-RES-22:** las conciliaciones no vencen automáticamente; sus avisos son privados.
- **RN-RES-23:** al perder todos los accesos grupales por salida o retirada, se liberan automáticamente las reservas en estado *Reservada*.
- **RN-RES-24:** conservar acceso al deseo por otro grupo impide esa liberación automática.
- **RN-RES-25:** las reservas *Compradas* o *Entregadas* no se liberan automáticamente por pérdida de membresía.
- **RN-RES-26:** el responsable aún autorizado puede cerrar una reserva comprada como *Cancelada* si no realizará la entrega.
- **RN-RES-27:** cerrar una reserva sin entrega solo libera el deseo si sigue pendiente y no existen otros bloqueos.
- **RN-RES-28:** declarar una compra no equivale a registrar un regalo realizado; debe conservarse su trazabilidad.
- **RN-RES-29:** el historial de regalos realizados excluye reservas canceladas y compras sin entrega confirmada.
- **RN-RES-30:** toda liberación automática se registra y actualiza la disponibilidad de forma consistente.

## 5. Propuestas aún no aprobadas

- **RP-RES-01:** admitir saltos controlados entre etapas del flujo (por ejemplo, declarar entrega sin registrar compra).
- **RP-RES-02:** ofrecer un flujo específico de corrección para reservas declaradas *Entregadas* por error, conservando los eventos previos.
- **RP-RES-04:** el historial de regalos conserva una referencia descriptiva independiente de la lista original para permitir eliminar listas antiguas sin perder registros de regalos realizados.
- **RP-RES-05:** los cambios sustanciales a deseos reservados generan un aviso privado al responsable.

> `RP-RES-03` se retiró porque la conciliación al confirmar recepción ya fue aprobada y está recogida en las reglas RN-RES-16 a RN-RES-22. Los identificadores restantes se conservan por trazabilidad.

## 6. Casos pendientes y dependencias

### DP-RES-01 — Cuenta inactiva

Definir qué constituye inactividad, quién puede solicitar una resolución excepcional y cómo evitar liberar compromisos legítimos sin autorización.

### DP-RES-02 — Eliminación de cuenta

Definir la resolución de reservas individuales activas y la conservación mínima de trazabilidad. Las colaborativas se abordarán en RF-06.

### DP-RES-03 — Pérdida de membresía

**Parcialmente resuelto:** reservas *Reservadas* se liberan al perder el último acceso grupal por salida o retirada; las *Compradas* y *Entregadas* no.

**Pendiente:** quién y cómo resuelve las reservas compradas/entregadas sin restituir permisos sobre el grupo.

### DP-RES-04 — Recepción con reserva activa

**Resuelto funcionalmente:** el deseo pasa a cumplido, se abre conciliación privada, y solo se atribuye la entrega al responsable tras su confirmación válida. No hay vencimiento automático. El diseño técnico definirá los registros y transiciones exactas.

### DP-RES-05 — Cambios a deseos reservados

Definir qué modificaciones deben generar avisos privados y qué ocurre con modificaciones sustanciales.

### DP-RES-06 — Cambios o cancelación de ocasión

Definir qué avisos y opciones de reorganización se ofrecen cuando cambia la fecha o se cancela una ocasión, sin cancelar automáticamente una reserva.

### DP-RES-07 — Frecuencia de recordatorios

Definir cuándo mostrar recordatorios de revisión y cómo evitar avisos repetitivos.

### DP-RES-08 — Deseos repetibles

Definir su disponibilidad después de una recepción y cómo evitar reservas simultáneas no deseadas. Relacionado con RF-03 y RF-07.

### DP-RES-09 — Regalos colaborativos

Distinguir la autoridad del responsable individual de la del organizador y participantes de un regalo colaborativo. Se resolverá en RF-06.

### DP-RES-10 — Visibilidad de responsables entre grupos

**Resuelto:** la identidad del responsable solo es visible para quienes comparten al menos un grupo con él; los demás grupos autorizados ven únicamente que el regalo está reservado o no disponible, sin exponer nombres ni membresías ajenas. La identidad se muestra según los permisos de perfil de RF-01.

### DP-RES-11 — Lista retirada de un grupo

Precisar qué ocurre con las reservas del grupo al dejar de compartir la lista. No se debe extrapolar automáticamente la regla de salida de un integrante.

### DP-RES-12 — Corrección de entrega

Definir cómo rectificar declaraciones de entrega equivocadas, sin borrar su trazabilidad ni alterar indebidamente el historial.

## 7. Fuera de alcance del MVP

- Pagos, transferencias y compras dentro de WishList.
- Verificación automática de compras, facturas o comprobantes.
- Seguimiento de envíos y entregas físicas.
- Liberación automática por antigüedad de una reserva.
- Chat interno y resolución automatizada de disputas.
- Recomendaciones de regalos mediante IA.

## 8. Consideraciones técnicas y pruebas

- Las reservas tienen ID propio y referencian el ID estable del deseo.
- Las operaciones de reserva y liberación deben ser transaccionales e idempotentes cuando corresponda.
- La base de datos y el backend deben impedir reservas incompatibles ante concurrencia; no basta con deshabilitar botones.
- Las respuestas deben filtrar campos según el destinatario y la membresía actual, incluso ante acceso directo a la API.
- La liberación por salida o retirada debe resolverse de forma atómica con el cambio de membresía o con una garantía de consistencia equivalente.
- Las transiciones y conciliaciones deben conservar eventos mínimos de auditoría.
- Las pruebas deberán cubrir: dos reservas simultáneas; deseo compartido entre grupos; acceso del destinatario; salida con y sin acceso alternativo; reserva comprada no entregada; recepción con conciliación; archivo de lista; y cambios de ocasión.

## 9. Dependencias documentales

- **RF-01:** identidad, autenticación y visibilidad de perfiles.
- **RF-02:** grupos, invitaciones, membresías y salida/retirada.
- **RF-03:** listas compartidas, deseos y archivo.
- **RF-04:** ocasiones y recordatorios internos.
- **RF-06:** regalos colaborativos y sus participantes.
- **RF-07:** historial de regalos realizados y recibidos.

---

**Estado del documento:** consolidado para revisión. Las reglas de negocio confirmadas quedan separadas de propuestas y decisiones pendientes. No se considera cerrado RF-05 hasta resolver las excepciones que afecten el MVP.
