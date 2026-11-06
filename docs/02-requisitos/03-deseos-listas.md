# RF-03 — Listas de deseos

**Producto:** WishList  
**Versión:** 0.1  
**Estado:** Propuesta para revisión  
**Prioridad:** MVP

## 1. Objetivo

Permitir que los usuarios creen y administren listas de deseos, compartan dichas listas con sus grupos y consulten los deseos de otros integrantes para facilitar la organización de regalos.

Las listas deberán mantener información consistente entre todos los grupos donde estén compartidas, respetando la privacidad del destinatario y evitando reservas duplicadas.

## 2. Requisitos funcionales

### RF-WISH-01 — Creación de listas

El sistema debe permitir que un usuario autenticado cree múltiples listas de deseos.

**Historia de usuario**

Como usuario, quiero crear listas de deseos para organizar los regalos que me gustaría recibir.

**Criterios de aceptación**

- El usuario puede crear una o varias listas.
- Cada lista debe tener un nombre obligatorio.
- El nombre de una lista puede repetirse.
- Cada lista pertenece a un único usuario.
- El usuario puede consultar todas sus listas.
- Las listas nuevas son privadas hasta que su propietario decida compartirlas.
- Cada lista recibe un identificador interno único.

### RF-WISH-02 — Gestión de listas

El sistema debe permitir al propietario editar, archivar y eliminar sus listas de deseos, protegiendo la información asociada a regalos gestionados.

**Historia de usuario**

Como usuario, quiero administrar mis listas de deseos, archivando aquellas que ya no utilizo y eliminando las que no contienen información que deba conservarse.

**Criterios de aceptación**

- El propietario puede modificar el nombre de una lista.
- El propietario puede consultar los deseos asociados a sus listas.
- El propietario puede archivar una lista en cualquier momento.
- Una lista archivada deja de aparecer entre las listas activas.
- El propietario puede consultar y restaurar sus listas archivadas.
- Archivar una lista no cancela reservas ni propuestas existentes.
- Los participantes autorizados conservan acceso a sus organizaciones de regalos activas, sin revelar información al destinatario.
- El propietario puede eliminar definitivamente una lista únicamente cuando no existan reservas ni propuestas activas.
- Si existen reservas o propuestas activas, el sistema informa que la lista puede archivarse, pero no eliminarse definitivamente.
- Los registros históricos de regalos realizados o recibidos se conservan de manera independiente de la lista original.
- La eliminación definitiva requiere confirmación.
- Otros integrantes no pueden editar, archivar ni eliminar listas ajenas.
- Las modificaciones autorizadas se reflejan en todos los grupos donde corresponda.

### RF-WISH-03 — Creación de deseos

El sistema debe permitir agregar deseos a una lista propia.

**Historia de usuario**

Como usuario, quiero registrar los regalos que me gustaría recibir para que mis grupos tengan ideas cuando llegue una ocasión especial.

**Datos propuestos del deseo**

| Campo | Obligatorio | Descripción |
|---|---|---|
| Nombre | Sí | Identificación del regalo deseado |
| Descripción | No | Características o preferencias |
| Precio estimado | No | Valor orientativo |
| Enlace | No | Referencia externa al producto |
| Imagen | No | Referencia visual opcional |
| Lista | Sí | Lista a la que pertenece |

**Criterios de aceptación**

- Un deseo debe tener un nombre.
- La descripción es opcional.
- El precio estimado puede omitirse.
- Cuando se ingresa un precio, debe ser un valor válido no negativo.
- El enlace debe tener un formato válido cuando se proporciona.
- El usuario puede crear deseos únicamente en sus propias listas.
- Los deseos nuevos quedan asociados a la lista seleccionada.
- Los integrantes autorizados pueden consultar los deseos de listas compartidas.

### RF-WISH-04 — Modificación de deseos

El sistema debe permitir al propietario actualizar la información de sus deseos.

**Historia de usuario**

Como usuario, quiero modificar mis deseos cuando cambien mis preferencias o encuentre información más precisa sobre un regalo.

**Criterios de aceptación**

- El propietario puede modificar nombre, descripción, precio estimado y enlace.
- Los cambios se reflejan en todos los grupos autorizados.
- Otros integrantes no pueden modificar deseos ajenos.
- La modificación de un deseo no debe alterar automáticamente las reservas existentes.
- Los cambios se deben validar antes de guardarse.

**Decisión pendiente**

Definir si modificar sustancialmente un deseo que ya fue reservado debe generar un aviso privado para quien lo reservó.

### RF-WISH-05 — Compartición de listas

El sistema debe permitir compartir una lista completa con uno o varios grupos a los que pertenezca su propietario.

**Historia de usuario**

Como usuario, quiero compartir mis listas con distintos grupos para que mis familiares y amigos conozcan mis deseos sin tener que registrarlos nuevamente.

**Criterios de aceptación**

- El propietario puede elegir con qué grupos compartir una lista.
- Solo puede compartirla con grupos donde tenga una membresía activa.
- La lista conserva su identidad original.
- Compartirla no genera copias independientes.
- Los integrantes autorizados ven los deseos asociados.
- El propietario puede consultar con qué grupos compartió cada lista.
- Las modificaciones de la lista se reflejan en todos los grupos autorizados.
- Las reservas de deseos compartidos deben mantenerse sincronizadas globalmente.

### RF-WISH-06 — Consulta de listas compartidas

El sistema debe permitir que los integrantes consulten las listas compartidas por otras personas de sus grupos.

**Historia de usuario**

Como integrante de un grupo, quiero consultar los deseos de otras personas para elegir regalos que realmente les interesen.

**Criterios de aceptación**

- Solo los integrantes autorizados pueden acceder a una lista compartida.
- La interfaz identifica al propietario de la lista.
- Se muestran los deseos y su información pública.
- Se muestra la disponibilidad de los deseos según las reglas de privacidad.
- Un integrante no puede modificar listas ajenas.
- El sistema debe impedir el acceso mediante enlaces directos cuando no exista autorización.
- El acceso debe validarse en el backend.

### RF-WISH-07 — Visibilidad de reservas

El sistema debe mostrar información diferente según si quien consulta una lista es su propietario o un posible regalador.

**Historia de usuario**

Como usuario, quiero que las personas de mis grupos puedan organizar regalos sin que yo descubra cuáles están preparando para mí.

**Criterios de aceptación**

- El propietario puede consultar sus deseos.
- El propietario no puede consultar las reservas, compras o propuestas de regalos destinados a él.
- Los demás integrantes autorizados pueden consultar si un deseo está disponible o reservado.
- Un deseo reservado aparece como no disponible para nuevas reservas individuales en todos los grupos donde se comparte.
- La reserva no revela información de otros grupos al propietario.
- Las restricciones deben aplicarse también a las respuestas de la API.
- El propietario puede marcar un deseo como recibido.

La gestión detallada de reservas y estados se especificará en el módulo RF-05.

### RF-WISH-08 — Retirada de listas compartidas

El sistema debe permitir que el propietario deje de compartir una lista con un grupo.

**Historia de usuario**

Como usuario, quiero controlar qué grupos tienen acceso a mis listas para mantener mis preferencias de privacidad.

**Criterios de aceptación**

- El propietario puede retirar una lista de un grupo.
- El sistema actualiza los permisos de acceso correspondientes.
- Los integrantes que pierdan autorización no podrán consultar la lista mediante enlaces anteriores.
- La lista y sus deseos no se eliminan por dejar de compartirse.
- El acceso desde otros grupos autorizados se mantiene.
- Las reservas existentes no deben eliminarse automáticamente.

**Decisión pendiente**

Definir qué ocurre cuando se retira una lista de un grupo que tiene integrantes con reservas activas.

### RF-WISH-09 — Gestión del estado de los deseos

El sistema debe permitir distinguir deseos pendientes, cumplidos y archivados.

**Historia de usuario**

Como usuario, quiero identificar qué deseos todavía tengo pendientes y cuáles ya recibí para mantener mis listas actualizadas.

**Criterios de aceptación**

- Los deseos nuevos se registran como pendientes.
- Un deseo pendiente puede pasar a cumplido cuando el propietario confirma haberlo recibido.
- Los deseos cumplidos dejan de aparecer como pendientes.
- Los deseos cumplidos pueden consultarse en el historial correspondiente.
- Un deseo puede archivarse sin eliminar su información.
- Los estados relacionados con reservas y compras no deben confundirse con los estados personales del deseo.

**Decisión pendiente**

Definir las condiciones para reactivar deseos cumplidos y el comportamiento de los deseos que pueden regalarse repetidamente.

## 3. Reglas de negocio

- **RN-WISH-01:** cada lista pertenece a un único usuario.
- **RN-WISH-02:** un usuario puede crear múltiples listas.
- **RN-WISH-03:** las listas son privadas por defecto.
- **RN-WISH-04:** únicamente el propietario puede modificar sus listas y deseos.
- **RN-WISH-05:** una lista puede compartirse con varios grupos.
- **RN-WISH-06:** compartir una lista no crea copias independientes.
- **RN-WISH-07:** los deseos mantienen una identidad única independientemente de los grupos donde se consulten.
- **RN-WISH-08:** las reservas se mantienen consistentes entre los grupos autorizados.
- **RN-WISH-09:** un deseo reservado no puede recibir una segunda reserva individual incompatible.
- **RN-WISH-10:** el propietario no puede acceder a información que revele las reservas o propuestas destinadas a él.
- **RN-WISH-11:** el propietario puede confirmar que recibió un deseo.
- **RN-WISH-12:** los deseos recibidos dejan de estar pendientes y pueden consultarse en el historial.
- **RN-WISH-13:** los permisos de acceso dependen de las listas compartidas y de las membresías activas.
- **RN-WISH-14:** retirar una lista de un grupo no debe eliminar automáticamente sus deseos ni su historial.
- **RN-WISH-15:** la validación de permisos debe realizarse en el backend.
- **RN-WISH-16:** archivar una lista la retira de las listas activas e impide nuevas reservas. Las reservas y propuestas existentes se conservan y permanecen accesibles para sus participantes autorizados hasta su resolución.
- **RN-WISH-17:** las listas archivadas pueden ser restauradas por su propietario.
- **RN-WISH-18:** la eliminación definitiva de una lista solo está permitida cuando no existen reservas ni propuestas activas. Los registros históricos de regalos realizados o recibidos deben conservarse independientemente de la lista original.
- **RN-WISH-19:** el archivo de una lista no debe revelar al destinatario información sobre reservas, compras o propuestas de regalos.
- **RN-WISH-20:** cuando el propietario marca un deseo como recibido y existe una reserva o propuesta activa, el sistema registra la recepción sin eliminar automáticamente la organización existente. La conciliación de estados se definirá en el módulo de reservas.

## 4. Fuera del alcance inicial

- Compartición individual de deseos con permisos independientes.
- Listas públicas accesibles sin pertenecer a un grupo.
- Búsqueda automática de productos en tiendas.
- Seguimiento automático de precios.
- Compras y pagos dentro de WishList.
- Recomendaciones de regalos generadas por inteligencia artificial.
- Importación automática de listas desde tiendas externas.

## 5. Decisiones pendientes

### DP-WISH-01 — Comportamiento de listas archivadas

**Estado:** Parcialmente resuelto.

Se confirma que las listas pueden archivarse sin eliminar sus deseos ni cancelar las organizaciones de regalos existentes.

**Resuelto:** archivar una lista impide nuevas reservas y propuestas, pero conserva las organizaciones activas (ver RN-RES-11).

Pendiente de definir:

- Si continúa visible para todos los grupos con los que estaba compartida.
- Qué ocurre con las listas archivadas que contienen regalos colaborativos aún pendientes.

La eliminación definitiva queda restringida a listas sin reservas ni propuestas activas; los registros históricos se conservan de manera independiente de la lista original.

### DP-WISH-02 — Modificaciones de deseos reservados

Definir si los participantes deben recibir algún aviso cuando el propietario cambia información importante de un deseo reservado.

### DP-WISH-03 — Retirada de listas compartidas

Determinar cómo se gestionan las reservas existentes cuando el propietario deja de compartir una lista con un grupo.

### DP-WISH-04 — Deseos repetibles

Definir cómo registrar deseos que pueden regalarse más de una vez y cómo se relacionan con el historial.

### DP-WISH-05 — Imágenes

Definir si las imágenes se almacenarán directamente en WishList o si inicialmente se utilizarán enlaces a imágenes externas.

### DP-WISH-06 — Moneda de los precios

Definir si los precios estimados utilizarán una moneda configurable o una moneda predeterminada.

## 6. Consideraciones técnicas

- Las listas y deseos deben tener identificadores internos únicos.
- La relación entre listas y grupos debe permitir compartir una lista sin duplicar sus registros.
- Las consultas deben validar tanto la autorización del usuario como la membresía actual en el grupo.
- Las reservas deben tener una fuente de verdad única para evitar inconsistencias entre grupos.
- Las restricciones de privacidad deben aplicarse en el backend, no únicamente en la interfaz.
- La eliminación de listas y deseos debe considerar las dependencias con reservas e historial.
- Los campos opcionales deben validarse cuando se proporcionen.
- Las operaciones concurrentes sobre reservas deben resolverse en el módulo correspondiente.

---

**Estado:** propuesta funcional pendiente de revisión de decisiones abiertas.