# Bitácora – Semana 3

## Tema de la semana
Actuadores

## Actividades realizadas
Durante esta semana se implementó el control de una **cortina automatizada** mediante un **PLC Siemens LOGO!**.

El sistema utiliza un **motorreductor DC bidireccional tipo motor amarillo**, el cual permite controlar el movimiento de **subida y bajada de la cortina**.

El control del motor se realizó mediante **dos relevadores electromecánicos modelo MY4NJ con base PYF14A**, conectados a las salidas del LOGO!, permitiendo controlar el sentido de movimiento del motor.

El PLC utilizado tiene las siguientes características:

- Voltaje de alimentación: 24 V AC/DC  
- Entradas: 8 digitales  
- Salidas: 4 salidas a relé  
- Hasta 10 A @ 230 VAC  
- Hasta 3 A @ 24 VDC  
- Interfaz de comunicación: Ethernet integrada  
- Pantalla: LCD retroiluminada  
- Compatibilidad: Software LOGO! Soft Comfort  

El sistema utiliza sensores para determinar condiciones del sistema y posición de la cortina:

- Sensor inductivo  
- Sensor fotoeléctrico  
- Sensor capacitivo  
- Sensor cortina arriba  
- Sensor cortina en medio  
- Sensor cortina abajo  

También se implementó señalización mediante una **torre de luz**, utilizando dos colores:

- **Verde:** sistema listo  
- **Rojo:** sistema en proceso  

Además, se implementó un **interlock lógico obligatorio**, el cual evita que el sistema active simultáneamente las funciones de **subir y bajar**, lo cual podría provocar daños en el motor o en el sistema mecánico.

---

## Tabla de entradas y salidas

| Señal | Tipo | Descripción |
|------|------|-------------|
| I1 | Entrada | Sensor capacitivo |
| I2 | Entrada | Sensor inductivo |
| I3 | Entrada | Sensor fotoeléctrico |
| I4 | Entrada | Sensor cortina arriba |
| I5 | Entrada | Sensor cortina en medio |
| I6 | Entrada | Sensor cortina abajo |
| Q1 | Salida | Motor subir |
| Q2 | Salida | Motor bajar |
| Q3 | Salida | Lámpara roja |
| Q4 | Salida | Lámpara verde |

---

## Tabla de Interlocks

| Condición | Q1 (Motor subir) | Q2 (Motor bajar) | Explicación |
|----------|-----------------|-----------------|-------------|
| Cortina arriba activada | 0 | Puede activarse | Evita que el motor siga subiendo |
| Cortina abajo activada | Puede activarse | 0 | Evita que el motor siga bajando |
| Q1 activo | 1 | 0 | No se permite activar bajar al mismo tiempo |
| Q2 activo | 0 | 1 | No se permite activar subir al mismo tiempo |
| Sistema en movimiento | 1 o 0 | 1 o 0 | Se activa la lámpara roja |
| Sistema detenido | 0 | 0 | Se activa la lámpara verde |

El interlock se implementó mediante la lógica del **programa en LOGO!**, utilizando bloques lógicos y biestables RS para controlar las salidas.

---

## Esquema funcional

El sistema se compone de los siguientes elementos:

Sensores  
(inductivo, fotoeléctrico y capacitivo)

Sensores de posición de la cortina  
(arriba, medio y abajo)

↓  

PLC LOGO!  
(procesamiento de la lógica de control)

↓  

Relevadores de control de potencia

↓  

Motor DC bidireccional

↓  

Movimiento mecánico de la cortina

Adicionalmente, el PLC controla la **torre de señalización**:

- Q3 → Lámpara roja (sistema en proceso)  
- Q4 → Lámpara verde (sistema listo)

---

## Evidencia de seguridad

El sistema implementa medidas de seguridad dentro del programa del PLC:

**Interlock de motor**

Se implementó un interlock lógico que evita que las salidas **Q1 y Q2** se activen al mismo tiempo, cumpliendo con la condición obligatoria de **no subir y bajar simultáneamente**.

**Control por sensores de posición**

Los sensores de posición permiten detectar cuando la cortina está:

- arriba
- en medio
- abajo

Esto permite detener el motor cuando se alcanza un límite y evitar movimientos indebidos.

**Señalización visual**

El sistema utiliza una torre de luz para indicar el estado del sistema:

- Luz verde → sistema listo  
- Luz roja → sistema en movimiento  

Esto permite identificar visualmente el estado del sistema durante la operación.

---

## Decisiones de ingeniería

| Decisión | Alternativas | Justificación |
|--------|-------------|---------------|
| Uso de relevadores para controlar el motor | Puente H electrónico o driver de motor | Los relevadores permiten controlar fácilmente el sentido de giro del motor con un sistema simple |
| Implementar interlock en el PLC | Interlock cableado | La lógica en el PLC permite modificar y ajustar el comportamiento del sistema |
| Uso de torre de luz | LEDs simples | Permite identificar claramente el estado del sistema |
| Uso de motorreductor DC | Motor DC sin reductora | La reductora proporciona mayor torque para mover la cortina |

---

## Problema técnico encontrado
Durante la implementación del sistema el **PLC LOGO! no aceptaba el programa correctamente**, lo que dificultaba la carga de la lógica de control.

También se detectó que algunos **sensores presentaban detecciones falsas de señal**, lo que provocaba activaciones incorrectas del sistema.

---

## Solución aplicada
Para resolver el problema se realizaron varias acciones:

- Reset del PLC LOGO!  
- Reconexión del cableado de sensores  
- Ajuste en la lógica de programación  
- Pruebas repetidas del sistema hasta lograr un funcionamiento correcto

---

## Conexión con el curso
Durante esta semana se aplicaron conceptos del curso **MR2022**, principalmente relacionados con:

- Actuadores
- Control de motores DC
- Programación de PLC
- Interlocks de seguridad
- Integración de sensores y actuadores

---

## Autoevaluación
- ⬜ Muy perdido
- ⬜ Con dudas
- ☑ Entendiendo
- ⬜ Dominando
