# Hito 4 – Control + HMI + Validación

## Descripción general del sistema

En esta etapa del proyecto se implementó el comportamiento completo del sistema de control de la cortina automatizada utilizando un **PLC Siemens LOGO!**.  

El objetivo de este hito fue integrar la **lógica de control, la interfaz de interacción tipo HMI y la validación del sistema mediante pruebas funcionales**, asegurando que el sistema responda correctamente ante diferentes condiciones de operación.

El sistema controla el movimiento de una **cortina industrial** mediante un **motorreductor DC bidireccional**, el cual es activado mediante relevadores controlados por el PLC.

Durante esta etapa se realizaron pruebas del comportamiento del sistema utilizando una **interfaz HMI simulada**, lo que permitió validar el funcionamiento del sistema antes de una implementación física completa.

---

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

---

## Transiciones de estado

| Evento | Transición |
|------|------|
| Pulsador de inicio | Inicial → Bajando |
| Sensor intermedio activado | Bajando → Intermedio |
| Sensor inferior activado | Intermedio → Abajo |
| Fin del temporizador | Abajo → Subiendo |
| Sensor intermedio activado | Subiendo → Intermedio |
| Sensor superior activado | Intermedio → Inicial |
| Activación sensor de seguridad | Cualquier estado → Alarma |

En el estado de **alarma**, el sistema detiene el motor inmediatamente para evitar riesgos.

---

# Diagrama funcional del sistema

Sensores de posición y seguridad  
↓  
PLC Siemens LOGO!  
↓  
Relevadores de potencia  
↓  
Motorreductor DC  
↓  
Movimiento de la cortina

El PLC también controla la señalización visual del sistema mediante una torre de luz.
<img width="955" height="664" alt="image" src="https://github.com/user-attachments/assets/680ad8d5-5149-4958-a152-434ec268b201" />


---
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

---

# Ejecución de pruebas

Durante la validación se realizaron múltiples pruebas del sistema.

| Prueba | Resultado |
|------|------|
| Ciclo completo de cortina | PASS |
| Detección sensor superior | PASS |
| Detección sensor inferior | PASS |
| Interlock de motor | PASS |
| Sensor de seguridad | PASS |
| Estabilidad del movimiento | FAIL |

El último caso se debe a limitaciones del sistema mecánico.

---

# Problemas encontrados

Durante la integración y validación del sistema se detectaron diversos problemas técnicos.

## 1 Incompatibilidad de firmware

El PLC inicialmente no aceptaba el programa debido a que la **versión de firmware del dispositivo no coincidía con la versión del programa proporcionado**.

**Solución**

Se ajustó la configuración del proyecto para coincidir con la versión del firmware del PLC.

---

## 2 Sensor invertido

Durante el montaje de los sensores se detectó que uno de ellos estaba **invertido**, lo que provocaba señales incorrectas en el PLC.

**Solución**

Se ajustó la lógica de programación para corregir el comportamiento del sensor.

---

## 3 Distancia insuficiente entre imán y sensor

El sensor no detectaba correctamente la posición debido a una **distancia insuficiente entre el imán y el sensor**.

**Solución**

Se realizaron ajustes mecánicos y de posicionamiento para mejorar la detección.

---

## 4 Velocidad del motor

El motorreductor utilizado presenta una **velocidad mayor a la requerida**, lo que provoca que la cortina sobrepase ligeramente la posición de los sensores antes de que el PLC detenga el movimiento.

Actualmente el sistema funciona correctamente cuando el movimiento se controla manualmente, pero en operación automática la velocidad del motor genera inconsistencias en la detección de posición.

**Solución**

Para mitigar este problema durante las pruebas se **acortó la distancia de recorrido de la cortina**, de manera que el sistema pudiera detenerse dentro del rango de detección de los sensores.

Este ajuste permitió mejorar la detección de posición durante la validación del sistema. Sin embargo, se identificó que una solución más robusta en futuras iteraciones sería implementar **control de velocidad del motor**, por ejemplo mediante un controlador PWM o un sistema de reducción mecánica adicional.
---

# Evidencia de funcionamiento

Para validar el comportamiento del sistema se realizaron
Se registraron capturas del sistema durante diferentes estados de operación.

Con apoyo de **herramientas de inteligencia artificial**, se estructuró el proceso de validación y documentación de las pruebas realizadas.

Adicionalmente, se grabaron videos cortos del funcionamiento del sistema mostrando:

- ciclo completo de operación
- activación de sensores
- respuesta del sistema ante condiciones de seguridad

Debido al tamaño de los archivos, los videos se encuentran almacenados en **Google Drive**.

Enlace a evidencias:



---

# Conclusiones

El sistema de control de la cortina fue implementado exitosamente utilizando un PLC Siemens LOGO! y un motorreductor DC controlado mediante relevadores.

La implementación de una máquina de estados permitió estructurar el comportamiento del sistema de manera clara y segura.

Las pruebas realizadas permitieron validar el funcionamiento del sistema y detectar diversos problemas técnicos durante la etapa de integración.

Aunque el sistema responde correctamente a nivel lógico, se identificó que la velocidad del motor es mayor a la requerida para el sistema, lo que representa una oportunidad de mejora futura mediante control de velocidad o ajustes mecánicos.
