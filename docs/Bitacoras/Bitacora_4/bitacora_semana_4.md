# Bitácora – Semana 4

## Tema de la semana

Control y validación del sistema

---

# Actividades realizadas

Durante esta semana se completó la integración del sistema de control de la cortina automatizada utilizando un **PLC Siemens LOGO!**.

Se implementó el comportamiento completo del sistema mediante una **máquina de estados**, lo que permitió organizar el funcionamiento de la cortina en diferentes etapas de operación, incluyendo los estados de movimiento, reposo y condiciones de seguridad.

Las principales actividades realizadas fueron:

- implementación de la lógica completa de control en el PLC
- verificación del funcionamiento de los sensores de posición
- pruebas del sistema de subida y bajada del motor
- ajuste de la lógica de control para evitar activaciones simultáneas del motor
- ejecución de pruebas funcionales del sistema
- documentación de resultados obtenidos durante la validación

---

# Decisiones de ingeniería

| Decisión | Justificación |
|------|------|
| Uso de máquina de estados | Permite organizar el comportamiento del sistema de forma clara y estructurada |
| Implementación de interlock en el PLC | Protege el motor evitando que se activen simultáneamente las direcciones de giro |
| Uso de sensores de posición | Permite detener automáticamente la cortina si detecta la presencia de un usuario |
| Uso de torre de luz | Permite identificar fácilmente el estado de operación del sistema |

---

# Problemas técnicos encontrados

Durante la implementación se presentaron diversos problemas:

- incompatibilidad entre la versión del firmware del PLC y el programa
- sensor instalado en posición invertida
- distancia insuficiente entre el sensor y el imán
- velocidad excesiva del motor

Estos problemas afectaban el comportamiento correcto del sistema durante las pruebas iniciales.

---

# Solución aplicada

Para resolver los problemas detectados se realizaron las siguientes acciones:

- ajuste de configuración del proyecto para coincidir con el firmware del PLC
- corrección lógica del sensor invertido en la programación
- ajustes mecánicos en la posición del sensor para mejorar su detección
- modificación del recorrido de la cortina para mejorar la detección de los sensores
- repetición de pruebas hasta lograr un comportamiento más estable del sistema

---

# Conexión con el curso

Durante esta semana se aplicaron diversos conceptos del curso:

- programación de PLC
- control lógico
- diseño de máquinas de estados
- integración de sensores y actuadores
- validación de sistemas de control

---

# Autoevaluación

- ⬜ Muy perdido  
- ⬜ Con dudas  
- ☑ Entendiendo  
- ⬜ Dominando
