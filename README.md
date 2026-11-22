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
│   ├── dataset_cruzado_siniestralidad.csv
│   ├── dataset_cruzado_limpio.csv
│   ├── dataset_con_features.csv
│   ├── dataset_segmentado.csv
│   └── dataset_con_anomalias.csv
├── notebooks/
│   ├── 01_adquisicion_siniestralidad.ipynb
│   └── 02_calidad_y_eda.ipynb
└── final/
    ├── README.md
    ├── 03_producto_analitico.ipynb
    ├── resumen_ejecutivo.md
    └── ficha_limitaciones_etica.md
```

## Fuentes de datos

| Fuente | Descripción | Período | Registros |
|---|---|---|---|
| F1 — Sectores Críticos | Tramos viales priorizados con georreferenciación (ANSV / Datos Abiertos Colombia) | 2015-2019 | 316 tramos |
| F2 — Víctimas fatales | Microdatos de personas fallecidas en siniestros viales (ANSV) | 2021-2024 (certificado) | 32.832 registros |

## Estado del proyecto

- ✅ Sesión 1 (Ficha): Completa
- ✅ Sesión 2 (Repositorio): Estructura creada
- ✅ Sesión 3 (Fuentes): Descargadas
- ✅ Sesión 4 (Adquisición): Notebook de lectura, cruce, verificación y diccionario de datos
- ✅ Checkpoint 1 (Calidad de datos): Tipos corregidos y valores atípicos tratados
- ✅ Checkpoint 1 (Análisis exploratorio): Estadísticas, 4 gráficos e historia de hallazgos
- ✅ Checkpoint 1 (Ingeniería de características): Variables derivadas, codificación one-hot, escalado y pipeline reproducible
- ✅ Checkpoint 2 (Segmentación): K-means, jerárquico y DBSCAN con interpretación de clusters
- ✅ Checkpoint 2 (Modelo predictivo): Árbol de decisión con y sin fuga de datos, métricas completas
- ✅ Checkpoint 2 (Técnica complementaria): Detección de anomalías con Isolation Forest
- ⏳ Checkpoint 3 (Producto final): En construcción

## Decisión de cruce

Las dos fuentes están a niveles distintos:

- F1 tiene una fila por tramo de carretera.
- F2 tiene una fila por persona fallecida.

Por eso, antes del merge, se agrupa F2 por Departamento + Municipio para obtener el total de fallecidos por municipio. Luego se cruza con F1 usando esa misma clave.

## Resultado del cruce

- 316 filas antes del merge → 316 filas después (no se duplicaron ni perdieron filas).
- 25 tramos (7.9%) quedaron sin pareja porque el nombre del municipio está escrito distinto entre fuentes.
- Mitigación: usar el código `divipola` del DANE como clave de cruce.

## Calidad de datos — Checkpoint 1

- Tipos corregidos: `divipola` pasó de `int64` a `object` (es un código, no una cantidad).
- 4 valores atípicos tratados con justificación:
  - `PR` con "sd" → marcado con `PR_valido`
  - `Tramo` con NaN → marcado con `Tramo_valido`
  - `total_fallecidos` con 25 NaN → se dejan como NaN (no se imputan con 0)
  - `Fallecidos` con outliers reales → se conservan (son los tramos más críticos del país)

## Análisis exploratorio — Checkpoint 1

- Variable numérica clave: `Fallecidos` — media 11.33, mediana 10.00 (distribución sesgada a la derecha).
- Variable categórica clave: `ENTIDAD` — ANI 141 tramos, INVIAS 126, DEPTO 48, CIUDAD 1.
- 4 gráficos construidos: histograma, boxplot, barras por entidad y scatter `Fallecidos` vs `total_fallecidos`.
- 5 hallazgos conectados con la pregunta de negocio: concentración en pocos corredores, entidades administradoras, top tramos letales, impacto del 7.9%, correlación histórica-reciente.

## Ingeniería de características — Checkpoint 1

Se crearon 4 variables derivadas con sentido para el proyecto:

| Variable | Fórmula / Criterio | Propósito |
|---|---|---|
| `ratio_reciente_historico` | `total_fallecidos / Fallecidos` | Comparar evolución reciente vs histórica por municipio |
| `es_corredor_nacional` | `ENTIDAD in ['ANI', 'INVIAS-OTROS']` | Distinguir red nacional de vías locales |
| `nivel_riesgo` | Bajo (≤7), Medio (8-14), Alto (>14) | Clasificar tramos por nivel de riesgo |
| `region` | Departamento → Región natural | Análisis comparativo por región |

- Codificación: one-hot encoding de `ENTIDAD`, `nivel_riesgo` y `region` (13 columnas nuevas).
- Escalado: `Fallecidos_escalado` con StandardScaler (media 0, std 1).
- Pipeline reproducible: función `pipeline_ingenieria()` — verificada ejecutando dos veces con resultado idéntico.

### Resultado de la ingeniería de características

Dataset con features exportado: `data/dataset_con_features.csv` con 316 filas × 33 columnas.

## Sección 1 — Segmentación (Checkpoint 2)

- Variables usadas: `Fallecidos_escalado` + `log_ratio_reciente_escalado`.
- K-means: k=3 elegido por método del codo.
  - Cluster 0 — Tramos de alto riesgo histórico (24.02 fallecidos promedio, 46 tramos).
  - Cluster 1 — Tramos de riesgo medio y estable (11.06 fallecidos, 170 tramos).
  - Cluster 2 — Tramos de bajo riesgo histórico con tendencia al alza (5.95 fallecidos, ratio 72.55, 100 tramos).
- Jerárquico: coincide 100% con K-means en los dos clusters extremos.
- DBSCAN: eps=0.3 da 3 clusters + 26 outliers (8.2%). Los otros eps colapsan a 1-2 clusters.
- Apriori: NO aplica — el dataset tiene una fila por tramo, sin estructura de canasta.
- Dataset exportado: `data/dataset_segmentado.csv` (316 × 38).

## Sección 2 — Modelo predictivo y evaluación (Checkpoint 2)

- Objetivo: `nivel_riesgo_Alto` (binario: 1 = alto riesgo, 0 = bajo/medio).
- Modelo 1 (con fuga): incluye `Fallecidos` → accuracy 1.000 en train y test (fuga de datos documentada).
- Modelo 2 (sin fuga): excluye `Fallecidos` → accuracy 0.747 en test usando solo variables exógenas.
- Métricas: matriz de confusión, accuracy, precisión, recall, F1.
- Sobreajuste: controlado con `max_depth=4` (modelo 1) y `max_depth=3` (modelo 2).
- Conclusión: las variables exógenas predicen parcialmente el riesgo; en Checkpoint 3 se probará regresión logística.

## Sección 3 — Técnica complementaria elegida: Detección de anomalías (Checkpoint 2)

- Técnica: Isolation Forest (Unidad 7, Ruta 1 — Anomalías).
- Variables usadas: `Fallecidos_escalado` + `log_ratio_reciente_escalado`.
- Configuraciones comparadas:
  - `contamination=0.05` (5%): 15 anomalías detectadas (4.7%).
  - `contamination=0.10` (10%): 32 anomalías detectadas (10.1%).
  - `contamination=0.15` (15%): 48 anomalías detectadas (15.2%).
- Perfil de las anomalías (contamination=5%): los tramos anómalos tienen 20.20 fallecidos promedio (vs 10.89 en los normales) y un ratio reciente/histórico de 208.79 (vs 18.97). El algoritmo detecta tramos con combinaciones extremas de letalidad y deterioro reciente.
- Ficha de limitaciones: documentada en el notebook (Sección 3.5). El parámetro `contamination` es una decisión de diseño, la técnica detecta anomalías estadísticas (no necesariamente errores), y los falsos positivos podrían llevar a descartar tramos que sí requieren intervención.
- Dataset exportado: `data/dataset_con_anomalias.csv` (316 × 42).

## Producto analítico final — Checkpoint 3

El producto analítico final se encuentra en `final/03_producto_analitico.ipynb` e incluye 6 visualizaciones que combinan los hallazgos de todas las unidades:

1. Top 10 departamentos con más tramos de alto riesgo (Cluster 0).
2. Fallecidos promedio por entidad y cluster de riesgo.
3. Top 10 tramos con más fallecidos históricos.
4. Scatter de segmentación: letalidad histórica vs deterioro reciente.
5. Detección de anomalías con Isolation Forest.
6. Comparación de modelos predictivos: con vs sin fuga de datos.

## Documentos del Checkpoint 3

- Resumen ejecutivo: `final/resumen_ejecutivo.md` (1 página con problema, pregunta, fuentes y hallazgos).
- Ficha de limitaciones, riesgos y ética: `final/ficha_limitaciones_etica.md`.
- Documentación del proceso: `final/README.md` (índice de las 8 unidades y cómo reproducir).

## Herramientas

- Python 3
- pandas
- numpy
- scikit-learn (K-means, AgglomerativeClustering, DBSCAN, IsolationForest, DecisionTree)
- matplotlib
- seaborn
- Google Colab
- GitHub
