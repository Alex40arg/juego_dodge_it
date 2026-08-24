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
