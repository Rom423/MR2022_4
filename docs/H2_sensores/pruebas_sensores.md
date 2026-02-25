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

### Caso de Prueba 5 – Posición inferior de cortina
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
