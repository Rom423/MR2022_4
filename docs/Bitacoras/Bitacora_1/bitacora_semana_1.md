# Bitácora – Semana 1

## Tema de la semana
Descripción de un sistema mecatrónico
Durante esta semana se trabajó con el relé programable Siemens LOGO! 12/24RCE, identificando los componentes que conforman un sistema mecatrónico y las funciones del sistema de control mediante la práctica del secuenciador de luces.
## Actividades realizadas
**Análisis del problema:** diseñar un secuenciador de luces con tiempos específicos (5 s, 1 s, 4 s) en ciclo infinito.

**Identificación de los elementos del sistema mecatrónico:**
**Sistema de salidas:** LEDs o luces  que conectan a las salidas del sistema.
**Actuadores:** Para este caso se utilzaron los LED's como actuadores.
**Controlador** módulo lógico Siemens LOGO.
**Fuente de energía:** alimentación 12/24V DC.
**Software:** programación en LOGO Soft Comfort.
Diseño del diagrama en el software.
Simulación del funcionamiento.  
## Decisiones de ingeniería
| Decisión                              | Alternativas                                  | Justificación                                                                                                            |
| ------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Usar temporizadores en cascada    | Uso de contadores internos            | Los temporizadores permiten controlar directamente los tiempos de activación de cada salida de forma secuencial. |
| Implementar ciclo automático continuo | Activación manual con botón de inicio         | Se requería que el sistema funcionara indefinidamente sin intervención del usuario.                                      |
| Asignar una salida por etapa          | Usar una sola salida con lógica combinacional | Facilita la visualización del comportamiento secuencial y simplifica la depuración.                                      |

## Problema técnico encontrado
Durante la simulación inicial, las salidas se activaban simultáneamente o no se realizaba de manera correcta el parpadeo de los LED's. Esto ocurrió porque los temporizadores no estaban correctamente encadenados y, ya que se tenian conexiones erroneas, no se había condicionado el arranque del siguiente temporizador al término del anterior.

## Solución aplicada
Se reorganizó la lógica conectando cada temporizador al estado de salida del anterior (encadenamiento lógico).
Además, se verificó que cada bloque tuviera correctamente configurado el tiempo y que la señal de reinicio permitiera repetir el ciclo indefinidamente.

Después del ajuste, el sistema cumplió con la secuencia 5s - 1s - 4s - reinicio automático

## Conexión con el curso
**¿Qué concepto de MR2022 aplicaste esta semana?**

Esta semana se aplicaron conceptos fundamentales de MR2022 – Mecatrónica relacionados con la integración de los elementos que conforman un sistema mecatrónico. En particular, se trabajó en la integración de actuadores (salidas digitales y LEDs), el controlador lógico programable Siemens LOGO! 12/24RCE, y el entorno de programación como herramienta de  control.
Se puso en práctica el uso de lógica de control programable (PLC), diseñando un sistema de control secuencial basado en temporizadores para cumplir con tiempos específicos de activación. 

## Autoevaluación
- ⬜ Muy perdido
- ⬜ Con dudas
- ☑ Entendiendo
- ⬜ Dominando
