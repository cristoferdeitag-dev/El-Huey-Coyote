# BITÁCORA — El Huey Coyote

Memoria viva del proyecto. Entradas más recientes arriba. Nunca borrar historial, solo agregar.

## 2026-07-10 · anette (cont. 46) — MÓVIL "Sobre mí" P3: texto centrado EXACTO en la línea gris
- **Ani (2231):** "baja un chirris el texto de la línea gris, debe quedar perfectamente centrado con la línea gris".
- **Diagnóstico medido (no a ojo):** la "línea gris" es la banda oscura (#38 4e 44 aprox) inclinada del póster 3. Ajusté su eje por regresión sobre el fondo `conoces-fondo-v2.webp`: **ángulo real 11.009°** (el texto tenía 10.8493), eje va de **x=500 a x=963** → centro a lo largo **x=731.5** (NO 752.1 como asumió cont.45; ese valor venía del rect del SVG, no medido), **grosor perpendicular ~44px** (±22 del eje).
- **Cap height real de la fuente:** extraje `sCapHeight=719/1000` del woff2 `swiss-721-bt-n9` → a 25.5px las mayúsculas miden 18.33px. El centro óptico de un texto en mayúsculas es `baseline − capHeight/2` = baseline − 9.17 (NO el bbox de tinta: el acento de "QUÉ" sube a 24.6 y "¿"/"Q" bajan 2px bajo la baseline; centrar por bbox lo habría dejado chueco).
- **Fix:** `translate(752.1 1701) rotate(10.8493)` → **`translate(731.5 1707.8) rotate(11.009)`** (línea ~1989, `text.nvblk`). Baja 6.8px, recorre 20.6px a la izq, y empata el ángulo con la banda.
- **Verificación en navegador (Playwright, 390px):** mapeé los píxeles del texto (#ccc) al marco de la banda (u = a lo largo, v = perpendicular).
  - ANTES: v ∈ [−19.0, −1.1], centro **−10.0** → holgura 3.0 arriba vs 23.1 abajo (se salía por arriba). u centro +20.4 (cargado a la der.).
  - DESPUÉS: v ∈ [−8.8, +8.9], centro **+0.1** → holgura **13.2 arriba / 13.1 abajo**. u centro +2.4, márgenes 70.0 izq / 65.2 der. Centrado en ambos ejes.
- **Gotcha preview:** `#portada.portada-overlay` tapa la sección en el screenshot; hay que `display:none` antes de capturar. Y el server local cachea `index.html` → recargar con `?bust=`.
- **Deploy:** push `461ad94` → GH Actions → elhueycoyote.com/preview/sitio/.
- **PENDIENTE (sin cambios):** título/subtítulo reales p2 y p3, decisión título p3, "El mero mero".

## 2026-07-10 · anette (cont. 45) — MÓVIL "Sobre mí": contorno azul resumen P1 + HUEY rojo/COYOTE verde P2
- **Ani (2224):** 3 correcciones: (1) resumen P1 le falta contorno AZUL; (2) P2 faltan colores de "Huey" y "Coyote"; (3) P3 texto gris "¿POR QUÉ SE HIZO" incompleto.
- **(1) HECHO:** añadí clase `.nvblue` (fill+stroke #0477c3 15px, paint-order stroke fill, linejoin/linecap round) y una 3ª capa `<text class="nvblue">` con las mismas 5 tspans del resumen, ANTES de nvout. Orden de pintado: azul (abajo, stroke 15) → blanco (nvout, stroke 9) → rojo (nvred). Verificado en render: rojo→blanco→azul. ⚠️ CORRIGE cont.44 (que había puesto solo blanco+rojo — Ani SÍ quiere el azul, el #0477c3 del SVG eran polígonos pero el resumen SÍ va con contorno azul).
- **(2) HECHO:** en nvital SU NOMBRE (línea ~1978), "HUEY" envuelto en `<tspan fill="#c1272d">` (rojo). En nvital COYOTE (línea ~1979), "COYOTE" en `<tspan fill="#244f37">` (verde). Colores tomados del SVG de Ani (st7 rojo / st62 verde). Verificado en render.
- **(3) HECHO:** Ani (msg 2227) dio el texto: "¿POR QUÉ SE HIZO FAMOSO?" en mayúsculas. Cambié la tspan; a 30.4px "FAMOSO?" se pegaba al borde der. del póster → bajé a **25.5px**. Ani (2229) pidió centrarlo en la barra. Detecté por análisis de color la barra gris DENTRO del póster (SVG x 509.7→994.4, centro **752**, cy 1694.8; hay que excluir el fondo metálico gris que contamina). Cambié a `text-anchor="middle"` translate **752.1 1701** (baja el baseline para centrar vertical también). Verificado: centrado horizontal y vertical en la barra.
- **Deploy:** push `0870ff6` (fixes 1+2, run 29064874173 OK) y `772b952` (fix 3). Vivo en elhueycoyote.com/preview/sitio/.
- **PENDIENTE:** los de cont.44 (título/subtítulo reales p2/p3, decisión título p3, "El mero mero"). Las 3 correcciones del msg 2224+2227 CERRADAS.

## 2026-07-10 · anette (cont. 44) — MÓVIL "Sobre mí": NUEVO fondo limpio + overlay RECONSTRUIDO desde SVG de Ani
- **Ani (2218/2219):** mandó (1) nueva imagen de fondo LIMPIA (pósters sin texto horneado — el filler ya no está; poster 1 conserva logo+EL VATO+PARA LATAM que son arte) y (2) el SVG `Yameconoces-movil_¿Ya me conoces-.svg` con el texto CORRECTO + posiciones + tamaños. Objetivo: cambiar fondo + reconstruir todo el texto vivo para que calce EXACTO con el SVG.
- **Fondo:** PNG 1086×1937 → `assets/conoces/conoces-fondo-v2.webp` (403KB). `<img class="bg">` ahora `conoces-fondo-v2.webp?v=0710v2`.
- **Overlay reconstruido (`<g id="Texto">` reemplazado, 16 <text> nuevos, clases nv*):** el SVG usa el MISMO viewBox (1086.3×1936.5) → transforms calzan directo. Fuentes mapeadas: Swiss721BT-Black→swiss-721-bt 900 (nvblk), Bold→700 (nvbold), BoldItalic→700 italic (nvital). Resumen p1 = 2 capas (nvout blanco stroke 9 + nvred rojo) — el diseño NUEVO es contorno BLANCO+rojo (NO azul; el #0477c3 del SVG son polígonos decorativos st45, ya en el fondo). Extraje texto+tamaños+fills del SVG por script (`scratchpad/rebuild_overlay.py`).
- **Texto correcto por elemento (del SVG):** p1 resumen "CANTAUTOR MEXICANO…PUEDE SER HUMOR PURO." (24px, translate 521.7 768 rot6.7 scale.7 skewX.3). p2: NACIÓ EN: GUADALAJARA/HACE DE TODO: CANTA,/TOCA 21 INSTRUMENTOS,/COMPONE Y BAILA. (st2 22px ital, 24.5 974). SU NOMBRE (234.46 1093.88 sc.7593) + COYOTE (255.02 1179.56). UN MÚSICO QUE/SIEMPRE BUSCA/SACARTE UNA/SONRISA (st5 32px, 199.82 1323.92). Título/subtítulo p2 siguen placeholder (UN TÍTULO COOL/AQUÍ UN SUBTÍTULO). p3: cuerpos st29/st26/st23, "Cervantino"(st15)/"Cumbre Tajín"(st19) (reemplazan Palabra súper cool), "¿POR QUÉ SE HIZO"(st37, reemplaza ALGO PARA RESALTAR), título "TEXTO MÁS TEXTO"+subtítulo "SUBTÍTULO LINDO Y NICE" placeholder.
- **⚠️ Título p3 pendiente de decidir:** el SVG trae "TEXTO MÁS TEXTO" (placeholder) pero Ani (msg 2214) pidió "Hace canciones sobre absolutamente TODO." ahí. Dejé el del SVG y le pregunté cuál. (Al 34px del SVG "Hace canciones…" no cabe en 1 renglón.)
- **Harness preview:** `preview7.cjs` ahora intercepta `conoces-fondo-v2.webp`→`bgv2_full.png`. Clases cv* viejas quedan sin uso (inocuas).
- **PENDIENTE:** título/subtítulo reales p2 y p3; decisión título p3; "El mero mero" (st20 del SVG, Monotxt) NO incluido (no está en fondo nuevo ni overlay — ver si Ani lo quiere).

## 2026-07-09 · anette (cont. 43) — MÓVIL: texto2 cabe en póster, texto3 más grande, + TÍTULO póster 3
- **Ani (2214):** (a) texto2 (SU NOMBRE): que no se salga del póster 2 SIN moverlo de lugar. (b) texto3 (UN MÚSICO): aún MÁS grande, centrado entre las 2 estrellitas. (c) Póster 3: donde dice "TEXTO MÁS TEXTO" poner "Hace canciones sobre absolutamente TODO." (resto del póster 3 NO tocar aún).
- **cv71 (texto2):** 17→**15px** (letter-spacing -0.3), MISMA posición translate(215 1095). A 15px las 3 líneas caben dentro del póster con margen.
- **cv72 (texto3):** 18→**20px** (más grande). Estrellas localizadas por análisis de color (rojo oscuro): SVG (221,1325) y (447,1326), punto medio **x=334**; el hueco limpio entre guitarra (der. x~250) y violín (izq x~414) es angosto (~164). Reescrito en **4 renglones cortos** (UN MÚSICO QUE / SIEMPRE BUSCA / SACARTE UNA / SONRISA.) para caber centrado sin tocar instrumentos. text-anchor middle, translate(334 1286).
- **cv19 (título póster 3):** era texto vivo. "TEXTO MÁS TEXTO" → "Hace canciones sobre / absolutamente TODO." (2 renglones, `.cv19` 33.8→**20px**, subido translate y 1215.9→1188 para caber arriba del subtítulo sin tocarlo). Casing tal cual la escribió Ani (TODO en mayúsculas). NOTA: el título quedó más chico que el subtítulo placeholder (SUBTÍTULO LINDO Y NICE) — temporal, el subtítulo se cambiará después.
- **PENDIENTE:** subtítulo + filler + "Palabra súper cool" + "ALGO PARA RESALTAR" del póster 3 (Ani los dará después). Título/subtítulo p2 (UN TÍTULO COOL / AQUÍ UN SUBTÍTULO) siguen placeholder.

## 2026-07-09 · anette (cont. 42) — MÓVIL "Sobre mí": texto2 al CUADRADO morado + texto3 más grande
- **Ani (2210, captura cuadrado morado):** texto2 (SU NOMBRE) DEBE ir en el cuadrado morado (área cream a la DER. del sax, bajo la guitarra acústica, arriba del violín) y NO salirse de él. Texto3 (UN MÚSICO): un poco MÁS grande que los otros dos (17px).
- **cv71 (texto2):** movido a `translate(215 1095)` (dentro del cuadrado, der. del sax), sigue 17px. Las 3 líneas caben: línea1 libra el sax, líneas 2-3 (largas) llegan casi al borde der. del póster pero DENTRO. Left-align desde x215 (borde izq del cuadrado).
- **cv72 (texto3):** subido 17→**18px** (más grande que texto1/2). Las 2 estrellitas rojas están MUY juntas (~SVG 163 y 334, gap ~171) y el renglón1 es largo (~225 a 18px > gap) → no cabe ENTRE ellas sin tocar; lo recentré en `translate(255 1300)` sobre el punto medio de las estrellas (text-anchor middle). Queda balanceado entre las 2 (izq abajo, der arriba), diagonal. "BUSCA" queda cerca de la estrella der. (la inclinación lo pasa apenas por arriba) — 18px es ~lo más grande que cabe entre estrellas.
- **PENDIENTE:** título/subtítulo p2 reales; texto real póster 3.

## 2026-07-09 · anette (cont. 41) — MÓVIL "Sobre mí": textos p2 todos a 17px + MAYÚSCULAS confirmado
- **Ani (2206):** póster 1 "ya quedó perfecto, no le muevas" (20px se queda). Póster 2: texto1 (NACIÓ) bajar un chirris; texto2 (SU NOMBRE) y texto3 (UN MÚSICO) al MISMO tamaño que texto1 (**17px**); texto2 que no salga del póster; texto3 centrado ENTRE las 2 estrellitas. **Confirmó: TODO el póster 2 en MAYÚSCULAS** (ya estaba así — buena decisión de consistencia).
- **cv70 (texto1):** translate y 930→940 (bajado un chirris). Sigue 17px.
- **cv71 (texto2 SU NOMBRE) 14→17px:** a 17px las 3 líneas largas NO caben a la derecha del sax (se salían del póster) ni arriba-izq (teclado+sax bloquean; la inclinación -12.4 sube la línea 1 al saxofón). Probé text-anchor:end (der.) → la inclinación BAJA el extremo izq de las líneas largas y chocaban con texto3. Solución: left-align en la banda LIMPIA debajo del sax/teclado: `translate(35 1216)` gap 30. Línea 1 apenas libra el sax; líneas 2-3 limpias; dentro del póster. Las 2 estrellitas rojas: izq ~(153,1332), der ~(417,1281).
- **cv72 (texto3 UN MÚSICO) 14→17px + text-anchor:middle:** `translate(285 1302)` (=punto medio X de las 2 estrellitas) centrado. 2 líneas gap 32. Queda centrado entre las estrellitas, sin chocar guitarra/violín.
- **NOTA a Ani:** texto2 a 17px no cabía a la derecha del sax sin salirse → lo puse en la banda limpia justo debajo del sax. Si lo quiere en otro lado, avisar.
- **PENDIENTE:** título/subtítulo p2 reales; texto real póster 3.

## 2026-07-09 · anette (cont. 40) — MÓVIL "Sobre mí": +tamaño p1, reubicar cv71 (morado) + TERCER texto cv72 (verde)
- **Ani (2202, captura marcas morado+verde):** (a) póster 1 aún MÁS grande. (b) párrafo p2 "Nació en Guadalajara" un chirris más grande + subirlo un chirris. (c) el 2º texto (SU NOMBRE) va en la marca MORADA (der. del sax, bajo la guitarra acústica) — lo reubiqué ahí. (d) TERCER texto NUEVO en la marca VERDE (entre guitarra eléctrica y violín): "Un músico que siempre busca sacarte una sonrisa."
- **Póster 1:** `.cv16,.cv20` **18→20px**. OJO: a 20px "MEZCLA" (renglón 2) toca la esquina DOBLADA del póster. Avisado a Ani; si no le gusta, bajar a 19 o re-wrap. Abajo no se desborda ("HUMOR PURO." cabe).
- **Párrafo p2 cv70:** **16→17px** + subido (translate y 938.1→930). Libra guitarra/estrellitas.
- **cv71 (SU NOMBRE):** movido translate (80 1240)→**(255 1140)** = área morada, der. del sax, bajo guitarra acústica. Libra sax (izq) y violín (abajo-der).
- **cv72 (NUEVO, verde):** `.cv72` swiss-721-bt 700 14px. `<text class="cv72" transform="translate(230 1330) rotate(-12.4) scale(.8 1)">` 2 líneas: "UN MÚSICO QUE SIEMPRE BUSCA / SACARTE UNA SONRISA." en el hueco entre guitarra eléctrica (izq) y violín (der). Insertado tras cv71.
- **Caso:** cv72 y cv71 en MAYÚSCULAS (consistencia). Ani los escribe en minúsculas → avisado.
- **PENDIENTE:** título/subtítulo p2 reales; texto real póster 3 (baked HISTORIE TIME).

## 2026-07-09 · anette (cont. 39) — MÓVIL "Sobre mí": +tamaño ambos textos + SEGUNDO texto p2 (Su nombre significa…)
- **Ani (2198, captura marca morada):** "está súper bien el acomodo". (a) aumentar un poco el tamaño de ambos textos. (b) agregar el SEGUNDO texto del póster 2 en la zona MORADA (área vacía central, debajo del saxofón): "Su nombre significa… / Huey = grande, ingenioso, magnífico. / Coyote = curioso, juguetón y divertido." (= el texto viejo que se quitó en cont.37, ahora regresa como bloque aparte).
- **Tamaños:** resumen p1 `.cv16,.cv20` **17→18px** (a 18 "MEZCLA/DEMUESTRA" quedan al filo del doblez, legibles; si toca, bajar). Párrafo p2 `.cv70` **15→16px** (libra guitarra y estrellitas).
- **Segundo texto (NUEVO `.cv71`):** swiss-721-bt 700, 14px, #241a1a. `<text class="cv71" transform="translate(80 1240) rotate(-12.4) scale(.8 1)">` 3 líneas gap 30. Colocado en el hueco limpio central (debajo del sax, arriba de guitarra eléctrica/violín, a la izq del violín). Iteré posición 1190→1240 (a 1190 encimaba el saxofón). Insertado en el SVG después de cv70.
- **Caso:** cv71 lo dejé en MAYÚSCULAS (consistente con el 1er párrafo y con la petición de cont.36); Ani lo escribió en minúsculas → avisado, cambio fácil si lo quiere así.
- **PENDIENTE:** título/subtítulo p2 reales (placeholder); texto real póster 3 (baked HISTORIE TIME).

## 2026-07-09 · anette (cont. 38) — MÓVIL "Sobre mí": ajustes de salto de línea (MEZCLA up p1, NACIÓ solo p2)
- **Ani (2194):** (a) Póster 1: subir "MEZCLA" al mismo renglón que "CANCIÓN," y reacomodar. (b) Póster 2: "Nació en Guadalajara." en su PROPIO renglón y "Hace de todo:…" en renglón aparte (deshacer la unión de cont.37-seguimiento).
- **Póster 1:** reflow con MEZCLA arriba → renglón 2 "CUALQUIER IDEA EN UNA CANCIÓN, MEZCLA" (37 chars) se estiraba hasta la esquina DOBLADA del póster a 20px. Bajé `.cv16,.cv20` **20→17px** (37 chars@17 ≈ ancho de 31 chars@20 = zona limpia), contornos `.cv60` 10→9, `.cv16` 6→5. 5 líneas (y 0/34/68/102/136): CANTADOR…CONVIERTE / …CANCIÓN, MEZCLA / GÉNEROS…DEMUESTRA / QUE LA MÚSICA…SER / HUMOR PURO. Cabe sin tocar el doblez.
- **Póster 2:** revertido a 4 renglones con "NACIÓ EN GUADALAJARA." solo (tspans y 40/63/86/109), mantengo **15px** (más grande que los 14 orig). Libra guitarra y estrellitas. Ani ACEPTA que el renglón 1 quede corto (lo quiere como frase aparte).
- **PENDIENTE:** título/subtítulo p2 reales; texto real póster 3 (baked HISTORIE TIME).

## 2026-07-09 · anette (cont. 37) — MÓVIL "Sobre mí": TEXTO NUEVO pósters 1 y 2 (copy real)
- **Ani (2185/2186):** cambiar copy. Aclaró: la imagen que mandó es la versión ESCRITORIO (referencia), los cambios van en MÓVIL. (a) Póster 2 párrafo: quitar "Su nombre significa…" → "Nació en Guadalajara. / Hace de todo: Canta, toca 21 instrumentos, compone, baila y tiene un ameizing estilo." (b) Póster 1 resumen (abajo de EL VATO): quitar "Nacido en Guadalajara…" → "Cantador mexicano que convierte cualquier idea en una canción, mezcla géneros, rompe reglas y demuestra que la música también puede ser humor puro."
- **Póster 1 (resumen, 3 capas cv16/cv60 azul, cv16 blanco, cv20 rojo):** copy nuevo es ~3× más largo y hay poco espacio bajo EL VATO. Reduje `.cv16,.cv20` font-size **28→20px**, contornos proporcionales `.cv60` stroke 14→10, `.cv16` stroke 8→6 (mantiene el look del doble contorno sin blobbing). 6 líneas, gap 32 (tspans y 0/32/64/96/128/160), mismo transform `translate(514.2 776) scale(.8 1)`. Cabe en el área verde del póster sin salirse. cv16/cv20/cv60 se usan SOLO aquí (verificado) → reescalar no rompe nada.
- **Póster 2 (párrafo cv70):** copy nuevo = 4 líneas (más largo). Para no pasar de las estrellitas reduje font **17→14px**, letter-spacing -0.2, interlineado apretado (tspans y 38/60/82/104), transform x -8→2 (margen izq). Libra guitarra (der.) y estrellitas (abajo). swiss-721-bt self-hosted → render fiel.
- **DECISIÓN de caso:** ambos textos los dejé en MAYÚSCULAS (póster 2 = mismo estilo que Ani aprobó en cont.36; póster 1 = estilo display doble-contorno siempre fue mayúsculas). Ani escribió el copy en minúsculas → si lo quiere tal cual (minúsculas), cambio en 1 seg. Avisado en el mensaje.
- **TRADEOFF avisado:** el párrafo del póster 2 quedó más chico que antes (14px) porque el copy nuevo es más largo y debe caber sobre las estrellitas.
- **Ani (2190):** rebalancear ambos para que no quede hueco a la derecha; póster 2 además un poquito más grande, sin pasar de estrellitas.
  - **Póster 1:** reparto de 6→**5 renglones más llenos** (tspans y 0/34/68/102/136, ~30-31 chars c/u, solo el último corto). Llena bien el ancho. 20px sin cambio.
  - **Póster 2:** el hueco era sobre todo el renglón 1 ("NACIÓ EN GUADALAJARA." quedaba corto). Probé 3 renglones muy llenos pero "COMPONE," chocaba con el cuerpo de la guitarra (los renglones de en medio topan con ella). Solución: **4 renglones** con el renglón 1 ya lleno ("NACIÓ EN GUADALAJARA. HACE DE TODO:"), font **14→15px** (más grande), tspans y 40/63/86/109. Iteré 16→15px: a 16 "TODO:" rozaba el mástil; 15 lo despega y da margen a estrellitas. Libra guitarra (mástil arriba, cuerpo en medio) y estrellitas.
- **PENDIENTE:** título/subtítulo p2 reales (siguen placeholder UN TÍTULO COOL / AQUÍ UN SUBTÍTULO); texto real póster 3 (baked HISTORIE TIME).

## 2026-07-09 · anette (cont. 36) — MÓVIL "Sobre mí": 3 afinados (título p2 izq, párrafo mayúsculas, quitar picos p1)
- **Ani (2175, con captura marcada):** (a) turquesa sobre "UN TÍTULO COOL" (p2) → moverlo un poco a la IZQUIERDA (COOL invadía el póster 1). (b) el párrafo que ella pasó → TODO MAYÚSCULAS + un poquito más grande, cuidando NO chocar con la guitarra (der.) ni pasar de las estrellitas (abajo). (c) morado sobre el contorno azul del resumen p1 ("NACIDO EN GUADALAJARA.") → quitar los PICOS.
- **FIX (c) picos:** eran los vértices en `miter` del stroke grueso. Añadido `.cv16,.cv20,.cv60 { stroke-linejoin: round; stroke-linecap: round; }` → contorno azul liso. Verificado (crop_resumen).
- **FIX (a) título:** cv18 `translate(6.8 → -12)`. En mi chromium la fuente cae en fallback (más angosta que la Typekit del iPhone de Ani) → no puedo medir el ancho real; -12 es margen intermedio: COOL libra el póster 1 y "UN" se lee (queda al borde izq, como ya estaba en el original .ai, que Ani no objetó). Si en su pantalla COOL aún toca o "UN" se corta, ajustar X.
- **FIX (b) párrafo cv70:** tspans a MAYÚSCULAS ("SU NOMBRE SIGNIFICA… / HUEY = GRANDE, INGENIOSO, MAGNÍFICO. / COYOTE = CURIOSO, JUGUETÓN Y DIVERTIDO."). Mayúsculas = ya se ve con más cuerpo/grande que el original en minúsculas. font-size 17px, **letter-spacing -0.4px** (acorta líneas ~7px), **translate x 12.7→1** (despega las puntas derechas de la guitarra). Interlineado 46/76/106 (no pasa de estrellitas). swiss-721-bt es self-hosted → el render local es fiel. "MAGNÍFICO." y "DIVERTIDO." dejan huequito antes de la guitarra. Iteré 19→17→16→17px: 19 chocaba, 16 rozaba, 17+(-0.4 ls)+(x1) es el punto que libra guitarra Y mantiene tamaño.
- **Ani (2182):** mover el párrafo un poquito a la IZQUIERDA → cv70 `translate x 1 → -8`. Verificado: se lee completo (nada cortado), borde izq sigue sobre el póster, y "DIVERTIDO./MAGNÍFICO." quedan aún más lejos de la guitarra. Deploy aparte.
- **PENDIENTE:** título/subtítulo p2 reales (siguen placeholder); texto real póster 3 (baked HISTORIE TIME). GitHub deploys lentos hoy.

## 2026-07-09 · anette (cont. 35) — MÓVIL "Sobre mí": pósters a posiciones EXACTAS del SVG (arregla recorte + texto movido)
- **Ani (2171):** pósters 2 y 3 "recortados" y "se movió UN TÍTULO COOL / AQUÍ UN SUBTÍTULO". Pidió revisar el SVG original para ver dónde va el texto.
- **Diagnóstico:** mi rebuild limpio (cont.34) NO tiene doble PERO puse pósters 2/3 en posiciones % aproximadas que NO coinciden con el SVG → recortados + texto (que sí está en posiciones del SVG: título 6.8, subtítulo 12.7) desalineado. Renderé el SVG original (`rsvg-convert` → svg-original.png) = fuente de verdad del layout.
- **Datos del SVG (útiles a futuro):** image1=778×1080 (foto póster1). Título cv18 `translate(6.8 904.9)`, subtítulo cv17 `translate(12.7 938.1)`, resumen p1 `translate(514.2 797) scale(.7 1)`. El filler denso "COSAS SOBRE.../HISTORIE TIME..." NO es texto en el SVG → son PATHS outlined (grep: "COSAS" aparece 1 vez=cv25 editable; "HISTORIE"/"PODEMOS" 0 veces). Por eso no se puede quitar como texto; hay que cubrirlo.
- **FIX:** base = `conoces-fondo-p1ok` (= render SVG #Fondo + póster1 Ani = posiciones EXACTAS del SVG, ya aprobado). Cubrí el filler del póster 2 con `poster2-tight` **escala 1.12** (poster2-tight es ~12% más chico que el póster del SVG; escala hallada por búsqueda de correlación de bordes) en pos (-35,706) → cubre el filler sin doble, en la posición del SVG. Verificado visual (p2cover-test) + sección completa (full7) vs svg-original: cuadra. Resultado `conoces-fondo-p2fix.webp` (341KB). Pósters 1 y 3 sin tocar. Título/subtítulo ya estaban en 6.8/12.7.
- **Deploy c6a4ab6 success.** En vivo: p2fix.webp (349KB).
- **LECCIÓN:** para el layout, la fuente de verdad es el SVG original (`inbox/1783543043971-AgAD8AgAAnmtcEY.svg`) / su render. p1ok = ese layout con póster1 corregido. NO usar posiciones % inventadas.
- **PENDIENTE:** título/subtítulo p2 reales; texto real póster 3 (sigue filler baked HISTORIE TIME, mismo reto de paths). Sin uso: conoces-fondo-clean, p2clean, conoces-wall, poster2/3.webp.

## 2026-07-09 · anette (cont. 34) — MÓVIL "Sobre mí": ELIMINA doble cartel (rebuild limpio) + 2 fixes
- **Ani (2164, con captura):** confirmó el DOBLE CARTEL (circuló en morado el borde superior del póster 2 duplicado). + "regresa UN TÍTULO COOL / AQUÍ UN SUBTÍTULO a su lugar (los moviste, se ven mal)" + "magnífico con ingenioso, divertido con juguetón (líneas completas)".
- **Causa del doble:** mi re-horneado (cont.33) pegó `poster2-tight` DESALINEADO sobre el póster 2 horneado → sus orillas (franjas del borde) se asomaban. Correlación de fase daba shift ~(29,-25) pero el chase era frágil.
- **FIX definitivo = REBUILD TOTAL del fondo (`conoces-fondo-clean.webp`):** pared limpia (fondo-sin-posters) → póster 1 Ani → póster 2 LIMPIO (poster2-tight) → póster 3 CON su texto (extraído del horneado notext por MÁSCARA poster3-tight; las paredes de fondo-sin-posters y notext coinciden, diff ~10). CERO pósters baked que se asomen → imposible el doble. Verificado póster 3 sin rim. Todo a 1086×1937.
- **Título/subtítulo:** cv18 X -5.2→6.8, cv17 X 0.7→12.7 (valores originales del .ai; en cont.24 los había movido a la izq).
- **Párrafo cv70:** 3 líneas completas "Su nombre significa… / Huey = grande, ingenioso, magnífico. / Coyote = curioso, juguetón y divertido." (17px, alineado bajo subtítulo en 12.7). Ya no orphans.
- **Deploy b5bdd90 success.** En vivo: clean.webp (403KB), párrafo en 1 renglón c/u. Pósters 1 y 3 sin tocar.
- **PENDIENTE:** título/subtítulo p2 siguen placeholder (UN TÍTULO COOL / AQUÍ UN SUBTÍTULO) — "UN" queda al borde izq del póster (posición original .ai); si Ani lo quiere más adentro, mover X. Texto real de p2 título/subtítulo y de póster 3 (sigue baked HISTORIE TIME) pendientes. Sin uso: p1ok, p2clean, conoces-fondo-notext, conoces-wall, poster2/3.webp.

## 2026-07-09 · anette (cont. 33) — MÓVIL "Sobre mí": texto real PÓSTER 2 (párrafo) + fondo re-horneado limpio
- **Ani (2157):** poner el texto real en el párrafo debajo de "AQUÍ UN SUBTÍTULO" del póster 2: "Su nombre significa… / Huey = grande, ingenioso, magnífico. / Coyote = curioso, juguetón y divertido."
- **Reto:** el párrafo denso viejo ("COSAS SOBRE...") estaba BAKED en el fondo. FIX: re-horneé el fondo pegando `poster2-tight.png` (póster 2 LIMPIO, sin texto) sobre el póster 2 del fondo `conoces-fondo-p1ok` → `conoces-fondo-p2clean.webp` (alineado con las %s del .ai, verificado que cubre todo el texto viejo sin asomar). Pósters 1 y 3 intactos.
- **Párrafo vivo:** nuevo `<text class="cv70">` (swiss-721-bt 700, oscuro #241a1a) debajo del subtítulo (mismo transform que cv17 + tspans con offset y, siguiendo el tilt -12.4). Quitado el placeholder cv25.
- **Ani (2160):** "PORQUE VEO DOBLE CARTEL? No muevas los otros textos. Reduce el párrafo para que no pase de las estrellitas." → (a) revisé mis 2 fondos (p1ok live + p2clean nuevo): pósters INDIVIDUALES, sin doble → el doble que veía es artefacto de caché/deploy-a-medias (deploy tardó ~9 min en cola). Le pedí refresh fuerte + captura si persiste. (b) reduje cv70 24→19px + tspans más juntos (44/69/94/119/144) → ya no pasa de las estrellitas. NO moví otros textos.
- **Deploy:** 5db276f success. En vivo: bg=conoces-fondo-p2clean?v=0709p2, font 19px, párrafo presente.
- **PENDIENTE:** título/subtítulo póster 2 siguen placeholder (UN TÍTULO COOL / AQUÍ UN SUBTÍTULO) — pedí a Ani los reales. Falta texto póster 3 (sigue baked "HISTORIE TIME"; mismo método: re-hornear con poster3-tight + texto vivo). GitHub deploys MUY lentos hoy (~5-9 min en cola).

## 2026-07-09 · anette (cont. 32) — MÓVIL "Sobre mí": micro-ajuste tamaño/posición texto póster 1
- **Ani (2153):** "reduce un chirris el tamaño y sube todo el texto un poquito". FIX: fuente `.cv16,.cv20` 30→28px; contornos proporcionales cv60 15→14px, cv16 9→8px; posición `translate(514.2 797)`→`(514.2 776)` (sube ~21u ≈ 7-8px en pantalla) en las 3 capas. Deploy ca4386c success, en vivo. Verificado local (resumen-check4) + curl (font-size 28px + translate 776 confirmados).
- **PENDIENTE:** sigue el texto final de pósters 2 y 3.

## 2026-07-09 · anette (cont. 31) — MÓVIL "Sobre mí": texto póster 1 afinado (2 vueltas)
- **Ani (2146):** cambió el texto a "Nacido en Guadalajara. Hace de todo: Canta, toca 21 instrumentos, compone, baila y tiene un estilo ameizing para la moda". Lo puse en 6 líneas mixtas.
- **Ani (2149):** "acomoda para que no quede tanto espacio vacío a la derecha y escribe todo en MAYÚSCULAS". → reescribí en CAPS y rebalanceé a 5 líneas más largas que llenan el ancho del póster (líneas ~22-25 chars). Menos gap a la derecha.
- **"ameizing" SE MANTIENE** tal cual — es Spanglish intencional del estilo cotorro de la marca (amazing→ameizing), NO errata. (Nota: regla `feedback_ani_autocorregir_ortografia` es para typos reales, no para flavor de marca.)
- Edición vía `replace_all` en los tspans internos (mismo texto en las 3 capas cv60/cv16/cv20 del doble contorno). y = 0/46/92/138/184.
- **Verificado** (preview4.cjs) + en vivo. Deploy 8093d6f success (GitHub tardó ~5 min en cola). Texto CAPS confirmado en vivo x3 capas.
- **PENDIENTE:** texto final pósters 2 y 3 (títulos vivos: p2 cv18/cv17/cv25; p3 cv19/cv30/cv7x2/cv15). OJO texto denso baked en fondo.

## 2026-07-09 · anette (cont. 30) — MÓVIL "Sobre mí": Ani APRUEBA sección + texto final PÓSTER 1
- **Ani (msg 2143):** "Ahora sí quedó muy bien" ✅ (aprobó la sección: pósters 2/3 como antes + póster 1 detrás del 2). Mandó el TEXTO FINAL del póster 1.
- **Texto póster 1 (resumen/bio):** reemplacé el placeholder por 6 líneas: "Nació en: Guadalajara / Hace de todo: / Canta / Toca 21 instrumentos / Compone / Baila". Editado con `replace_all` en el bloque interno de tspans (mismo contenido en las 3 capas cv60 azul / cv16 blanco / cv20 rojo → doble contorno). y = 0/46/92/138/184/230.
- **Verificado** (preview4.cjs): 6 líneas caben sobre el póster 1, legibles con doble contorno. Deploy 4f7a18a success, en vivo (texto aparece x3 = las 3 capas). https://elhueycoyote.com/preview/sitio/?v=0709fix
- **PENDIENTE:** texto final de póster 2 (instrumentos) y póster 3 (verde) — se lo pedí. Cuando llegue → editar los `<text>` vivos (póster 2: cv18 UN TÍTULO COOL, cv17 subtítulo, cv25 COSAS SOBRE; póster 3: cv19 TEXTO MÁS TEXTO, cv30 subtítulo, cv7 Palabra súper cool x2, cv15 ALGO PARA RESALTAR). OJO: el texto denso "COSAS SOBRE/HISTORIE TIME" está BAKED en el fondo (conoces-fondo-p1ok.webp) — si Ani quiere cambiarlo, hay que re-hornear el fondo o taparlo; los títulos sí son vivos.

## 2026-07-09 · anette (cont. 29) — MÓVIL "Sobre mí": revert pósters 2/3 + póster 1 detrás del 2 (fondo horneado correcto)
- **Ani (msg 2140):** "El póster 1 ya quedó bien PERO ahora veo raro los otros dos. Déjalos como estaban antes. El único cambio era que el póster 1 estuviera detrás del póster 2." → mi cont.28 (capas con pósters 2/3 LIMPIOS + texto vivo) los dejó sparse/raros; ella quiere el look DENSO de antes (texto baked del fondo).
- **Insight clave:** en el fondo horneado `conoces-fondo-notext.webp`, el póster 2 YA está delante del póster 1 (su orilla crema tapa la banda PARA LATAM). El bug del z-order lo causaba MI overlay `poster1-img` (z5) que tapaba al 2, no el fondo.
- **FIX (PIL composite):** horneé la imagen CORRECTA del póster 1 de Ani DENTRO del fondo, en su capa, protegiendo al póster 2. Máscara de pegado = `alpha(p1) - alpha(p2)` (poster2-tight como máscara del 2, escalado/posicionado con las %s del .ai a 2x). Así el póster 1 correcto cubre al mal-horneado PERO no invade al póster 2 → 2 sigue delante. Resultado `conoces-fondo-p1ok.webp` (887KB).
- **HTML:** bg = `conoces-fondo-p1ok.webp?v=0709fix`; QUITADOS los 3 overlays poster1/2/3-img (ya no hacen falta, todo baked en el fondo). CSS de esas clases quedó muerto (inocuo). Texto vivo (resumen doble-contorno + títulos placeholder) intacto.
- **Verificado** (preview4.cjs): pósters 2/3 idénticos a antes (texto denso), póster 1 correcto detrás del 2, sin doble. Deploy c8d5c70 success. En vivo: 0 elementos poster1-img, bg=conoces-fondo-p1ok. https://elhueycoyote.com/preview/sitio/?v=0709fix
- **Sin uso** (no borrados): conoces-wall.webp, poster2.webp, poster3.webp (de cont.28), conoces-fondo-notext.webp, poster1-latam-logo.webp.
- **PENDIENTE:** texto final de cada póster (Ani lo tiene) → editar `<text>` vivos.

## 2026-07-09 · anette (cont. 28) — MÓVIL "Sobre mí": pósters en CAPAS (arregla doble póster 1 + z-order)
- **Ani reportó 2 bugs:** (1) veía el póster 1 DOBLE, (2) el póster 1 debía ir DETRÁS del póster 2. Causa raíz: el fondo `conoces-fondo-notext.webp` traía los 3 pósters horneados (poster 1 en fuente mala) Y encima yo ponía el overlay de Ani → doble; y el overlay estaba en z5 (encima de todo) → poster 1 tapaba al 2.
- **FIX = arquitectura en CAPAS (la correcta):** encontré los assets limpios de la versión previa: `fondo-sin-posters.png` (pared 1086×1937 con "El mero mero", SIN pósters) y `poster2-tight.png`/`poster3-tight.png` (pósters 2 y 3 individuales, transparentes, SIN texto horneado). Los convertí a webp: `conoces-wall.webp` (268KB), `poster2.webp` (52KB), `poster3.webp` (70KB).
- **Nuevo stack de la sección:** bg = pared limpia → `poster1-img` (imagen de Ani, z3) → `poster2-img`/`poster3-img` (z4, ENCIMA del 1) → texto vivo SVG (z6). Pósters 2/3 posicionados con las %s calibradas del .ai (poster-2 top57.87/left24.54/w49.08; poster-3 top78.83/left70.26/w52.67). Verificado con preview3.cjs (intercept webp→PNG + oculta splash): póster 1 único, póster 2 tapando al 1, sin texto doble en 2/3. ✅
- **Bonus:** al usar pósters limpios + texto vivo, se quitó el texto doble que también tenían pósters 2/3 (baked + live).
- **Deploy:** efe0bee success. Assets en vivo (wall 274KB, p2 53KB, p3 71KB). En vivo: https://elhueycoyote.com/preview/sitio/?v=0709w1
- **PENDIENTE:** Ani dijo que tiene el TEXTO FINAL de cada póster → se lo pedí desglosado (p1 resumen; p2 título/subtítulo/medio; p3 título/subtítulo/2 palabras/resaltar). Cuando llegue → editar los `<text>` vivos del overlay. Queda sin uso (no borrado) `conoces-fondo-notext.webp`.

## 2026-07-09 · anette (cont. 27) — MÓVIL "Sobre mí": corrección de imagen del póster 1
- Ani avisó que la 1ª imagen que mandó era equivocada; mandó la CORRECTA (`inbox/1783579756515-AQADxwtrG3mteEZ8.jpg`, también 1086×1937, blanco de fondo). Mismo flood-fill → reemplacé `poster1-latam-logo.webp` (mismo bbox x396-1085/y0-1062, 148KB). Cache-bust `?v=0709p1b`. Deploy 04e85d6 success (GitHub tardó ~6 min en cola, no falló). En vivo verificado: index referencia v0709p1b, 0 texto vivo EL VATO. Esperando review de Ani (PARA LATAM/logo idénticos? grosor del doble contorno del resumen?).

## 2026-07-09 · anette (cont. 26) — MÓVIL "Sobre mí": póster 1 = imagen de Ani (opción 2) + 2 gotchas resueltos
- **Ani eligió opción 2** y mandó el póster 1 completo con TODOS los elementos-imagen (PARA LATAM, logo, EL VATO, foto, estrellas) en fuentes correctas. Instrucción extra: "EL VATO DE LAS ROLITAS RANDOM también es parte del póster, quita el texto vivo para que no se vea doble."
- **GOTCHA 1 (Telegram mata transparencia):** la imagen llegó como `.jpg` con fondo BLANCO (Telegram comprime fotos → pierde alpha). Pero medía **1086×1937 = el lienzo exacto**, así que recorté el blanco con **flood-fill desde los bordes** (PIL ImageDraw.floodfill, thresh 55, 12 semillas en bordes) → preserva blancos internos (franjas de PARA LATAM, estrellas). Quedó limpio, sin halo. Guardado `assets/conoces/poster1-latam-logo.webp` (148KB, RGBA). LECCIÓN: si Ani manda PNG transparente, pedir que lo mande como ARCHIVO/documento, no como foto (igual que el SVG Embed). El flood-fill es el plan B si llega como foto.
- **Integración:** `<img class="poster1-img">` (z-index 5) encima del fondo, debajo del texto vivo (z6). Lienzo completo 1086×1937 → encaja solo, tapa el póster 1 mal-horneado del fondo. Quité el texto vivo EL VATO (cv27/28/29/31) y los `<use #image/#image1>` redundantes del overlay.
- **GOTCHA 2 (RESUELTO el misterio de la "cortina verde"):** mi chromium-1148 headless mostraba una cortina verde tapando todo. NO era bug de la sección — es el **splash/PORTADA** (`position:fixed` z-9998, línea 90) que en headless nunca se levanta (se abre por interacción/localStorage). Ani ya está pasado el splash → ella SÍ ve los pósters. ADEMÁS chromium-1148 **no decodifica** `conoces-fondo-notext.webp` (muestra ícono roto; Safari del iPhone sí). **Para verificar local:** interceptar los webp con `page.route`→PNG decodificado (PIL) + ocultar los `position:fixed` a pantalla completa. Script: scratchpad/preview2.cjs. Con eso vi la sección REAL: póster 1 correcto, sin EL VATO doble, resumen con doble contorno. ✅
- **Doble contorno resumen:** cv60 azul 15px → cv16 blanco 9px → cv20 rojo. En el preview se ve un pelín grueso (15px sobre fuente 30px); si Ani lo pide más fino, bajar cv60 a ~11-12px y cv16 a ~6px.
- **Deploy:** commit 0169a7a. Pósters 2/3 (COSAS SOBRE/HISTORIE TIME) siguen con posible doble (baked + live cv25) — placeholder, se ve al cambiar copy real.
- **Archivos:** `preview/sitio/index.html` + nuevo `assets/conoces/poster1-latam-logo.webp`. Fuente JPG de Ani: `/root/.claude/channels/telegram-anette/inbox/1783578685381-AQADxQtrG3mteEZ8.jpg`.

## 2026-07-09 · anette (cont. 25) — MÓVIL "Sobre mí": doble contorno + DIAGNÓSTICO de fondo (fuentes rsvg)
- **Ani (msg 2129):** póster 1 sigue mal — (1) logo El Huey Coyote no se ve bien, (2) PARA LATAM no se ve igual a su diseño ("esos 2 NO SON TEXTO, deberían verse IGUALITOS"), (3) le quité el contorno blanco al texto de abajo de EL VATO — ese texto lleva **DOS contornos: blanco (adentro) y luego azul (afuera)**.
- **FIX doble contorno (✅ desplegado, commit 9f5c483):** revertí cont.24 (había puesto cv16 azul, matando el blanco). Ahora resumen = 3 capas: `cv60` azul stroke 15px (afuera) → `cv16` blanco stroke 9px (medio) → `cv20` rojo (relleno). `paint-order: stroke fill` en las 3. Texto rojo, contorno blanco, contorno azul externo.
- **DIAGNÓSTICO CLAVE (raíz de PARA LATAM/logo):** el fondo `conoces-fondo-notext.webp` **SÍ tiene texto horneado** (a pesar del nombre) — decodifiqué el webp (PIL) y trae PARA LATAM, COSAS SOBRE, HISTORIE TIME, logo, TODO baked por **rsvg-convert en fuentes fallback** (rsvg NO tiene las trial de Adobe: Cocogoose de PARA LATAM, la script del logo). Por eso PARA LATAM/logo NO se ven como su diseño y ningún ajuste de overlay lo corrige. `seccion-diseno-anette.png` (1086×1937) SÍ es su diseño perfecto (fuentes correctas) — está en assets/conoces/.
- **Bug adicional detectado:** en mi render headless (local Y en vivo liv6) la sección muestra una **cortina verde con el logo cubriéndolo todo** (el `<use #image1>` del overlay se agranda a scale(1) tapando el collage de pósters). Ani SÍ ve los pósters en su device (¿cache/versión previa?) → hay divergencia render-mío vs lo-que-ella-ve. PENDIENTE resolver la oclusión del #image1.
- **PLAN propuesto a Ani (mensaje enviado):** como rsvg no puede pintar sus fuentes, PARA LATAM + logo necesitan venir como IMAGEN de ella. Opción A (recomendada): exportar la sección/póster como PNG plano desde Illustrator (queda IDÉNTICO, es su render) y yo dejo solo el copy editable como texto vivo encima. Opción B: mandarme PARA LATAM + logo como PNG transparente aparte y sigo con el resto vivo. Esperando su elección.
- **Archivos:** `preview/sitio/index.html` (capas resumen cv60/cv16/cv20). Sin cambios al webp.

## 2026-07-08 · anette (cont. 20) — MÓVIL: sección "Ya me conoces/Sobre mí" pasada a TEXTO VIVO (no horneado)
- **Pedido de Ani:** que el texto de esa sección NO sea parte de la imagen de los pósters (regla dura de Cris: texto vivo, no horneado). Ani mandó el SVG de la sección.
- **Gotcha export 1:** el 1er SVG (177KB) traía las imágenes ENLAZADAS (Link), no incrustadas → al renderizar salía el texto pero el fondo blanco. Le pedí re-exportar con **"Incorporar/Embed"**. El 2º SVG (9.3MB) ya trajo 21 imágenes embebidas. LECCIÓN: siempre pedir SVG con imágenes **Embed**, no Link.
- **Estructura del SVG (muy limpia, Ani la armó en 2 capas):** grupo `#Fondo` (metal + 3 pósters + títulos de marca "PARA LATAM"/"EL VATO DE LAS ROLITAS RANDOM" horneados como arte del póster) y grupo `#Texto` (todo el texto bio + filler + "El mero mero"). Lienzo 1086.3×1936.5 (el estándar móvil).
- **Método (fiel al playbook fondo+texto vivo):**
  - **bg** = render de `#Fondo` solo → `assets/conoces/conoces-fondo-notext.webp` (rsvg-convert -w 2172, 765KB). Pósters LIMPIOS, sin texto ni mero-mero.
  - **overlay** = `#Texto` completo como **SVG inline vivo** (`<svg class="conoce-live" viewBox="0 0 1086.3 1936.5">`), 161KB. Los títulos/subtítulos/resumen son `<text>` VIVOS y editables; el filler "COSAS SOBRE…" y párrafos venían **outlined (paths)** desde Illustrator (no editables — así los exportó Ani).
  - **Fuentes:** swap de las trial del SVG a las self-hosted del sitio: Swiss721BT-*→`"swiss-721-bt"` (Black=900/Bold=700/BoldItalic=700i), OpenSansCondensed-ExtraBold→`"open-sans-condensed"`. Todas ya viven en `assets/fonts/`.
  - **Namespacing:** renombré todas las clases `stN`→`cvN` en el overlay para que NO choquen con futuros SVG de Ani (dos SVG inline con `.st2` distinto se pisarían).
- **Mero mero:** venía en `#Texto` como paths (versión Monotxt de Ani). Se **quitó el `<div class="mero-mero">` de la página** (animado) porque duplicaba/encimaba con el del SVG. Ahora sale sencillo, el de Ani (estático, look grabado). Si Ani quiere de vuelta la animación → follow-up.
- **Pósters ahora ESTÁTICOS:** se perdió el zoom secuencial (`poster-zoom`) porque los 3 pósters quedaron horneados en un solo bg. Para re-animar habría que separar los 3 pósters sin texto (Ani exportaría cada uno) o aplicar movimiento por póster. Avisado a Ani como opción.
- **Texto = PLACEHOLDER todavía** ("UN TÍTULO COOL", "AQUÍ UN SUBTÍTULO", "Palabra súper cool", etc.). Cuando llegue el copy real de "Sobre mí" → cambiar palabras en los `<text>` del overlay = edición directa al instante (sin re-exportar).
- **Verificado:** render headless local + EN VIVO (fuentes cargan por HTTP, posiciones exactas, mero-mero sencillo). Deploy GH Actions success al 1er intento. Cache-buster `?v=0708liv2`.
- **Archivos:** `preview/sitio/index.html` (sección #sobre-mi reescrita + CSS `.conoce-live`) + `preview/sitio/assets/conoces/conoces-fondo-notext.webp` (nuevo). Quedaron sin uso (no borrados): `poster1/2/3.webp`, `conoces-fondo-limpio.webp`.
- **Fuente SVG guardada:** `/root/.claude/channels/telegram-anette/inbox/1783543043971-AgAD8AgAAnmtcEY.svg` (embebido, 9.3MB).

## 2026-07-08 · anette (cont. 24) — MÓVIL "Sobre mí": aclaración clave de Ani (PARA LATAM/logo = imagen)
- **Ani aclaró:** "PARA LATAM y el logo El Huey Coyote SÍ son parte de la imagen del póster, no son texto." ⇒ el DOBLADO/estilo raro de PARA LATAM era porque yo tenía un `<text>` vivo (Poppins) ENCIMA del "PARA LATAM" que YA viene impreso en la foto del póster. Verifiqué en bg5 (sin texto): la banda negra ya trae "PARA LATAM" en letras blancas con contorno, y el logo "El" también → son parte de la imagen. FIX: quité PARA LATAM del overlay (clases cv41/cv42). El tema **Cocogoose queda RESUELTO/MOOT** — nunca debió ser texto mío.
- **Rectángulo azul → contorno en las letras:** Ani: "quítame ese rectángulo azul y ponle un contorno azul AL TEXTO." FIX: eliminé el `<rect>`; cambié la capa de contorno del resumen (cv16, era blanca #fff) a **azul #0477c3** → el texto del resumen queda rojo con contorno azul. ✅
- **Qué es texto vivo vs imagen (confirmado por render de bg5 sin texto):** IMAGEN (baked en pósters): PARA LATAM, logo El Huey Coyote, bloques "COSAS SOBRE…" (póster2), "HISTORIE TIME…" (póster3), instrumentos. TEXTO VIVO (overlay): EL VATO DE LAS ROLITAS RANDOM, resumen (AQUÍ VA…), UN TÍTULO COOL + AQUÍ UN SUBTÍTULO (póster2), TEXTO MÁS TEXTO + SUBTÍTULO LINDO Y NICE + Palabra súper cool + ALGO PARA RESALTAR (póster3). Mero-mero = imagen (se quitó el div de la página).
- **Deploy:** falló 1ª vez (FTP flaky) → `gh run rerun --failed` OK. En vivo: https://elhueycoyote.com/preview/sitio/?v=liv6
- **Archivos:** solo `preview/sitio/index.html` (overlay). bg5 sin cambios respecto a cont.23.

## 2026-07-08 · anette (cont. 23) — MÓVIL "Sobre mí": 3ª ronda (Ani revisa EN VIVO desde su iPhone)
- Ani mandó captura de la página EN VIVO (barra Telegram + elhueycoyote.com) anotada. Puntos:
- **Turquesa (resumen):** quería que el contorno azul contornee TODO el texto rojo/blanco ("una línea azul que contornea todo ese texto"). FIX: quité las 3 cajas onduladas baked (clase st35 #0477c3) del bg; dibujé un **rect redondeado azul** (stroke #0477c3, `vector-effect:non-scaling-stroke`, mismo transform que el texto) que envuelve todo el bloque del resumen. Bbox medido con getBBox en Playwright (local x=-2.9 y=-32 w=563.9 h=132.8 → rect x=-12 y=-42 w=586 h=154 rx16). ✅
- **Morado (póster 2):** los bloques COSAS indentados los había recorrido -42 (mucho); Ani pidió "un poco a la derecha" → cambié a **-12**. ✅
- **PENDIENTE — PARA LATAM (azul):** Poppins 900 NO le convence, quiere el estilo exacto de **Cocogoose** (comercial, no self-hosted, sin @font-face embebida en su SVG). BLOQUEADO: pedí a Ani el archivo .otf/.ttf de Cocogoose (lo tiene en su Illustrator). Sin eso no se puede clavar.
- **PENDIENTE — logo póster 1:** el logo "El Huey Coyote" está impreso sobre la **esquina doblada/rasgada** de la foto del póster (efecto póster viejo), por eso solo se lee "El" y "...oyote" queda sobre el doblez. Probé ensanchar viewBox (-40..1200): revela más póster pero el logo sigue sobre el doblez (es propiedad de la IMAGEN del póster). Pedí a Ani decidir: dejarlo (look callejero) / mandar la imagen del póster con el logo despejado / que reposicione.
- **En vivo:** https://elhueycoyote.com/preview/sitio/?v=liv5 — deploy success.
- **Archivos:** `index.html` + `assets/conoces/conoces-fondo-notext.webp`.

## 2026-07-08 · anette (cont. 22) — MÓVIL "Sobre mí": afinado 2ª ronda de Ani
- **PARA LATAM (blanco):** Cocogoose no está self-hosted (fuente comercial). Sustituto = **Poppins 900** (el lookalike libre más citado de Cocogoose), ya cargado vía Google Fonts (agregué `;900` a la URL de la línea 14). Overlay: clase cv41/cv42 (ex Cocogoose) → `font-family:'Poppins'; font-weight:900`. Si Ani quiere el look EXACTO, mandar el .otf/.ttf de Cocogoose y self-hostear.
- **Resumen (morado):** el texto tenía `scale(.7 1)` (comprimido) → quedaba más corto que el recuadro azul (3 paths ondulados baked, clase st35 #0477c3). Subí a `scale(.8 1)` → el texto ahora coincide en largo con el recuadro en las 3 líneas.
- **Póster 2 (rojo):** título (cv18) y subtítulo (cv17) recorridos ~12u a la izquierda; los 3 bloques "COSAS SOBRE" indentados (grupos st26 con minX>150) recorridos `translate(-42 0)` para alinearlos con el bloque de arriba.
- **Verificado en vivo** (Poppins 900 carga por HTTP): https://elhueycoyote.com/preview/sitio/?v=liv4 — deploy success.
- **Archivos:** `index.html` (URL Google Fonts +900, sección) + `assets/conoces/conoces-fondo-notext.webp` (bg con COSAS recorridos).

## 2026-07-08 · anette (cont. 21) — MÓVIL "Sobre mí": correcciones de Ani (todo el texto a vivo)
- **Feedback de Ani (imagen anotada):** póster 3 OK. Póster 2: mover texto marcado un chirris a la izquierda. Póster 1: (turquesa) "EL VATO DE LAS ROLITAS RANDOM" más grande que el original y se sale del recuadro; (morado) el recuadro azul y el texto del resumen deben coincidir en largo; (blanco) "PARA LATAM" con estilo distinto al original.
- **Causa raíz:** en cont.20 rasterizé el `#Fondo` con rsvg, que NO tiene las fuentes trial (Cocogoose, OpenSansCondensed) → "PARA LATAM" y "EL VATO…" quedaron horneados con **fuente fallback** (más ancha/grande, estilo equivocado). LECCIÓN: rsvg no rendea las @font-face del sitio; NO hornear texto con rsvg.
- **Fix:** ahora **TODO el texto es vivo** (overlay SVG). bg = SVG sin NINGÚN `<text>` (solo gráficos: pósters + recuadro azul + filler outline + mero mero) → rsvg ya no depende de fuentes. overlay = 16 `<text>` con fuentes self-hosted CORRECTAS: EL VATO/ROLITAS→`open-sans-condensed`, bio/resumen→`swiss-721-bt`. Resultado: EL VATO ya cabe (turquesa ✓), resumen dentro del recuadro (morado ~✓).
- **Pendientes con Ani (no resueltos solo):** (1) PARA LATAM usaba **Cocogoose** — NO está self-hosted; puse swiss-721-bt (cercano) provisional. Para el look exacto: Ani manda el archivo de Cocogoose y se self-hostea. (2) Póster 2 "mover a la izquierda": el título ya está en x≈7 (pegado al borde izq), moverlo más lo recorta — pedí a Ani precisar cuál texto/cuánto sobre la versión en vivo (no metí un cambio que veía mal). (3) recuadro azul línea "IMPORTANTE" la caja queda un pelín más ancha que la palabra (cajas horneadas).
- **En vivo:** https://elhueycoyote.com/preview/sitio/?v=liv3 — deploy success 1er intento.
- **Archivos:** `preview/sitio/index.html` + `assets/conoces/conoces-fondo-notext.webp` (regenerado sin texto).

## 2026-07-08 · anette (cont. 19) — MÓVIL: teléfono ÉCHAME UN GRITO igualado al compu
- **Contexto:** arrancamos sesión en la versión **MÓVIL** (`preview/sitio/`, en vivo https://elhueycoyote.com/preview/sitio/). Ojo: todo el trabajo reciente (cont.13–18) fue en el **compu** (`preview/compu/`); el móvil traía cambios desde el 10-jun.
- **Cambio:** teléfono de la sección ÉCHAME UN GRITO `2226740285` → **`2224440001`** (igual que el compu, cont.18). Editado texto visible + `data-text` del destello en `.ech-phone` (línea ~1752). Formato plano sin espacios (como estaba).
- **Verificado:** grep local (0 rastros del viejo) + deploy GH Actions **success al 1er intento** (esta vez el FTP no fue flaky) + curl en vivo confirma número nuevo y 0 ocurrencias del viejo.
- **Archivo:** solo `preview/sitio/index.html`.

## 2026-07-07 · anette (cont. 18) — Teléfono ÉCHAME UN GRITO + cartel SHOWS AGOTADOS auto-cierre + USB del headliner como botón jukebox + pósters YA ME CONOCES + fondo nuevo ÉCHAME
- **Teléfono ÉCHAME UN GRITO:** cambiado `2226740285` → **`2224440001`** (texto visible + `data-text` del destello). `.ech-phone` línea ~447.
- **Cartel SHOWS AGOTADOS (Próximos Shows):** al click en una ciudad aparece; ahora **se auto-cierra a los 5s** (`setTimeout` con `clearTimeout` para reiniciar el contador al re-click y no dejar timers colgados). Se conserva el cierre manual al click sobre el cartel. JS en el IIFE de `#sec-proximos`.
- **⭐ USB del headliner = BOTÓN jukebox (2º disparador de "Pon una rolita"):**
  - Ani mandó el **fondo sin la USB horneada** + la **USB suelta**. Swap del bg del headliner a `bg-headliner-changarro-0707.webp` (para no tener 2 USBs) + monté la USB como `<button class="hl-usb-btn" id="usb-jukebox">` con `<img usb-boton.webp>`.
  - **Recorte transparente:** 1ª versión (flood-fill binario) quedó **"carcomida"** (borde dentado + fleco blanco del JPEG) — Ani lo rechazó 2 veces. **FIX definitivo:** Ani mandó una **USB nueva más grande/nítida**; recorté con **matte suave por canal-mínimo** (`alpha = clip((250-min(RGB))/(250-234))`, anti-aliasing real) + refill de specular internos vía componentes conectados (scipy.ndimage) dejando transparentes los aros del llavero. Verificado compositando sobre naranja al zoom. **LECCIÓN:** para cutout de foto-producto sobre blanco, matte suave (ramp) > flood-fill binario (deja borde carcomido).
  - **Posición/tamaño:** medido por **composición en PIL sobre el bg real** (no headless — no cargaba los webp): `left:89.3% top:39.2% width:10.5cqw`, centrada en la bandera naranja, sin tocar el toldo ni encimar los textos "USB'S"/slogan.
  - **Animación (2 iteraciones de pedido):** de temblor continuo infinito → **un solo tembloreo que decae** (`@keyframes usb-tiembla`, amplitud translateX+rotate decreciente), duración **1.4s** (Ani lo quiso "un poquito más rápido" desde 2s). **Re-disparable** por `mouseenter` (hover) Y `click` vía JS (remove class → reflow `void offsetWidth` → add class; limpieza en `animationend`). El temblor va en el `<img>` interno para no pelearse con el `translate(-50%,-50%)` que centra el botón.
  - **Click** → `document.body.classList.toggle('music-open')` (mismo panel de rolitas que el FAB "Pon una rolita").
- **Pósters ¿YA ME CONOCES? (cascada):** Ani pidió que **ya NO se pausen al hover** y que **re-disparen al volver a la sección**. (1) Eliminé la regla `.poster-hit:hover ~ .poster-ov{animation-play-state:paused}` + quité los divs `.poster-hit` del HTML. (2) Cambié el disparador de `.shown` a una clase dedicada **`.posters-falling`** manejada por un **IntersectionObserver propio que NO desobserva**: la prende al entrar (con reflow para reiniciar la cascada desde arriba) y la apaga al salir → re-dispara cada re-entrada. El `.reveal.shown` (fade-in) sigue permanente e intacto.
- **Fondo nuevo ÉCHAME UN GRITO:** swap a `bg-echame-cabina-0707.webp` (cabina nueva de Ani; misma cabina, solo cambió el local derecho a "Plaza de la Tecnología"). Verifiqué por composición que el número (`48.8%/54.25%`) y los 4 botones sociales (`.ech-fb/ig/email/tt`, ~59-61%) **siguen cuadrando** (booth idéntico) antes de subir.
- **Assets nuevos:** `preview/sitio/assets/compu/` → `bg-headliner-changarro-0707.webp`, `usb-boton.webp`, `bg-echame-cabina-0707.webp`.
- **Deploy gotcha:** el rsync FTP de GH Actions **falló 2 veces** (flaky de Hostinger); fix = `gh run rerun <id>`, 2º intento success. Todo verificado en vivo por curl. Cache-busters: `?v=0707j..0707r`.
- **Archivo de código:** solo `preview/compu/index.html`.

## 2026-07-07 · anette (cont. 17) — PRÓXIMOS SHOWS: centrado en el cartel + contorno verde uniforme/centrado + grosor
- **Posición en el cartel naranja:** Ani pidió mover el letrero a la izquierda (varias) y luego **centrarlo en el cartel naranja del bg**. Medí el centro del cartel por render headless + análisis de píxeles (PIL bbox): centro = **11.93% / 36.57%**. Lo centré **horizontalmente en `left:11.93%`** (recorriéndolo un pelín a la derecha desde 11.4%, porque de tanto moverlo a la izquierda se había pasado del centro). **NO centré verticalmente al 100%** (top se quedó en 35.4%): el centro vertical geométrico del cartel cae sobre la **trompeta baked** del bg y la tapaba; se dejó el bloque texto+trompeta balanceado.
- **⭐ Contorno verde CENTRADO/UNIFORME (lección clave):** el verde estaba hecho con **8 `drop-shadow` encadenados** → por el compounding quedaba **corrido ~5px arriba-izquierda** (medido: offset verde↔relleno = −5.6/−3.7px). Se veía descentrado. **FIX = capas anidadas con `-webkit-text-stroke`** (matemáticamente centrado, sin compounding):
  - HTML: `<span class="w">TEXTO<span class="f" data-text="TEXTO">TEXTO</span></span>` por palabra.
  - `.ml-prox .w` = contorno verde: `-webkit-text-stroke:.075em #4eb32e; -webkit-text-fill-color:transparent; paint-order:stroke` (pinta SOLO el trazo verde, centrado en el glifo).
  - `.ml-prox .f` = capa ENCIMA (absolute, left/top:0, width:100%): relleno degradado + `-webkit-text-stroke:.012em #111; paint-order:stroke fill` (línea oscura fina + relleno). Alineada pixel a pixel con `.w` porque heredan misma tipografía/line-height.
  - Migrados a `.f` los selectores del destello blanco (`.ml-prox .f::after`) y el `margin-top` (`.ml-prox>.w+.w`).
  - Grosor del verde en **em** (escala con font-size cqw). Ani pidió engrosarlo 2 veces: `.045em`→`.058em`→**`.075em`** (en vivo). Con .075em las letras siguen legibles (no se cierran los huecos de O/R).
- **LECCIÓN:** para un contorno de texto **perfectamente centrado y parejo**, usar `-webkit-text-stroke` (capa duplicada detrás), NO drop-shadows encadenados (compilan y corren el contorno). El text-stroke va centrado en el path del glifo por definición.
- **Verificación:** render headless 2x + crop + medición de centroides (ojo: la trompeta verde del bg contamina el centroide si no se recorta la región). Cada paso desplegado y verificado en vivo por curl antes de avisar a Ani.
- **Archivo:** solo `preview/compu/index.html`. Cache-busters usados: `?v=0707d..0707i`.

## 2026-07-06 · anette (cont. 16) — HERO compu: YA ME CONOCES amarillo + PRÓXIMOS SHOWS (fuente/verde/acento)
- **Contexto:** Ani mandó el empaquetado del Headliner (Drive → `/tmp/.../huey-headliner/`, `.ai` + `WP HeyCoyote (60).png`). Cambios pedidos: (1) YA ME CONOCES pasó a amarillo; (2) PRÓXIMOS SHOWS "colorearlo exactamente igual".
- **YA ME CONOCES** (`.ml-conoces`): rojo `#b8323f` → **amarillo `#e6a804`** + contorno oscuro `.9px #3a120e` (antes gris claro) + `paint-order:stroke fill`. Fuente = Eds Market Bold Slant (ya la tenía; Ani la aprobó tal cual). Muestreado del .ai.
- **PRÓXIMOS SHOWS** — largo camino (varios "no" de Ani hasta dar con lo correcto):
  - La fuente del .ai estaba **TRAZADA** (curvas) → el archivo no guarda el nombre. Probé open-sans→swiss-721 (mal). **Ani aclaró: la fuente es la MISMA que la versión MÓVIL** = `open-sans-condensed` 800 italic. Revertido a esa (la que compu ya tenía de origen).
  - **Contorno verde:** al revertir quedó .5px (casi invisible). Subido al **verde EXACTO de la móvil: 4 direcciones a 1px** (`drop-shadow ±1px`). Visible sin ahogar el acento (el de 8-dir/1.1px sí lo ahogaba).
  - **Acento de la Ó a mitad:** con `line-height:.9` la caja es corta y el acento sobresale; el degradado recortado al texto solo llenaba la mitad baja. Primer intento (`background-size:100% 200%` para extender la zona lima) se veía bien en chrome headless pero **fallaba en el navegador real de Ani (acento seguía a mitad)** — el truco background-size no es confiable cross-browser (Safari/móvil). **Fix DEFINITIVO (como la móvil):** `line-height:1.1` (la caja incluye el acento completo) + degradado simple `linear-gradient(180deg,#eef60a 0%,#ddfc0b 28%,#e06396 64%,#e8388d 100%)`, SIN truco background-size. Rellena el acento en TODOS los navegadores. Los 2 renglones se juntan con `.ml-prox span+span{margin-top:-.26em}`.
  - **Ajuste final (Ani OK 'ya quedó super'):** PRÓXIMOS SHOWS movido un chirris a la izquierda dentro del cartel (`left:12.3%`→`11.9%`), conservando contorno verde y acento intactos.
  - **Grosor del verde:** Ani lo quería un poco más grueso → verde a **8 direcciones ~1.1px** (con el acento ya relleno, el verde grueso no lo tapa). LECCIÓN: verificar cambios de `background-clip:text`/`background-size` NO solo en chrome headless — pueden diferir en el navegador real del usuario; el método robusto es caja (line-height/padding) que incluya el glifo, no trucos de posición de degradado.
- **GOTCHA / método:** el compu es HTML/CSS puro; los letreros son TEXTO VIVO sobre el bg del headliner. Fuentes **self-hosted** en `preview/sitio/assets/fonts/*.woff2` → cargan en `file://`, así que se puede renderizar/verificar local con `google-chrome --headless` saltando la cortina (`#portada{display:none}` + quitar `cortina-bajada` + `scrollIntoView('#headliner')`). El navegador MCP Playwright suele estar ocupado por otra instancia; usar chrome headless directo.
- **Archivo:** solo `preview/compu/index.html`. Deploy: push a `main` → GH Actions FTP a Hostinger.
- **En vivo:** https://elhueycoyote.com/preview/compu/

## 2026-07-01 · anette (cont. 15)
- **Qué:** (a) Footer copy (compu) → de "© 2026 El Huey Coyote · Todos los derechos reservados" a **"© 2026 · Todos los derechos a HTM"**. (b) Fix efecto ¿YA ME CONOCES?: el póster central arrastraba un cacho del cartel izquierdo durante la caída.
- **Causa raíz (b):** `conoces-poster2-full.webp` es un PNG de canvas completo (2032×1143) que se anima como bloque (translateY). Tenía una **veta diagonal fantasma** (resto del póster izq) en x 678–722 (rows 51%–93%), fuera del cartel real. Al caer el póster central, esa veta bajaba con él → parecía que "agarraba" un pedazo del izquierdo.
- **Fix (b):** con PIL borré el alpha de todo x<723 (el cuerpo real del cartel empieza exacto en x=723 = borde de la banda "PARA LATAM"). Guardé como **`conoces-poster2-full-v2.webp`** (nombre nuevo para bustar hcdn que ignora ?query) y actualicé el src del `.poster-ov-2`. Verificado congelando la caída en Playwright (currentTime 720ms, translateY -32px): ya no hay cacho.
- **Flujo:** análisis columna-por-columna del alpha → limpieza PIL → edit HTML (src + copy) → frame congelado Playwright → deploy.
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** OK de Ani. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 14)
- **Qué:** FOOTER (compu) — Ani: logo un chirris más grande + subirlo un poco. Logo `clamp(108px,11.5vw,182px)` → **`clamp(120px,12.8vw,200px)`** (~184px en 1440); margen `0 auto 14px` → **`-14px auto 10px`** (sube 14px con margin-top negativo, sin chocar con el borde amarillo). Deploy falló 1ª vez (rate-limit SSH), rerun OK.
- **En vivo:** https://elhueycoyote.com/preview/compu/

## 2026-07-01 · anette (cont. 13)
- **Qué:** FOOTER (compu) — Ani pidió REGRESAR al orden original (columna única centrada, como antes del reacomodo de 2 columnas) y hacer el logo un poco más grande. CONSERVA el tagline "Música, Sudor y Cumbia".
- **Cómo:** Eliminé `.footer-top/.footer-left/.footer-right` y los wrappers HTML → HTML vuelve a: logo → tag → btn CIERRA → social → mail → copy (todo `margin auto`, centrado). CSS restaurado a valores originales (footer-inner max-width 900px, tag #ffe9a6 600, social 19px, mail margin auto 22px, btn-cierra margin 6px auto 24px). Logo subido de clamp(90,10vw,156) a **clamp(108px,11.5vw,182px)** (~165px en viewport 1440).
- **Nota:** el reacomodo de 2 columnas (cont.11/12) quedó descartado; solo sobrevive el cambio de tagline.
- **Flujo:** edit → screenshot footer Playwright → deploy.
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** OK de Ani. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 12)
- **Qué:** FOOTER (compu) — Ani: quitar los rectángulos oscuros que puse detrás de CIERRA y del panel derecho (venían del recorte de su screenshot, no los quería).
- **Cómo:** `.footer-right` → background transparent, sin border/shadow/radius, padding 0 (sigue flex column centrado). `.btn-cierra` → revertido a transparent, sin border, padding 0 (vuelve al neón flotante, font-size clamp 26-46px). Layout 2 columnas y contenido intactos.
- **En vivo:** https://elhueycoyote.com/preview/compu/

## 2026-07-01 · anette (cont. 11)
- **Qué:** FOOTER (compu) reacomodado a 2 columnas según collage de Ani + cambio de tagline.
- **Layout:** `.footer-top` (flex row, space-between, align center, wrap) con: **izq** `.footer-left` (columna: logo grande + botón neón `.btn-cierra` CIERRA EL CHANGARRO, ahora dentro de caja oscura rgba(9,13,11,.5) redondeada) · **der** `.footer-right` (panel oscuro rgba(9,13,11,.5) redondeado 16px, shadow: tagline + redes + correo, centrado). El `© ... Todos los derechos reservados` (`.footer-copy`) va CENTRADO debajo del `.footer-top`.
- **Tagline:** "Puro corrido, pura fiesta" → **"Música, Sudor y Cumbia"** (color #ffe08a).
- **Cambios CSS clave:** footer-inner max-width 900→1120px; logo margin auto→0 y height subió (clamp 120-200px); btn-cierra ya no transparent/centrado, ahora caja oscura margin 0; conserva neon-flicker.
- **Sin tocar:** links redes (FB/IG/TikTok), correo contacto@elhueycoyote.com, fondo footer-fondo.webp, JS de btn-cierra (baja cortina).
- **Flujo:** edit → screenshot footer Playwright (coincide con collage) → deploy.
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** OK de Ani. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 10)
- **Qué:** Cartel SHOWS AGOTADOS (sello) — Ani: quitar el temblor final + reducir sombra. Aprobado ("Así está súper").
- **Cómo:** `@keyframes cartel-sello` simplificado a 3 pasos (0% scale 1.75 → 60% scale .97 → 100% scale 1), rotación fija -3° (sin wobble), sin overshoot de escala; duración .5→.42s. Sombra `drop-shadow(0 12px 24px .5)` → `drop-shadow(0 5px 10px .28)`.
- **En vivo:** https://elhueycoyote.com/preview/compu/

## 2026-07-01 · anette (cont. 9)
- **Qué:** Cartel "SHOWS AGOTADOS" (PRÓXIMOS SHOWS, compu) — Ani PIDIÓ QUITAR el desenrollado (cont.8) y cambiarlo por efecto **SELLO/estampado**.
- **Cómo:** `@keyframes cartel-sello` (.5s, cubic-bezier(.2,.7,.3,1)): 0% opacity 0 + `scale(1.75)` (grande, cayendo desde arriba) → 55% opacity 1 + `scale(.93)` (golpe de impacto, aplasta) → 72% `rotate(-4.4deg) scale(1.05)` + 86% `rotate(-2.2deg) scale(.99)` (wobble del "thump") → 100% `rotate(-3deg) scale(1)`. `transform-origin:50% 50%`. Conserva el tilt base -3°. Reduced-motion: sin animación, cae a rotate(-3deg) scale(1).
- **Removido:** todo lo de `cartel-unroll` (perspective/rotateY/scaleX/origin izquierdo) de cont.8.
- **Flujo:** edit → frame de impacto congelado Playwright → deploy a preview (animación no se ve en foto).
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** OK de Ani sobre el sello. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 8)
- **Qué:** Cartel "SHOWS AGOTADOS" (PRÓXIMOS SHOWS, compu) — animación de ENTRADA tipo "desenrollar de lado / pegar etiqueta" al hacer click en los shows.
- **Cómo:** `@keyframes cartel-unroll` (.85s, cubic-bezier(.16,.84,.3,1.02)) con `perspective(1100px)` + `rotateY(-92deg→0)` + `scaleX(.12→1)` + `transform-origin:0% 50%` (desde el borde izquierdo) + `translateX(-4%→0)`, conservando el tilt base `rotate(-3deg)`. Rebotecito de "pegado" en 72% (rotateY 9deg, scaleX 1.03). `backface-visibility:hidden`. Reduced-motion: sin animación, cae directo a rotate(-3deg).
- **Detalle:** la animación va en `#sec-proximos.agotados-on .agotados-cartel` (se dispara al añadir la clase por el click). El estado base tiene el cartel "enrollado" (rotateY -92, scaleX .12) oculto.
- **Flujo:** edit → frame intermedio congelado Playwright (getAnimations().pause()+currentTime) → deploy a preview para que Ani lo vea EN VIVO (animación no se aprecia en foto estática).
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** OK de Ani sobre la animación. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 7)
- **Qué:** Cartel "SHOWS AGOTADOS" (PRÓXIMOS SHOWS, compu) — nuevo: al click en shows (JUN–NOV, `.dl-row .c`) sale un cartel PNG que cubre la lista morada; click en el cartel lo cierra. `agotados-cartel.png` transparente (756×559) hecho de JPG de Ani por flood-fill de esquinas quitando near-white. left:9% top:20.5% width:38% height:auto (tamaño natural, sin estirar, movido a la derecha por Ani). JS IIFE ~línea 561.
- **En vivo:** https://elhueycoyote.com/preview/compu/

## 2026-07-01 · anette (cont. 6)
- **Qué:** "El mero mero" (¿YA ME CONOCES?, compu) — otro chirris menos oscuro (iteración final de tono). #26272a→**#2a2b2e**, contorno .5px→.45px rgba(34,35,38,.58), sombra oscura .64→.58. Aprobado por Ani.
- **Historial de tono (mero-mero):** original #2c2d2f (stroke gris-claro) → v1 #202124 (muy oscuro) → #26272a (un chirris menos) → **#2a2b2e** (otro chirris menos, FINAL). Contorno pasó de gris-claro a negro fino, ahora .45px.
- **Flujo:** edit → render/compara Playwright → OK Ani → commit 05d9383 → push → deploy success → verificado en vivo.
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** siguiente detalle con Ani. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 5)
- **Qué:** Headliner (compu) — arreglé el rayo de luz (shine) del botón ¿YA ME CONOCES? que se veía apagado vs los demás.
- **Causa raíz:** `.ml-conoces::after` tenía una regla ESPECIAL vieja (comentario "YA ME CONOCES es casi blanco → banda fina con filo oscuro"): usaba `background-image` con filos oscuros rgba(70,72,84) + `mix-blend-mode:normal` + `background-size:200%`. Pero el texto de conoces ya es ROJO (#b8323f), no blanco → esa regla dejaba el rayo deslavado. Los demás botones usan el `::after` compartido (franja blanca `rgba(255,255,255,.95)` + `mix-blend-mode:screen` + size 230%).
- **Fix:** reduje `.ml-conoces::after` a solo `{white-space:nowrap}` → hereda el destello blanco/screen compartido = brilla igual que CON LA RAZA/ÉCHAME/PA'LLEVAR. Orden del rayo intacto (delays: prox 0s, conoces .5s, conlaraza 1s, echame 1.5s, pallevar 2s).
- **Verificación (técnica útil):** para fotografiar el shine (que es animación transitoria), inyecté un <style> con `.ml-*::after{animation:none!important;opacity:1!important;background-position:52% 0!important}` tras añadir `body.headliner-anim`, congelando la franja sobre las letras. Antes: rojo plano; después: letras blancas al pasar el rayo.
- **Flujo:** diagnóstico → edit → antes/después congelado (Playwright) → OK de Ani → commit db1dc60 → push → deploy success (a la 1ª) → verificado en vivo (regla reducida a white-space:nowrap).
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** siguiente sección/detalle con Ani. ÉCHAME UN GRITO: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 4)
- **Qué:** ¿YA ME CONOCES? (compu) — texto grabado "El mero mero": marqué un poco más la línea negra. Cambio: contorno `-webkit-text-stroke` de gris-claro (.3px rgba(206,210,214,.5)) a **negro fino (.5px rgba(30,31,34,.66))**; relleno #2c2d2f→**#26272a**; sombra oscura reforzada (.55→.64 opacity). Mantiene el efecto rayado-en-metal (highlight claro arriba-izq intacto).
- **Iteración:** v1 fue más oscura (#202124, stroke .55px/.8, shadow .72) → Ani pidió "un chirris menos oscuro" → bajado al tono final. Aprobado por Ani.
- **GOTCHA (Playwright/screenshots):** tras recargar el preview, SIEMPRE hay que **cliquear #btn-changarro** (cerrar la portada/splash verde) ANTES de fotografiar cualquier sección; la portada es overlay fijo y si no se cierra el screenshot del elemento captura el splash verde, no la sección. Además: forzar `img.loading='eager'` + `img.decode()` y esperar, y para el headliner/conoces poner posters en hold (animationDelay -3s, play-state paused) para foto determinista.
- **Flujo:** edit → render antes/después (comparación 3-way PIL) → OK de Ani → commit 5ec3696 → push → deploy success (a la 1ª) → verificado en vivo.
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** siguiente sección/detalle por definir con Ani. ÉCHAME UN GRITO: aún falta su SVG para afinar coords del texto vivo (tel/redes/email).

## 2026-07-01 · anette (cont. 3)
- **Qué:** (1) ¿YA ME CONOCES?: bajé la espera del bucle de pósters de 6s a ~4s (ciclo 8s→6s; recalculé %s de los 3 keyframes: drops 12%/19.5%/27%, hold-out 91.667%). (2) CON LA RAZA (#sec-collabs): rediseñé la galería de tira-uniforme a COLLAGE variado con aparición por tandas.
- **Galería collage:** altura por clase `.sz-lg(30vw)/.sz-md(23vw)/.sz-sm(17vw)`, forma por `.fr-sq(1/1)/.fr-wide(4/3)/.fr-tall(3/4)` con `object-fit:cover`. Conservé bordes de color b1-b6 y rotaciones (pasadas a custom props `--rot`/`--ty`). Reveal por tandas: `@keyframes gp-reveal` (opacity+translateY+scale usando var() para conservar rotación) con `animation-delay` inline por foto → tandas de 3,4,3,3,3 (verificado: 0/.09/.18 · .6/.69/.78/.87 · 1.15/1.24/1.33 · 1.65/1.74/1.83 · 2.15/2.24/2.33). Auto-pan + lightbox intactos.
- **GOTCHA:** custom props (`--rot`,`--ty`) SÍ resuelven dentro de @keyframes por-elemento → permite animar opacity/scale conservando la rotación estática de cada foto sin wrappers extra. Tamaños render verificados: 208×263 (chica) → 605×471 (grande).
- **Flujo:** ediciones (3) → renders/checks Playwright (dur 6s, delays por tanda, variedad de tamaños) → OK de Ani → commit 66f871c → push → deploy success (a la 1ª) → verificado en vivo (keyframes 6s + gp-reveal + clases sz/fr en prod).
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** Ani prueba en vivo (afinar variación/velocidad de tandas si quiere). Siguiente sección por definir. ÉCHAME: aún falta su SVG para coords del texto vivo.

## 2026-07-01 · anette (cont. 2)
- **Qué:** (1) Ani mandó OTRA imagen de LA MERCH (compu) → nuevo fondo `bg-merch-llevele-0701b.webp` (2032×1095, mismo layout, cambia tratamiento de título + logos "Auuu" con ícono coyote); coords del texto vivo intactas. (2) Reprogramé el efecto de los pósters de ¿YA ME CONOCES? de transition-una-vez a **animación en bucle**.
- **Animación bucle (¿YA ME CONOCES?):** Reemplacé las transitions `.shown` por `@keyframes conoces-drop-1/2/3` (8s infinite). Ciclo = cascada in escalonada (poster1 0-0.72s, poster2 0.45-1.17s, poster3 0.9-1.62s) → hold visible hasta 7.5s (~6s de espera) → fade/lift out 7.5-8s → repite. Bounce en la caída (cubic-bezier(.2,.85,.3,1.18)).
- **Hover-pause:** Activé las `.poster-hit` (antes display:none) a `display:block; z-index:5; pointer-events:auto`; regla `#sec-conoces .poster-hit:hover ~ .poster-ov{animation-play-state:paused}` pausa los 3 pósters al hover de cualquiera; al salir, reanuda. Verificado en Playwright (running↔paused).
- **GOTCHA:** para bucle limpio, keyframe 0% y 100% = mismo estado (raised + opacity0), así el loop no parpadea; el fade-out (93.75%-100%) sube+desvanece a la vez para que la subida sea invisible.
- **Flujo:** renders locales (Playwright, congelar ciclo con animationDelay -3s + play-state paused para screenshot determinista de la fase hold) → OK de Ani → commit 07619bb → push → deploy success 20s (a la 1ª) → verificado en vivo (bg HTTP 200 + keyframes + hover-rule en prod).
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** Ani prueba en vivo (feedback timing de los 6s / velocidad de caída). Siguiente sección por definir. ÉCHAME: aún falta su SVG para afinar coords del texto vivo (tel/redes/email).

## 2026-07-01 · anette (cont.)
- **Qué:** Reemplacé fondos de 2 secciones compu con imágenes nuevas de Ani: LA MERCH → `bg-merch-llevele-0701.webp` ("Llevele, llevele · Pirateria Oficial / La Original"); ÉCHAME UN GRITO → `bg-echame-cabina-0701.webp` (cabina telefónica "Échame un grito · Exceso de flow"). Además Ani mandó el SVG `NuevoLaMerch-compu` con las coords actualizadas del texto vivo → reacomodé las 12 posiciones de precios/etiquetas (líneas 342-353) para que los precios caigan sobre las líneas punteadas.
- **Coords (SVG viewBox 2031.9×1095.3 → % del contenedor):** etiquetas izq x=190.4→9.37%, precios izq x≈803→39.53%; etiquetas der x=1067→52.52%, precios der x≈1680→82.67%. Tops derivados de baseline−ascent.
- **Decisión:** Nombres versionados `-0701` (bustear CDN Hostinger). Ambas imágenes con dims idénticas a los fondos previos (MERCH 2032×1095, ÉCHAME 2032×1143) → sin deformación.
- **Flujo:** renders locales (Playwright, abrir changarro + forzar reveal/eager) → OK de Ani → commit 0941b8d → push. **Deploy falló 1ª vez** (rate-limit SSH firewall, corte a ~5s en rsync) → esperé 100s → `gh run rerun --failed` → success 25s → verificado en vivo (ambos HTTP 200 + coords en prod).
- **GOTCHA:** el `img.bg` de secciones tiene `loading=lazy`; en el 1er screenshot salió negro porque no había cargado. Forzar `img.loading='eager'` + scrollIntoView antes de capturar.
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** Ani manda SVG de ÉCHAME para afinar coords del texto vivo (tel/redes/email). Siguiente sección a definir por Ani.

## 2026-07-01 · anette
- **Qué:** Reemplacé el fondo del Headliner (versión compu) por la imagen nueva del changarro de Ani (Huey con guitarra + banquitos + banderas). JPG→webp: `preview/sitio/assets/compu/bg-headliner-changarro-0701.webp`; ref actualizada en `preview/compu/index.html` (antes `bg-headliner-usb.webp`).
- **Decisión:** Nombre versionado (`-0701`) para bustear CDN Hostinger (hcdn ignora ?query en imgs). Misma dimensión 2032×1143 → letreros de texto vivo (PRÓXIMOS SHOWS, ¿YA ME CONOCES?, CON LA RAZA/ÉCHAME UN GRITO, PA' LLEVAR, USB'S/CON CUMBIAS PA' ENAMORAR) alineados sin mover.
- **Flujo:** render local (Playwright) → OK de Ani → commit f25dbf9 → push → GH Actions FTP deploy success → verificado en vivo (HTTP 200, 285KB).
- **En vivo:** https://elhueycoyote.com/preview/compu/
- **Pendiente:** Ani decide siguiente sección.

