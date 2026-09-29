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

```text
siniestralidad-vial-colombia/
├── README.md
├── .gitignore
├── data/
│   ├── SECTORES_CRITICOS_DE_SINIESTRALIDAD_VIAL_20260912.csv
│   ├── versionfinal.csv
│   ├── version_parcial_a_diciembre.csv
│   └── dataset_cruzado_siniestralidad.csv
└── notebooks/
    └── 01_adquisicion_siniestralidad.ipynb
```

## Fuentes de datos

| Fuente | Descripción | Período | Registros |
| :--- | :--- | :--- | :--- |
| F1 — Sectores Críticos | Tramos viales priorizados con georreferenciación (ANSV / Datos Abiertos Colombia) | 2015-2019 | 316 tramos |
| F2 — Víctimas fatales | Microdatos de personas fallecidas en siniestros viales (ANSV) | 2021-2024 (certificado) | 32.832 registros |

## Estado del proyecto

- Sesión 1 (Ficha): Completa
- Sesión 2 (Repositorio): Estructura creada
- Sesión 3 (Fuentes): Descargadas
- Sesión 4 (Adquisición): Notebook de lectura, cruce, verificación y diccionario de datos completo

## Decisión de cruce

Las dos fuentes están a niveles distintos:
- F1 tiene una fila por tramo de carretera
- F2 tiene una fila por persona fallecida

Por eso, antes del merge, se agrupa F2 por Departamento + Municipio para obtener el total de fallecidos por municipio. Luego se cruza con F1 usando esa misma clave.

## Resultado del cruce

- 316 filas antes del merge, 316 filas después (no se duplicaron ni perdieron filas).
- 25 tramos (7.9%) quedaron sin pareja porque el nombre del municipio está escrito distinto entre fuentes (ej. CARTAGENA DE INDIAS vs CARTAGENA, PATIA (El Bordo) vs EL BORDO).
- Mitigación Unidad 3: usar el código divipola del DANE como clave de cruce y un diccionario de equivalencias municipio a divipola.

## Herramientas
- Python 3
- pandas
- Google Colab
- GitHub
