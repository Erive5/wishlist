# Prototipos UX/UI de WishList

Prototipos visuales aprobados en Claude (dirección «minimalismo cálido»). Sirven como **referencia visual** para implementar el frontend: no son código de producción ni contienen lógica real.

Cada archivo es una copia exacta del Artifact publicado, con su HTML, CSS y JavaScript en línea. No dependen de otros archivos del repositorio.

## Cómo abrirlos

1. Abre la carpeta `docs/04-disenio/prototipos/`.
2. Haz doble clic en el archivo `.html` o arrástralo a un navegador moderno (Chrome, Edge, Firefox o Safari).

No necesitan servidor ni instalar dependencias.

- **Fuente:** la tipografía Geist se carga desde Google Fonts. Sin conexión a internet se usa la fuente del sistema y el diseño se mantiene, aunque cambia ligeramente el dibujo de las letras.
- **Selectores:** el selector que está sobre el teléfono cambia de pantalla. Recuerda la última pantalla vista en el almacenamiento local del navegador.
- **Acciones:** los botones dentro del teléfono no ejecutan acciones reales; muestran un aviso.
- **Tema:** el fondo de la página sigue el tema claro u oscuro del sistema; las pantallas del teléfono son siempre claras.

## Archivos y pantallas

| Archivo | Pantallas representadas |
|---|---|
| `01-inicio.html` | Inicio (propuesta minimalista refinada): saludo, avisos de ocasiones a 14 y 7 días, votos pendientes, regalos que organizas, próximas ocasiones, mis listas y mis grupos. |
| `02-pantallas-principales.html` | Inicio (referencia) · Lista de deseos de otra persona (Lucía) · Detalle de deseo, con estado «Disponible» y «Reservado por ti» · Regalo colaborativo con aportes estimados y votación de ingreso. |
| `03-flujos-mvp.html` | **Grupos:** Mis grupos, Detalle de grupo, Votación de ingreso, Crear o unirse · **Mis listas:** Listas propias, Detalle de lista, Crear deseo, Editar y compartir lista · **Ocasiones:** Próximas, Detalle de ocasión, Nueva ocasión · **Regalos:** Organizo, Recibidos, Realizados · **Acceso y perfil:** Inicio de sesión (con estado de error), Registro, Verificar correo, Perfil. |

Notas:
- `01` y `02` muestran precios en euros; `03` usa CLP, la moneda de referencia acordada después. La moneda definitiva sigue pendiente (DP-WISH-06).
- Todos los datos son ficticios.

## Ilustraciones

Los personajes son SVG originales escritos en línea dentro del JavaScript de cada prototipo, como cadenas de texto (`const giftBuddy = …`). Comparten el color de trazo `INK = "#15172A"`.

Se extrajeron copias idénticas como archivos independientes en `frontend/public/assets/illustrations/`. Los prototipos siguen usando su versión en línea; no dependen de esos archivos.

| Personaje | Archivo SVG | Código original en los prototipos | Dónde aparece |
|---|---|---|---|
| Caja de regalo que saluda | `gift-buddy.svg` | `giftBuddy` · `01` línea 234 · `02` línea 359 · `03` línea 533 | Saludo de Inicio, Inicio de sesión, Registro, cabecera de Regalos |
| Globo | `balloon-buddy.svg` | `balloonBuddy` · `01` línea 252 · `02` línea 377 · `03` línea 551 | Aviso de cumpleaños, Mis listas, Ocasiones, Detalle de ocasión, Nueva ocasión, Registro |
| Dos amigos chocando las manos | `friends-buddy.svg` | `friendsBuddy` · `01` línea 261 · `02` línea 386 · `03` línea 560 | Mis grupos, Regalo colaborativo, Editar y compartir, Crear o unirse |
| Jarrón de cerámica | `vase-buddy.svg` | `vaseBuddy` · `02` línea 470 · `03` línea 644 | Detalle de deseo, Nuevo deseo |
| Sobre con sello | `envelope-buddy.svg` | `envelopeBuddy` · `03` línea 1043 | Verificar correo |
| Subrayado ondulado | `squiggle-underline.svg` | `squiggle` · `01` línea 272 · `02` línea 397 · `03` línea 571 | Bajo «Ana» en el saludo y bajo el logotipo en Inicio de sesión |

En los archivos extraídos solo cambió el envoltorio técnico del `<svg>`: se añadió `xmlns` y se quitaron `class` y `aria-hidden`. En los tres que no tenían tamaño se añadieron `width` y `height` iguales a su `viewBox`. Las formas, colores, expresiones y proporciones son las originales.

Los detalles decorativos que no son personajes están hechos solo con CSS y siguen en los prototipos:

- confeti de puntos (`.hero .dot`, `.fhero .dot`, `.prof .dot`);
- punto naranja junto a la fecha (`.date::before`);
- línea punteada de la línea de tiempo de ocasiones (`.tl::before`);
- puntos animados de solicitud pendiente (`.pend i`, en `03`).

Los íconos de interfaz (inicio, grupos, calendario, etc.) son trazos SVG genéricos definidos en el objeto `P` de cada prototipo; no forman parte de los personajes.

## Pendiente

Faltan por diseñar:

- recuperar contraseña;
- editar perfil y cambiar el correo;
- editar, archivar o eliminar un deseo existente;
- confirmaciones de salir del grupo, cancelar ocasión y eliminar lista;
- detalle de un regalo en el historial;
- vista del destinatario al marcar un deseo como recibido;
- retirada de integrantes (umbral pendiente en DP-GRP-03).
