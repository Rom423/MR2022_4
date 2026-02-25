# Matriz Técnica de Sensores

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


La selección combinada de sensores establece una arquitectura equilibrada entre confirmación mecánica, detección metálica precisa, protección activa contra obstáculos y detección auxiliar de materiales no metálicos. Esta integración mejora la seguridad operativa, reduce el riesgo de activaciones erróneas y asegura confiabilidad en condiciones industriales exigentes, cumpliendo con criterios técnicos de robustez y funcionalidad.
