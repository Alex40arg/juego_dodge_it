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
