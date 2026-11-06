# RF-07 — Historial de regalos

**Producto:** WishList  
**Versión:** 0.1 
**Estado:** Aprobado parcialmente; decisiones del MVP confirmadas y excepciones pendientes  
**Prioridad:** MVP

## 1. Objetivo

Permitir que cada usuario consulte los deseos registrados en WishList que confirmó haber recibido y los regalos cuya entrega se le atribuyó de manera válida, para recordar obsequios anteriores y evitar repeticiones no deseadas.

El MVP construirá el historial exclusivamente a partir de deseos existentes en WishList. No permitirá registrar manualmente regalos que nunca estuvieron en una lista. WishList no verifica pagos, compras ni entregas físicas: trabaja con declaraciones de sus usuarios.

## 2. Conceptos y límites

- **Historial de recibidos:** deseos de WishList cuya recepción confirmó su propietario, con independencia de que exista una reserva o se conozca al regalador.
- **Historial de realizados:** regalos originados en deseos de WishList cuya entrega o participación se atribuyó a un usuario tras la confirmación prevista en RF-05 y, cuando corresponda, RF-06.
- **Reserva:** intención de regalar; no equivale a una entrega.
- **Compra declarada:** dato de organización; no equivale a una entrega.
- **Conciliación:** resolución privada de la atribución de una reserva cuando el destinatario confirma la recepción.
- **Regalo colaborativo:** puede tener varios participantes; cada uno confirma individualmente su colaboración y el regalo se le atribuye solo tras la conciliación con la recepción, conforme a RF-06.

Cada persona consulta únicamente sus propios historiales. Los registros históricos no deben revelar reservas ni coordinaciones secretas a quien recibirá el regalo.

## 3. Requisitos funcionales

### RF-HIS-01 — Consultar regalos recibidos

El sistema debe permitir consultar los deseos cuya recepción confirmó el propietario.

**Historia de usuario**

Como usuario, quiero revisar los regalos que recibí para recordar obsequios anteriores y gestionar mis deseos.

**Criterios de aceptación**

- Solo el destinatario puede consultar su historial personal de recibidos.
- El historial incluye únicamente deseos existentes en WishList confirmados como recibidos.
- Cada recepción presenta, al menos, nombre del regalo y fecha de recepción declarada.
- Puede asociarse a una ocasión registrada, cuando corresponda.
- La recepción puede registrarse aunque no exista una reserva previa.
- Compartir la lista con varios grupos no duplica una misma recepción.
- La vista no muestra automáticamente quién reservó, compró o coordinó el regalo.
- Los registros permanecen consultables si la lista se archiva o se elimina conforme a RF-03.

### RF-HIS-02 — Consultar regalos realizados

El sistema debe permitir consultar las entregas que fueron atribuidas válidamente al usuario y que se originaron en deseos de WishList.

**Historia de usuario**

Como usuario, quiero revisar qué he regalado a cada persona para no repetir innecesariamente los mismos obsequios.

**Criterios de aceptación**

- El usuario solo puede consultar su propio historial de regalos realizados.
- Cada registro indica destinatario, regalo y fecha de entrega declarada o confirmada.
- Puede vincularse con una ocasión existente.
- El usuario puede consultar sus registros agrupados o filtrados por destinatario.
- Las reservas sin entrega y las compras no entregadas no aparecen como regalos realizados.
- Una recepción de deseo sin entrega atribuida no genera registro en el historial de regalos realizados; WishList no inventa una atribución.
- Las entregas colaborativas pueden atribuirse a varios participantes cuando se cumplan las reglas de RF-06.
- La consulta no expone historiales privados de terceros.

### RF-HIS-03 — Registrar recepción desde un deseo

El sistema debe generar un registro de recepción cuando el propietario confirma que recibió un deseo de WishList.

**Historia de usuario**

Como destinatario, quiero marcar como recibido un deseo para actualizar mi lista y conservar un registro de lo que recibí.

**Criterios de aceptación**

- Solo el propietario del deseo puede confirmar su recepción.
- El deseo pasa a cumplido conforme a RF-03 y RF-05, con las excepciones que se definan para deseos repetibles.
- Se registra un evento de recepción sin duplicaciones por grupos compartidos ni reintentos de la misma operación.
- Si no existe reserva, el registro puede conservarse sin regalador identificado.
- Si hay una reserva activa, comienza la conciliación privada prevista en RF-05.
- Confirmar la recepción no atribuye automáticamente la entrega al responsable de la reserva.
- La operación no revela identidades de quienes reservaron ni detalles ocultos de coordinación.

### RF-HIS-04 — Atribuir regalos realizados mediante conciliación

El sistema debe reflejar en el historial de realizados únicamente entregas atribuidas mediante una confirmación válida conforme a RF-05.

**Historia de usuario**

Como responsable de una reserva, quiero confirmar si efectivamente entregué el regalo para que mi historial sea correcto.

**Criterios de aceptación**

- Cuando el destinatario confirma una recepción con reserva activa, su responsable recibe una solicitud privada de conciliación.
- Si el responsable confirma su participación en la entrega, el regalo puede incorporarse a su historial de realizados.
- Si declara que no lo entregó, el regalo no se le atribuye.
- Una compra declarada no se convierte automáticamente en regalo realizado.
- La conciliación pendiente no revierte la recepción confirmada.
- Una misma entrega no puede duplicarse por cambios de lista, grupo o reintentos.
- En regalos colaborativos, cada participante confirma individualmente su colaboración y el regalo se le atribuye solo tras la conciliación con la recepción, conforme a RF-06.

### RF-HIS-05 — Conservar registros históricos

El sistema debe preservar la consistencia de los registros históricos ante cambios en listas y membresías.

**Historia de usuario**

Como usuario, quiero conservar mi historial aunque archive listas o deje de pertenecer a un grupo, para no perder mis registros anteriores.

**Criterios de aceptación**

- Archivar una lista no elimina sus registros históricos.
- Eliminar una lista permitida por RF-03 no elimina los datos mínimos de entregas y recepciones que corresponda conservar.
- Abandonar un grupo no concede acceso al historial de otras personas.
- La eliminación de cuentas y la retención de datos personales se resolverán conforme a RF-01 y la política aplicable.
- El backend comprueba la autorización en todas las operaciones y consultas de historial.

## 4. Reglas de negocio confirmadas

- **RN-HIS-01:** el historial de regalos realizados es personal y no puede consultarse como historial global de otra persona.
- **RN-HIS-02:** el historial de recibidos pertenece al destinatario y no expone automáticamente información de reservas o coordinaciones.
- **RN-HIS-03:** una reserva o compra declarada no equivale a un regalo realizado.
- **RN-HIS-04:** el destinatario puede confirmar un deseo como recibido sin identificar al regalador.
- **RN-HIS-05:** la atribución de un regalo realizado requiere la confirmación válida prevista en RF-05 y, cuando corresponda, RF-06.
- **RN-HIS-06:** una recepción con reserva activa inicia conciliación privada y no se revierte por su resultado.
- **RN-HIS-07:** el MVP solo genera registros históricos a partir de deseos previamente registrados en WishList.
- **RN-HIS-08:** el MVP no permite registrar manualmente regalos que nunca estuvieron en una wishlist, aunque el destinatario sea usuario de WishList.
- **RN-HIS-09:** el historial no admite destinatarios externos a WishList.
- **RN-HIS-10:** compartir una lista con varios grupos no duplica sus registros de recepción o entrega.
- **RN-HIS-11:** los pagos, compras y entregas reales ocurren fuera de WishList.

## 5. Funcionalidad diferida a una versión posterior

### Registro manual de regalos fuera de listas

**Fuera del MVP.** En una versión posterior podría permitirse registrar regalos que nunca aparecieron como deseos, exclusivamente entre personas con cuenta en WishList.

**Decisión funcional confirmada para esa versión:** un regalo realizado creado manualmente requerirá confirmación del destinatario antes de quedar atribuido como entrega confirmada. Una declaración unilateral del regalador no modificará el historial del destinatario.

La implementación futura deberá especificar solicitudes de confirmación, rechazo, corrección y notificaciones. Esta decisión **no modifica** el flujo de conciliación de reservas existentes establecido en RF-05.

## 6. Decisiones pendientes

### DP-HIS-01 — Atribución de regalos colaborativos

**Resuelto:** cada participante de una propuesta colaborativa confirma individualmente si colaboró en el regalo; solo quien confirmó y cuya entrega quedó conciliada con la recepción aparece en su historial de realizados. La reserva, la participación aprobada o el aporte estimado no prueban por sí solos una entrega. El destinatario no puede consultar participantes ni confirmaciones antes de la recepción.

### DP-HIS-02 — Correcciones de historial

Definir cómo corregir una fecha, descripción o atribución errónea y conservar trazabilidad sin duplicar eventos.

### DP-HIS-03 — Deseos repetibles

Definir cómo registrar recepciones distintas de un mismo deseo repetible, sin confundirlas con reintentos de una misma confirmación.

### DP-HIS-04 — Eliminación de cuentas

Definir qué datos deben eliminarse, anonimizarse o conservarse legítimamente cuando una persona elimina su cuenta. Dependencia de RF-01.

### DP-HIS-05 — Recepción sin regalador identificado

El MVP permite registrar la recepción de un deseo sin identificar al regalador. Queda pendiente definir si más adelante el destinatario podrá asociar voluntariamente a un usuario registrado sin acceder a reservas privadas.

## 7. Fuera del alcance inicial

- Registro manual de regalos que no figuran previamente en una wishlist.
- Registro de regalos recibidos que no corresponden a deseos existentes.
- Solicitudes de confirmación de regalos manuales externos a listas.
- Registro de destinatarios que no tengan cuenta en WishList.
- Historial público o consulta global de regalos hechos a otras personas.
- Verificación automática de pagos, compras o entregas.
- Seguimiento de precios históricos o comprobantes.
- Recomendaciones automáticas mediante IA basadas en el historial.
- Estadísticas avanzadas de gasto por persona u ocasión.

## 8. Consideraciones técnicas

- Separar los eventos de recepción, las reservas y las atribuciones de entrega.
- Utilizar identificadores internos, nunca nombres o apodos, como claves de relación.
- Preservar los datos históricos mínimos necesarios cuando una lista deja de existir, sujeto a las políticas de retención aplicables.
- Validar identidad, autorización y privacidad en cada operación del backend.
- Garantizar idempotencia para evitar duplicados ante reintentos.
- No tratar automáticamente una reserva comprada como entrega confirmada.
- Los futuros registros manuales deberán diseñarse separadamente; no deben introducirse campos o flujos innecesarios en el MVP.

## 9. Dependencias documentales

- **RF-01:** identidad, privacidad y eliminación de cuentas.
- **RF-02:** grupos y membresías.
- **RF-03:** estados de deseos y ciclo de vida de listas.
- **RF-04:** ocasiones especiales.
- **RF-05:** reservas, estados, entrega y conciliación.
- **RF-06:** regalos colaborativos y atribución de participantes.

---

**Estado del documento:** las decisiones sobre registros manuales, alcance del historial del MVP y atribución de regalos colaborativos están confirmadas. Permanecen pendientes las correcciones, los deseos repetibles y las políticas de eliminación de cuentas.
