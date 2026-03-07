### Tabla de Interlocks

| Condición | Q1 (Motor subir) | Q2 (Motor bajar) | Explicación |
|----------|-----------------|-----------------|-------------|
| Cortina arriba activada | 0 | Puede activarse | Evita que el motor siga subiendo cuando la cortina ya está en la posición superior |
| Cortina abajo activada | Puede activarse | 0 | Evita que el motor siga bajando cuando la cortina ya está en la posición inferior |
| Q1 activo | 1 | 0 | El interlock impide activar la bajada mientras el motor está subiendo |
| Q2 activo | 0 | 1 | El interlock impide activar la subida mientras el motor está bajando |
| Sistema en movimiento | 1 o 0 | 1 o 0 | Se activa la lámpara roja indicando que el sistema está en proceso |
| Sistema detenido | 0 | 0 | Se activa la lámpara verde indicando que el sistema está listo |
