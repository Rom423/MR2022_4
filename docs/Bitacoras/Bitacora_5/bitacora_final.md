# Bitácora Final – Cierre del Proyecto

## Evolución del aprendizaje
Al inicio del curso el conocimiento sobre la integración de sistemas mecatrónicos era principalmente teórico y se enfocaba en comprender los componentes de forma individual. Con el desarrollo de las prácticas semanales, el aprendizaje evolucionó hacia la aplicación práctica de estos conceptos, especialmente en la programación del PLC Siemens LOGO!, el uso de sensores industriales y la implementación de actuadores.

A lo largo del proyecto se fortalecieron habilidades como el análisis de problemas, la programación de lógica de control, la interpretación de diagramas eléctricos y la detección de fallas en sistemas reales. En comparación con el inicio del curso, ahora se tiene una mejor comprensión de cómo integrar sensores, actuadores y controladores para construir un sistema automatizado funcional.

## Decisiones clave
La decisión de ingeniería más importante fue implementar interlocks de seguridad dentro del PLC para evitar la activación simultánea de las direcciones del motor.

Esta decisión fue fundamental porque permitió proteger el sistema y evitar daños en el motorreductor. Además, al implementarlo mediante lógica programable dentro del PLC, se logró un sistema más flexible y fácil de modificar sin necesidad de cambiar el cableado físico del circuito.

## Integración del sistema
El sistema mecatrónico desarrollado integra sensores, actuadores, controlador y señalización de la siguiente manera:

Los sensores (sensor de presencia y sensores de posición de la cortina) detectan el estado del entorno y envían señales de entrada al PLC Siemens LOGO!, que funciona como el controlador del sistema.

El PLC procesa estas señales mediante la lógica programada (máquina de estados, temporizadores e interlocks) y decide qué acción ejecutar. Con base en esta lógica, activa las salidas del sistema, que controlan los actuadores, principalmente el motorreductor DC que mueve la cortina a través de relevadores electromecánicos.

Además, el PLC controla una torre de señalización, que funciona como interfaz visual para el usuario, indicando el estado del sistema mediante luces: verde para sistema listo y rojo para sistema en movimiento.

De esta manera, el sistema logra una interacción coordinada entre detección, procesamiento y acción, característica fundamental de los sistemas mecatrónicos.

## Mejoras futuras
Si se tuviera una semana adicional para continuar con el proyecto, se podrían implementar varias mejoras:

- agregar una interfaz HMI más avanzada, como una pantalla para visualizar el estado del sistema  
- mejorar el control de velocidad del motor utilizando un controlador PWM  
- integrar más sensores de seguridad, por ejemplo sensores anti-atrapamiento  
- optimizar la estructura de la programación del PLC para hacerla más modular  
- mejorar la organización del cableado y la protección eléctrica del sistema  

Estas mejoras permitirían aumentar la seguridad, funcionalidad y facilidad de operación del sistema.

## Valoración del proyecto
- ⬜ Poco útil
- ⬜ Aceptable
- ⬜ Útil
- ☑ Muy valioso
