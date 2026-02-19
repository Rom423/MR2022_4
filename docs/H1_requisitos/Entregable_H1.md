# Hito 1 – Análisis y Requerimientos

## Descripción del problema
Describe el problema de ingeniería que se resolverá.

## Requerimientos del sistema
### Funcionales
- 

### Técnicos
- 

### Seguridad
- 

## Diagrama de bloques
## Diagrama de Arquitectura del Sistema

```mermaid
graph TD
    %% Definición de Nodos
    S[<b>Sensores</b><br/>S1-S6]
    C[<b>Controlador LOGO</b>]
    P[<b>Etapa de Potencia</b><br/>Relevador HH54P]
    A[<b>Actuadores</b><br/>- Motor 2VDC<br/>- Lámpara ANDON]
    F[<b>Fuente 110 -> 24VDC</b>]

    %% Conexiones
    S -->|Señales| C
    C -->|Señales de control| P
    P -->|Energía| A

    %% Estilo para la fuente (que está aislada en tu imagen)
    style F stroke-dasharray: 5 5

## Conclusiones del análisis
Breve reflexión técnica.
