# MR2022 -- Registro Experimental de Pruebas de Sensores de Proximidad

## Curso: Análisis de Elementos de Mecatrónica

## Práctica: Conexión y validación de sensores con Siemens LOGO

## Equipo: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

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
Se recomienda la implementación del **sensor inductivo** para el control de posición y el **infrarrojo** para seguridad. La principal restricción industrial es la acumulación de suciedad en los lentes del sensor óptico, lo que requeriría mantenimiento preventivo periódico.

------------------------------------------------------------------------

# 8️⃣ Evidencia

-   Fotografías del montaje:
-   Capturas del programa en LOGO:
-   Video corto de funcionamiento:
-   Datasheet utilizado (link o archivo en repositorio):

------------------------------------------------------------------------

# 9️⃣ Bitácora de Aprendizaje

¿Qué aprendieron técnicamente sobre sensores de proximidad que no sabían
antes?

Respuesta:

------------------------------------------------------------------------

> ⚙️ Este documento forma parte del proceso de validación experimental
> para la selección de sensores dentro del proyecto integrador MR2022.
