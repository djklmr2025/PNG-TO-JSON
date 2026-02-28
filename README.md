# PNG-TO-JSON 🔷

**Toolkit de conversión para [flow-diagram-creator](https://github.com/djklmr2025/flow-diagram-creator)**

Conjunto de herramientas standalone (`.html`) que transforman imágenes `.png` en objetos JSON compatibles con el sistema de diagramas — vectores puros, stikers de 2 capas o publicación directa a la biblioteca.

---

## 🗂️ Archivos

### `menu.html` — Panel de inicio
Menú visual que presenta y enlaza las 3 herramientas. Ábrelo como punto de entrada.
No requiere servidor — abre directo en el navegador.

---

### `png-to-stiker.html` ⭐ RECOMENDADO — Convertidor PNG → Stiker 2 Capas

**¿Qué hace?**
Convierte cualquier PNG en un **Stiker de 2 capas** compatible con el sistema:

| Capa | Tipo | Función |
|------|------|---------|
| 1 | `polygon` | Silueta/contorno vectorial del objeto (BASE) |
| 2 | `image` | PNG original flotando encima (SKIN visual) |

**¿Por qué este enfoque?**
- El `polygon` es el elemento real del sistema: puede seguir rutas, animarse y ser parte de Agrupados.
- La `image` es solo la "piel" visual — el usuario ve la imagen original con todos sus detalles.
- Se publica via **`/api/publish-project`** que sí acepta imágenes (sube los base64 a Vercel Blob automáticamente).
- Funciona para cualquier `.png`, `.jpg` o `.webp` — incluyendo objetos complejos o futuros PNGs 3D.

**Controles:**
- **Simplificación**: ajusta cuántos puntos tiene el polígono de silueta (↑ = menos puntos, más suave)
- **Opacidad de relleno**: transparencia del polígono base (0% = invisible, solo actúa como hitbox/ruta)
- **Color de silueta**: color del polígono base

**Preview en vivo:** original · silueta · composición final (las 3 vistas antes de publicar)

---

### `png-to-vector-json.html` — Convertidor PNG → Vector Puro

**¿Qué hace?**
Vectoriza completamente el PNG usando **ImageTracer**: convierte los píxeles en paths/polígonos SVG reales.
El resultado es un JSON con **cero elementos `image`** — válido para guardar en **Agrupados** sin error.

**Casos de uso:**
- Iconos, logos, formas simples con pocos colores
- Elementos que necesitas reutilizar como vector editable en la biblioteca
- Cuando quieres que el objeto sea 100% vectorial (escalable infinitamente)

**Controles:**
- **Colores** (2–32): cuántos colores distintos detecta el vectorizador
- **Suavizado** (omit): elimina paths pequeños/ruido (↑ = más limpio, menos detalle)
- **Detalle** (ltres): sensibilidad a curvas (↓ = más fiel al original)
- **Escala**: tamaño de salida del vector
- **Tipo de elemento**: `polygon` (nativo del engine) o `path` (SVG path data)

**Publica via:** `/api/publish-grouped` o `/api/publish`

---

### `metro-publisher.html` — Metro CDMX (Vector Redibujado)

**¿Qué hace?**
Vagón del Metro CDMX dibujado manualmente como vector puro: **39 elementos, 0 imágenes**.

| Tipo | Cantidad | Descripción |
|------|----------|-------------|
| `polygon` | 4 | Cuerpo principal, techo, cabinas |
| `rectangle` | 21 | Ventanas, puertas, franjas, bogies, detalles |
| `circle` | 12 | Ruedas con aro interior |
| `line` | 2 | Divisores centrales de puertas |

**Características:**
- Preview renderizado en canvas antes de publicar
- JSON editable directamente en la UI
- Publish Key y carpeta `metro/cdmx` prefillados
- Publica a **Agrupados** sin error de validación

---

## 🚀 Uso rápido

1. Descarga o clona este repo
2. Abre `menu.html` en el navegador (doble clic, sin servidor)
3. Elige la herramienta según tu caso:
   - **PNG con detalle visual** → `png-to-stiker.html`
   - **PNG simple para Agrupados** → `png-to-vector-json.html`
   - **Metro CDMX** → `metro-publisher.html`
4. Ajusta parámetros, haz clic en **Publicar** o descarga el `.json`
5. En el editor: importa el `.json` o búscalo en Biblioteca

---

## 🔌 Endpoints que usa cada herramienta

| Herramienta | Endpoint | Acepta imágenes |
|-------------|----------|-----------------|
| `png-to-stiker.html` | `/api/publish-project` | ✅ Sí (sube a Blob) |
| `png-to-vector-json.html` | `/api/publish-grouped` / `/api/publish` | ❌ No (vector puro) |
| `metro-publisher.html` | `/api/publish-grouped` | ❌ No (vector puro) |

---

## 🔑 Configuración

Todos los archivos tienen los campos de **URL del sistema** y **Publish Key** editables en la UI.
La clave se guarda en `localStorage` del navegador para no escribirla cada vez.

Valores por defecto:
```
URL: https://flow-diagram-creator.vercel.app
PUBLISH_KEY: (configurar en campo de UI)
```

---

## 🧩 Compatibilidad con flow-diagram-creator

Todos los JSON generados siguen la estructura base del sistema:

```json
{
  "name": "nombre-del-elemento",
  "camera": { "x": 0, "y": 0, "zoom": 1 },
  "elements": [
    { "type": "polygon", ... },
    { "type": "image", ... }
  ]
}
```

Compatible con:
- ✅ Importar JSON desde el editor
- ✅ Insertar desde Biblioteca → Agrupados / Stkers
- ✅ Usar como actor en rutas de movimiento (el `polygon` base)
- ✅ `?mode=sticker&id=...` y `?mode=preview&id=...`
- ✅ Inyección vía `/api/inject` en canvases en vivo

---

## 📁 Estructura del repo

```
PNG-TO-JSON/
├── menu.html                ← Panel de inicio (empieza aquí)
├── png-to-stiker.html       ← PNG → Stiker 2 capas ⭐
├── png-to-vector-json.html  ← PNG → Vector puro (Agrupados)
├── metro-publisher.html     ← Metro CDMX vector listo
├── metro-cdmx.json          ← JSON del metro (referencia)
└── README.md
```

---

## 🔗 Links

- **Sistema principal:** [flow-diagram-creator.vercel.app](https://flow-diagram-creator.vercel.app)
- **Repo principal:** [djklmr2025/flow-diagram-creator](https://github.com/djklmr2025/flow-diagram-creator)
- **Este repo:** [djklmr2025/PNG-TO-JSON](https://github.com/djklmr2025/PNG-TO-JSON)

---

*Parte del ecosistema flow-diagram-creator · djklmr2025*
