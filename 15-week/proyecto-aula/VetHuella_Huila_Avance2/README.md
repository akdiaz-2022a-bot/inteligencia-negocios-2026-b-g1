# VetHuella Huila — Avance 2

## Modelo de datos y dashboard interactivo

**Asignatura:** Inteligencia de Negocios  
**Proyecto:** Análisis de inasistencia veterinaria — 2025

### Objetivo

Desarrollar un modelo dimensional y un tablero interactivo en Power BI para analizar la inasistencia a citas veterinarias.

### Modelo dimensional

Se implementó un esquema estrella compuesto por:

- **Fact_Citas:** tabla central de citas veterinarias.
- **Dim_Servicio:** clasificación de servicios.
- **Dim_Paciente:** información de pacientes.
- **Dim_Canal:** canales de reserva.
- **Dim_Fecha:** análisis temporal.

Las relaciones son de uno a muchos, con filtrado desde las dimensiones hacia la tabla de hechos.

### Indicadores DAX

- Total de citas.
- Citas atendidas.
- Citas no asistidas.
- Citas canceladas.
- Citas reprogramadas.
- Tasa de inasistencia.
- Validación del total de citas.

### Resultados generales

| Indicador | Resultado |
|---|---:|
| Total de citas | 1.500 |
| Citas atendidas | 1.017 |
| Citas no asistidas | 189 |
| Citas canceladas | 172 |
| Citas reprogramadas | 122 |
| Tasa de inasistencia | 15,67 % |
| Diferencia de validación | 0 |

La tasa de inasistencia se calcula como citas no asistidas divididas entre la suma de citas atendidas y no asistidas.

### Dashboard

Se desarrollaron visualizaciones para analizar:

- Inasistencias por servicio.
- Tendencia mensual de inasistencias.
- Inasistencias por canal de reserva.
- Distribución de citas por estado.

Se incorporaron filtros interactivos por servicio, canal, especie y año-mes.

### Evidencias

La carpeta `evidencias` contiene capturas del modelo dimensional, medidas DAX, tablero y validación de indicadores.
