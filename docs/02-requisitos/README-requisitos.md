# WishList — Requisitos funcionales

**Versión:** 0.1  
**Estado:** Primera ronda de especificaciones completada; pendiente auditoría transversal.

## 1. Propósito

Este directorio contiene las especificaciones funcionales de WishList, una aplicación para organizar listas de deseos y coordinar regalos entre integrantes de grupos privados.

Cada módulo documenta:

- Objetivo y alcance funcional.
- Requisitos e historias de usuario.
- Criterios de aceptación verificables.
- Reglas de negocio.
- Exclusiones del MVP.
- Decisiones pendientes y dependencias.

La visión general del producto se encuentra en [Visión y alcance](../01-producto/vision-y-alcance.md).

## 2. Índice de módulos

| Módulo | Descripción | Estado |
|---|---|---|
| [RF-01 — Usuarios y autenticación](01-usuarios-autenticacion.md) | Registro, verificación de correo, identidad, perfiles y seguridad. | Aprobado funcionalmente |
| [RF-02 — Grupos y membresías](02-grupos-membresias.md) | Creación de grupos, invitaciones, votaciones y permisos. | Parcialmente aprobado |
| [RF-03 — Listas de deseos](03-deseos-listas.md) | Creación, compartición, visibilidad y gestión de listas. | Parcialmente aprobado |
| [RF-04 — Ocasiones especiales](04-ocasiones.md) | Cumpleaños, celebraciones y recordatorios. | Aprobado funcionalmente |
| [RF-05 — Reservas de regalos](05-reservas.md) | Reservas, disponibilidad global, estados y conciliación. | Parcialmente aprobado |
| [RF-06 — Regalos colaborativos](06-regalos-colaborativos.md) | Propuestas, votaciones, participantes y aportes estimados. | Parcialmente aprobado |
| [RF-07 — Historial de regalos](07-historial.md) | Historial personal de regalos recibidos y realizados. | Parcialmente aprobado |

Los estados corresponden a la revisión funcional realizada hasta el momento. No representan funcionalidades implementadas.

## 3. Convenciones

### Identificadores

Cada módulo utiliza un prefijo para sus requisitos y reglas:

| Módulo | Requisitos | Reglas |
|---|---|---|
| Autenticación | RF-AUTH | RN-AUTH |
| Grupos | RF-GRP | RN-GRP |
| Listas de deseos | RF-WISH | RN-WISH |
| Ocasiones | RF-OCC | RN-OCC |
| Reservas | RF-RES | RN-RES |
| Regalos colaborativos | RF-COL | RN-COL |
| Historial | RF-HIST | RN-HIST |

Se utiliza `DP` para decisiones pendientes y `RP` para reglas propuestas, cuando corresponda.

Los identificadores existentes no deben reutilizarse para requisitos diferentes.

### Estados de documentación

- **Propuesta:** pendiente de revisión funcional.
- **Parcialmente aprobado:** contiene reglas confirmadas y decisiones pendientes.
- **Aprobado funcionalmente:** las decisiones principales están confirmadas; pueden existir excepciones documentadas.
- **Validado transversalmente:** se revisó su consistencia con los demás módulos.

## 4. Principios funcionales

Las especificaciones deben respetar las siguientes decisiones:

1. WishList organiza regalos, pero no procesa compras ni pagos.
2. Las listas pertenecen a sus usuarios y pueden compartirse entre grupos.
3. Las reservas mantienen una disponibilidad global por deseo.
4. El destinatario no puede consultar la organización privada de sus regalos.
5. La gobernanza de los grupos es horizontal y utiliza votaciones.
6. Los regalos colaborativos permiten aportes estimados, no transferencias.
7. El historial diferencia regalos recibidos de regalos realizados.
8. Las operaciones protegidas requieren autorización en el backend.

## 5. Alcance y control de cambios

Los documentos describen el comportamiento esperado del producto, independientemente de la tecnología utilizada para implementarlo.

Cuando se modifique una regla:

1. Actualizar el requisito o regla correspondiente.
2. Revisar sus dependencias con otros módulos.
3. Actualizar las decisiones pendientes afectadas.
4. Ajustar la versión y el estado del documento cuando corresponda.

Las decisiones todavía abiertas no deben interpretarse como funcionalidades aprobadas.

## 6. Próximos pasos

- Realizar auditoría transversal de los siete módulos.
- Consolidar las reglas de privacidad y autorización.
- Identificar las decisiones que bloquean el MVP.
- Documentar los flujos principales en `docs/03-flujos/`.
- Diseñar wireframes y validar la experiencia de usuario.
- Definir la arquitectura técnica antes de implementar.

---

**Nota:** este README es un índice y una referencia de organización. Las especificaciones detalladas se mantienen en los documentos de cada módulo.
