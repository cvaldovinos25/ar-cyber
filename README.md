# ¡Busca a Salcotín!

Experiencia de realidad aumentada para el celular en la que la persona escanea un código QR, busca a Salcotín a su alrededor con la cámara y, al atraparlo, gana un premio que puede canjear en caja.

Funciona directo en el navegador, sin instalar ninguna app, y no depende de motores ni servicios externos: todo el código, las imágenes y la librería 3D viven en este repositorio y se publican con GitHub Pages.

**Sitio:** https://cvaldovinos25.github.io/ar-swap/

## Cómo se juega

1. **Pantalla de inicio.** Aparece el cartel "¡Busca a Salcotín!" sobre el fondo del proyecto. Al tocar **Comenzar**, el navegador pide permiso para usar la cámara y, en iPhone, también el movimiento del teléfono.
2. **Búsqueda.** La cámara trasera se muestra de fondo y el cartel queda flotando frente a la persona por unos segundos. Después se encoge y lo reemplaza un texto de ayuda en la parte de abajo, que alterna entre "¡Escanea tu alrededor!" y "¡Atrapa a Salcotín cuando lo encuentres!".
3. **Salcotín se escapa.** Salcotín flota alrededor de la persona, nunca justo enfrente. Cada 5 segundos sale corriendo a otro lugar con un salto de dibujo animado: se agacha, sale disparado en arco estirándose e inclinándose, y aterriza aplastado con un rebote. El salto es más rápido de lo que se puede seguir girando el celular, así que hay que volver a buscarlo.
4. **Termómetro.** En la esquina superior derecha, un termómetro indica qué tan cerca está: **frío** (celeste) cuando está lejos, **tibio** (amarillo) cuando está cerca de entrar en pantalla y **¡caliente!** (rojo, latiendo) cuando está a la vista.
5. **Premio.** Al tocar a Salcotín, se encoge, aparece la imagen del premio en su lugar, sale confeti de tres colores y abajo aparece el mensaje "¡Toma un pantallazo y canjea tu premio en caja!".

## Cómo funciona por dentro

| Parte | Qué hace |
| --- | --- |
| Cámara (`getUserMedia`) | Muestra la cámara trasera de fondo, a pantalla completa. |
| Sensores (`DeviceOrientation`) | Leen el giro del celular y mueven la cámara 3D en la misma dirección, para que las imágenes queden "fijas" en el espacio. |
| three.js (`lib/three.min.js`) | Dibuja a Salcotín, el premio, el cartel y el confeti encima de la cámara. Es la versión r128, con licencia MIT, guardada en el propio repositorio. |

El rastreo es **de rotación** (3DOF): las imágenes se mantienen en su lugar cuando la persona gira, pero no se acercan ni se alejan si camina. Para esta mecánica de buscar girando no hace falta más, y a cambio funciona igual en iPhone y en Android.

En computador, donde no hay sensores de movimiento, se puede mirar alrededor **arrastrando con el mouse**, lo que sirve para hacer pruebas.

## Estructura del repositorio

```
index.html          Estructura de la página: pantallas de inicio, carga y error, termómetro y texto de ayuda
css/style.css       Estilos: fondo, botones, termómetro, textos
js/main.js          Toda la lógica de la experiencia
lib/three.min.js    Librería 3D (three.js r128)
assets/
  fondo.png         Imagen de fondo (degradado) de la pantalla de inicio
  intro.png         Cartel "¡Busca a Salcotín!"
  1.png             Salcotín
  2.png             Premio Pampers (el que se usa por defecto)
  3.png             Premio Nenitos
  4.png             Premio Huggies
  amarillo.png      Confeti amarillo
  celeste.png       Confeti celeste
  rosa.png          Confeti rosa
.nojekyll           Le indica a GitHub Pages que publique los archivos tal cual
```

## Cómo personalizarlo

Casi todo se ajusta al inicio de `js/main.js`, sin tocar el resto del código.

**Cambiar el premio.** En `ASSETS`, cambia la línea `prize: 'assets/2.png'` por `'assets/3.png'` (Nenitos) o `'assets/4.png'` (Huggies).

**Cambiar los textos.** Los mensajes de ayuda están en `HINT_MESSAGES` y el mensaje final en `PRIZE_MESSAGE`.

**Ajustes de `CONFIG`** más útiles:

| Ajuste | Para qué sirve |
| --- | --- |
| `MOVE_INTERVAL_MS` | Cada cuánto salta Salcotín a otro lugar (5000 = 5 segundos). |
| `DASH_TRAVEL_MS` | Velocidad del salto. Más alto = más lento y fácil de seguir. |
| `DASH_HOP_HEIGHT` | Qué tan alto es el arco del salto, en metros. |
| `DASH_MIN_DEG` | Cuánto tiene que alejarse como mínimo en cada salto, en grados alrededor de la persona. |
| `RADIUS_RANGE` | A qué distancia de la persona puede aparecer, en metros. |
| `FRONT_EXCLUSION_DEG` | Zona frente a la persona (a cada lado) donde nunca aparece al saltar. |
| `PLANE_WIDTH` / `PLANE_HEIGHT` | Tamaño de Salcotín y del premio. |
| `INTRO_HIDE_DELAY_MS` | Cuánto dura el cartel de inicio antes de cambiar al texto de ayuda. |
| `THERMO_WARM_MARGIN` | Qué tan pronto se pone "tibio" el termómetro. |
| `SHOW_POINTER` | `true` muestra una flecha que indica hacia dónde girar. |

**Cambiar el fondo.** Reemplaza `assets/fondo.png` por otra imagen con el mismo nombre. Si usas otro nombre, actualiza la línea `--fondo` al inicio de `css/style.css`. Para que la barra del navegador combine, cambia también el color de `theme-color` en `index.html`.

## Publicar cambios

1. Sube o reemplaza los archivos en GitHub (**Add file → Upload files** y luego **Commit changes**).
2. Espera a que la pestaña **Actions** quede en verde (uno o dos minutos).
3. Abre el sitio en el celular. Si ves la versión anterior, ábrelo en una pestaña privada para evitar la caché.

La publicación está configurada en **Settings → Pages → Deploy from a branch**, con la rama `main` y la carpeta `/ (root)`.

## Requisitos para quien juega

- Abrir el link en **Safari** (iPhone) o **Chrome** (Android). Los navegadores internos de Instagram, WhatsApp u otras apps suelen bloquear la cámara.
- Aceptar los permisos de cámara y de movimiento.
- El sitio tiene que abrirse con `https://` (GitHub Pages ya lo hace).
