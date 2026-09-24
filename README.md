# Organigrama — Hospital (prototipo jugable)

Primer nivel completo del juego de organigramas: **Hospital**, con las dos fases del GDD (lluvia de áreas + tablero de armado y conexiones), pantalla de tutorial (con explicación de qué es un organigrama), selector de dificultad (Fácil/Normal/Difícil), selección de nivel (los otros dos escenarios están dejados como "Próximamente") y pantalla de victoria con tiempo y errores.

Es un único archivo `index.html` sin dependencias externas ni build: funciona abriéndolo directamente en el navegador, y es lo único que necesita GitHub Pages para publicarlo.

## Cómo subirlo a GitHub Pages con GitHub Desktop

1. En GitHub Desktop: **File → New repository**. Elegí un nombre (ej. `organigrama-juego`) y como "Local Path" una carpeta vacía.
2. Copiá `index.html` (y este `README.md` si querés) adentro de esa carpeta local del repo.
3. En GitHub Desktop vas a ver los archivos como cambios sin confirmar. Escribí un mensaje de commit y hacé **Commit to main**.
4. **Publish repository** (arriba a la derecha). Podés dejarlo público o privado — para GitHub Pages gratis en un repo normal tiene que ser público.
5. Andá a la página del repo en GitHub.com → **Settings → Pages**.
6. En "Build and deployment" → "Source" elegí **Deploy from a branch**, branch **main**, carpeta **/(root)** → **Save**.
7. Esperá 1-2 minutos y arriba te va a aparecer la URL (algo como `https://tu-usuario.github.io/organigrama-juego/`). Ahí ya lo podés abrir desde PC o desde el celular.

Cada vez que hagas un cambio: commit + push desde GitHub Desktop, y GitHub Pages lo redespliega solo en un par de minutos.

## Qué probar

- **Desktop:** click para atrapar áreas en la fase 1, y arrastrar con mouse en la fase 2.
- **Celular:** tocar para atrapar, y arrastrar con el dedo en la fase 2 (funciona con Pointer Events, no con drag-and-drop nativo de HTML, así que anda igual en touch).
- El botón **?** (arriba a la derecha) reabre el tutorial en cualquier momento.

## Estructura de datos (por si querés ajustar el organigrama)

Adentro del `<script>`, al principio, están `HOSPITAL_NODES` (los 8 nodos del organigrama con `id`, `name`, `tier` de 1 a 3, `parent` y `icon`) y `DISTRACTORS` (las áreas de otras industrias que caen como señuelo). Cambiar nombres, íconos o la jerarquía es cuestión de editar esos dos arrays.

## Cómo sumar los otros dos escenarios (Videojuegos, Supermercado)

El código ya está armado para eso, aunque hoy solo hay data cargada para Hospital:

- Habría que convertir `HOSPITAL_NODES` en un objeto `LEVELS = { hospital: {...}, videogames: {...}, supermarket: {...} }`, con sus propios nodos y, para cada uno, usar como distractores los nodos de los otros dos escenarios.
- `startGame()` ya recibe el `level` clickeado desde `.level-card` — hoy lo ignora y siempre arma Hospital; solo falta que lea `LEVELS[level]` en vez de la constante fija.
- Sacar la clase `locked` de las otras dos `level-card` en el HTML cuando tengan su data.

No hace falta tocar nada de la lógica de fases (lluvia, drag & drop, conexiones): es genérica y ya funciona para cualquier jerarquía de 3 niveles.

## Estado de este prototipo

Probado de punta a punta (fase 1 → fase 2 → conexiones → victoria) en viewport de escritorio y de celular. Cosas que quedaron afuera de este primer corte, para sumar después si querés:

- Los otros dos escenarios (Videojuegos, Supermercado).
- Guardar el mejor tiempo entre partidas (hoy se resetea al recargar la página).
- Reordenar automáticamente las cajas dentro de cada nivel para minimizar cruces de líneas (el GDD lo menciona; hoy las líneas se ven bien pero pueden cruzarse si las áreas quedan en un orden poco prolijo).
