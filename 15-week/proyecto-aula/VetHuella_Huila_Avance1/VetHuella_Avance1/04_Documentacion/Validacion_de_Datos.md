# Validación de datos — comprobaciones independientes

Se inspeccionaron los archivos fuente y el CSV limpio de referencia incluidos en este ZIP, **no el motor de consultas interno del PBIX**.

| Prueba | Resultado |
|---|---|
| Cantidad de filas brutas | 1512 |
| Filas duplicadas por ID en origen | 12 |
| Filas limpias de referencia | 1500 |
| IDs únicos en limpio | 1500 |
| Errores de coincidencia de fecha al comparar IDs y parsear ISO/dd-mm | 0 |
| IDs de servicio sin correspondencia en catálogo | 0 |
| IDs de paciente sin correspondencia en catálogo | 0 |
| IDs de canal sin correspondencia en catálogo | 0 |
| Valores nulos en campos obligatorios del CSV limpio | 0 |

**Advertencia:** `id_cita_origen` es nulo en todas las filas; es opcional. Las pruebas no sustituyen la comprobación del PBIX en Power BI Desktop.
