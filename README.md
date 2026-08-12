# Remo — Metrónomo y Rutas

App web (PWA) de metrónomo de remo indoor con rutas guiadas por el mundo,
gamificación, y sistema de suscripción Premium. Pensada para gente que rema
con remoergómetro (Concept2, WaterRower, etc.), tanto en casa como en el
gimnasio.

Es una **single-page app en un solo archivo HTML** (`index.html`) — todo el
HTML, CSS y JavaScript vive ahí dentro, sin build step, sin frameworks, sin
`npm install`. Se edita directamente y se sirve tal cual.

---

## Estructura del proyecto

```
remo-app/
├── index.html            # La app completa (HTML + CSS + JS)
├── manifest.json          # Configuración de instalación como PWA
├── service-worker.js      # Caché offline (ver "Actualizar la app" abajo)
├── icons/                  # Iconos de la app (192px, 512px, versiones "maskable")
└── images/routes/          # Fondos reales por ruta (ver images/routes/README.md)
```

---

## Qué hace la app (funciones ya implementadas)

### Metrónomo
- Vista en primera persona: manos + remos + bote, sincronizados con el ciclo
  real de una remada (**Catch → Impulso → Recuperación**).
- Selector de relación Catch:Recuperación (1:1.5 / 1:2 / 1:3), visible tanto
  en el metrónomo libre como durante una rutina (no se oculta al empezar).
- Sonido de clic o de agua (elegible en Personalización); el de agua es
  función Premium.
- Anillo de ritmo sincronizado con el mismo reloj interno que anima
  manos/remo (letra C/I/R dentro del anillo).
- Cuenta atrás "3, 2, 1, ¡Rema!" con sonido, y la pantalla hace scroll
  automático hacia la escena al pulsar Empezar.
- **Wake Lock**: la pantalla del móvil no se apaga mientras se está remando.

### Rutas (8 rutas reales del mundo)
Zambezi, Támesis (Henley), Charles River (Boston), Lago Lucerna, Sena
(París), Schuylkill (Filadelfia), Bahía de Sídney, Nilo.

- Cada ruta tiene varios tramos con su propio SPM objetivo; el ritmo cambia
  solo, con un aviso visual ("Sube/Baja a X spm").
- Agrupadas por dificultad (Fácil/Medio/Difícil/Extremo), calculada con una
  fórmula basada en duración total y SPM máximo (no a mano) — así las rutas
  nuevas se clasifican solas.
- **Pausar y reanudar de verdad**: al pausar (o cambiar de pestaña) se guarda
  el segundo exacto dentro del tramo; al reanudar continúa ahí, no reinicia.
- Botón "↻ Reiniciar ruta" para volver al tramo 0 sin perder la selección.
- En horizontal, si hay una rutina en marcha, la interfaz se simplifica
  (imagen grande, solo Reanudar/Reiniciar/Guardar).
- Guardado mínimo de **10 minutos** para que una sesión cuente como
  entrenamiento válido (hay un modo de prueba en Personalización que baja
  esto a 15s, para testear sin esperar).
- Buzón "Pide tu ruta": el usuario sugiere rutas nuevas, se envían por email
  (vía FormSubmit.co, sin backend propio) a la dirección configurada en
  `FEEDBACK_EMAIL` dentro del código.

### Clima / hora del día (función Premium)
Día, Amanecer, Atardecer, Lluvia — cambia el tono de cielo/agua, reposiciona
el sol (al este en amanecer, al oeste en atardecer, asumiendo que siempre se
rema hacia el norte), y añade nubes grises + lluvia animada en el clima de
lluvia.

### Fondos con fotos reales (función Premium, opcional)
Sistema de imágenes por convención de nombre — **no hace falta tocar código**
para añadir fotos nuevas. Ver `images/routes/README.md` para el detalle
completo. Resumen:

- Un archivo `images/routes/{id_ruta}_day.jpg` es suficiente; el filtro de
  clima (arriba) se aplica automáticamente encima para Amanecer/Atardecer/Lluvia.
- Si además existe `{id_ruta}_{clima}.jpg` específico, se usa esa en vez del
  filtro.
- Si no existe ninguna imagen, cae automáticamente al dibujo ilustrado (SVG)
  — nunca rompe nada.
- Cuando hay foto activa, se ocultan el sol/nubes/agua ilustrados y solo se
  ve el bote/manos/remos superpuestos encima de la foto real.

### Personalización
- Tono de piel.
- Bote: 6 formas (Clásico, Vikingo, Kayak, Góndola, Bote Dragón, Ceremonial)
  × 6 colores — combinables.
- Remo: 6 formas × 6 colores.
- **Desbloqueo por NIVEL del jugador** (no por compras) — cuanto más subes de
  nivel, más combinaciones tienes disponibles. Premium las desbloquea todas
  al instante como ventaja de suscripción, no como compra suelta.
- Vista previa en vivo de bote + remos antes de aplicar.

### Gamificación (Progreso / Inicio)
- **Nivel y XP**: 600 XP por nivel; XP ganada por sesión = base + minutos +
  distancia + bonus si fue récord personal.
- **Rachas**: días consecutivos entrenando; se rompe si pasan más de 48h.
- **52 logros** (distancia, tiempo, sesiones, rachas, calorías, récords,
  nivel, y logros especiales como "Madrugador", "Explorador del mundo",
  "Ritmo de sprint"...).
- **Misiones diarias**: 3 misiones que rotan cada día (deterministas por
  fecha), con XP extra.
- **Objetivos semanales**: 90 min / 20 km / 3 entrenamientos.
- **Calendario tipo GitHub**: mapa de calor de los últimos ~84 días según
  minutos remados.
- **Comparación contigo mismo** (no ranking global): "este mes remas un X%
  más/menos que el mes pasado", etc.
- **Modal de resumen post-entrenamiento**: XP ganada, nivel, racha, récord,
  logros nuevos desbloqueados, y botón "Compartir resultado" (usa
  `navigator.share` nativo, con fallback a copiar al portapapeles).

### Récords personales
Se compara la distancia remada en la MISMA ruta entre intentos — a igual
duración (la ruta siempre dura lo mismo), más distancia = remaste más
fuerte. **Solo cuentan las sesiones completadas enteras**, no las guardadas
a medias (con el botón manual de guardado), para que la comparación sea justa.

### Premium (suscripción, mock/demo)
- 3,99 €/mes o 43,89 €/año (con selector de plan).
- El estado `isPremium` es una variable local (demo) — **no hay cobro real
  todavía**. Para cobros de verdad en las tiendas, haría falta integrar
  Google Play Billing / Apple In-App Purchase (Apple y Google exigen su
  propio sistema de pago para suscripciones digitales dentro de apps
  publicadas en sus tiendas, no permiten Stripe/PayPal directamente).
- Hay un interruptor "Modo de desarrollador" en Personalización
  (`devUnlockAll`) que desbloquea todo temporalmente para probar sin
  necesidad de ser Premium de verdad ni subir de nivel.

### Rutinas personalizadas (Mis rutinas, función Premium)
Constructor de intervalos propios (nombre + tramos con SPM y duración),
guardado persistente, pestaña dedicada separada de las rutas predefinidas.

### Diseño visual
Rediseñado siguiendo el estilo "Nike Run Club / Strava / Apple Fitness"
(limpio, minimalista, sin emojis como iconos, sin estética gamer/infantil):
- Sistema de iconos propio en SVG inline (función `icon()` en el JS) — sin
  librerías de iconos externas, para que la PWA funcione offline.
- Tokens de diseño en `:root` (espaciado, sombras, radios, colores).
- Barra de navegación inferior fija, compacta, con icono + texto pequeño.
- Animaciones discretas (fade al cambiar de pestaña, feedback al pulsar
  botones).

### Pantalla de Inicio
Landing interna con: hero + CTA, tarjetas de beneficios, "cómo funciona" en
3 pasos, nivel/XP, misiones del día, estadísticas si ya hay historial (o
invitación a empezar si es nuevo).

---

## Cómo funciona el almacenamiento

La app usa `window.storage.get/set/delete/list` para guardar historial,
preferencias, nivel, etc. Dentro de Claude.ai ese objeto existe de forma
nativa. **Fuera de Claude** (esta carpeta, publicada en cualquier hosting),
`index.html` incluye al principio del `<script>` una capa de compatibilidad
que crea automáticamente `window.storage` usando `localStorage` del
navegador si detecta que no existe — así todo el código de guardado funciona
igual en ambos sitios sin tener que tocarlo.

Una limitación conocida: el buzón de "pide tu ruta" usaba almacenamiento
*compartido* dentro de Claude (visible entre usuarios); fuera de Claude, con
`localStorage`, cada persona solo ve sus propias sugerencias en su propio
dispositivo — por eso se cambió a enviarlas por email en su lugar (ver
`FEEDBACK_EMAIL` en el código).

---

## Actualizar la app (importante)

Cada vez que se modifique `index.html` de forma real (no solo texto/imágenes),
hay que **subir en 1 el número de versión** dentro de `service-worker.js`:

```js
const CACHE_NAME = 'remo-app-v18';  // subir a v19, v20, etc.
```

Esto fuerza a que los móviles descarguen la versión nueva en vez de quedarse
con la cacheada por el service worker (el offline caching es "cache-first
con actualización en segundo plano").

Después de cualquier cambio, hay que volver a subir la **carpeta entera**
(no solo `index.html`) al hosting (actualmente Netlify).

---

## Cómo probarla en local

No hace falta ningún build:

```bash
npx serve .
# o simplemente abrir index.html directamente en el navegador
```

## Cómo desplegarla

1. Sube la carpeta entera (tal cual) a un hosting estático — Netlify, Vercel,
   GitHub Pages, etc. (Actualmente se usa Netlify Drop.)
2. Con la URL pública, se pueden generar los paquetes para las tiendas de
   apps (Android/iOS) con [PWABuilder.com](https://pwabuilder.com).
3. **Importante**: Apple/Google exigen que las suscripciones digitales dentro
   de apps publicadas en sus tiendas usen su propio sistema de pago
   (In-App Purchase / Play Billing), no un proveedor externo — pendiente de
   implementar si se quiere monetizar de verdad dentro de las tiendas.

## Añadir fondos reales a las rutas

Ver [`images/routes/README.md`](images/routes/README.md) — resumen: un solo
archivo `{ruta}_day.jpg` por ruta es suficiente, el resto de climas se
generan con un filtro automático.

---

## Estado actual / pendientes conocidos

- Solo `images/routes/seine_day.jpg` existe de momento (imagen de prueba);
  el resto de rutas usan el dibujo ilustrado.
- El pago Premium es una simulación local, no hay cobro real integrado.
- Aún no está publicada en ninguna tienda de apps (en fase de pruebas,
  desplegada solo en Netlify).
- Pendiente de confirmar cómo se comporta el audio de la app cuando suena
  música de fondo (Spotify/Apple Music) al mismo tiempo.
