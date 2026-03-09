| Estado       | S1 | S2 | S3 | S4 | P | T | Q1 Subir | Q2 Bajar | Q3 Roja | Q4 Verde | Descripción               |
| ------------ | -- | -- | -- | -- | - | - | -------- | -------- | ------- | -------- | ------------------------- |
| Inicial      | 1  | 1  | 0  | 0  | 0 | 0 | 0        | 0        | 1       | 0        | Cortina arriba            |
| Inicio       | 1  | 1  | 0  | 0  | 1 | 0 | 0        | 1        | 1       | 0        | Cortina bajando           |
| Intermedio   | 1  | 0  | 1  | 0  | 0 | 0 | 0        | 1        | 1       | 0        | Movimiento descendente    |
| Abajo        | 1  | 0  | 0  | 1  | 0 | 0 | 0        | 0        | 0       | 1        | Sistema habilitado        |
| Temporizador | 1  | 0  | 0  | 1  | 0 | 1 | 1        | 0        | 1       | 0        | Cortina subiendo          |
| Subiendo     | 1  | 0  | 1  | 0  | 0 | 0 | 1        | 0        | 1       | 0        | Movimiento ascendente     |
| Final        | 1  | 1  | 0  | 0  | 0 | 0 | 0        | 0        | 1       | 0        | Regreso al estado inicial |
| Seguridad    | 0  | *  | *  | *  | * | * | 0        | 0        | 1       | 0        | Persona detectada         |
