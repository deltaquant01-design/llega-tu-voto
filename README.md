# ¿Llega tu voto?

Probabilidad de escaño en las 52 circunscripciones para las generales del 29 de noviembre de 2026, con los resultados oficiales del 23-J y la última encuesta del CIS. Dice, provincia a provincia, si un voto a la izquierda del PSOE (Sumar o Podemos) llega a escaño o se pierde, y dónde un punto más para el PSOE decide un diputado.

Es una página estática: no tiene servidor, ni base de datos, ni cuentas. Se instala en el móvil como app y funciona sin conexión.

**Autor:** David S. Iglesias · Estrategia & Letras · @D_S_Iglesias

## Qué hay en esta carpeta

- `index.html`, `sw.js`, `manifest.webmanifest`, `icons/`: la app ya construida. Es lo que sirve GitHub Pages desde la raíz.
- `fuente/`: el código, los datos y las pruebas para regenerarla.

## Cómo se calcula

1. **Base.** Resultados oficiales del 23-J de 2023 por circunscripción. El reparto D'Hondt con barrera del 3 % provincial reproduce los 350 escaños reales (lo comprueban las pruebas).
2. **Encuesta.** El CIS da porcentajes nacionales. Se trasladan a cada provincia con el cambio proporcional de cada partido respecto a 2023, ajustando para que cada provincia sume 100 % y el total nacional coincida con la encuesta. Los partidos territoriales solo puntúan donde ya tenían voto.
3. **Listas locales.** Las que superaron el 2 % en 2023 (Soria ¡Ya!, Teruel Existe, UPL, Por Ávila, Nueva Canarias, CUP y otras) compiten con su peso de entonces. El resto de «Otros» crece de forma pareja en todas las provincias.
4. **Probabilidad.** 4.000 simulaciones con error común de encuesta y ruido propio de cada provincia. La cifra es el porcentaje de simulaciones en que el partido saca al menos un escaño.
5. **Veredicto.** 70 % o más, «llega». Entre 30 y 70 %, «en el alambre». Menos de 30 %, «se pierde».

## Límites que conviene decir en voz alta

- No hay encuestas provinciales oficiales. La traslación a provincias es un modelo, no una medición.
- En 2023 Podemos y Sumar fueron juntos casi en todas partes. La encuesta fija el total de cada uno; dentro de cada provincia se supone que reparten el voto de Sumar 2023 en proporción.
- Vox no se presentó en Santa Cruz de Tenerife en 2023. Se le asigna la cuota media de Canarias.
- Las cifras de «Otras encuestas» (media de Sigma Dos y 40dB) están tomadas de prensa y no se han verificado en origen. Están marcadas como aproximadas.
- El CIS y el resto de sondeos difieren varios puntos entre sí. Cambiar de encuesta en la página cambia el titular.
- El ruido de las simulaciones (8 % en los grandes, 15 % en los pequeños, 6 % provincial) es razonable pero no está calibrado contra errores históricos.

## Fuentes

- Resultados 23-J: Ministerio del Interior, Congreso de los Diputados, julio 2023, por circunscripción (`fuente/datos/`).
- Encuesta: [CIS, barómetro de septiembre 2026](https://www.cis.es/documents/d/guest/es3577mar_A-pdf) (estudio 3577, campo 1-4 de septiembre, n = 4.042).
- Sigma Dos y 40dB: citadas por prensa.

## Publicar en GitHub Pages

1. Sube el contenido de esta carpeta a un repositorio público.
2. En *Settings*, *Pages*: *Deploy from a branch*, rama `main`, carpeta `/ (root)`.
3. En uno o dos minutos queda en `https://USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/`.

Todas las rutas son relativas, así que funciona colgando de una subcarpeta.

## Actualizar con un CIS nuevo

1. Edita las cifras en `fuente/scripts/make-data.mjs` (objetos `cis` y `otras`) y la etiqueta del barómetro.
2. En `fuente/`: `node scripts/make-data.mjs`, `npm test`, `node scripts/build.mjs`, `node scripts/build-standalone.mjs`, `python3 scripts/make-icons.py`, `node scripts/build-pwa.mjs`.
3. Copia el contenido de `fuente/dist-pwa/` a la raíz del repositorio y confirma los cambios. Las apps ya instaladas se renuevan solas al abrirse con internet.

## Requisitos para reconstruir

Node 20 o superior para las pruebas y la construcción (`npm test`). Python 3 con Pillow para los iconos. Para volver a extraer los datos del fichero oficial hace falta además `pip install numbers-parser`.

## Estructura de `fuente/`

- `src/dhondt.js`, `src/model.js`, `src/sim.js`: reparto, proyección provincial por swing con raking y simulación Monte Carlo.
- `src/data.js`: generado por `scripts/make-data.mjs` a partir de `prov2023.json` y de las cifras de las encuestas.
- `src/ui.js`, `src/style.css`, `src/index.template.html`: la interfaz.
- `tests/`: 39 pruebas, entre ellas la que reproduce los 350 escaños de 2023 y las que cubren Ceuta, Melilla, Soria y las listas locales.
- `docs/superpowers/`: especificación de diseño y plan de implementación.

Salud, fuerza y rectitud.
