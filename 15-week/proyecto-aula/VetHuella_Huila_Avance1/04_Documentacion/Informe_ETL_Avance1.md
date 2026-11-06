# Informe técnico — Avance 1: Datos y ETL

**Proyecto:** VetHuella Huila — Análisis de inasistencia a citas veterinarias  
**Institución / asignatura:** CORHUILA — Electiva Inteligencia de Negocios, 2026-B  
**Integrantes:** Angie Karina Díaz Yela y Angie Yorlieth Villanueva Cano  
**Empresa:** ficticia, ubicada en Neiva, Huila  
**Período:** 1 de enero al 31 de diciembre de 2025  
**Herramienta:** Power BI Desktop / Power Query

## 1. Problema y objetivo
Identificar servicios veterinarios y franjas horarias con mayor tasa de inasistencia para orientar la confirmación previa de citas. Pregunta: ¿Qué servicios y franjas horarias presentan la mayor tasa de inasistencia en 2025?

## 2. Fuentes
- `citas_2025_bruto.csv`: citas sintéticas, nueve columnas, 1512 filas originales.
- `catalogos_vethuella.xlsx`, hoja `Servicios`: 5 servicios, cuatro columnas.
- `catalogos_vethuella.xlsx`, hoja `Pacientes`: 400 pacientes ficticios, dos columnas.
- `canales.csv`: 3 canales, dos columnas.
- `citas_2025_limpio.csv`: archivo de referencia limpio suministrado anteriormente, con 1500 filas y diez columnas. **No se presume que fue exportado del PBIX entregado.**

## 3. Grano y claves
**Grano de `Fact_Citas`: una fila representa una cita veterinaria programada, identificada por `id_cita`.** `id_servicio`, `id_paciente` e `id_canal` vinculan las dimensiones. Los estados `Cancelada con anticipación` y `Reprogramada` no se incluyen en el denominador de los KPI de asistencia e inasistencia. `id_cita_origen` está vacío en los archivos actuales y no debe usarse para deducir relaciones de reprogramación.

## 4. Flujo ETL observado
1. **Extraer:** importar CSV de citas, Excel de catálogos y CSV de canales en Power Query.
2. **Encabezados y tipos:** promover encabezados y convertir `fecha_cita` a fecha, `hora_cita` a hora, claves a texto y duración a entero. En capturas posteriores se observan `Dim_Paciente` y `Dim_Canal` con encabezados correctos.
3. **Limpiar texto:** paso `Cleaned Text` en `Fact_Citas` para estandarizar valores textuales.
4. **Normalizar estados:** paso `Estados normalizados` para unificar diferencias de mayúsculas, espacios y tildes.
5. **Normalizar fechas:** paso `Fechas normalizadas`, necesario por coexistir fechas `dd/mm/aaaa` e ISO `aaaa-mm-dd` en la fuente.
6. **Eliminar duplicados:** paso `Sin duplicados`, con criterio de identificador de cita; las capturas muestran 1.500 identificadores únicos al perfilar el conjunto completo.
7. **Cargar:** se recibió `VetHuella_Fase1_ETL.pbix`, un archivo Power BI válido como contenedor. No se ha ejecutado Power BI Desktop en este entorno ni validado visualmente el modelo cargado desde ese archivo.

## 5. Auditoría reproducible de los CSV
| Comprobación | Resultado |
|---|---:|
| Registros en archivo bruto | 1512 |
| IDs de cita distintos en bruto | 1500 |
| Filas adicionales por ID duplicado | 12 |
| Registros en CSV limpio de referencia | 1500 |
| IDs distintos en CSV limpio | 1500 |
| IDs duplicados en CSV limpio | 0 |
| Servicios | 5 |
| Pacientes ficticios | 400 |
| Canales | 3 |
| Estados distintos antes | 6 |
| Estados distintos después | 4 |

**Estados de origen:** `Atendida` (1007), `No asistió` (189), `Cancelada con anticipación` (172), `Reprogramada` (124), ` atendida ` (18), ` NO ASISTIO ` (2).  
**Estados limpios de referencia:** `Atendida` (1017), `No asistió` (189), `Cancelada con anticipación` (172), `Reprogramada` (122).

Las 12 filas duplicadas del archivo bruto corresponden a IDs `C00050` a `C00061`. El archivo limpio de referencia contiene 1.500 IDs únicos. La diferencia en recuentos por estado refleja también la eliminación de duplicados, no únicamente la normalización de texto.

## 6. Tres problemas de calidad documentados

**Problema A — Estados inconsistentes.** En origen existen `Atendida` y ` atendida `, además de `No asistió` y ` NO ASISTIO `. Tratamiento: recortar espacios, normalizar mayúsculas/minúsculas y tildes y unificar categorías. Evidencias: 01, 03 y 04.

**Problema B — Fechas heterogéneas.** El CSV original combina fechas en `dd/mm/aaaa` y en formato ISO. Tratamiento: interpretación explícita de ambos formatos y conversión a fecha. Evidencias: 01, 02 y 05. No interpretar automáticamente las fechas ambiguas como mes/día.

**Problema C — Identificadores duplicados.** Hay 1.512 filas brutas para 1.500 IDs; 12 filas son adicionales. Tratamiento: deduplicación por `id_cita`, conservando una fila por cita. Evidencias: 01, 06 y 07.

## 7. Validaciones y limitaciones
- Los recuentos anteriores se verificaron directamente sobre los archivos CSV incluidos en el paquete.
- Las capturas evidencian pasos de Power Query y el perfilado del conjunto completo (1.500 IDs únicos, sin errores visibles en las columnas mostradas).
- La validación de fechas, claves y campos se describe en `Validacion_de_Datos.md`.
- El CSV limpio de referencia incorpora `franja_horaria`; en las capturas de Power Query se observan nueve columnas de `Fact_Citas`. **No se afirma que la columna `franja_horaria` ya exista dentro del PBIX.** Puede agregarse en la siguiente fase de modelado si no está creada.
- No se afirma haber probado relaciones de Power BI ni medidas DAX; esas actividades corresponden al Avance 2.

## 8. KPI planificados (no calculados todavía en Power BI)
- **Citas elegibles:** citas con estado `Atendida` o `No asistió`.
- **Tasa de inasistencia:** citas `No asistió` / citas elegibles × 100.
- **Tasa de asistencia:** citas `Atendida` / citas elegibles × 100.
- **Horas de agenda perdidas:** suma de `duracion_programada_min` para citas `No asistió` / 60.

## 9. Datos sintéticos y privacidad
Se emplean datos ficticios. Los IDs de pacientes son sintéticos y no incluyen información identificable de personas reales. La generación se basa en citas fechadas en 2025, cinco servicios, 400 pacientes y tres canales; el paquete no contiene el script original de generación, por lo que no se atribuye una semilla ni una distribución estadística no documentada.

## 10. Evidencias
Ver `05_Evidencias/` y `Indice_de_Evidencias.md`. Las imágenes muestran el proceso en Power Query; la auditoría cuantitativa proviene de los CSV, que se incluyen para permitir reproducirla.
