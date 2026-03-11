# MR2022_4

# Proyecto Mecatrónico – MR2022

## Problema
El sistema resuelve la necesidad de automatizar cortinas industriales utilizadas para la segregación de espacios y control de accesos. El desafío principal es la gestión de seguridad de cargas pesadas, mitigando riesgos de accidentes con personal y vehículos en zonas de alto tránsito logístico mediante el uso de un prototipo a escala.

## Arquitectura del sistema
Insertar un diagrama de bloques.

[Sensores] → [Siemens LOGO] → [Relés] → [Motor DC]

## Componentes utilizados
-Sensor inductivo (LJ12A3): Detecta el cierre mecánico de la cortina mediante bordes metálicos.
-Sensor capacitivo (LJC18A3): Redundancia para detectar objetos no metálicos.
-Sensor óptico/Infrarrojo (E3F-DS30P1): Funciona como barrera de seguridad para detección de personas/obstáculos.
-Sensor magnético (FESTO SME-8): Identifica la posición del émbolo (utilizado en el contexto de integración).
-Siemens LOGO! V8: Cerebro del sistema que procesa la lógica programada en LOGO!Soft Comfort.
-Relés: Actúan como interfaz de potencia para el manejo del motor.
-Motor DC 24V: Actuador rotatorio para el enrollado/desenrollado de la malla.
-Torre de luces: Señalización visual (Roja: Proceso / Verde: Operación permitida).

## Lógica de control
-Descenso: Se activa por pulsador siempre que la zona esté libre.
-Seguridad: Si se detecta un objeto durante el descenso, el sistema invierte la marcha inmediatamente hacia el ascenso total.
-Límites: Sensores de posición (arriba, intermedio, abajo) detienen el motor automáticamente al alcanzar los extremos mecánicos.
-Interlock: Existe un bloqueo lógico que impide que las bobinas de "subir" y "bajar" se activen al mismo tiempo.

## Resultados de pruebas

| Prueba | Resultado Esperado | Resultado Obtenido | Estado |
| :--- | :--- | :--- | :---: |
| **Accionamiento de Subida** | El motor debe enrollar la cortina hasta el límite superior. | El motor operó correctamente tras ajustar el cableado inicial. |
| **Accionamiento de Bajada** | El motor debe desenrollar la cortina hasta el límite inferior. | Funcionamiento correcto y fluido del mecanismo. |
| **Interlock Lógico** | Al presionar subir y bajar simultáneamente, el sistema debe bloquear una salida. | El bloqueo lógico evitó cortocircuitos o daños en los relevadores. |
| **Seguridad (Sensor Óptico)** | Detener o invertir la marcha al detectar presencia (S1). | El sistema se detuvo inmediatamente al detectar un obstáculo. |
| **Límites de Carrera** | Detención automática al tocar los sensores de posición (S2, S4). | Sensores ajustados; el motor se detiene con precisión en los extremos. |
| **Señalización Visual** | Luz roja en movimiento y verde cuando el sistema está listo. | La torre de luces cambió de estado según la lógica programada. |
| **Detección de Objetos** | Identificación de materiales metálicos y no metálicos. | Los sensores inductivos y capacitivos enviaron señales estables al PLC. |

### Ajustes realizados durante la validación
* **Corrección de Entradas:** Se reordenó el cableado de los sensores que presentaban señales invertidas en el PLC.
* **Estabilidad de Señal:** Se eliminaron activaciones falsas en los sensores mediante el ajuste de sensibilidad y el reinicio de la lógica en *LOGO! Soft Comfort*.
* **Refuerzo de Seguridad:** Se cambió la lógica de una condición `OR` a una `AND` para obligar al cumplimiento de todos los parámetros de seguridad antes de permitir el movimiento.

## Video demo
(link al video)

## Equipo
Mauricio Rodríguez Martínez
Esteban Armando Ramírez Aviña
Massimo Orozco Ocampo
Juan Arturo López Delgado

