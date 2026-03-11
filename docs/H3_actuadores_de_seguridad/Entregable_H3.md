# Hito 3 – Control e Integración

## Descripción de la lógica de control

La lógica de control fue implementada en un **PLC Siemens LOGO!** utilizando el software **Siemens LOGO! Soft Comfort**. El sistema controla el movimiento de una **cortina de seguridad** mediante un **motor DC bidireccional**.

El PLC recibe señales de sensores de posición y de un **sensor óptico de presencia**. Con base en estas señales, el programa activa las salidas correspondientes que controlan relevadores encargados de energizar el motor en cada dirección.

Las salidas principales del sistema son:

- **Q1:** activa el motor para **subir la cortina**
- **Q2:** activa el motor para **bajar la cortina**

Se implementó un **interlock lógico** que evita que ambas direcciones del motor se activen simultáneamente, previniendo daños en el motor o en los relevadores.

El sistema también incluye señalización visual mediante una torre de luz:

- **Q3:** luz roja cuando el sistema está en proceso  
- **Q4:** luz verde cuando el sistema está listo o habilitado para operar  

El sistema incorpora un **sensor óptico de seguridad** encargado de detectar la presencia de una persona cerca de la cortina. Para aumentar la seguridad del sistema, este sensor se utiliza **negado dentro de la lógica de control**, lo que significa que **el sistema solo permite movimiento cuando el sensor NO detecta presencia**.

Durante el desarrollo del programa se realizó un ajuste en la lógica de seguridad, reemplazando una condición **OR** por una condición **AND**, con el objetivo de exigir que **todas las condiciones de seguridad se cumplan simultáneamente antes de permitir el movimiento del motor**.

Los sensores de posición permiten detectar cuando la cortina se encuentra:

- completamente arriba  
- en posición intermedia  
- completamente abajo  

Esto permite detener el motor automáticamente al alcanzar los límites mecánicos.

---

# Diagrama de conexión del sistema

A continuación se muestra el diagrama eléctrico general del sistema de control implementado para la cortina industrial. En este diagrama se pueden observar las conexiones entre el PLC Siemens LOGO!, los sensores de posición, el sensor óptico de presencia, los relevadores de potencia y el motor DC que acciona la cortina.

Este diagrama fue utilizado durante la etapa de integración para verificar las conexiones de entradas, salidas y alimentación del sistema.

<img width="961" height="740" alt="image" src="https://github.com/user-attachments/assets/5cd50df2-420f-4495-8cb7-e45e62b30724" />

---

# Entradas y salidas

## Entradas

| Entrada | Tipo | Función |
|------|------|------|
| I1 | Sensor | Sensor óptico de presencia (1 = persona detectada, 0 = zona libre) |
| I2 | Sensor | Sensor cortina arriba |
| I3 | Sensor | Sensor cortina intermedia |
| I4 | Sensor | Sensor cortina abajo |
| I5 | Pulsador | Pulsador de inicio |

---

## Salidas

| Salida | Tipo | Función |
|------|------|------|
| Q1 | Actuador | Motor subir |
| Q2 | Actuador | Motor bajar |
| Q3 | Indicador | Lámpara roja (proceso) |
| Q4 | Indicador | Lámpara verde (operación permitida) |

---

# Ecuaciones lógicas del sistema

## Motor bajar

Activación del motor para bajar la cortina:

```

SET(M_bajar) = P • ¬S1 • S2
RESET(M_bajar) = S4
Q2 = M_bajar • ¬S1 • ¬M_subir

```

Donde:

- **S1** = sensor óptico de presencia  
- **¬S1** = zona libre (no se detecta persona)

---

## Temporizador

El temporizador inicia cuando la cortina llega a la posición inferior y no hay presencia detectada.

```

T_IN = S4 • ¬S1
T = TON(10 s)

```

---

## Motor subir

El motor de subida se activa automáticamente cuando el temporizador termina.

```

SET(M_subir) = T
RESET(M_subir) = S2
Q1 = M_subir • ¬S1 • ¬M_bajar

```

---

## Permisivo del sistema

El sistema puede operar únicamente cuando la cortina está abajo, no hay presencia detectada y el temporizador aún no ha finalizado.

```

R = S4 • ¬S1 • ¬T

```

---

## Señalización visual

```

Q4 = R
Q3 = ¬R

```

---

# Tabla de verdad simplificada

| Estado | S1 | S2 | S3 | S4 | P | T | Q1 Subir | Q2 Bajar | Q3 Roja | Q4 Verde | Descripción |
|------|------|------|------|------|------|------|------|------|------|------|------|
| Inicial | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | Cortina arriba |
| Inicio | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0 | Cortina bajando |
| Intermedio | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | Movimiento descendente |
| Abajo | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | Sistema habilitado |
| Temporizador | 0 | 0 | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 | Cortina subiendo |
| Subiendo | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | Movimiento ascendente |
| Final | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | Regreso al estado inicial |
| Seguridad | 1 | * | * | * | * | * | 0 | 0 | 1 | 0 | Persona detectada |

*El símbolo `*` indica que el valor de la entrada no afecta el resultado.*

---

# Pruebas realizadas

| Prueba | Resultado esperado | Resultado obtenido |
|------|------|------|
| Activar subida del motor | El motor debe subir la cortina | Inicialmente el motor no respondió |
| Revisión del cableado del motor | El motor debería activarse correctamente | Se detectó un problema en la conexión |
| Nueva prueba de subida | El motor debe subir la cortina | Funcionamiento correcto después del ajuste |
| Activar bajada del motor | El motor debe bajar la cortina | Funcionamiento correcto |
| Activar subir y bajar simultáneamente | El sistema debe bloquear una de las salidas | El interlock evitó la activación simultánea |
| Verificar sensores de posición | Cada sensor debe enviar señal correcta | Se detectó que algunos sensores estaban invertidos |
| Corrección de conexión de sensores | Los sensores deben detectar correctamente | Los sensores comenzaron a responder correctamente |
| Prueba de detección de sensores | El PLC debe recibir señales estables | Se detectaron señales falsas en algunos sensores |
| Revisión del sistema | El sistema debe operar de forma estable | Después de ajustes en la lógica el sistema funcionó correctamente |
| Prueba de señalización | Luz roja en proceso y verde en reposo | Funcionamiento correcto |

---

# Ajustes realizados

Durante las pruebas de integración se detectaron diversos problemas que requirieron ajustes tanto en el hardware como en la lógica de control.

Inicialmente el motor no respondía correctamente. Se realizó una revisión completa del cableado del motor y de los relevadores de potencia. Después de corregir las conexiones se logró activar el motor correctamente.

Posteriormente se detectó que algunos sensores estaban conectados a entradas incorrectas del PLC, lo que provocaba lecturas invertidas en el sistema. Se reorganizaron las conexiones para que cada sensor correspondiera a su entrada definida en la programación.

También se observaron activaciones falsas en algunos sensores. Para solucionar este problema se realizaron las siguientes acciones:

- reinicio del PLC LOGO!  
- revisión y reconexión del cableado de sensores  
- ajustes en la lógica de programación en Siemens LOGO! Soft Comfort  
- repetición de pruebas funcionales  

Después de varias iteraciones de prueba y corrección, el sistema alcanzó un funcionamiento estable y consistente.
```
