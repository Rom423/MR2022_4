# Bitácora – Semana 3

## Tema de la semana
Actuadores

---

# Actividades realizadas

Durante esta semana se implementó el control de una **cortina automatizada** utilizando un **PLC Siemens LOGO!**.

El sistema controla el movimiento de subida y bajada de la cortina mediante un **motorreductor DC bidireccional**. El cambio de dirección del motor se realiza utilizando **dos relevadores electromecánicos**, controlados por las salidas del PLC.

El sistema utiliza sensores para detectar el estado del sistema y la posición de la cortina:

- Sensor de presencia  
- Sensor cortina arriba  
- Sensor cortina en medio  
- Sensor cortina abajo  

También se implementó señalización mediante una **torre de luz**:

- **Verde:** sistema listo  
- **Rojo:** sistema en proceso  

Para proteger el sistema se implementó un **interlock lógico**, que evita que el motor reciba simultáneamente la orden de subir y bajar.

---

# Tabla de entradas y salidas

| Señal | Tipo | Descripción |
|------|------|-------------|
| I1 | Entrada | Sensor de presencia |
| I2 | Entrada | Sensor cortina arriba |
| I3 | Entrada | Sensor cortina en medio |
| I4 | Entrada | Sensor cortina abajo |
| I5 | Entrada | Pulsador de inicio |
| Q1 | Salida | Motor subir |
| Q2 | Salida | Motor bajar |
| Q3 | Salida | Lámpara roja |
| Q4 | Salida | Lámpara verde |

---

# Lógica de seguridad (Interlock)

| Condición | Q1 (Subir) | Q2 (Bajar) | Función |
|----------|------------|------------|--------|
| Cortina arriba | 0 | Permitido | Evita seguir subiendo |
| Cortina abajo | Permitido | 0 | Evita seguir bajando |
| Motor subir activo | 1 | 0 | Evita activar bajada simultánea |
| Motor bajar activo | 0 | 1 | Evita activar subida simultánea |

Este interlock se implementó en el **programa del PLC** utilizando bloques lógicos y biestables RS.

---

# Esquema funcional del sistema

Sensores de presencia y posición  
↓  
PLC LOGO! (control lógico)  
↓  
Relevadores de potencia  
↓  
Motorreductor DC  
↓  
Movimiento de la cortina

El PLC también controla la torre de señalización:

- Q3 → lámpara roja (sistema en movimiento)  
- Q4 → lámpara verde (sistema listo)

---

# Medidas de seguridad implementadas

**Interlock de motor**

Se evita que las salidas **Q1 y Q2** se activen simultáneamente, evitando conflictos en el motor.

**Sensores de posición**

Los sensores detectan cuando la cortina alcanza sus límites superior e inferior, deteniendo el motor automáticamente.

**Señalización visual**

La torre de luz permite identificar rápidamente el estado del sistema:

- verde → sistema listo  
- rojo → sistema en movimiento  

---

# Decisiones de ingeniería

| Decisión | Justificación |
|--------|---------------|
| Uso de relevadores para controlar el motor | Permite controlar fácilmente el sentido de giro del motor |
| Implementar interlock en el PLC | Permite modificar y ajustar la lógica sin cambiar el cableado |
| Uso de torre de luz | Facilita identificar el estado del sistema durante la operación |
| Uso de motorreductor DC | Proporciona mayor torque para mover la cortina |

---

# Problemas encontrados

Durante la implementación se presentaron dos problemas principales:

- El **PLC no aceptaba inicialmente el programa**, lo que dificultaba la carga de la lógica.
- Algunos **sensores generaban detecciones falsas**, provocando activaciones incorrectas.

---

# Solución aplicada

Para resolver estos problemas se realizaron las siguientes acciones:

- reinicio del PLC  
- revisión y reconexión del cableado de sensores  
- ajustes en la lógica de programación  
- pruebas repetidas hasta lograr un funcionamiento estable

---

# Conexión con el curso

Durante esta semana se aplicaron conceptos relacionados con:

- actuadores  
- control de motores DC  
- programación de PLC  
- interlocks de seguridad  
- integración de sensores y actuadores  

---

# Autoevaluación

- ⬜ Muy perdido  
- ⬜ Con dudas  
- ☑ Entendiendo  
- ⬜ Dominando  
