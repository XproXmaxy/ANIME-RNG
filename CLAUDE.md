# Anime RNG: contexto de diseño (notas de traspaso)

Este archivo es local y NO está commiteado. El usuario pidió no hacer commits ni cambios definitivos en este repo hasta que apruebe una propuesta. El repo (XproXmaxy/ANIME-RNG) está vacío: no tiene ningún commit.
Habla con el usuario en español. Los personajes y nombres son siempre originales: nunca uses personajes, nombres ni arte de animes reales, y no copies el diseño de juegos existentes (el usuario pidió copiar "Anime Dice" y se rechazó). Sí se pueden reutilizar mecánicas del género y estructuras de pantalla.

## Dónde está el diseño (fuente de verdad)
- **Sistema de diseño "Kitsune Roll"** (artifact de tipo Design System): https://claude.ai/artifact/SEk19zXuuEzBogkK1PW7FJ
  - Para cambiarlo: lee primero `project/design-system.json` y los archivos que vayas a tocar con Artifact `read` (`path`), edítalos y publica solo los archivos cambiados. El índice va en la última publicación, con `lastChange` actualizado.
  - Archivos: `project/README.md` (guía), `project/tokens.json`, `project/components/bundle.css` (todos los estilos), `project/components/bundle.js` (envuelve textos en `.tx` para el texto con degradado y borde), `project/components/kr3d.js` (personajes 3D con three.js r128 desde jsDelivr), `project/components/<Componente>/preview.html` + `README.md`.
  - Iconos: 28 SVG en el grupo de assets "Icons" (la versión v2 tiene degradados). Sus ids de blob están en `assetGroups.Icons.files` del índice.
- **Informe de investigación + 3 propuestas con prototipos**: https://claude.ai/artifact/1ac8Unmgy3dkjWbPszccM3
- Los scripts de generación locales (pueden desaparecer si se recicla el contenedor) están en el scratchpad de la sesión anterior: `ds/build.py`, `new_comps.py`, `tree.py`, `home.py`, `render.py`. Si no existen, reconstruye a partir de los archivos publicados en el artifact.

## Estilo elegido y estado actual
- El usuario quiere el estilo **cartoon colorido tipo simulador de Roblox** (propuesta A, el más cercano a lo que le gusta de Anime Dice), con un **acabado profesional y muy detallado**: le pareció "soso" cuando era plano.
- Receta del acabado (capa pro en bundle.css), en este orden: contorno `#1a1030` de 3–4px → cara en degradado (`-shine` arriba, base, `-shade` abajo) → textura de cuadros al 7,5% (18px) → brillo superior → labio 3D (4px de shade + 3px de contorno) → sombra difusa en el suelo.
- Texto de juego: relleno en degradado (`--t1` → `--t2`) + borde del 20% del tamaño (`-webkit-text-stroke` en el span, relleno en `::after`) + sombra caída. Dinero en lima → verde; probabilidades en el color de su rareza; Secreto en arcoíris.
- Fuentes: Fredoka 700 (display; en Roblox es `FredokaOne`) y Nunito 800/900.
- Rarezas: Común, Poco común, Raro, Épico, Legendario, Mítico, Secreto (el Secreto lleva arcoíris animado). Variantes: Normal, Dorado, Arcoíris.
- Personajes: modelos 3D de bloques con cabeza chibi y cara anime pintada (en Roblox van en un ViewportFrame). Nombres de ejemplo: Ronin del Eclipse, Oni Carmesí, Kitsune Blanca, Reina de la Tormenta, etc.
- Mundo: cielo azul, mar, plaza de arena con palmeras y "tienda de dados"; colores suaves y sin contorno, para que la UI destaque encima.

## Componentes hechos (en el artifact del sistema de diseño)
HomeScreen (pantalla principal 1600×900), NavTab (Hub/Inicio/Tienda), SideButton (Tienda/Rebirth/Índice/Regalos), HudBar (dinero + Friend Boost + Mochila/ROLL/Mejoras con AUTO y anillo de recarga), Window, IndexGrid (3D, filtros Normal/Dorado/Arcoíris), Backpack (3D, $/s, gemas de rasgo, Equipar mejores, 182/350), UpgradeTree (hexágonos, ramas de color, caminos animados, niveles, mejoras definitivas, panel de detalle), GamepassCard, RebirthPanel, Billboard (3D + "1 en 72 Qi"), BoostBadge ("¡OP!"), ZoneLabel (Torres, Fusión, Rasgos, Ranking), Leaderboard (Más raro/Tiradas/Dinero), RarityBadge.
- **Excluidos a propósito:** máquina de Pociones y Misiones (el usuario no las quiere).
- El Skill Tree del juego tiene 6 ramas: Suerte (Luck), Dinero (Money), Ataque (Attack), Vida (Health), Enfriamiento de tirada (Roll Cooldown) e Inventario. El UpgradeTree del sistema todavía muestra 7 ramas antiguas: alinéalo con estas 6 cuando se retome.

## Pendiente (siguiente paso propuesto al usuario)
1. Que el usuario confirme si este nivel de acabado (HomeScreen) es el que busca.
2. Crear 4 pantallas nuevas con el mismo acabado: revelación al tirar (destello del color de la rareza antes de revelar, más espectáculo cuanto más rara, botón Saltar), recompensa diaria (7 días), ajustes (silenciar, saltar animación, tirada múltiple, fondo animado y bajo rendimiento) y notificaciones + ventanas de confirmación.
3. Que el usuario pase sus mecánicas reales (probabilidades, economía): los números actuales son ejemplos y no hay que inventarlos.

## Hallazgos clave de la investigación
- No existen juegos "Anime RNG" open source populares; muchos repos con "RNG" son spam o exploits.
- El estilo de las capturas de referencia ya circula como kit clonado (Stud-UI-Clean-Roblox), así que hay que darle identidad propia.
- Hacer: anticipar la rareza antes de revelarla, botón para saltar, probabilidades honestas (el mismo módulo que el servidor), "1 en N" con separador de miles, duplicados como "xN".
- Evitar el efecto "casi lo consigues" y las tablas del servidor copiadas en el cliente.
- Roblox tiene StyleSheet (tokens y temas) y ajustes de accesibilidad: PreferredTransparency, PreferredTextSize y movimiento reducido.
