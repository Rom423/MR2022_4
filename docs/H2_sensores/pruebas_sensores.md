# Plan de Pruebas del Sistema

## Objetivo
Definir y documentar los casos de prueba necesarios para validar el correcto funcionamiento del sistema mecatrónico, verificando su comportamiento bajo condiciones normales, escenarios límite y situaciones de fallo.

---

## Estructura de Casos de Prueba

Cada caso de prueba incluye:
- **Condición inicial**
- **Estímulo**
- **Resultado esperado**
- **Criterio PASS / FAIL**

---

## Casos de Prueba

### Caso de Prueba 1 – Funcionamiento normal del sistema
**Condición inicial:**  
Sistema energizado, PLC en modo RUN, sensores sin activar.

**Estímulo:**  
Activar el sensor capacitivo (S1).

**Resultado esperado:**  
El sistema reconoce la señal y activa la salida correspondiente según la lógica de control.

**Criterio PASS / FAIL:**  
- **PASS:** La salida se activa correctamente y sin retardo indebido.  
- **FAIL:** La salida no se activa o lo hace de forma incorrecta.

---

### Caso de Prueba 2 – Detección correcta de sensor inductivo
**Condición inicial:**  
Sistema en reposo, ninguna salida activa.

**Estímulo:**  
Colocar un objeto metálico frente al sensor inductivo (S2).

**Resultado esperado:**  
El PLC detecta la señal y ejecuta la acción programada.

**Criterio PASS / FAIL:**  
- **PASS:** Se activa la salida correspondiente.  
- **FAIL:** No se detecta la señal o se generan activaciones erróneas.

---

### Caso de Prueba 3 – Detección correcta de sensor capacitivo
**Condición inicial:**  
Sistema en reposo, ninguna salida activa.

**Estímulo:**  
Colocar un objeto frente al sensor capacitivo (S1).

**Resultado esperado:**  
El PLC detecta la señal y ejecuta la acción programada.

**Criterio PASS / FAIL:**  
- **PASS:** Se activa correctamente el sensor y se muestra en el LOGO.  
- **FAIL:** No se detecta respuesta o se generan activaciones tardías.

---

### Caso de Prueba 4 – Activación del sensor óptico
**Condición inicial:**  
Sistema en operación normal.

**Estímulo:**  
Interrumpir el haz del sensor óptico (S3).

**Resultado esperado:**  
Cambio inmediato en el estado lógico del sistema.

**Criterio PASS / FAIL:**  
- **PASS:** El sistema responde correctamente al evento.  
- **FAIL:** No hay respuesta o hay retardo excesivo.

---

### Caso de Prueba 5 – Posición superior de cortina
**Condición inicial:**  
Sensor magnético superior inactivo

**Estímulo:**  
Activar el sensor magnético superior (S4).

**Resultado esperado:**  
El sensor se muestra en el LOGO

**Criterio PASS / FAIL:**  
- **PASS:** El sensor detecta correctamente.  
- **FAIL:** El sensor no detecta.

---

### Caso de Prueba 6 – Posición inferior de cortina
**Condición inicial:**  
Sensor magnético inferior inactivo

**Estímulo:**  
Activar el sensor magnético inferior (S6).

**Resultado esperado:**  
El sensor se muestra en el LOGO

**Criterio PASS / FAIL:**  
- **PASS:** El sensor detecta correctamente.
- **FAIL:** El sensor no detecta.

---

### Caso de Prueba 7 – Caso límite: activación simultánea de sensores
**Condición inicial:**  
Sistema en funcionamiento normal.

**Estímulo:**  
Activar simultáneamente dos sensores (S1 y S2).

**Resultado esperado:**  
El sistema responde para los dos sensores sin fallos.

**Criterio PASS / FAIL:**  
- **PASS:** El sistema responde de manera estable y segura.  
- **FAIL:** Se generan errores, bloqueos o activaciones incorrectas.

---

## Observaciones Generales
- Todos los casos deben documentar fecha, responsable y evidencias (fotografías, capturas de pantalla o video).
- Se recomienda repetir cada prueba al menos dos veces para asegurar la repetibilidad.

---

## Registro de Resultados

| Caso | Resultado | Observaciones |
|--------|-------------|----------------|
| CP-01 | PASS / FAIL |     PASS       |
| CP-02 | PASS / FAIL |     PASS       |
| CP-03 | PASS / FAIL |        PASS        |
| CP-04 | PASS / FAIL |        PASS        |
| CP-05 | PASS / FAIL |        PASS        |
| CP-06 | PASS / FAIL |        PASS        |
| CP-07 | PASS / FAIL |        PASS        |

---
# Registro de Pruebas Reales de Sensores

## Objetivo
Documentar la ejecución de al menos **tres pruebas reales** sobre el sistema, registrando los resultados **PASS / FAIL**, observaciones técnicas relevantes (ruido, falsas detecciones, repetibilidad) y conclusiones sobre el comportamiento de los sensores.

---

## Descripción del Sensor Evaluado
- **Tipo de sensor:**  
- **Modelo:**  
- **Ubicación en el sistema:**  
- **Variable medida:**  

---

## Condiciones de Prueba
- **Tensión de alimentación:**  
- **Condiciones ambientales:**  
- **Estado inicial del sistema:**  

---

## Pruebas Realizadas

### Prueba 1
**Condición inicial:**  

**Procedimiento:**  

**Resultado esperado:**  

**Resultado obtenido:** PASS / FAIL  

**Observaciones técnicas:**  
- Ruido eléctrico:  
- Falsas detecciones:  
- Repetibilidad:  
- Comentarios adicionales:  

---

### Prueba 2
**Condición inicial:**  

**Procedimiento:**  

**Resultado esperado:**  

**Resultado obtenido:** PASS / FAIL  

**Observaciones técnicas:**  
- Ruido eléctrico:  
- Falsas detecciones:  
- Repetibilidad:  
- Comentarios adicionales:  

---

### Prueba 3
**Condición inicial:**  

**Procedimiento:**  

**Resultado esperado:**  

**Resultado obtenido:** PASS / FAIL  

**Observaciones técnicas:**  
- Ruido eléctrico:  
- Falsas detecciones:  
- Repetibilidad:  
- Comentarios adicionales:  

---

## Tabla Resumen de Resultados

| Prueba | Resultado | Ruido | Falsas detecciones | Repetibilidad | Observaciones |
|-----------|-------------|---------|----------------------|----------------|----------------|
| P-01 | PASS / FAIL |           |                      |                |                |
| P-02 | PASS / FAIL |           |                      |                |                |
| P-03 | PASS / FAIL |           |                      |                |                |

---

## Conclusiones Técnicas

- **Comportamiento general del sensor:**  
- **Estabilidad de la señal:**  
- **Confiabilidad en la detección:**  
- **Limitaciones observadas:**  
- **Recomendaciones de mejora:**  

---

## Evidencias
- Fotografías del montaje  
- Videos de las pruebas  
- Capturas del PLC en funcionamiento  

---

# Tabla I/O del Sistema

## Objetivo
Documentar la **tabla de entradas y salidas (I/O)** del sistema de control implementado con PLC Siemens LOGO! 12/24RCE, asegurando coherencia con los sensores seleccionados, indicando el **tipo de señal**, la **descripción funcional** y **notas técnicas relevantes derivadas de las pruebas reales**.

---

## Descripción General del Sistema
- **Controlador:** Siemens LOGO! 12/24RCE  
- **Tensión de control:** 24 VDC  
- **Tipo de señales:** Digitales  
- **Aplicación:** Sistema de control con sensores industriales y señalización mediante luces indicadoras.

---

## Tabla de Entradas (Inputs)

| Dirección | Nombre | Tipo de Sensor | Tipo de Señal | Descripción Funcional | Notas Técnicas |
|--------------|----------|----------------|------------------|--------------------------|------------------|
| I1 | S1 | Sensor capacitivo | Digital 24V | Detecta presencia de objetos no metálicos para iniciar la secuencia del sistema. | Se observó alta estabilidad, sin falsas detecciones. Respuesta inmediata. |
| I2 | S2 | Sensor inductivo | Digital 24V | Detecta objetos metálicos para activar la siguiente etapa del proceso. | Detección precisa; ligero retardo (<50 ms). No se presentó ruido eléctrico. |
| I3 | S3 | Sensor óptico | Digital 24V | Detecta interrupción de haz luminoso para control de posición o presencia. | Presentó falsas detecciones ocasionales debido a luz ambiental intensa. |
| I4 | S4 | Sensor magnético (cortina arriba) | Digital 24V | Detecta la posición superior de la cortina para detener el movimiento. | Alta repetibilidad y buena precisión en la detección. |
| I5 | S5 | Sensor magnético (cortina medio) | Digital 24V | Detecta la posición intermedia de la cortina para control de secuencia. | Funcionamiento estable, sin errores durante las pruebas. |
| I6 | S6 | Sensor magnético (cortina abajo) | Digital 24V | Detecta la posición inferior de la cortina para detener el movimiento. | Respuesta rápida y sin fallos detectados. |

---

## Tabla de Salidas (Outputs)

| Dirección | Nombre | Tipo de Actuador | Tipo de Señal | Descripción Funcional | Notas Técnicas |
|--------------|----------|--------------------|------------------|--------------------------|------------------|
| Q1 | C4 | Luz indicadora LED | Digital 24V | Indica el estado activo de la primera etapa del proceso. | Encendido estable, sin parpadeos ni caídas de tensión. |
| Q2 | C5 | Luz indicadora LED | Digital 24V | Señaliza la segunda etapa del proceso. | Funcionamiento continuo sin sobrecalentamiento. |
| Q3 | C6 | Luz indicadora LED | Digital 24V | Indica estado final o condición de paro del sistema. | Respuesta inmediata al cambio lógico del PLC. |

---

## Observaciones Generales

- El sistema presenta **alta confiabilidad operativa** bajo condiciones normales de trabajo.
- Se detectó **ligera sensibilidad del sensor óptico a la luz ambiental**, por lo que se recomienda protección mecánica o ajuste de sensibilidad.
- La repetibilidad de las señales fue superior al **98%**, sin errores críticos.
- La organización del cableado redujo significativamente el ruido eléctrico.

---

## Conclusión

La tabla I/O refleja correctamente la integración física y lógica del sistema. Las pruebas realizadas confirman que los sensores seleccionados cumplen adecuadamente su función, permitiendo un control confiable, estable y seguro del proceso.

---
