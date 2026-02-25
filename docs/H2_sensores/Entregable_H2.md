# Hito 2 – Selección de Componentes

# Sensores seleccionados

| Sensor | Variable | Tipo de señal | Justificación |
|---------|-----------|---------------|----------------|
| **Magnético FESTO SME-8** | Posición del émbolo del cilindro neumático | Digital PNP/NPN (hasta 500 mA) | Sensor especializado para cilindros neumáticos. Tecnología Reed estándar industrial. Protección IP68. Permite alimentar directamente el LED verde del semáforo y la entrada del robot sin relés adicionales. Alta confiabilidad en ambientes húmedos o con aceite. |
| **Inductivo LJ12A3** | Presencia de metal (cierre mecánico de cortina) | Digital NPN-NO | Detecta bordes metálicos a 4 mm con alta consistencia. Cuerpo de latón niquelado resistente. No se activa por humedad o contacto humano. Solución robusta y económica para confirmación de cierre metálico. |
| **Capacitivo LJC18A3** | Presencia de materiales no metálicos (plástico, madera, líquido o aislamiento) | Digital NPN/PNP configurable | Permite detectar objetos no metálicos que el sensor inductivo no puede identificar. Cuerpo M18 con ajuste de sensibilidad (1–10 mm). Útil como sensor redundante o en aplicaciones para inicializar la actividad de la cortina.|
| **Infrarrojo E3F-DS30P1** | Presencia de obstáculos/personas | Digital PNP/NPN | Funciona como cortina de luz de seguridad. Detecta objetos sin contacto físico (alcance 30 cm). Previene colisiones y protege tanto al motor como al personal durante el descenso de la cortina. |

---

# Actuadores seleccionados

| Actuador | Función | Tipo | Justificación |
|-----------|----------|------|----------------|
| **Motor eléctrico AC/DC** | Movimiento de enrollado o desplazamiento mecánico | Actuador rotativo eléctrico | Permite control continuo y preciso del movimiento. Puede integrarse con sensores inductivo o capacitivo para confirmación de cierre. Bajo mantenimiento comparado con sistemas puramente mecánicos. |
| **Semáforo industrial LED** | Indicación visual de estado (verde/rojo) | Actuador luminoso | Permite señalización clara al operario. Puede ser alimentado directamente por la salida del sensor magnético (hasta 500 mA), reduciendo complejidad eléctrica. |

---

# Riesgos y consideraciones

## ¿Qué podría fallar y cómo se mitiga?

### 1. Falsas detecciones por humedad (sensor capacitivo)
- **Riesgo:** El sensor capacitivo puede activarse por acumulación de humedad, condensación o polvo.
- **Mitigación:**
  - Ajustar correctamente el potenciómetro de sensibilidad.
  - Implementar limpieza periódica.
  - Usarlo como respaldo y no como único sensor de seguridad.

---

### 2. Fallo por desgaste mecánico del sensor Reed
- **Riesgo:** Pérdida de señal con el tiempo por fatiga del contacto interno.
- **Mitigación:**
  - Migrar a sensores de estado sólido (ej. SMC D-M9B o FESTO SMT-8M).
  - Implementar mantenimiento predictivo basado en número de ciclos.

---

### 3. Falsos reflejos en sensor infrarrojo
- **Riesgo:** Activación incorrecta por superficies brillantes o suciedad en el lente.
- **Mitigación:**
  - Limpieza preventiva del lente.
  - Ajuste correcto del ángulo de instalación.
  - Uso de sensores con haz enfocado si se requiere mayor precisión.

---

### 4. Falla eléctrica por sobrecorriente
- **Riesgo:** Daño en salidas del sensor o entradas del robot.
- **Mitigación:**
  - Verificar corriente máxima (500 mA en SME-8).
  - Incorporar protección con fusibles o relevadores intermedios si la carga excede el límite permitido.



