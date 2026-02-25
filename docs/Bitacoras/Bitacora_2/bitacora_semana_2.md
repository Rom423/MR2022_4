# Bitácora – Semana 2

## Tema de la semana
(Sensores / Actuadores / Control / HMI)

## Actividades realizadas
**Análisis del problema:** Se elaboró el diagrama de conexiones en draw.io, identificando entradas, salidas, alimentación, sensores y actuadores, cuidando la correcta asignación de terminales y la organización del cableado.

**Identificación de los componentes del sistema mecatrónico:** 

**Sensores:** sensor capacitivo, sensor inductivo, sensor óptico, sensores magnéticos.
**Actuadores:** Luces indicadoras (rojo, amarillo, verde) y motores.
**Controlador** Módulo lógico Siemens LOGO! 12/24RCE
**Fuente de energía:** alimentación 12/24V DC.
**Otros elementos:** Clemas, cableado, herramienta y protección eléctrica.

**Construcción física del sistema:**
Se realizó el montaje real del circuito, conectando la fuente, el PLC, sensores y actuadores conforme al diagrama, verificando polaridades, continuidad y correcta fijación de los componentes.

**Pruebas de funcionamiento:**
Se energizó el sistema y se verificó la correcta detección de los sensores, así como la activación adecuada de las salidas según.

## Decisiones de ingeniería
| Decisión                              | Alternativas                                  | Justificación                                                                                                            |
| ------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Diseñar el diagrama en draw.io    | Croquis a mano            | Permite mayor claridad, orden, facilidad de corrección y documentación del sistema. |
| Uso de sensores industriales reales | Simulación únicamente         | Se buscó una experiencia práctica completa y validación real del funcionamiento del sistema. |
| Separar alimentación y señales          | Conexión directa sin clemas | Mejora la seguridad, orden y mantenimiento del sistema. 
| Implementar protecciones en entradas y salidas    | Conexión directa sin protección           | Reduce el riesgo de daño al PLC y mejora la confiabilidad del sistema. |                                 

## Problema técnico encontrado
Durante el montaje inicial, se presentaron fallas en la lectura de algunos sensores, provocadas por errores en el cableado (cables flojos o mal pelados) y conexiones cruzadas en las clemas.

## Solución aplicada
Se revisó completamente el diagrama eléctrico y se comparó con el montaje físico, corrigiendo la conexiones erróneas, reorganizando el cableado y verificando continuidad. También se comprobó la correcta asignación de entradas y salidas en el LOGO.

Tras estos ajustes, el sistema funcionó correctamente, permitiendo la detección adecuada de los sensores y la activación precisa de los actuadores, validando el diseño propuesto.
## Conexión con el curso
En esta semana se aplicaron los principios fundamentales de integración de sistemas mecatrónicos, abordando la relación entre diseño, simulación y construcción física. Se fortalecieron habilidades relacionadas con la interpretación de diagramas eléctricos, la implementación práctica de sistemas de control, el manejo de sensores industriales y como interactúan diversos dispositivos.

Además, se desarrolló el pensamiento ingenieril al diagnosticar fallas, proponer soluciones y validar el funcionamiento real del sistema, consolidando los conocimientos teóricos mediante la práctica experimental.

## Autoevaluación
- ⬜ Muy perdido
- ☑ Con dudas
- ⬜ Entendiendo
- ⬜ Dominando
