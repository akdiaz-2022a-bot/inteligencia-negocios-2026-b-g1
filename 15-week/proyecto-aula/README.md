
# Proyecto de aula · Corte 3 · Fase 0 — Definición

**Título:** Tablero de inteligencia de negocios en Power BI para analizar la inasistencia a citas por servicio y franja horaria en la clínica veterinaria ficticia VetHuella Huila, Neiva, 2025.

**Asignatura:** Electiva Inteligencia de Negocios · 2026-B · Corte 3  
**Docente:** Jesús Ariel González Bonilla  
**Institución:** Corporación Universitaria del Huila — CORHUILA  
**Fecha de revisión:** jueves 29 de octubre de 2026  
**Integrantes:** Angie Karina Díaz Yela y Angie Yorlieth Villanueva Cano.  
**Modalidad:** En pareja; estudio de caso **100 % ficticio**, sin datos de pacientes o tutores reales.

## 4. Ficha del proyecto (Fase 0)

| Campo | Su respuesta |
|---|---|
| **Integrantes (máximo dos)** | Angie Karina Díaz Yela; Angie Yorlieth Villanueva Cano. |
| **Empresa y tipo (real o ficticia)** | **VetHuella Huila** — clínica veterinaria ficticia, ubicada de manera simulada en Neiva, Huila, Colombia. |
| **A qué se dedica** | VetHuella Huila es una clínica veterinaria ficticia que presta servicios ambulatorios para perros y gatos: consulta general, vacunación, desparasitación, controles preventivos y seguimiento clínico programado. Organiza su atención mediante citas agendadas por WhatsApp, teléfono o recepción, en jornadas de mañana y tarde. Los registros simulados de agenda y atención permitirán estudiar el cumplimiento de las citas para apoyar la planeación del personal y las estrategias de confirmación. No presta, dentro del alcance analizado, servicios de hospitalización ni urgencias sin cita. |
| **Problema** | La administración programa los turnos y asigna personal usando el número de citas agendadas, pero no tiene un análisis oportuno de cuáles citas se incumplen por tipo de servicio, mes y franja horaria. Por ello puede sobreestimar la demanda que realmente se atenderá, dejar horarios vacíos y destinar recursos a jornadas con mayor inasistencia, mientras desconoce dónde conviene reforzar los recordatorios y la confirmación previa. La información está dispersa en registros de agenda, atenciones y catálogo de servicios. La decisión que se quiere mejorar es **en qué servicios y franjas horarias priorizar las confirmaciones de cita**. |
| **Pregunta de negocio** | **¿Qué servicios veterinarios y franjas horarias presentan la mayor tasa de inasistencia a citas programadas en VetHuella Huila durante enero–diciembre de 2025, para priorizar las confirmaciones previas?** |
| **KPI principal y fórmula** | **Tasa de inasistencia a citas (%)** = (**número de citas con estado `No asistió`** / **número de citas programadas elegibles**) × 100. Son elegibles las citas con fecha prevista dentro del período y estado final `Atendida` o `No asistió`; las citas `Cancelada con anticipación` y `Reprogramada` no se incluyen hasta contar con su nueva cita registrada como evento independiente. Se calcula por servicio, franja y período. |
| **KPI de apoyo** | **1. Tasa de asistencia (%)** = (citas `Atendida` / citas programadas elegibles) × 100. **2. Horas de agenda perdidas por inasistencia (h)** = suma de la duración programada en minutos de las citas `No asistió` / 60. El segundo indicador permite dimensionar las horas reservadas que no terminaron en atención, no prueba pérdida económica directa. |
| **Fuentes de datos y formato** | **`citas_2025.csv`**: registro sintético de agenda, con código de cita, fecha y hora, códigos de servicio y paciente, canal, estado final y duración programada (CSV). **`servicios.xlsx`**: catálogo ficticio con código, servicio, categoría y duración estándar (Excel). **`pacientes.xlsx`**: catálogo anonimizado y ficticio con código de paciente y especie (Excel; sin nombres de tutores ni datos personales). **`canales.csv`**: catálogo de los canales de agendamiento (CSV). **`calendario`**: tabla de fechas calculada en Power BI (DAX), no fuente externa. |
| **Autorización (solo empresa real)** | **No aplica:** la empresa y todos los datos son ficticios; no se emplearán registros de personas o pacientes reales. |

## Alcance y criterios de datos sintéticos

- **Período de análisis:** 1 de enero al 31 de diciembre de 2025 (12 meses cerrados).
- **Volumen comprometido para la Fase 1:** al menos **1.500 citas base** (una fila por cita; al menos 1.000 citas válidas después del ETL). Se usará un único identificador por evento de cita, sin contar duplicados de calidad como citas adicionales.
- **Grano de la tabla de hechos:** cada fila representa **una cita veterinaria programada** con su estado final, duración prevista y códigos de dimensión; una reprogramación válida crea una nueva cita enlazable, no una duplicación del evento original.
- **Modelo estrella previsto:** `Fact_Citas` relacionada mediante claves con `Dim_Calendario`, `Dim_Servicio`, `Dim_Paciente` y `Dim_Canal`. La franja horaria (mañana/tarde) se derivará de la hora de la cita y se conservará para segmentación. Relaciones de 1 a varios desde dimensiones hacia hechos.
- **Variables mínimas en hechos:** `id_cita`, `fecha_cita`, `hora_cita`, `id_servicio`, `id_paciente`, `id_canal`, `estado_final`, `duracion_programada_min`, `id_cita_origen` (si reprogramación). Catálogos con códigos únicos.
- **Reglas de simulación:** consultas generales más frecuentes que controles especializados; duración según servicio; distribución de citas por día de semana y horario laboral; inasistencias sintéticas distintas por franja y servicio; variación moderada por mes; estados finales mutuamente excluyentes. Son supuestos de simulación, **no resultados ni estadísticas reales de Neiva**.
- **Problemas intencionales de calidad:** (1) filas duplicadas de una misma cita por exportación; (2) fechas mixtas `DD/MM/AAAA` y `AAAA-MM-DD`; (3) estados con diferencias de mayúsculas, espacios y tildes (`No asistió`, `NO ASISTIO`, etc.). Se corregirán y documentarán con capturas antes/después en Power Query en la Fase 1.
- **Producción de datos:** se generarán registros mediante Python (semilla reproducible) y se exportarán a CSV/Excel, documentando el método y las reglas en el repositorio.
- **Precaución de medición:** `Cancelada con anticipación` y `Reprogramada` quedarán visibles en los datos, pero fuera del denominador de tasa de inasistencia y tasa de asistencia; la suma de ambas tasas será 100 % en un filtro de citas elegibles. El KPI de horas se interpretará únicamente como tiempo reservado no utilizado.

## Objetivo general

Desarrollar un tablero de inteligencia de negocios en Power BI que permita identificar los servicios veterinarios y franjas horarias con mayor inasistencia a citas programadas en la clínica ficticia VetHuella Huila durante 2025, para fundamentar acciones de confirmación y una programación más eficiente de la agenda.

## Objetivos específicos

1. Generar y depurar un conjunto de citas sintéticas de doce meses mediante Power Query, documentando problemas de calidad y sus correcciones.
2. Construir un modelo estrella con una tabla de hechos y dimensiones de calendario, servicio, paciente y canal, junto con medidas DAX verificables.
3. Diseñar un tablero interactivo con filtros por mes, servicio y franja, visualizaciones comparativas y pruebas de interacción.
4. Formular una recomendación accionable para la clínica a partir de los indicadores calculados y sustentados.

## Fases y entregables

| Fecha | Fase | Evidencia |
|---|---|---|
| **29/10/2026** | 0 · Definición | Esta ficha en `15-week/proyecto-aula/README.md`. |
| **05/11/2026** | 1 · Datos y ETL | CSV/Excel sintéticos; Power Query antes/después; diccionario de datos. |
| **12/11/2026** | 2 · Modelo, DAX y tablero | Modelo estrella; 5 o más medidas DAX; `.pbix`, capturas y prueba del clic. |
| **19/11/2026** | 3 · Historia, informe y sustentación | Hallazgos calculados, recomendaciones, FO-IV-159, ZIP de anexos y exposición de 7 minutos. |

> **Estado:** Fase 0 preparada para revisión; todavía no se han generado resultados del tablero. No se presentan cifras inventadas como hallazgos.
>
> **Entrega final:** el archivo FO-IV-159 con datos identificatorios se entrega **únicamente en Moodle**, nunca en GitHub. El repositorio aloja la ficha y avances sin identificaciones personales.
