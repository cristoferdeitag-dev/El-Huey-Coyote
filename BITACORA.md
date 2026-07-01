# BITÁCORA — El Huey Coyote

Memoria viva del proyecto. Entradas más recientes arriba. Nunca borrar historial, solo agregar.

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

