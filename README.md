# Organigrama (prototipo jugable)

Juego de organigramas con **tres escenarios jugables**: Hospital, Estudio de Videojuegos y Supermercado. Cada uno tiene las dos fases del GDD (lluvia de áreas + tablero de armado y conexiones), pantalla de tutorial (con explicación de qué es un organigrama), selector de dificultad (Fácil/Normal/Difícil) y pantalla de victoria con tiempo y errores.

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

- **Desktop:** click para atrapar áreas en la fase 1, arrastrar con mouse las cajas en la fase 2, y arrastrar una línea desde cada área hasta su superior para conectarla.
- **Celular:** tocar para atrapar, y arrastrar con el dedo tanto las cajas como las líneas de conexión (todo con Pointer Events, no con drag-and-drop nativo de HTML, así que anda igual en touch).
- El botón **?** (arriba a la derecha, ya no se superpone con el cronómetro) reabre el tutorial en cualquier momento.
- El selector de dificultad (pantalla de selección de nivel) solo cambia la velocidad de caída y de aparición de áreas en la fase 1 — nada más. Cada escalón de dificultad quedó más lento que antes (Difícil ≈ la vieja Normal, Normal ≈ la vieja Fácil, y Fácil es aún más lenta que eso).
- Al conectar las áreas, el nivel se reordena solo para que las líneas no se crucen (agrupa cada área bajo su superior directo). En pantallas angostas, si un nivel tiene muchas áreas, se angostan para entrar todas en una fila en vez de saltar de línea (saltar de línea también produce cruces).
- Los tres escenarios (Hospital, Estudio de Videojuegos, Supermercado) ya están desbloqueados y son totalmente jugables desde la pantalla de selección de nivel. Cada uno usa como distractores en la fase 1 las áreas de los otros dos escenarios.

## Estructura de datos (por si querés ajustar el organigrama)

Adentro del `<script>`, al principio, está el objeto `LEVELS`, con una entrada por escenario (`hospital`, `videogames`, `supermarket`). Cada entrada tiene `label` (el nombre que se muestra en el HUD y en la pantalla de victoria) y `nodes` (los nodos del organigrama con `id`, `name`, `tier` de 1 a 3, `parent` e `icon`). Los distractores de la fase 1 de cada escenario se calculan solos a partir de los nodos de los otros escenarios (función `getDistractorPool`), así que no hay que mantener una lista aparte.

Las velocidades de cada dificultad están en el objeto `DIFFICULTIES` (`fallBase`, `fallMin`, `spawnMs` por nivel de dificultad).

Para sumar un cuarto escenario: agregar una entrada más a `LEVELS` con la misma forma (nodos en tier 1/2/3, cada uno con su `parent`), y una `.level-card` más en el HTML con su `data-level` correspondiente. No hace falta tocar la lógica de fases (lluvia, drag & drop, conexiones): es genérica y ya funciona para cualquier jerarquía de 3 niveles.

## Estado de este prototipo

Los tres escenarios están jugables de punta a punta (fase 1 → fase 2 → conexiones → victoria), en las 3 dificultades. Cosas que quedaron afuera de este corte, para sumar después si querés:

- Guardar el mejor tiempo entre partidas (hoy se resetea al recargar la página).
- Animar el reordenamiento de las cajas (hoy el orden se corrige al instante, sin transición).
