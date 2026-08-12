# Imágenes de palas

Un PNG por cada forma de pala con este nombre exacto:

    {id_de_pala}.png

## IDs válidos

    spoon    → Clásica   ✅ ya existe
    round    → Redonda
    hatchet  → Hacha

## Formato de la imagen

- **Fondo**: negro puro (#000000). El filtro SVG convierte luminancia en
  alfa, así que el negro se vuelve transparente y el blanco toma el color elegido.
- **Color de la pala**: blanco puro (#FFFFFF). Puede tener brillo suave en
  los bordes (glow) — queda bien con el filtro.
- **Orientación**: cuello/collar a la IZQUIERDA, cara de la pala a la DERECHA.
  El collar ocupa el ~10-15 % izquierdo de la imagen.
- **Resolución sugerida**: 1344 × 840 px (ratio ~1.6:1).
- **Tamaño en pantalla**: se renderiza a 65 × 40 unidades SVG dentro de
  un canvas de 500 × 320.

---

## Prompt para ChatGPT / DALL-E

Usa este prompt (sustituye `[TIPO]` por la forma que quieras):

```
Create a PNG image of a rowing oar blade (pala de remo) seen from the side.

Blade type: [TIPO — e.g. "round spoon blade" / "hatchet/big blade" / "classic spoon blade"]

Requirements:
- Pure black background (#000000), no transparency, no gradient background.
- The blade shape is pure white (#FFFFFF) with a soft white glow on the edges.
- Horizontal orientation: the collar/sleeve (cuello) is on the LEFT side,
  the blade face extends to the RIGHT.
- The collar takes up roughly 10-15% of the total width on the left.
- The blade body fills most of the right portion of the image.
- No oar shaft — only the blade head and collar visible.
- Clean, simple, vector-like style. No text, no watermarks.
- Image size: 1344 × 840 px.
```

### Tipos para cada forma

| Archivo     | Sustituir `[TIPO]` por                                      |
|-------------|-------------------------------------------------------------|
| `round.png`   | `round/oval rowing blade (symmetric elliptical shape)`    |
| `hatchet.png` | `hatchet/big-blade rowing blade (flat rectangular top with curved bottom, asymmetric)` |
