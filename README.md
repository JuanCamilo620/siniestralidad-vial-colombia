# Proyecto de Minería de Datos — Siniestralidad Vial Colombia

## Problema
¿Qué sectores viales, horarios y condiciones concentran la mayor siniestralidad vial en Colombia, para priorizar intervenciones de infraestructura y controles de tránsito?

## Integrantes
- Andrés Felipe Patiño
- Bryan Bedoya Calderón
- Juan Camilo Arenas Villa

## Repositorio
https://github.com/JuanCamilo620/siniestralidad-vial-colombia

## Estructura del repositorio

siniestralidad-vial-colombia/
├── README.md
├── data/
│   ├── SECTORES_CRITICOS_DE_SINIESTRALIDAD_VIAL_20260912.csv
│   ├── versionfinal.csv
│   └── version_parcial_a_diciembre.csv
└── notebooks/
    └── 01_adquisicion_siniestralidad.ipynb

## Fuentes de datos

| Fuente | Descripción | Período | Registros |
| :--- | :--- | :--- | :--- |
| **F1** — Sectores Críticos | Tramos viales priorizados con georreferenciación (ANSV / Datos Abiertos Colombia) | 2015–2019 | 316 tramos |
| **F2** — Víctimas fatales | Microdatos de personas fallecidas en siniestros viales (ANSV) | 2021–2024 (certificado) | 32.832 registros |

## Estado del proyecto

- ✅ **Sesión 1 (Ficha):** Completa
- ✅ **Sesión 2 (Repositorio):** Estructura creada
- ✅ **Sesión 3 (Fuentes):** Descargadas
- ✅ **Sesión 4 (Adquisición):** Notebook de lectura, cruce y diccionario de datos completo

## Decisión de cruce

Las dos fuentes están a niveles distintos:
- F1 tiene **una fila por tramo de carretera**
- F2 tiene **una fila por persona fallecida**

Por eso, antes del merge, se agrupa F2 por **Departamento + Municipio** para obtener el total de fallecidos por municipio. Luego se cruza con F1 usando esa misma clave.

## Herramientas
- Python 3
- pandas
- Google Colab
- GitHub
