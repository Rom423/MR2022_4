# Hito 3 – Control e Integración

## Descripción de la lógica de control
La lógica de control fue implementada en el PLC **Siemens LOGO!** para controlar el movimiento de una cortina mediante un **motor DC bidireccional**.

El sistema utiliza sensores para detectar el estado de la cortina y controlar el movimiento del motor. Dependiendo de las señales recibidas por las entradas del PLC, se activan las salidas que controlan los relevadores del motor.

Las salidas **Q1 y Q2** controlan el movimiento del motor:

- **Q1:** activa el motor para subir la cortina  
- **Q2:** activa el motor para bajar la cortina  

Se implementó un **interlock lógico** que evita que ambas salidas se activen al mismo tiempo, cumpliendo la condición de **no subir y bajar simultáneamente**.

Además, el sistema incluye señalización visual mediante una torre de luz:

- **Q3:** luz roja cuando el sistema está en proceso  
- **Q4:** luz verde cuando el sistema está listo  

Los sensores permiten detectar la posición de la cortina (arriba, medio y abajo), lo cual ayuda a controlar el movimiento y evitar que el motor continúe funcionando cuando se alcanza un límite.

---

## Entradas y salidas

| Entrada | Tipo | Función |
|--------|------|---------|
| I1 | Sensor | Sensor capacitivo |
| I2 | Sensor | Sensor inductivo |
| I3 | Sensor | Sensor fotoeléctrico |
| I4 | Sensor | Sensor cortina arriba |
| I5 | Sensor | Sensor cortina en medio |
| I6 | Sensor | Sensor cortina abajo |

| Salida | Tipo | Función |
|--------|------|---------|
| Q1 | Actuador | Motor subir |
| Q2 | Actuador | Motor bajar |
| Q3 | Indicador | Lámpara roja (proceso) |
| Q4 | Indicador | Lámpara verde (listo) |

---

## Pruebas realizadas

| Prueba | Resultado esperado | Resultado obtenido |
|------|------------------|------------------|
| Activar subida del motor | El motor debe subir la cortina | Inicialmente el motor no respondió |
| Revisión del cableado del motor | El motor debería activarse correctamente | Se detectó un problema en la conexión |
| Nueva prueba de subida | El motor debe subir la cortina | Funcionamiento correcto después del ajuste |
| Activar bajada del motor | El motor debe bajar la cortina | Funcionamiento correcto |
| Activar subir y bajar simultáneamente | El sistema debe bloquear una de las salidas | El interlock evitó la activación simultánea |
| Verificar sensores de posición | Cada sensor debe enviar señal correcta | Se detectó que algunos sensores estaban invertidos |
| Corrección de conexión de sensores | Los sensores deben detectar correctamente | Los sensores comenzaron a responder correctamente |
| Prueba de detección de sensores | El PLC debe recibir señales estables | Se detectaron señales falsas en algunos sensores |
| Revisión del sistema | El sistema debe operar de forma estable | Después de ajustes en la lógica, el sistema funcionó correctamente |
| Prueba de señalización | Luz roja en proceso y verde en reposo | Funcionamiento correcto |

---

## Ajustes realizados
Durante las pruebas se detectaron varios problemas que requirieron ajustes en el sistema.

En una de las primeras pruebas el **motor no respondía correctamente**, por lo que se revisó el cableado y las conexiones de los relevadores. Después de ajustar las conexiones se logró activar el motor.

También se detectó que **algunos sensores estaban conectados de forma invertida**, lo que provocaba lecturas incorrectas en el PLC. Se corrigieron las conexiones para que cada sensor correspondiera a su entrada correcta.

Además, algunos sensores presentaban **detecciones falsas**, lo que generaba activaciones inesperadas. Para solucionar esto se realizaron las siguientes acciones:

- Se realizó un **reset del PLC LOGO!**  
- Se revisó y reconectó el cableado de sensores  
- Se realizaron **ajustes en la lógica de programación** en LOGO! Soft Comfort  
- Se repitieron varias pruebas hasta obtener un funcionamiento más estable del sistema

El proceso incluyó varias iteraciones de prueba y corrección hasta lograr que el sistema operara de forma correcta.
