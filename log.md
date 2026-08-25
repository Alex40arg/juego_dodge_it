# Registro de versiones

## ver_01 — 24 de agosto de 2026

Primera prueba de concepto jugable.

- Se creó un área de juego que aprovecha todo el espacio restante de la ventana.
- Se ubicó un HUD vertical y legible en el lateral izquierdo.
- El jugador controla un círculo luminoso mediante el movimiento directo del mouse, sin clics, retardo ni inercia.
- Se agregaron obstáculos circulares que rebotan dentro del campo.
- La partida comienza con un obstáculo y suma otros a los 15, 30 y 45 segundos.
- Los impactos pausan brevemente la acción, muestran una señal clara y luego permiten continuar con protección temporal.
- El HUD registra tiempo, impactos, obstáculos, mejor tramo sin choques y estado de la partida.
- Al finalizar se muestra un pequeño reporte y la opción de volver a probar.
- Los principales tamaños, velocidades y tiempos quedaron agrupados al comienzo del JavaScript en `GAME_CONFIG`.

## ver_02 — 24 de agosto de 2026

Ajuste inicial de escala y velocidad solicitado después de probar la primera versión.

- Se duplicó el diámetro visual del jugador: `PLAYER_RADIUS` pasó de 23 a 46 píxeles.
- Se duplicó el diámetro visual de los obstáculos: `OBSTACLE_RADIUS` pasó de 29 a 58 píxeles.
- Se aumentó moderadamente la velocidad de los obstáculos, de 112 a 120 píxeles por segundo.
- No se modificó todavía ningún otro comportamiento del juego.

## ver_03 — 24 de agosto de 2026

Aviso visual previo a la activación de cada obstáculo.

- Se conservaron los ajustes manuales del usuario: jugador de 50 px, obstáculos de 60 px y velocidad de 200 px/s.
- Cada obstáculo nuevo aparece primero inmóvil y realiza tres parpadeos.
- Durante el aviso previo, el obstáculo no se mueve, no cuenta en el HUD y no puede provocar impactos.
- Después del tercer parpadeo, el obstáculo se activa, comienza a moverse y se suma al contador.
- Se agregaron `SPAWN_FLASH_COUNT` y `SPAWN_FLASH_CYCLE_MS` a `GAME_CONFIG` para poder ajustar el aviso fácilmente.

## ver_04 — 24 de agosto de 2026

Confinamiento del puntero, pausa con Escape y protección frente a clics accidentales.

- Un clic izquierdo inicial solicita Pointer Lock y comienza la partida.
- Durante la captura, el jugador se mueve directamente con `movementX` y `movementY` y queda limitado a los bordes del campo.
- Escape libera el puntero y pausa el juego; un nuevo clic izquierdo vuelve a capturarlo y continúa desde el mismo estado.
- Se diferencian una solicitud de captura pendiente y una pérdida real de Pointer Lock para evitar pausas falsas.
- El clic derecho y el menú contextual quedan deshabilitados únicamente dentro del área de juego.
- La partida también se pausa si la ventana pierde el foco.

## ver_05 — 24 de agosto de 2026

Corrección de la recaptura intermitente después de pausar con Escape.

- Cada solicitud de Pointer Lock recibe un identificador propio.
- Los rechazos atrasados de una solicitud anterior ya no pueden reemplazar el estado de una captura posterior.
- La notificación `pointerlockerror` y el rechazo de la promesa se procesan una sola vez.
- Si Chrome rechaza un intento, el juego conserva el diálogo `Juego pausado` y permite realizar otro clic limpio.
- Se eliminó el diálogo confuso `Intentá nuevamente`.

## ver_06 — 24 de agosto de 2026

Colisiones físicas entre obstáculos activos.

- Los obstáculos activos ahora chocan y rebotan entre sí.
- El rebote conserva el impulso mediante una resolución elástica basada en la masa proporcional al área de cada círculo.
- Se corrige la penetración entre círculos para impedir que queden superpuestos o vibren pegados.
- Se realizan dos pasadas breves de resolución por cuadro para estabilizar encuentros múltiples.
- Después de una colisión, los obstáculos continúan respetando los límites del campo.
- Los obstáculos que todavía están parpadeando no participan en colisiones hasta activarse.

## ver_07 — 24 de agosto de 2026

Velocidad constante y activación segura de obstáculos.

- Después de cada choque, ambos obstáculos conservan la dirección resultante pero recuperan la velocidad configurada.
- Se evita que un círculo quede detenido o que otro gane velocidad de manera permanente por una colisión oblicua.
- Los obstáculos en advertencia continúan siendo intangibles y permanecen quietos durante los tres parpadeos.
- Antes de activarse, un obstáculo superpuesto busca el espacio libre más cercano sin empujar ni desviar a los obstáculos activos.
- La colisión física comienza solamente después de que el nuevo obstáculo está separado y activo.

## ver_08 — 24 de agosto de 2026

Menú principal y configuración de dificultad.

- Se agregó una portada visual con los botones `Jugar` y `Configuración`.
- Se incorporaron los perfiles Inicial, Medio y Avanzado con valores preparados de tamaño, velocidad, frecuencia, cantidad y duración.
- El perfil Personalizado habilita controles independientes para el tamaño del jugador, el tamaño de los obstáculos, la velocidad, la frecuencia de aparición, la cantidad total y la duración.
- Las duraciones disponibles son 1, 3 y 5 minutos, además de una modalidad sin límite de tiempo.
- La configuración seleccionada se aplica al tamaño de los círculos, su velocidad constante, el calendario de apariciones y el reloj de la partida.
- La pausa y el reporte final permiten regresar al menú principal.
- La pantalla de configuración se compacta en monitores de poca altura para mantener visibles las acciones principales.

## ver_09 — 24 de agosto de 2026

Primera implementación de la Luz guía.

- Se agregó un objetivo verde estático que se alcanza por contacto, sin utilizar botones del mouse.
- La luz aparece en posiciones aleatorias alejadas del jugador y separadas de obstáculos activos o en advertencia.
- Al alcanzarla suma una meta, produce un destello verde suave y reaparece después de una pausa breve.
- Un aro exterior indica visualmente el tiempo disponible; si vence, la luz cambia de lugar y se registra como objetivo vencido sin provocar impacto.
- Si un obstáculo cubre la luz durante un momento, esta se reubica sin penalización.
- El HUD muestra las metas alcanzadas y el reporte final incluye alcanzadas y vencidas.
- Los perfiles Inicial, Medio y Avanzado incorporan tamaños y tiempos propios para la luz guía.
- El modo Personalizado permite modificar el tamaño de la luz y el tiempo disponible para alcanzarla.
- La etiqueta de configuración `Cantidad total` se aclaró como `Cantidad de obstáculos`.

## ver_10 — 25 de agosto de 2026

Reloj continuo durante impactos y primera capa de audio.

- El tiempo general de la partida continúa descendiendo mientras se muestra el aviso de impacto.
- La partida puede llegar a cero y finalizar directamente desde el estado de impacto.
- La pausa manual con Escape continúa deteniendo el reloj.
- Se agregó un efecto sintetizado ascendente al alcanzar una luz verde.
- Se agregó un efecto sintetizado descendente al producirse un impacto.
- El código quedó preparado para reproducir `music.mp3` en bucle desde el inicio efectivo de la partida hasta su finalización.
- La música se pausa junto con la pausa manual, continúa durante los impactos y se detiene al terminar o regresar al menú.
- Configuración incorpora controles independientes para activar efectos y música, además de un volumen separado para cada canal.
- Los controles de volumen quedan deshabilitados visual y funcionalmente al apagar su canal.
