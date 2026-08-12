# Cómo añadir fondos reales a las rutas

No hace falta tocar el código. Solo pon el archivo en esta carpeta con el nombre exacto:

    {id_de_la_ruta}.jpg   ← formato preferido
    {id_de_la_ruta}.png   ← también válido (la app lo prueba si el .jpg no existe)

**Solo necesitas UNA imagen por ruta** (la de "día"). Para Amanecer, Atardecer y
Lluvia, la app aplica un filtro de color automático sobre esa misma imagen —
no hace falta generar 4 versiones de cada sitio.

Si en algún momento quieres una foto específica de verdad para un clima
concreto (por ejemplo, una foto real de noche), puedes añadir también
`{ruta}_dawn.jpg`, `{ruta}_dusk.jpg` o `{ruta}_rain.jpg` — si existe, la app
usa esa imagen en vez del filtro; si no existe, usa el filtro sobre la base.

## IDs de ruta válidos

    zambezi, henley, charles, lucerne, seine, schuylkill, sydney, nilo

## Ruta libre (metrónomo libre)

    libre.png   ← fondo fijo del modo libre, no requiere Premium

## Ejemplo mínimo (lo normal)

    seine.jpg    ← con esta sola ya se ven las 4 variantes de clima (con filtro)
    charles.png  ← también válido

## Requisitos de la imagen

- Formato JPG o PNG.
- Proporción aproximada 25:16 (por ejemplo 1000x640, 1500x960) — no pasa nada si no es exacta,
  la app la recorta automáticamente para rellenar el marco sin deformarla.
- Recomendado: menos de 200 KB para que cargue rápido en móvil (redimensiona/comprime
  antes de subirla si el archivo original es muy pesado).

## Estado actual

    libre.png       ✅
    seine.jpg       ✅
    charles.png     ✅
    henley.png      ✅
    lucerne.png     ✅
    nilo.png        ✅
    schuylkill.png  ✅
    sydney.png      ✅
    zambezi.png     ✅
