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

### Caso de Prueba 3 – Activación del sensor óptico
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

### Caso de Prueba 4 – Posición superior de cortina
**Condición inicial:**  
Cortina en movimiento hacia arriba.

**Estímulo:**  
Activar el sensor magnético superior (S4).

**Resultado esperado:**  
Detención inmediata del movimiento.

**Criterio PASS / FAIL:**  
- **PASS:** El movimiento se detiene correctamente.  
- **FAIL:** La cortina continúa su desplazamiento.

---

### Caso de Prueba 5 – Posición inferior de cortina
**Condición inicial:**  
Cortina en movimiento hacia abajo.

**Estímulo:**  
Activar el sensor magnético inferior (S6).

**Resultado esperado:**  
El sistema detiene el movimiento y queda en reposo.

**Criterio PASS / FAIL:**  
- **PASS:** El sistema se detiene sin errores.  
- **FAIL:** No se detiene o se genera una acción incorrecta.

---

### Caso de Prueba 6 – Caso límite: activación simultánea de sensores
**Condición inicial:**  
Sistema en funcionamiento normal.

**Estímulo:**  
Activar simultáneamente dos sensores (S1 y S2).

**Resultado esperado:**  
El sistema responde según la prioridad lógica programada sin fallos.

**Criterio PASS / FAIL:**  
- **PASS:** El sistema responde de manera estable y segura.  
- **FAIL:** Se generan errores, bloqueos o activaciones incorrectas.

---

### Caso de Prueba 7 – Caso de fallo: desconexión de un sensor
**Condición inicial:**  
Sistema funcionando normalmente.

**Estímulo:**  
Desconectar físicamente el sensor óptico (S3).

**Resultado esperado:**  
El sistema detecta la anomalía o entra en estado seguro.

**Criterio PASS / FAIL:**  
- **PASS:** El sistema mantiene operación segura.  
- **FAIL:** Se generan fallas, activaciones inesperadas o bloqueo del sistema.

---

## Observaciones Generales
- Todos los casos deben documentar fecha, responsable y evidencias (fotografías, capturas de pantalla o video).
- Se recomienda repetir cada prueba al menos dos veces para asegurar la repetibilidad.

---

## Registro de Resultados

| Caso | Resultado | Observaciones |
|--------|-------------|----------------|
| CP-01 | PASS / FAIL |                |
| CP-02 | PASS / FAIL |                |
| CP-03 | PASS / FAIL |                |
| CP-04 | PASS / FAIL |                |
| CP-05 | PASS / FAIL |                |
| CP-06 | PASS / FAIL |                |
| CP-07 | PASS / FAIL |                |

---
