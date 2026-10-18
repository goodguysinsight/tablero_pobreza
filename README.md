# Pobreza monetaria en el Perú, 2004-2025

Tablero interactivo con la incidencia de la pobreza monetaria y de la pobreza extrema por departamento, área urbana y rural y a nivel nacional, a partir de la Encuesta Nacional de Hogares (ENAHO) del INEI, módulo 34 (Sumaria).

## Contenido

| Ruta | Descripción |
|---|---|
| `index.html` | Tablero autocontenido: incluye los datos y los límites departamentales (funciona solo). |
| `data/pobreza.js` | Resultados del do-file por año y ámbito (660 filas), copia de los datos incluidos en `index.html`. |
| `data/limites.js` | Contornos simplificados de los 25 departamentos (INEI). |
| `data/pobreza_monetaria_2004_2025.csv` | Los mismos resultados en CSV. |
| `stata/pobreza_departamental_2004_2025.do` | Descarga, procesamiento y estimación en Stata. |

## Publicar en GitHub Pages

1. Cree un repositorio (por ejemplo, `tablero_pobreza`) y suba todos los archivos manteniendo la estructura de carpetas.
2. En **Settings → Pages**, elija *Deploy from a branch*, rama `main`, carpeta `/ (root)`.
3. El tablero quedará en `https://<usuario>.github.io/tablero_pobreza/`.

El tablero también se abre localmente con doble clic en `index.html` (requiere conexión para cargar d3 y las fuentes).

## Actualizar los datos

1. Ejecute `stata/pobreza_departamental_2004_2025.do` (agregue el código INEI del nuevo año en `local cAAAA`).
2. Copie `cap0201.xlsx` (población oficial INEI al 30 de junio) en `inputs/`; el do-file la usa para calcular las personas en pobreza.
3. Convierta la hoja `BD_larga` del Excel resultante al formato `window.PM_POBREZA = [ ... ];` (un objeto por fila, con los nombres de variable del `.dta`) y reemplace ese bloque dentro de `index.html`. El tablero no recalcula nada: toma tasas, intervalos, población oficial y personas en pobreza tal como salen de Stata.

## Metodología

- Pobreza = `pobreza` 1 o 2; pobreza extrema = `pobreza` 1.
- Ponderador: `factor07 × mieperho`; diseño muestral con conglomerado (`conglome`) y estrato (`dominio × estrato`).
- Urbano: estrato 1-5; rural: estrato 6-8.
- Lima comprende Lima Metropolitana (provincia 1501) y Lima Provincias; Callao se presenta por separado.
- Calidad de la estimación (criterio INEI): CV mayor a 15% es referencial; en otro caso, confiable.
- Las cifras nacionales coinciden con las publicadas por el INEI en los 22 años (2004-2025).

Fuente: INEI, ENAHO 2004-2025. Elaboración: Good Guys Insight.
