# Imágenes de botes

Pon aquí un PNG por cada forma de bote con este nombre exacto:

    {id_del_bote}.png

## IDs válidos

    classic    → Clásico
    viking     → Vikingo
    kayak      → Kayak
    gondola    → Góndola
    dragon     → Bote Dragón
    ornate     → Ceremonial

## Cómo tiene que ser la imagen

- **Formato**: PNG con fondo transparente.
- **Color**: dibuja el bote en **blanco puro** (#FFFFFF). La app aplica
  automáticamente el color elegido por el usuario mediante un filtro SVG
  (feColorMatrix), así que no hagas falta generar una imagen por color.
- **Tamaño**: 500 × 320 px — coincide exactamente con el viewBox de la escena SVG.
  El bote debe ocupar la parte inferior (a partir de y ≈ 200 px hacia abajo).
- Si una imagen no existe, la app usa el dibujo SVG vectorial de fallback —
  nunca rompe nada.

## Filtro de color (cómo funciona)

Cuando el usuario elige, por ejemplo, "Océano" (#2f6f9e), la app genera:

    feColorMatrix values="0.18 0 0 0 0  0 0.44 0 0 0  0 0 0.62 0 0  0 0 0 1 0"

Blanco (1,1,1) × esa matriz → (0.18, 0.44, 0.62) = el color exacto elegido.

## Estado actual

    (ninguna imagen todavía — todos los botes usan el SVG de fallback)
