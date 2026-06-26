# Inventario Físico · MASTERPIECE — Tacna Shelter

Dashboard de una sola página para presentar el **resultado del conteo físico** y, en una segunda pestaña, el **análisis de riesgos** contra el saldo del anexo. Sin dependencias ni build: es un único `index.html`.

## Qué incluye
- **Pestaña 1 — Inventario Físico:** tabla de las 28 partidas con buscador, orden por columna e hipervínculo de evidencia por partida.
  - **Selector de columnas en vivo** con tres presets:
    - `Vista Cliente` — oculta Saldo Anexo, Sistema, Diferencia y Estatus (solo resultado físico).
    - `Vista Completa` — muestra todo.
    - `Auditoría` — muestra todo y filtra solo partidas con diferencia.
  - Los chips de columna permiten ajustar manualmente después de elegir un preset. Los KPIs cambian solos en Vista Cliente (no revelan diferencias).
- **Pestaña 2 — Riesgos:** resumen ejecutivo, tabla priorizada por severidad (% de variación vs. anexo) y banderas de calidad de datos.

## Publicar en GitHub Pages
1. Crea un repo (ej. `masterpiece-inventario`) y sube `index.html` y `README.md`.
2. `Settings → Pages → Build and deployment → Source: Deploy from a branch`, rama `main`, carpeta `/ (root)`.
3. En ~1 min queda en `https://<usuario>.github.io/masterpiece-inventario/`.

Para una demo rápida sin publicar, abre `index.html` directo en el navegador.

## Hipervínculos de evidencia (Drive)
Vienen en **modo demostración**. Dos formas de activarlos:
- En vivo: botón **“Configurar URL de evidencia (Drive)”** → pega la carpeta base. Se le añade el número de parte al final.
- Permanente: edita `CONFIG.evidenceBase` arriba del `<script>`.

```js
const CONFIG = {
  evidenceBase: "https://drive.google.com/drive/folders/XXXX?q="
};
```

## Integrar costo (impacto financiero)
El modelo ya está conectado. Solo agrega `costo:` (unitario) a cada partida en el arreglo `RAW`:

```js
{no:14, parte:"LABEL", ..., costo: 0.85},
```

La columna **Impacto $** del tab de Riesgos se calcula sola como `|Diferencia| × costo` y deja de mostrar “pendiente”.

## Cómo se recalculó
- `Total Físico = Inv. Locación + Inv. WIP`
- `Diferencia = Físico − Saldo Anexo`
- `Estatus` y `Severidad` recalculados; las partidas donde el archivo original no coincidía se marcan con la etiqueta **corregido** (ej. `MDFVNYL01`).
