# Máquina de estados del sistema

El comportamiento del sistema se modeló utilizando una **máquina de estados**, donde cada estado representa una condición de operación de la cortina.

## Estados definidos

| Estado | Descripción |
|------|------|
| Inicial | La cortina se encuentra completamente arriba |
| Bajando | El motor se activa para bajar la cortina |
| Intermedio | La cortina se encuentra entre sensores |
| Abajo | La cortina ha llegado al límite inferior |
| Temporizador | Se espera un tiempo antes de iniciar la subida |
| Subiendo | El motor se activa para subir la cortina |
| Alarma | Se detecta una condición de seguridad |
