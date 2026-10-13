# Módulos de Gestión

FOXcurity modulariza cada área administrativa y operativa de tu empresa.

## 1. Gestión de Personal y Contratos

- **Ficha del empleado:** Historial, certificaciones, TIP (*Tarjeta de Identidad Profesional*) y datos de contacto.
- **Alertas automáticas:** Notificaciones previas a la expiración de contratos temporales o renovaciones médicas.
- **Cálculo de vacaciones:** Balance automático de días acumulados vs. solicitados según convenio.

## 2. Inventario y Control de Material

Seguimiento punto a punto de equipo asignado:

1. Armamento reglamentario y munición.
2. Uniformidad y equipos de protección individual (EPIs).
3. Dispositivos de comunicación (radios, teléfonos de guardia).

## 3. Incidencias y Asistencias

Registro inmediato de faltas de puntualidad, ausencias justificadas o partes de servicio durante rondas.

!!! warning "Trazabilidad Inmutable"
    Todos los registros de incidencias y entregas de armamento quedan cifrados con marca de tiempo, cumpliendo con la normativa vigente de seguridad privada.

## Validación de Fichajes en Tiempo Real

El sistema valida automáticamente que el vigilante se encuentre dentro del perímetro del cliente antes de permitir el inicio del turno. 

A continuación, un extracto del motor de validación en Python:

```python hl_lines="12 13 14 15"
import datetime

def registrar_entrada(id_vigilante, latitud_actual, longitud_actual):
    """
    Registra el inicio de turno verificando el perímetro GPS.
    """
    servicio = db.obtener_servicio_activo(id_vigilante)
    
    # Comprueba la distancia en metros entre el vigilante y el objetivo
    distancia = calcular_distancia_gps(latitud_actual, longitud_actual, servicio.lat, servicio.lon)
    
    # Rango de tolerancia de 50 metros
    if distancia <= 50:
        hora_registro = datetime.datetime.now()
        db.guardar_fichaje(id_vigilante, servicio.id, hora_registro, "CORRECTO")
        return "Fichaje aceptado: En posición."
    else:
        db.crear_alerta(id_vigilante, "Fichaje fuera de rango")
        return "Error: Ubicación no válida para este servicio."