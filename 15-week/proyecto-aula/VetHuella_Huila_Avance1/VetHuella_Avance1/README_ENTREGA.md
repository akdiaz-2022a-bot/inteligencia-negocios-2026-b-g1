# VetHuella Huila — Entrega Avance 1 (Datos y ETL)

**Estudiantes:** Angie Karina Díaz Yela y Angie Yorlieth Villanueva Cano  
**Asignatura:** Electiva Inteligencia de Negocios, CORHUILA, 2026-B  
**Período analizado:** enero–diciembre 2025

## Contenido
- `01_PowerBI/`: proyecto `.pbix` entregado por las estudiantes.
- `02_Datos_Originales/`: CSV bruto y catálogos Excel/CSV.
- `03_Datos_Limpios_Referencia/`: CSV limpio de referencia preparado previamente, no exportado ni contrastado directamente con el PBIX.
- `04_Documentacion/`: informe ETL, diccionario de datos, validaciones e índice de evidencias.
- `05_Evidencias/`: capturas de importación, transformaciones y dimensiones.

## Resultado
Se documentan 1.512 filas originales, 12 duplicados por ID y 1.500 citas únicas. Se describen tres problemas de calidad: estados heterogéneos, fechas de formatos mixtos y duplicados.

## Cómo abrir
Abrir `01_PowerBI/VetHuella_Fase1_ETL.pbix` en Power BI Desktop. Si la ruta local de los archivos fuente ha cambiado, editar el paso `Source` de las consultas para apuntar a `02_Datos_Originales/` y pulsar **Cerrar y aplicar**. El proyecto puede conservar rutas absolutas del computador donde fue creado.

## Entrega en GitHub
Publicar README, documentación, capturas y, si el repositorio lo permite, los datos sintéticos en `15-week/proyecto-aula/` de su fork. Evitar subir el formulario institucional FO-IV-159, que contiene datos personales y se entrega solo por Moodle. Si el PBIX es demasiado grande para la interfaz web, subirlo mediante Git o al medio permitido por el docente.

## Alcance
Esta carpeta corresponde exclusivamente a la **Fase 1**. No incluye medidas DAX, relaciones definitivas ni tablero interactivo, previstos para Fase 2.
