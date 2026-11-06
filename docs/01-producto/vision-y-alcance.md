# WishList — Visión y alcance del producto

**Versión:** 0.1  
**Estado:** Borrador para revisión  
**Tipo:** Aplicación de organización colaborativa de regalos

## 1. Visión

WishList es una aplicación que permite a personas organizar sus deseos de regalos y coordinar obsequios dentro de grupos de familiares, amigos o personas cercanas.

Busca facilitar la elección de regalos, evitar duplicaciones, mantener las sorpresas y conservar un historial de los regalos realizados.

El producto se validará inicialmente con un grupo familiar, con la intención de evolucionar hacia una aplicación disponible para cualquier persona.

## 2. Problema que resuelve

Organizar regalos para fechas especiales presenta dificultades frecuentes:

- Desconocer qué desea o necesita recibir una persona.
- No saber qué presupuesto considerar.
- Comprar accidentalmente el mismo regalo que otra persona.
- Tener dificultades para coordinar regalos de mayor valor entre varios participantes.
- Olvidar fechas importantes.
- Repetir regalos por no recordar qué se ha obsequiado anteriormente.

Actualmente, estas situaciones suelen resolverse mediante conversaciones informales, listas dispersas o coordinación por mensajería.

## 3. Propuesta de valor

WishList centraliza la información necesaria para organizar regalos, permitiendo que los integrantes de un grupo:

- Consulten deseos y presupuestos estimados.
- Reserven regalos sin revelar la sorpresa al destinatario.
- Organicen regalos individuales o colaborativos.
- Compartan deseos pendientes entre distintos grupos.
- Consulten fechas y ocasiones especiales.
- Registren regalos realizados y revisen su historial personal.

## 4. Usuarios objetivo

Personas que desean organizar regalos para familiares, amistades, parejas u otros grupos cercanos.

La primera validación se realizará con integrantes de un grupo familiar.

## 5. Alcance funcional inicial

### Cuentas y grupos
- Registro e inicio de sesión de usuarios.
- Creación y participación en múltiples grupos.
- Incorporación mediante invitaciones y aprobación por mayoría de integrantes.
- Gestión de miembros y permisos.

### Listas de deseos
- Creación, edición y eliminación de deseos.
- Registro de descripciones, presupuestos estimados y enlaces opcionales.
- Compartición opcional de deseos pendientes entre grupos.
- Visualización coordinada de reservas sobre un mismo deseo.

### Ocasiones especiales
- Registro de cumpleaños.
- Creación de ocasiones adicionales, como Navidad o aniversarios.
- Consulta de próximas fechas importantes.

### Organización de regalos
- Reserva individual de deseos.
- Prevención de reservas duplicadas.
- Organización de regalos colaborativos.
- Registro de estados: reservado, comprado y entregado.
- Protección de la información que pueda revelar una sorpresa.

### Historial
- Consulta personal de regalos realizados y recibidos.
- Asociación de regalos con destinatarios y ocasiones.
- Registro de regalos a partir de deseos existentes en WishList; el registro manual de regalos ajenos a las listas queda fuera del alcance inicial.

## 6. Fuera del alcance inicial

WishList no incluirá inicialmente:

- Procesamiento de pagos o transferencias.
- Compras realizadas dentro de la aplicación.
- Gestión de envíos o entregas físicas.
- Chat interno.
- Registro de destinatarios externos a la aplicación.
- Recomendaciones automáticas mediante inteligencia artificial.
- Integraciones comerciales con tiendas.
- Funciones de monetización.

La coordinación conversacional podrá realizarse externamente, por ejemplo, mediante WhatsApp.

## 7. Principios del producto

**Privacidad:** la persona destinataria no podrá consultar las reservas ni las propuestas de regalos destinadas a ella.

**Colaboración:** las decisiones de incorporación a grupos se basarán en participación y mayoría, evitando depender exclusivamente de un administrador.

**Simplicidad:** las funcionalidades deben facilitar la organización sin introducir procesos innecesarios.

**Consistencia:** las reservas y estados deberán mantenerse sincronizados entre los grupos donde se comparta un deseo.

**Evolución:** el diseño debe permitir ampliar funcionalidades sin requerir una reconstrucción completa.

## 8. Criterios iniciales de éxito

- La validación familiar buscará comprobar que los usuarios pueden:
- Crear una cuenta y participar en un grupo.
- Registrar y consultar deseos.
- Identificar próximas ocasiones especiales.
- Reservar regalos evitando duplicaciones.
- Coordinar regalos entre varias personas.
- Mantener las sorpresas.
- Consultar un historial de regalos realizados.

La facilidad de uso y la utilidad percibida se evaluarán durante las pruebas con usuarios reales.

## 9. Decisiones pendientes

- Reglas de aprobación, rechazo y retirada de integrantes de grupos.
- Permisos específicos de cada operación.
- Condiciones para cancelar o finalizar reservas.
- Gestión de regalos colaborativos y cambios de participantes.
- Comportamiento de deseos repetibles y deseos cumplidos.
- Política de visibilidad del historial de regalos.
- Mecanismos de recordatorios y notificaciones.
- Selección definitiva del stack y estrategia de despliegue.

**Nota:** Este documento establece la visión y el alcance inicial. Las reglas detalladas, los casos excepcionales y los criterios de aceptación se definirán en documentos posteriores.