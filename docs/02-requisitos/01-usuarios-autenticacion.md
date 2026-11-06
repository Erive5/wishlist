# RF-01 — Usuarios y autenticación

**Producto:** WishList  
**Versión:** 0.3  
**Estado:** Aprobado funcionalmente, con decisiones pendientes identificadas  
**Prioridad:** MVP

## 1. Objetivo

Permitir que cada persona tenga una cuenta individual y segura en WishList, mediante la cual pueda participar en múltiples grupos, gestionar sus listas de deseos y organizar regalos para ocasiones especiales.

Cada cuenta tendrá una identidad técnica única, independiente de los nombres o apodos utilizados por sus propietarios.

## 2. Requisitos funcionales

### RF-AUTH-01 — Registro de usuario

El sistema debe permitir crear una cuenta mediante correo electrónico, contraseña, nombre, apellido y fecha de cumpleaños.

**Historia de usuario**

Como persona nueva en WishList, quiero registrarme para crear mis listas de deseos y participar en grupos de organización de regalos.

**Datos requeridos**

| Campo | Obligatorio | Único |
|---|---|---|
| Nombre | Sí | No |
| Apellido | Sí | No |
| Apodo | No | No |
| Correo electrónico | Sí | Sí |
| Contraseña | Sí | No aplica |
| Día de cumpleaños | Sí | No |
| Mes de cumpleaños | Sí | No |
| Año de nacimiento | No | No |

**Criterios de aceptación**

- El nombre y apellido son obligatorios y no pueden contener únicamente espacios.
- El apodo es opcional.
- El correo electrónico debe tener un formato válido y ser único entre las cuentas activas.
- La contraseña debe cumplir una política de seguridad definida durante el diseño técnico.
- El día y mes de cumpleaños son obligatorios.
- El año de nacimiento es opcional.
- El sistema debe validar que el día corresponda al mes seleccionado.
- Se permite registrar el 29 de febrero, aunque no se proporcione el año de nacimiento.
- Si se proporciona el año, la fecha completa debe ser válida.
- El registro con datos inválidos debe rechazarse sin crear una cuenta.
- Al registrar una cuenta válida, el sistema asigna un identificador interno único e inmutable.

### RF-AUTH-02 — Verificación de correo electrónico

El sistema debe verificar que el usuario tenga acceso al correo registrado antes de permitir su participación en grupos.

**Historia de usuario**

Como usuario, quiero verificar mi correo electrónico para activar las funcionalidades compartidas de WishList de forma segura.

**Criterios de aceptación**

- Al completar el registro, el sistema envía un mensaje de verificación.
- El enlace o código de verificación tiene una vigencia limitada.
- El mecanismo de verificación debe ser de un solo uso.
- El usuario puede solicitar el reenvío del mensaje de verificación.
- Una cuenta sin verificar no puede crear ni incorporarse a grupos.
- Una vez verificado el correo, se habilitan las funcionalidades compartidas.
- El sistema debe informar al usuario cuando su correo esté pendiente de verificación.

### RF-AUTH-03 — Inicio y cierre de sesión

El sistema debe permitir que los usuarios registrados inicien y cierren sesión de manera segura.

**Historia de usuario**

Como usuario registrado, quiero iniciar sesión para acceder a mis grupos, deseos y ocasiones especiales.

**Criterios de aceptación**

- El usuario puede iniciar sesión utilizando su correo electrónico y contraseña.
- Las credenciales válidas permiten acceder a la cuenta.
- Las credenciales inválidas impiden el acceso.
- Los mensajes de error no deben revelar si existe una cuenta asociada a un correo electrónico.
- El usuario puede cerrar sesión.
- Las funcionalidades privadas requieren autenticación.
- Una sesión cerrada no debe permitir continuar realizando operaciones autenticadas con sus credenciales de sesión anteriores.

### RF-AUTH-04 — Gestión del perfil

El sistema debe permitir consultar y modificar la información personal básica de la cuenta.

**Historia de usuario**

Como usuario, quiero mantener mis datos actualizados para que los integrantes de mis grupos puedan identificarme y conocer mi cumpleaños.

**Datos del perfil**

- Nombre: obligatorio y modificable.
- Apellido: obligatorio y modificable.
- Apodo: opcional y modificable.
- Correo electrónico: obligatorio, único y modificable mediante verificación.
- Día de cumpleaños: obligatorio.
- Mes de cumpleaños: obligatorio.
- Año de nacimiento: opcional y modificable.
- Imagen de perfil: fuera del MVP inicial.

**Criterios de aceptación**

- El usuario puede consultar sus datos personales.
- Solo el propietario de la cuenta puede modificar su perfil.
- El nombre y apellido no pueden quedar vacíos.
- El usuario puede agregar, modificar o eliminar su apodo.
- La modificación del correo electrónico requiere verificar la nueva dirección antes de reemplazar el correo confirmado.
- Los cambios en los datos visibles deben reflejarse en los grupos donde participa.
- La modificación del perfil no debe alterar su identificador interno ni las relaciones existentes con grupos, deseos o regalos.

### RF-AUTH-05 — Recuperación de acceso

El sistema debe ofrecer un mecanismo seguro para recuperar el acceso a una cuenta cuando el usuario olvide su contraseña.

**Historia de usuario**

Como usuario registrado, quiero restablecer mi contraseña para recuperar el acceso a WishList.

**Criterios de aceptación**

- El usuario puede solicitar recuperación mediante su correo electrónico.
- El sistema envía instrucciones de recuperación a la dirección registrada cuando corresponde.
- El enlace o código debe ser temporal y de un solo uso.
- La respuesta de la solicitud no debe revelar si existe una cuenta asociada al correo.
- La nueva contraseña debe cumplir la política de seguridad definida.
- Tras un restablecimiento exitoso, la contraseña anterior deja de ser válida.
- El mecanismo de recuperación debe invalidarse después de utilizarse correctamente.

### RF-AUTH-06 — Visibilidad del perfil

El sistema debe restringir la información personal de cada usuario a las personas con quienes comparte al menos un grupo activo.

**Historia de usuario**

Como usuario, quiero que mi perfil sea visible únicamente para las personas que pertenecen a mis grupos, para preservar mi privacidad.

**Criterios de aceptación**

- Un usuario puede consultar su propio perfil completo.
- Los integrantes que comparten un grupo activo pueden consultar el nombre, apellido, apodo y cumpleaños de otro integrante.
- Los usuarios que no comparten grupos no pueden consultar el perfil privado de otra persona.
- El correo electrónico no debe mostrarse a otros integrantes por defecto.
- El año de nacimiento no debe mostrarse a otros integrantes por defecto, aunque esté registrado.
- La visibilidad del perfil debe comprobarse en el backend.
- Cuando dos usuarios dejan de compartir grupos activos, se revoca el acceso al perfil privado del otro.
- La visibilidad del perfil no concede permisos para modificarlo.

### RF-AUTH-07 — Identificación visual de usuarios

El sistema debe facilitar la identificación de personas dentro de grupos sin exigir nombres de usuario únicos.

**Historia de usuario**

Como integrante de un grupo, quiero reconocer a sus participantes mediante sus nombres o apodos para organizar regalos para la persona correcta.

**Criterios de aceptación**

- El sistema permite que diferentes usuarios tengan el mismo nombre y apellido.
- El sistema permite que diferentes usuarios utilicen el mismo apodo.
- Cuando existe un apodo, se utiliza preferentemente como nombre de presentación.
- Cuando no existe un apodo, se muestra el nombre y apellido.
- En contextos donde sea importante distinguir personas, la interfaz debe disponer de información adicional, como el nombre completo junto al apodo.
- Todas las operaciones deben identificar internamente a los usuarios mediante su identificador único, nunca mediante nombres o apodos.

## 3. Reglas de negocio

- **RN-AUTH-01:** cada cuenta posee un identificador interno único e inmutable.
- **RN-AUTH-02:** cada correo electrónico debe ser único entre las cuentas activas.
- **RN-AUTH-03:** el nombre y apellido son obligatorios, pero no necesitan ser únicos.
- **RN-AUTH-04:** el apodo es opcional, modificable y no necesita ser único.
- **RN-AUTH-05:** el cumpleaños requiere un día y mes válidos. El año de nacimiento es opcional.
- **RN-AUTH-06:** una cuenta puede pertenecer a múltiples grupos.
- **RN-AUTH-07:** la participación en funcionalidades grupales requiere un correo electrónico verificado.
- **RN-AUTH-08:** la información privada de otros perfiles solo es accesible cuando existe al menos una membresía grupal activa compartida.
- **RN-AUTH-09:** las contraseñas nunca deben almacenarse en texto plano; deben protegerse mediante algoritmos seguros de hashing de contraseñas.
- **RN-AUTH-10:** todas las operaciones protegidas deben validar identidad y autorización en el backend.
- **RN-AUTH-11:** los nombres, apellidos y apodos no se utilizarán como identificadores para establecer relaciones entre entidades.
- **RN-AUTH-12:** el sistema utilizará preferentemente el apodo como nombre de presentación y recurrirá al nombre completo cuando corresponda.
- **RN-AUTH-13:** cambiar datos del perfil no debe modificar las relaciones existentes con grupos, deseos, ocasiones, reservas o regalos.
- **RN-AUTH-14:** un usuario no podrá modificar los datos personales de otra cuenta por pertenecer al mismo grupo ni por ejercer funciones administrativas dentro de este.

## 4. Fuera del alcance inicial

- Inicio de sesión mediante Google, Apple o redes sociales.
- Autenticación biométrica.
- Perfiles públicos.
- Búsqueda pública de usuarios.
- Nombres de usuario únicos tipo `@usuario`.
- Personalización avanzada del perfil.
- Imágenes de perfil personalizadas.
- Registro de destinatarios externos a WishList.

## 5. Decisiones pendientes

### DP-AUTH-01 — Eliminación de cuentas

Definir el comportamiento del sistema cuando un usuario elimina su cuenta, considerando:

- Eliminación o anonimización de información personal.
- Conservación mínima de registros históricos cuando corresponda.
- Impacto en grupos y membresías.
- Reservas y regalos colaborativos pendientes.

### DP-AUTH-02 — Cumpleaños del 29 de febrero

**Estado:** Resuelto en RF-04.

Los cumpleaños registrados el 29 de febrero se celebran el 28 de febrero durante los años no bisiestos, sin modificar la fecha de nacimiento registrada (ver RF-OCC-01 y RN-OCC-11).

### DP-AUTH-03 — Identificación de personas con nombres coincidentes

Evaluar durante el diseño UX si es necesario incorporar un mecanismo visual adicional para distinguir usuarios que compartan nombre, apellido y apodo.

No se exigirá unicidad de nombres para resolver este escenario.

## 6. Consideraciones técnicas

Las siguientes directrices orientarán el diseño técnico sin imponer todavía una implementación particular.

- La autenticación debe utilizar mecanismos seguros y ampliamente mantenidos.
- La autorización debe validarse desde el backend, independientemente de las restricciones visuales del frontend.
- Las credenciales, tokens y secretos deben manejarse de forma segura.
- Los identificadores internos de usuario no deben depender de información personal modificable.
- La validación de datos debe realizarse tanto en el frontend, para mejorar la experiencia de uso, como en el backend, para garantizar su integridad.
- Los controles de recuperación, verificación de correo y autenticación deberán considerar limitación de intentos y protección frente a abusos.

La solución definitiva de autenticación, almacenamiento y gestión de sesiones se establecerá durante la definición de arquitectura.

---

**Estado del documento:** aprobado funcionalmente, con decisiones pendientes identificadas que no impiden continuar con la especificación de otros módulos.