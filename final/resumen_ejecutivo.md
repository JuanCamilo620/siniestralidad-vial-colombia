# Resumen Ejecutivo — Siniestralidad Vial Colombia

**Proyecto de Minería de Datos · CIAF Educación Superior · X semestre**

**Integrantes:** Andrés Felipe Patiño, Bryan Bedoya Calderón, Juan Camilo Arenas Villa

---

## El problema de negocio

En Colombia, la siniestralidad vial es un problema de salud pública que causa miles de muertes al año. Sin embargo, **no todos los tramos viales son igualmente peligrosos**: unos pocos corredores concentran la mayoría de las muertes. El problema es que los decisores (alcaldías, gobernaciones, ANSV, INVIAS) no tienen una herramienta clara para **priorizar qué tramos intervenir primero** y en qué horarios concentrar los operativos.

## La pregunta que guió el proyecto

> **¿Qué sectores viales, horarios y condiciones concentran la mayor siniestralidad vial en Colombia, para priorizar intervenciones de infraestructura y controles de tránsito?**

**Decisor principal:** el coordinador de seguridad vial de una alcaldía o gobernación.

**Acción concreta:** priorizar qué tramos intervenir primero y en qué horarios concentrar los operativos.

## De dónde salieron los datos

Usamos dos fuentes oficiales:

| Fuente | Archivo | Período | Registros |
|---|---|---|---|
| **F1 — Sectores Críticos** | `SECTORES_CRITICOS_DE_SINIESTRALIDAD_VIAL_20260912.csv` (ANSV / Datos Abiertos Colombia) | 2015-2019 | 316 tramos georreferenciados |
| **F2 — Víctimas fatales** | `versionfinal.csv` (ANSV) | 2021-2024 | 32.832 registros certificados |

**Decisión de cruce:** como F1 tiene una fila por tramo y F2 una fila por persona fallecida, agrupamos F2 por Departamento + Municipio para obtener el total de fallecidos por municipio. Luego cruzamos con F1 usando esa clave. Resultado: **316 filas antes del merge → 316 filas después** (sin pérdidas).

## Hallazgos principales

1. **Concentración geográfica:** los tramos de alto riesgo se concentran en pocos departamentos (Valle del Cauca, Antioquia, Cundinamarca, Santander). Intervenir estos departamentos tiene el mayor impacto.

2. **Entidades administradoras:** INVIAS-OTROS y ANI concentran la mayoría de los tramos críticos. Los operativos deben coordinarse con estas entidades.

3. **Tramos más letales:** Tumaco–Pedregal (Nariño, 52 fallecidos), Cereté–San Pelayo (Córdoba, 43) y Barranquilla–Malambo–Palmar de Varela (Atlántico, 35).

4. **Segmentación en 3 clusters:** 46 tramos de alto riesgo histórico, 170 de riesgo medio, 100 de bajo riesgo con tendencia al alza (deterioro reciente).

5. **Anomalías detectadas:** 15 tramos con combinaciones extremas de letalidad y deterioro reciente. Requieren revisión manual.

6. **Modelo predictivo:** el modelo sin fuga de datos (sin `Fallecidos`) da accuracy 0.747, que es la métrica honesta usando solo variables exógenas.

## Recomendaciones

- **Prioridad 1 (infraestructura):** intervenir los 46 tramos del Cluster 0 (alto riesgo histórico).
- **Prioridad 2 (vigilancia):** monitorear los 100 tramos del Cluster 2 (bajo riesgo histórico pero deterioro reciente).
- **Prioridad 3 (revisión manual):** revisar los 15 tramos anómalos antes de cualquier decisión.

## Limitaciones

- El 7.9% de los tramos (25) no cruzaron entre fuentes por diferencias de nomenclatura municipal.
- El desfase temporal entre F1 (2015-2019) y F2 (2021-2024) limita la comparación directa.
- El modelo predictivo tiene fuga de datos si se incluye `Fallecidos` (documentado).
