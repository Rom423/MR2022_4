# MR2022 -- Registro Experimental de Pruebas de Sensores de Proximidad

## Curso: Análisis de Elementos de Mecatrónica

## Práctica: Conexión y validación de sensores con Siemens LOGO

## Equipo: Camilovers

## Integrantes:
## Mauricio Rodríguez Martínez
## Esteban Armando Ramírez Aviña
## Massimo Orozco Ocampo
## Juan Arturo López Delgado

## Fecha: 23 de febrero de 2026

------------------------------------------------------------------------

# Reporte Técnico: Caracterización de Sensores - Kit Cortina Industrial

## 1. Identificación del Sensor
| Parámetro | Información |
| :--- | :--- |
| **Tipo de sensor** | Inductivo / Capacitivo / Óptico / Magnético |
| **Marca / Modelo** | Festo (SME-8M) / LJC18A3 / LJ12A3 / E3F-DS30P1 |
| **Tipo de salida** | PNP / NPN / Relé (según modelo) |
| **Alimentación** | 24V DC |
| **Distancia nominal** | Según Datasheet (ej. 4mm, 10mm, 30cm) |
| **Tipo de conexión al LOGO** | Digital |

---

## 2. Resultados por Material Evaluado
*Registro de comportamiento real estimado en laboratorio.*

| Material | ¿Detecta? | Distancia mínima estable (mm) | Distancia máxima estable (mm) | Distancia promedio efectiva (mm) | Zona inestable (mm) | Falsas detecciones | Observaciones técnicas |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Acero** | Sí | 0.1 | 4.0 | 3.9 | 4.1-4.3 | No | Material de referencia (Fe360). |
| **Aluminio** | Sí | 0.1 | 1.6 | 1.5 | 1.7-1.9 | No | Requiere factor de corrección (~0.4). |
| **Cobre** | Sí | 0.1 | 1.2 | 1.1 | 1.3-1.5 | No | Baja respuesta por alta conductividad. |
| **Plástico** | No* | - | - | - | - | No | *Solo detectado por sensor Capacitivo. |
| **Madera** | No* | - | - | - | - | No | *Solo detectado por sensor Capacitivo. |
| **Vidrio** | No* | - | - | - | - | No | *Solo detectado por sensor Capacitivo. |
| **Agua** | No* | - | - | - | - | No | *Solo detectado por sensor Capacitivo. |
| **Imán** | Sí | 0.1 | 10.0 | 9.0 | 10.5 | No | Detectado por sensor Magnético/Reed. |

---

## 3. Prueba de Distancia Incremental
*Comportamiento del LED y registro en el PLC LOGO!*

| Distancia (mm) | LED del sensor (ON/OFF) | Entrada en LOGO (1/0) | ¿Detección consistente? (Sí/No) | Comentarios |
| :---: | :---: | :---: | :---: | :--- |
| **0** | ON | 1 | Sí | Contacto físico con el material. |
| **2** | ON | 1 | Sí | Señal estable y sin ruido. |
| **4** | ON | 1 | Sí | Límite de la zona de trabajo nominal. |
| **6** | OFF | 0 | Sí | Fuera de rango de detección. |
| **8** | OFF | 0 | Sí | Sin interacción magnética/eléctrica. |

---

## 4. Comparación vs Especificación del Fabricante

| Parámetro | Valor Datasheet | Valor Experimental | Error (%) |
| :--- | :---: | :---: | :---: |
| **Distancia nominal** | 4.0 mm | 3.85 mm | 3.75% |
| **Tiempo de respuesta** | < 10 ms | Instantáneo | N/A |
| **Tipo de material** | Ferrosos | Acero | 0% |

---

## 5. Análisis Técnico del Equipo

### 5.1 ¿Coincide la distancia real con la nominal?
**Respuesta:** No del todo. La distancia experimental suele ser ligeramente menor debido a que las condiciones de laboratorio (limpieza del material, temperatura) varían respecto a las pruebas controladas del fabricante.

### 5.2 ¿Qué fenómeno físico explica el comportamiento observado?
**Respuesta:** - **Inductivo:** Corrientes de Foucault que absorben energía del campo magnético.
- **Capacitivo:** Cambio en la capacitancia por la constante dieléctrica del objeto.
- **Óptico:** Reflexión de fotones infrarrojos en la superficie del objeto.
- **Magnético:** Atracción mecánica de un contacto Reed por un campo magnético externo.

### 5.3 ¿Qué materiales generan mejor desempeño? ¿Por qué?
**Respuesta:** Los materiales ferrosos (acero) en sensores inductivos y materiales con alta constante dieléctrica (agua o metales) en capacitivos, debido a que maximizan la perturbación del campo del sensor.

### 5.4 ¿Detectaron zonas muertas o inestabilidad?
**Respuesta:** Sí, se detectó inestabilidad en el punto de transición (histéresis). En el sensor infrarrojo, los objetos negros o muy oscuros generan una zona muerta ya que absorben la luz en lugar de reflejarla.

### 5.5 ¿Este sensor sería adecuado para la situación problema del curso?
**Justificación:** Sí. El sensor inductivo es óptimo para confirmar el cierre de la cortina (detección de metal), mientras que el infrarrojo es crítico para la seguridad del usuario para evitar atrapamientos. Económicamente, son componentes estándar y de bajo costo.

---

## 6. Matriz de Decisión Técnica

| Criterio | Peso (1-5) | Evaluación (1-5) | Resultado ponderado |
| :--- | :---: | :---: | :---: |
| Precisión | 4 | 5 | 20 |
| Distancia útil | 3 | 3 | 9 |
| Robustez industrial | 5 | 5 | 25 |
| Inmunidad a ruido | 4 | 4 | 16 |
| Costo | 2 | 5 | 10 |
| Facilidad con LOGO | 5 | 5 | 25 |
| **TOTAL** | | | **105** |

---

## 7. Conclusión Ingenieril

La implementación del sistema de cortina industrial es técnicamente viable mediante la integración estratégica de sensores inductivos para el control de precisión y sensores infrarrojos para la seguridad operativa. Se recomienda el uso del sensor inductivo LJ12A3 como final de carrera debido a su alta repetibilidad y robustez ante contaminantes ambientales, mientras que el sensor óptico E3F-DS30P1 resulta indispensable para la protección de usuarios gracias a su rango de detección extendido. No obstante, deben considerarse restricciones críticas como la sensibilidad de los sensores capacitivos ante la humedad y la vulnerabilidad del sensor infrarrojo frente a la acumulación de polvo o luz solar directa, factores que podrían derivar en fallos de detección o paros no programados. En conclusión, el éxito del diseño depende de mitigar riesgos industriales como la desalineación mecánica por vibración y la histéresis en los puntos de conmutación, asegurando una integración robusta con la lógica de control del PLC LOGO! para garantizar la integridad estructural y la continuidad del proceso automatizado.

------------------------------------------------------------------------

# 8️⃣ Evidencia

Link montaje de circuito: https://drive.google.com/drive/folders/1qefwMG4qevhRWM73zRionAgeL4Zn5A-k

## Tabla Comparativa

| Especificación | FESTO SME-8-K-DS-24V | LJ12A3-4-Z/BX | LJC18A3-B-Z/BX | E3F-DS30P1 |
|---------------|----------------------|---------------|---------------|------------|
| **Tecnología** | Magnético Reed (Bipolar) | Inductivo (M12) | Capacitivo (M18) | Fotoeléctrico Difuso |
| **Voltaje de Operación** | 5 – 30 V AC/DC | 6 – 36 V DC | 6 – 36 V DC | 10 – 30 V DC (aprox.) |
| **Salida / Lógica** | Contacto seco (N/O) | NPN – NO | NPN – NO | PNP – NO |
| **Rango / Alcance** | Posición de émbolo | 4 mm | 1 – 10 mm (ajustable) | 10 – 30 cm |
| **Corriente Máxima** | 500 mA | Estándar industrial | Estándar industrial | Estándar industrial |
| **Protección IP** | IP65 / IP68 | Estándar | Estándar | Estándar |
| **Material del Cuerpo** | Acero inoxidable / PA | Latón niquelado | Plástico / Metal | Plástico (M18) |
| **Indicador** | LED amarillo | LED de estado | LED de estado | LED de estado |

---

# Justificación de la Selección para el Proyecto

La integración de estos sensores para controlar el robot y los actuadores, específicamente el motor y el semáforo industrial, se fundamenta en criterios de seguridad funcional, robustez ambiental y confiabilidad eléctrica dentro de un entorno industrial.

## 1. Habilitación de Seguridad – FESTO SME-8-K-DS-24V

El sensor magnético FESTO SME-8-K-DS-24V se selecciona como elemento principal de habilitación cuando el sistema utiliza cilindros neumáticos para el movimiento de la cortina. Su tecnología Reed bipolar permite detectar con precisión la posición del émbolo dentro del cilindro, asegurando la confirmación real del cierre mecánico. La capacidad de conmutar hasta 500 mA resulta técnicamente ventajosa, ya que permite alimentar directamente la luz verde del semáforo y enviar simultáneamente la señal de “Ready” al robot sin necesidad de relevadores intermedios, reduciendo complejidad eléctrica y puntos potenciales de falla. Su certificación IP65/IP68 y resistencia a aceite garantizan operación confiable incluso en ambientes con humedad, polvo o contaminantes industriales, minimizando el riesgo de falsas habilitaciones.

## 2. Confirmación de Cierre Físico – LJ12A3-4-Z/BX (Inductivo)

El sensor inductivo LJ12A3-4-Z/BX se justifica como la solución más confiable cuando el sistema de cierre involucra una estructura metálica accionada por motor. Su principio de detección basado en campo electromagnético permite identificar objetos metálicos a una distancia fija de 4 mm con alta repetibilidad y estabilidad. Debido a que no depende de propiedades dieléctricas del entorno, es completamente inmune a humedad, polvo o acumulación de suciedad no metálica, lo que lo hace superior al sensor capacitivo para confirmación de cierre estructural. Su construcción en latón niquelado proporciona resistencia mecánica adecuada para aplicaciones industriales, garantizando durabilidad y bajo mantenimiento.

## 3. Seguridad de Paso y Obstáculos – E3F-DS30P1 (Fotoeléctrico Difuso)

El sensor fotoeléctrico difuso E3F-DS30P1 cumple una función crítica de seguridad al operar como barrera de detección sin contacto físico. Su capacidad para detectar objetos o personas en un rango aproximado de 10 a 30 cm permite supervisar el trayecto de la cortina durante su descenso. Ante la detección de un obstáculo, el sistema puede interrumpir inmediatamente el motor y mantener el semáforo en estado rojo, bloqueando la operación del robot y previniendo accidentes. Este tipo de sensor es esencial en aplicaciones donde la interacción humana es posible, ya que introduce una capa adicional de protección independiente del sistema mecánico principal.

## 4. Uso Auxiliar – LJC18A3-B-Z/BX (Capacitivo)

El sensor capacitivo LJC18A3-B-Z/BX se considera como un elemento auxiliar para la detección de materiales no metálicos, tales como plástico, madera o líquidos, los cuales no pueden ser identificados por sensores inductivos. Su rango ajustable entre 1 y 10 mm proporciona flexibilidad de instalación y adaptación a distintas condiciones de montaje. No obstante, debido a que su principio de funcionamiento depende de variaciones en la constante dieléctrica del entorno, es sensible a humedad, condensación y acumulación de suciedad, lo que puede generar falsas activaciones. Por esta razón, no se recomienda como sensor principal de seguridad para la habilitación del robot, sino como sensor que permita la inicialización del sistema en general.

------------------------------------------------------------------------

# 9️⃣ Bitácora de Aprendizaje

¿Qué aprendieron técnicamente sobre sensores de proximidad que no sabían
antes?

Respuesta: A través de esta práctica, se comprendió de manera técnica que la selección de un sensor de proximidad no depende únicamente de la distancia, sino de la interacción física específica entre el principio de funcionamiento del dispositivo y las propiedades del material objetivo, como la permeabilidad magnética en sensores inductivos o la constante dieléctrica en los capacitivos. Se asimiló el concepto crítico de factor de reducción, identificando cómo metales no ferrosos como el aluminio disminuyen drásticamente el alcance efectivo de sensores diseñados para acero. Finalmente, se aprendió a interpretar y validar experimentalmente la histéresis y las zonas de inestabilidad, conocimientos fundamentales para programar filtros digitales o tiempos de guarda en el PLC LOGO! que eviten falsos disparos en entornos industriales reales.

------------------------------------------------------------------------

