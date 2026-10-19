# Pobreza monetaria en el Perú, 2004-2025

Tablero interactivo con los principales indicadores de pobreza monetaria por departamento, área urbana y rural y a nivel nacional, a partir de la Encuesta Nacional de Hogares (ENAHO) del INEI, módulo 34 (Sumaria).

## Indicadores

| Indicador | Unidad | Años |
|---|---|---|
| Pobreza monetaria (incidencia, P0) | % de la población | 2004-2025 |
| Pobreza extrema | % de la población | 2004-2025 |
| Brecha de la pobreza (P1) | % | 2004-2025 |
| Severidad de la pobreza (P2) | % | 2004-2025 |
| Gasto real per cápita mensual | Soles de 2025 a precios de Lima Metropolitana | 2004-2025 |
| Vulnerabilidad monetaria (no pobres vulnerables) | % de la población | 2020-2025 |

Cada indicador incluye intervalo de confianza al 95%, coeficiente de variación y calidad de la estimación.

## Mapas: nivel y variación anual

El mapa departamental tiene dos modos:

- **Nivel**: seis grupos de departamentos (sextiles del año elegido). El rojo más oscuro corresponde a la situación más grave: mayor pobreza, brecha, severidad o vulnerabilidad, o menor gasto real.
- **Variación anual**: cambio respecto del año anterior, en puntos porcentuales para las tasas y en variación porcentual para el gasto real. Usa una paleta divergente rojo–azul oscuro:
  - **El azul significa mejora y el rojo deterioro, en todos los indicadores.**
  - En pobreza, pobreza extrema, brecha, severidad y vulnerabilidad, **bajar es azul**; en gasto real, **subir es azul**.
  - El blanco indica un cambio pequeño, dentro del primer umbral (±1 pp en pobreza monetaria; umbrales menores en pobreza extrema, brecha y severidad).
  - Los umbrales de las clases son fijos, por lo que los mapas de distintos años son comparables.
  - En el primer año de cada serie (2004; 2020 en vulnerabilidad) no hay variación.

El ranking sigue el modo elegido: en variación, las barras se ordenan del mayor deterioro a la mayor mejora.

## Contenido

| Ruta | Descripción |
|---|---|
| `index.html` | Tablero autocontenido: incluye los datos y los límites departamentales (funciona solo). |
| `data/pobreza.js` | Resultados del do-file por año y ámbito (660 filas), copia de los datos incluidos en `index.html`. |
| `data/limites.js` | Contornos simplificados de los 25 departamentos (INEI). |
| `pobreza_monetaria_2004_2025.csv` | Los mismos resultados en CSV (no se enlaza desde el tablero). |
| `stata/pobreza_departamental_2004_2025.do` | Descarga, procesamiento, estimación y validación en Stata. |

## Publicar en GitHub Pages

1. Cree un repositorio (por ejemplo, `tablero_pobreza`) y suba todos los archivos manteniendo la estructura de carpetas.
2. En **Settings → Pages**, elija *Deploy from a branch*, rama `main`, carpeta `/ (root)`.
3. El tablero quedará en `https://<usuario>.github.io/tablero_pobreza/`.

El tablero también se abre localmente con doble clic en `index.html` (requiere conexión para cargar d3 y las fuentes).

## Actualizar los datos

1. Ejecute `stata/pobreza_departamental_2004_2025.do` (agregue el código INEI del nuevo año en `local cAAAA`).
2. Copie en `inputs/` el archivo `cap0201.xlsx` (población oficial INEI al 30 de junio). El do-file extrae los deflactores del gasto real (`deflactores_base2025_new.dta` y `despacial_ldnew.dta`) del módulo 34 de 2025; si no puede, cópielos también en `inputs/`.
3. Convierta la hoja `BD_larga` del Excel resultante al formato `window.PM_POBREZA = [ ... ];` (un objeto por fila, con los nombres de variable del `.dta`) y reemplace ese bloque dentro de `index.html`. El tablero no recalcula nada: toma tasas, intervalos, variaciones, población oficial y personas en pobreza tal como salen de Stata.

## Metodología

- Pobreza = `pobreza` 1 o 2; pobreza extrema = `pobreza` 1.
- Brecha (P1) y severidad (P2): índices de Foster, Greer y Thorbecke con el gasto per cápita mensual `gashog2d / (12 × mieperho)` y las líneas `linea` (total) y `linpe` (extrema).
- Gasto real: réplica del kit oficial del INEI (ConstVarGasto): 8 grupos de gasto deflactados con el IPC de cada grupo por año y departamento y con el deflactor espacial; toda la serie en soles de 2025 a precios de Lima Metropolitana.
- Vulnerabilidad: población no pobre con gasto per cápita bajo la línea de vulnerabilidad del INEI (`pobrezav` = 3). La Sumaria solo trae esa línea desde 2020; el INEI no publica la vulnerabilidad por departamento.
- Ponderador: `round(factor07 × mieperho)`, como en el kit del INEI; diseño muestral con conglomerado (`conglome`) y estrato (`dominio × estrato`).
- Urbano: estrato 1-5; rural: estrato 6-8.
- Lima comprende Lima Metropolitana (provincia 1501) y Lima Provincias; Callao se presenta por separado.
- Personas en pobreza: tasa multiplicada por la población oficial estimada por el INEI al 30 de junio.
- Calidad de la estimación (criterio INEI): CV mayor a 15% es referencial; en otro caso, confiable.
- El do-file incluye hojas de validación contra el informe técnico del INEI *Evolución de la pobreza monetaria 2016-2025* (incidencia, brecha, severidad, gasto real y vulnerabilidad).

Fuente: INEI, ENAHO 2004-2025. Elaboración: Good Guys Insight.
