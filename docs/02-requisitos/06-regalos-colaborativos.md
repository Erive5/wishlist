# RF-06 — Regalos colaborativos

**Producto:** WishList  
**Versión:** 0.1
**Estado:** Aprobado parcialmente; casos excepcionales pendientes  
**Prioridad:** MVP

## 1. Objetivo

Permitir que integrantes de un mismo grupo organicen conjuntamente un regalo para otra persona, manteniendo la sorpresa, evitando reservas duplicadas entre grupos y registrando compromisos estimados sin gestionar pagos.

WishList es una herramienta de **organización**, no de compra, recaudación, transferencia ni entrega física de regalos.

## 2. Conceptos

- **Deseo:** regalo solicitado por una persona en una lista compartida (RF-03).
- **Propuesta colaborativa:** organización de un regalo en la que pueden participar varias personas; bloquea globalmente el deseo frente a reservas u organizaciones incompatibles.
- **Grupo organizador:** grupo único en el que se gestiona la propuesta durante el MVP. No limita la disponibilidad global del deseo.
- **Organizador:** integrante que crea la propuesta o convierte una reserva individual en colaborativa. Es el responsable inicial de su coordinación.
- **Participante:** usuario admitido a la propuesta mediante votación. El organizador es el primer participante.
- **Solicitante:** integrante del grupo organizador que pidió unirse y aún no ha sido aceptado.
- **Aporte estimado:** cantidad opcional que un participante declara que pretende aportar. No es un pago ni dinero recaudado.
- **Destinatario:** propietario del deseo; no puede consultar la propuesta ni su organización privada.

## 3. Requisitos funcionales

### RF-COL-01 — Crear propuesta colaborativa

**Historia de usuario**

Como integrante de un grupo, quiero crear una propuesta conjunta sobre un deseo disponible para organizar un regalo entre varias personas.

**Criterios de aceptación**

- El creador está autenticado, tiene correo verificado y pertenece activamente al grupo organizador.
- El deseo debe ser accesible desde una lista compartida con ese grupo, estar pendiente y no tener una organización incompatible activa.
- Nadie puede crear una propuesta para un deseo propio.
- La propuesta registra deseo original, grupo organizador, organizador, fecha y ocasión opcional.
- El organizador queda incorporado como primer participante.
- Al crearla, el deseo queda reservado globalmente, también en los demás grupos que ven la misma lista.
- Un intento simultáneo de reservar o proponer el mismo deseo no puede producir dos organizaciones incompatibles.
- Los integrantes de otros grupos solo ven que el deseo está ocupado; no acceden a la propuesta ni a sus participantes.

### RF-COL-02 — Convertir reserva individual en colaborativa

**Historia de usuario**

Como responsable de una reserva individual, quiero convertirla en una propuesta grupal sin liberar el deseo para poder sumar participantes.

**Criterios de aceptación**

- Solo el responsable de la reserva puede iniciar la conversión, mientras conserve autorización sobre el deseo.
- Debe seleccionar un grupo activo al que pertenezca y con el que la lista esté compartida.
- La conversión mantiene el bloqueo global del deseo sin intervalo de disponibilidad.
- La operación conserva la trazabilidad de la reserva individual anterior.
- El responsable pasa a ser organizador y primer participante.
- La conversión se realiza de forma atómica; si falla, la reserva original permanece vigente.
- El tratamiento de reservas ya compradas o entregadas al convertirlas queda pendiente de definición.

### RF-COL-03 — Solicitar participación

**Historia de usuario**

Como integrante del grupo organizador, quiero solicitar unirme a una propuesta para participar en la organización del regalo.

**Criterios de aceptación**

- Solo un integrante activo del grupo organizador y autorizado a ver el deseo puede solicitar ingreso.
- El destinatario no puede solicitar ingreso ni ver la propuesta.
- La solicitud inicia en estado pendiente y no concede acceso a la información privada de coordinación.
- Un usuario no puede tener dos solicitudes pendientes simultáneas para la misma propuesta.
- El solicitante puede retirar su solicitud antes de que se decida.
- Una solicitud rechazada puede presentarse nuevamente transcurridas 24 horas desde su rechazo, mediante una solicitud nueva con votación nueva.
- Los votos de solicitudes anteriores no se reutilizan.
- Las condiciones para limitar solicitudes reiteradas más allá del período de espera quedan pendientes.

### RF-COL-04 — Votar incorporación de participantes

**Historia de usuario**

Como participante de una propuesta, quiero votar solicitudes de incorporación para decidir colectivamente quién colabora.

**Criterios de aceptación**

- Cada participante actual dispone de un voto por solicitud; puede votar a favor o en contra.
- La aprobación requiere mayoría absoluta de los participantes actuales habilitados para esa votación.
- Si solo está el organizador, su voto favorable es suficiente.
- El solicitante no participa en su propia votación.
- Al aprobarse, el solicitante se incorpora como participante de la propuesta.
- Si ya no es posible alcanzar la mayoría favorable con los votos restantes, la solicitud se rechaza.
- El resultado de una votación resuelta no cambia por votos posteriores.
- El backend impide votos duplicados y resuelve de forma consistente las acciones concurrentes.
- Está pendiente definir cómo se tratan solicitudes sin respuesta y cambios de participantes mientras una votación está abierta.

### RF-COL-05 — Declarar aportes estimados

**Historia de usuario**

Como participante, quiero registrar opcionalmente cuánto pretendo aportar para facilitar la coordinación fuera de WishList.

**Criterios de aceptación**

- Participar no exige declarar un importe.
- Cada participante puede registrar o actualizar su propio aporte estimado.
- Los importes ingresados deben ser válidos y no negativos.
- Los participantes autorizados pueden consultar aportes y suma estimada de compromisos.
- La interfaz distingue claramente **compromisos estimados** de dinero efectivamente reunido.
- WishList no recibe pagos, registra datos bancarios ni verifica transferencias.
- La moneda se ajustará a la decisión que se adopte en RF-03 para precios estimados.
- El tratamiento de los aportes ante cambios excepcionales de participantes queda pendiente.

### RF-COL-06 — Compartir coordinación mediante WhatsApp

**Historia de usuario**

Como participante, quiero compartir un mensaje y un enlace a la propuesta para conversar sobre el regalo fuera de WishList.

**Criterios de aceptación**

- La aplicación permite compartir o copiar un mensaje resumido y un enlace.
- El enlace no concede acceso por sí mismo: exige autenticación y autorización.
- El contenido puede revelar el regalo; la interfaz advierte que no debe compartirse con el destinatario.
- Los usuarios sin autorización no acceden a detalles privados al abrir el enlace.
- WishList no incorpora chat ni lee o sincroniza mensajes de WhatsApp.

### RF-COL-07 — Seguimiento y cierre de propuesta

**Historia de usuario**

Como organizador, quiero actualizar la situación de una propuesta y cerrarla al finalizar o cancelar la organización.

**Criterios de aceptación**

- El organizador registra los cambios de estado de compra y entrega de la propuesta, conforme a las reglas comunes de RF-05.
- Una propuesta comprada sigue bloqueando el deseo.
- Si se cierra sin entrega y el deseo sigue pendiente, se libera el bloqueo si no existen otras restricciones (sujeto a RP-COL-02).
- El cierre no elimina la trazabilidad ni convierte los aportes en pagos confirmados.
- Cada participante confirma individualmente si efectivamente colaboró en el regalo. Una reserva, una solicitud de ingreso aprobada o un aporte estimado no prueban por sí solos una entrega, y la confirmación de participación no representa un pago verificado.
- Cuando el destinatario marca el deseo como recibido, se aplica la conciliación privada de RF-05.
- La recepción no atribuye automáticamente la entrega a todos los participantes.
- Un participante aparece en su historial de regalos realizados solo cuando confirmó su colaboración y la entrega quedó conciliada con la recepción.
- El destinatario no puede consultar los participantes ni sus confirmaciones antes de la recepción.
- Las correcciones de declaraciones erróneas y demás casos excepcionales quedan pendientes de definición.

### RF-COL-08 — Privacidad y disponibilidad entre grupos

**Historia de usuario**

Como destinatario, quiero mantener la sorpresa; como integrante de otro grupo, quiero saber si un deseo ya está reservado para evitar compras duplicadas.

**Criterios de aceptación**

- La propuesta pertenece a un único grupo organizador en el MVP; no se permite colaboración entre grupos diferentes.
- Los integrantes del grupo organizador que no participan pueden saber que existe una propuesta colaborativa asociada al deseo, solicitar incorporarse y consultar el estado de su solicitud.
- Antes de ser aceptados, no pueden consultar aportes individuales, conversaciones ni detalles internos de coordinación.
- Solo integrantes autorizados de ese grupo, según su participación y estado de solicitud, acceden a la información privada correspondiente.
- Integrantes de otros grupos donde esté compartida la lista ven el deseo como no disponible, sin acceder a la propuesta.
- El destinatario no ve la existencia, participantes, votaciones, aportes ni estados de compra o entrega de propuestas dirigidas a él.
- La API y los enlaces directos aplican los mismos controles de acceso que la interfaz.
- La propuesta no crea copias del deseo por grupo.

## 4. Reglas de negocio confirmadas

- **RN-COL-01:** una reserva individual puede convertirse en colaborativa sin liberar el deseo durante la conversión.
- **RN-COL-02:** cada propuesta colaborativa se organiza dentro de un único grupo durante el MVP.
- **RN-COL-03:** el bloqueo del deseo es global y se refleja en todos los grupos que comparten su lista.
- **RN-COL-04:** la incorporación de nuevos participantes requiere una solicitud y votación de participantes actuales.
- **RN-COL-05:** cada participante puede declarar un aporte estimado opcional, sin que WishList gestione dinero real.
- **RN-COL-06:** WishList no procesa pagos, compras ni transferencias; la coordinación conversacional puede realizarse por WhatsApp.
- **RN-COL-07:** la incorporación se aprueba por mayoría absoluta de participantes actuales habilitados.
- **RN-COL-08:** una solicitud rechazada puede volver a presentarse.
- **RN-COL-09:** no puede existir más de una solicitud pendiente por usuario y propuesta.
- **RN-COL-10:** cada nueva solicitud inicia una votación independiente, sin reutilizar votos.
- **RN-COL-11:** se deben prevenir solicitudes reiteradas abusivas; el mecanismo y eventual espera quedan por definir.
- **RN-COL-12:** el destinatario no participa en una propuesta para su propio deseo ni conoce su organización privada.
- **RN-COL-13:** las reservas y propuestas incompatibles sobre un deseo deben impedirse incluso ante solicitudes concurrentes.
- **RN-COL-14:** las solicitudes pendientes pueden retirarse antes de su resolución.

## 5. Reglas propuestas pendientes de aprobación

- **RP-COL-01 — Resuelta:** el organizador registra las declaraciones de compra y entrega de la propuesta; la atribución a cada participante requiere su confirmación individual de colaboración, sin que ello represente un pago verificado.
- **RP-COL-02:** al cerrar una propuesta sin entrega, su bloqueo se libera únicamente cuando el deseo sigue pendiente y no existen otras restricciones.
- **RP-COL-03:** los cambios de integrantes habilitados durante una votación pendiente provocarán un recálculo o resolución según una regla de consistencia por definir.

## 6. Fuera del alcance del MVP

- Participación en una misma propuesta de personas provenientes de grupos organizadores distintos (aunque el estado del deseo sí se sincroniza globalmente).
- **Salida voluntaria de participantes ya incorporados** a una propuesta colaborativa.
- Redistribución automática de compromisos estimados al cambiar los participantes.
- Reembolsos, devoluciones, compensaciones, cobros, recaudación o verificación de pagos.
- Chat interno y sincronización con mensajes de WhatsApp.
- Compras o entregas físicas dentro de WishList.
- Sorteos y recomendaciones de regalos con inteligencia artificial.

**Límite explícito:** una solicitud pendiente sí puede retirarse; una participación ya aprobada no tendrá flujo de salida voluntaria durante el MVP. Esto no elimina la necesidad de tratar las pérdidas involuntarias de acceso.

## 7. Casos pendientes y dependencias

### DP-COL-01 — Pérdida de membresía, eliminación o inactividad

**Estado:** Pendiente; debe resolverse antes de implementar los casos excepcionales.

- ¿Qué sucede si un participante es retirado del grupo, elimina su cuenta o queda inactivo?
- ¿Cómo se preservan los compromisos ya declarados sin mantener acceso privado al grupo?
- ¿Qué ocurre si el organizador es la persona afectada?
- ¿Quién puede continuar o cancelar una propuesta y mediante qué autorización?
- No se debe aplicar automáticamente a toda la propuesta la liberación de reservas individuales prevista en RF-05.

### DP-COL-02 — Votaciones y reiteración de solicitudes

- **Parcialmente resuelto:** una solicitud rechazada puede volver a presentarse transcurridas 24 horas.
- Definir un límite razonable de solicitudes repetidas adicional al período de espera, si fuera necesario.
- Definir manejo de solicitudes sin respuesta.
- Definir qué ocurre si cambia la composición de los votantes antes de decidir.

### DP-COL-03 — Compras, entregas, conciliación e historial

**Parcialmente resuelto:** el organizador registra las declaraciones de compra y entrega; cada participante confirma individualmente su colaboración y solo entonces, tras la conciliación de RF-05, puede atribuirse el regalo en su historial. La reserva, la participación aprobada o el aporte estimado no prueban por sí solos una entrega.

Pendiente de definir:
- Quién puede cancelar una propuesta y mediante qué autorización.
- Correcciones de declaraciones erróneas.
- El cierre de una propuesta sin entrega y su impacto en el historial (sujeto a RP-COL-02).

### DP-COL-04 — Conversión de reservas ya compradas o entregadas

- Definir si una reserva individual en estado comprado o entregado puede convertirse a colaborativa y cómo conservar su trazabilidad.

### DP-COL-05 — Propuestas sin nuevos participantes

- Definir si una propuesta puede continuar con el organizador como único participante y si puede volver a modalidad individual.

### DP-COL-06 — Moneda y cambios de aportes

- Vincular la moneda al criterio finalmente adoptado en RF-03.
- Precisar cómo se presentan los cambios en aportes estimados sin insinuar cobros ni pagos reales.

### DP-COL-07 — Salida voluntaria de participantes

**Estado:** Diferido a una versión posterior.

- El MVP no permitirá abandonar voluntariamente una propuesta después de la aprobación de ingreso.
- Se mantiene la posibilidad de retirar solicitudes pendientes.
- Una versión posterior podrá evaluar retirada voluntaria, redistribución de aportes y continuidad de la propuesta.
- La pérdida de membresía o eliminación de cuenta **no** se considera una salida voluntaria y debe atenderse mediante DP-COL-01.

## 8. Consideraciones técnicas y pruebas

- Una propuesta y una reserva individual deben respetar la **misma exclusividad global** sobre el deseo.
- Crear y convertir organizaciones, aprobar ingresos y actualizar estados deben ser operaciones consistentes y, cuando corresponda, transaccionales.
- La identidad se basa en identificadores internos de usuarios y grupos, no en nombres o apodos.
- La autorización se valida en el backend ante cada operación; compartir un enlace no concede acceso.
- Las votaciones deben impedir votos duplicados y resultados contradictorios ante concurrencia.
- Los cambios relevantes requieren trazabilidad sin exponer la sorpresa al destinatario.
- Pruebas prioritarias: conversión sin liberar bloqueo; intento de duplicación entre grupos; solicitudes duplicadas; mayoría de votos; nueva solicitud tras rechazo; destinatario sin acceso; enlace compartido no autorizado; registros de aportes opcionales; pérdidas involuntarias de acceso.

## 9. Dependencias documentales

- **RF-01:** autenticación, perfiles e identidad.
- **RF-02:** grupos, membresías y gobernanza.
- **RF-03:** listas compartidas, deseos y archivo.
- **RF-04:** ocasiones especiales.
- **RF-05:** reservas, bloqueo global, estados y conciliación.
- **RF-07:** historial personal de regalos realizados y recibidos.

---

**Estado del documento:** reglas principales aprobadas; la salida voluntaria de participantes queda fuera del MVP. La atribución en el historial está definida (confirmación individual de participantes más conciliación de RF-05). Permanecen pendientes los casos excepcionales, la cancelación de propuestas y las correcciones de declaraciones.
