# Asignación y Cuadrantes Operativos

El motor interno de FOXcurity automatiza la planificación de cuadrantes reduciendo costes de desplazamiento y evitando conflictos en los puestos de trabajo.

## Criterios de Asignación

El sistema pondera automáticamente dos factores críticos:

- **Proximidad geográfica:** Prioriza a los agentes que residen más cerca del puesto de servicio, reduciendo retrasos.
- **Gestión de vetos y restricciones:** Si un vigilante está vetado en un cliente específico (o tiene restricciones de turno), el sistema bloquea su asignación para evitar incidencias operativas.

## Algoritmo de compatibilidad
### Ejemplo conceptual del filtro de asignación
```python

def asignar_vigilante(vigilante, servicio):
    if vigilante.id in servicio.vetados:
        return False  # Bloqueado por restricción
    return vigilante.distancia_a(servicio.ubicacion) <= radio_maximo
```