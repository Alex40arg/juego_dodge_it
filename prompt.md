Quiero hacer una modificación localizada sobre el juego web actual “Esquivá”.

CONTEXTO DEL PROYECTO

El proyecto ya está funcionando y no quiero rehacerlo ni cambiar su mecánica.

Archivo principal a modificar:
- index.html

Otros archivos del proyecto:
- music.mp3
- log.md
- README.md
- carpeta old_versions con versiones anteriores del HTML
- carpetas referencia y screenshots

IMPORTANTE:
No modificar archivos dentro de old_versions.
No crear una segunda versión completa del juego.
Quiero mantener UNA sola base de código en index.html, pero con dos perfiles de calidad gráfica seleccionables.

PROBLEMA

El juego fue probado en una PC antigua con AMD A6-6300.

En esa máquina el juego funciona razonablemente bien, pero al alcanzar el objetivo verde se produce un tirón/congelamiento breve, aproximadamente de medio segundo.

El juego actual tiene bastantes efectos visuales:
- box-shadow
- glow
- shadowBlur en Canvas
- createRadialGradient
- animaciones visuales
- Canvas con devicePixelRatio de hasta 2

Quiero conservar la versión visual actual para computadoras más rápidas, pero agregar un modo más liviano para PCs antiguas.

OBJETIVO

Agregar en la pantalla de Configuración una nueva opción llamada:

“Calidad visual”

Debe tener dos opciones:

1. COMPLETA
   “Efectos y luces”

2. COMPATIBILIDAD
   “Mejor rendimiento en PCs antiguas”

La opción predeterminada debe ser COMPLETA.

No quiero alterar dificultades, gameplay, física, tamaños, velocidades, controles, audio, colisiones ni reglas del juego.

Solo debe cambiar el nivel de carga gráfica.

──────────────────────────────
MODO COMPLETA
──────────────────────────────

Debe conservar EXACTAMENTE el aspecto visual actual.

No eliminar ni simplificar ningún efecto existente cuando está seleccionado este modo.

Debe conservar:

- gradientes
- glows
- sombras
- pulsaciones
- animaciones
- efectos de impacto
- destello verde al alcanzar una meta
- comportamiento actual del Canvas
- devicePixelRatio actual, limitado como ahora hasta 2

La intención es que visualmente no exista ninguna diferencia respecto del juego actual.

──────────────────────────────
MODO COMPATIBILIDAD
──────────────────────────────

Debe mantener exactamente la misma mecánica, pero reducir considerablemente el trabajo gráfico.

Aplicar estas optimizaciones:

1. DESTELLO AL ALCANZAR EL OBJETIVO VERDE

Actualmente se utiliza una animación sobre `.play-shell` mediante:

`is-target-collected`

y `targetFlash`, que anima un box-shadow grande sobre todo el playfield.

En modo Compatibilidad NO utilizar ese gran box-shadow animado.

Reemplazarlo por un feedback visual mucho más económico.

Preferencia:

- un breve cambio verde en el borde del playfield

o

- una capa semitransparente verde que permanezca normalmente invisible y al alcanzar el objetivo anime solamente `opacity`.

El efecto debe durar aproximadamente lo mismo que ahora y seguir siendo claramente perceptible.

Evitar recalcular un gran blur sobre toda el área de juego.

2. EVITAR FORZAR REFLOW

Actualmente al alcanzar la meta se utiliza una técnica equivalente a:

`void playShell.offsetWidth`

para reiniciar la animación.

En modo Compatibilidad evitar ese mecanismo.

Implementar el feedback de manera que no necesite forzar una actualización síncrona del layout.

IMPORTANTE:
No romper el efecto del modo Completa.

3. CANVAS: DIBUJO SIMPLIFICADO

Actualmente `drawCircle()` utiliza:

- createRadialGradient()
- shadowColor
- shadowBlur

En modo Compatibilidad usar una variante simplificada.

Los círculos deben mantener:

- sus colores originales
- borde blanco
- símbolos interiores
- tamaño
- posición

Pero deben utilizar preferentemente:

- fillStyle sólido
- sin createRadialGradient()
- sin shadowBlur

o como máximo un shadowBlur muy pequeño si resulta prácticamente gratuito.

La claridad visual es más importante que conservar el glow.

4. DEVICE PIXEL RATIO

Actualmente `resizeCanvas()` utiliza aproximadamente:

`Math.min(window.devicePixelRatio || 1, 2)`

Mantener esto en modo COMPLETA.

En modo COMPATIBILIDAD utilizar:

`pixelRatio = 1`

Esto debe afectar únicamente a la resolución interna del Canvas.

El tamaño visual del área de juego NO debe cambiar.

5. OTROS EFECTOS

Revisar otros efectos gráficos del Canvas que puedan tener un costo significativo por frame.

En modo Compatibilidad se pueden simplificar solamente si:

- no afectan la mecánica
- no cambian tamaños ni posiciones
- no dificultan distinguir jugador, objetivo y obstáculos

Conservar siempre:

- círculo celeste del jugador
- círculo verde del objetivo
- colores de los obstáculos
- símbolos internos
- bordes blancos
- HUD
- fondo general
- cuadrícula del playfield

No quiero convertirlo en un juego visualmente pobre.

Debe seguir viéndose atractivo, solamente menos pesado.

──────────────────────────────
IMPLEMENTACIÓN
──────────────────────────────

Crear una variable/configuración clara para el perfil gráfico, por ejemplo:

visualQuality = "full"

o

visualQuality = "compatibility"

La implementación debe estar centralizada.

Evitar llenar el código de comprobaciones repetidas si se puede resolver con funciones auxiliares o configuración.

La opción elegida en Configuración debe aplicarse cuando comienza la partida tanto desde:

- botón Jugar del menú
- botón “Jugar con esta configuración”

Si se cambia la calidad gráfica entre partidas, el Canvas debe actualizar correctamente su resolución interna.

──────────────────────────────
INTERFAZ DE CONFIGURACIÓN
──────────────────────────────

Agregar el nuevo selector sin desordenar el layout actual.

Texto:

CALIDAD VISUAL

Completa
Efectos y luces

Compatibilidad
Mejor rendimiento en PCs antiguas

Puede implementarse como select, botones o control similar, manteniendo la estética existente.

No quiero que esta opción forme parte de “Nivel Personalizado”.

La calidad visual es una configuración independiente de la dificultad.

Por ejemplo:

Nivel:
Inicial / Medio / Avanzado / Personalizado

Calidad visual:
Completa / Compatibilidad

Audio:
...

──────────────────────────────
MUY IMPORTANTE
──────────────────────────────

NO modificar:

- dificultad Inicial
- dificultad Media
- dificultad Avanzada
- configuración Personalizada
- velocidades
- cantidad de obstáculos
- tamaños
- colisiones
- spawn
- objetivo verde
- pointer lock
- pausas
- controles
- sonidos
- música
- HUD
- sistema de resultados
- duración de las partidas

No refactorizar innecesariamente el resto del juego.

Hacer cambios mínimos y localizados.

No reescribir el proyecto desde cero.

No crear librerías externas.

Debe seguir siendo HTML/CSS/JavaScript puro.

──────────────────────────────
OBJETIVO DE LA PRUEBA
──────────────────────────────

Quiero poder probar el mismo juego en una PC AMD A6-6300 haciendo:

Configuración → Calidad visual → Completa

y después:

Configuración → Calidad visual → Compatibilidad

para comparar directamente el rendimiento.

El objetivo principal es eliminar el tirón que ocurre al alcanzar el objetivo verde y mejorar la fluidez general en hardware antiguo, sin perder la versión visual completa para PCs más potentes.

ANTES DE HACER CAMBIOS

1. Leer primero el archivo `log.md` completo para entender el contexto, decisiones previas, cambios ya realizados y estado actual del proyecto.

2. Revisar el `index.html` actual, que es la versión vigente y funcional.

3. Antes de modificar `index.html`, crear una copia EXACTA de la versión actual dentro de la carpeta:

`old_versions`

La copia debe hacerse SIN NINGÚN CAMBIO.

4. Revisar qué versiones existen actualmente dentro de `old_versions` y guardar la copia usando el número siguiente disponible.

Ejemplo:
si la última versión existente es:

`ver_10.html`

crear:

`ver_11.html`

5. Esa copia debe representar el estado actual del juego ANTES de aplicar esta nueva modificación.

6. Después de crear correctamente esa copia de respaldo, realizar todos los cambios solicitados únicamente sobre:

`index.html`

7. No modificar, sobrescribir ni borrar ninguna versión anterior dentro de `old_versions`.

IMPORTANTE:
El objetivo es mantener un historial incremental de versiones funcionales antes de cada cambio importante.

Al terminar, indicarme claramente:
- qué número de versión se guardó en `old_versions`
- qué archivos fueron modificados
- un resumen breve de los cambios realizados