# Hito 1 – Análisis y Requerimientos

## Descripción del problema
Descripción del problema
Aquí tienes una redacción cohesiva y técnica, presentada en un solo bloque de texto para facilitar su lectura continua:

La empresa busca automatizar un sistema de cortinas industriales de grandes dimensiones (hasta 4x7 metros) diseñadas para la segregación de espacios y el control de accesos en entornos de alto tránsito peatonal y vehicular. El principal desafío técnico radica en la gestión de la carga, ya que el diseño incorpora barras metálicas de 35 kg por cada dos metros de ancho para asegurar la tensión de la estructura. Debido a su ubicación en zonas críticas de logística, es imperativo que el sistema integre requerimientos técnicos robustos, como motores de alto torque y estructuras reforzadas, junto con dispositivos de seguridad funcionales que incluyan sensores de presencia y paros de emergencia para mitigar riesgos de accidentes. Como fase inicial del proyecto, se desarrollará un prototipo a escala de 1 metro de ancho por 2 metros de largo, utilizando una malla sintética tensada por una barra metálica de entre 1 y 2 kilogramos, con el fin de validar la lógica de automatización y la eficiencia del mecanismo de elevación antes de su implementación a escala real.

Realizaremos un prototipo de dimensiones de 1 metro de ancho con 2 de largo implementando 1 barra metálica de 1 a 2 kilogramos para tensar un trozo de malla sintética.

## Requerimientos del sistema
# Especificaciones del Proyecto: Cortina Industrial Automatizada

## 🛠️ Requerimientos Funcionales
El sistema está diseñado para la automatización inteligente de una cortina industrial, priorizando la seguridad y la eficiencia operativa.

* **Mecanismo de Tracción:**
    * Uso de un **actuador rotatorio** superior para el enrollado/desenrollado.
    * Barra de soporte integrada para guiar la rotación de la cortina.
* **Detección de Obstáculos:**
    * Implementación de un **sensor de proximidad infrarrojo** (elegido por su capacidad de detectar diversos materiales a diferencia de los inductivos/capacitivos).
    * **Lógica de seguridad:** Si se detecta un objeto durante el descenso, la cortina invierte el sentido hacia el ascenso total; si está estática, permanece arriba.
* **Control de Posición:**
    * Identificación de límites mediante sensores de altura (mínimo y máximo).
    * Automatización centralizada mediante **LOGO! Siemens V8**.

---

## ⚙️ Requerimientos Técnicos

### Control y Potencia
* **Software:** Programación en *LOGO!Soft Comfort V8.0* para la gestión de señales de entrada (sensores) y salidas (actuadores).
* **Alimentación:** Fuente de corriente continua de **24V / 14.5A (350W)**, dimensionada para cubrir el consumo de todos los componentes.

### Interfaz de Operación (HMI)
El sistema incluye una interfaz digital con las siguientes capacidades:
* **Control Total:** Botones de ascenso, descenso, paro y configuración de tiempos/velocidades.
* **Modos de Operación:**
    1.  **Manual:** Movimiento activo mientras se presione el botón (detección de límites habilitada).
    2.  **Automático:** Ciclo de arranque con ascenso a alta velocidad, tiempo de espera configurable y descenso a velocidad lenta.
* **Gestión de Usuarios:**
    * **Operador:** Control manual/automático y visualización de alarmas.
    * **Supervisor:** Acceso protegido por contraseña para modificar parámetros críticos (límites, velocidades y tiempos).

### Control Físico
* Botones físicos inalámbricos para control local (Subir / Bajar / Parar) para mayor ergonomía del usuario.

---

## 🛡️ Seguridad del Sistema

> [!IMPORTANT]
> La seguridad es el pilar del diseño para evitar daños a personal, vehículos u objetos.

1.  **Protección Anti-Colisión:** El sensor infrarrojo monitorea constantemente el rango de movimiento. Cualquier intrusión durante el cierre activa un protocolo de retroceso automático.
2.  **Paro de Emergencia:** Botón de emergencia virtual de alta visibilidad en la interfaz para detención inmediata ante situaciones extraordinarias.
3.  **Validación de Respuesta:** Pruebas de latencia y tiempo de reacción entre la interfaz y el modelo físico para asegurar que la detección de obstáculos sea efectiva y en tiempo real.



## Diagrama de bloques




## Conclusiones del análisis
Breve reflexión técnica.
