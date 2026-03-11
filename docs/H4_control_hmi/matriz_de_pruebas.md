# Matriz de pruebas de validación

Para validar el funcionamiento del sistema se definió una matriz de pruebas que incluye condiciones normales de operación y casos de seguridad.

| Caso | Condición inicial | Evento | Resultado esperado |
|----|----|----|----|
| 1 | Cortina arriba | Presionar inicio | Cortina comienza a bajar |
| 2 | Cortina bajando | Sensor intermedio activado | Sistema detecta posición intermedia |
| 3 | Cortina bajando | Sensor inferior activado | Motor se detiene |
| 4 | Cortina abajo | Esperar temporizador | Motor comienza a subir |
| 5 | Cortina subiendo | Sensor intermedio activado | Movimiento continúa |
| 6 | Cortina subiendo | Sensor superior activado | Motor se detiene |
| 7 | Sistema en movimiento | Activar sensor de seguridad | Motor se detiene |
| 8 | Sensor de seguridad activo | Intentar iniciar sistema | Sistema bloquea operación |
| 9 | Cortina arriba | Activar subir | No ocurre movimiento |
| 10 | Cortina abajo | Activar bajar | No ocurre movimiento |
| 11 | Activar subir y bajar simultáneamente | Interlock activo | Solo una salida se activa |
| 12 | Ciclo completo | Ejecutar operación | Cortina baja y vuelve a subir |
