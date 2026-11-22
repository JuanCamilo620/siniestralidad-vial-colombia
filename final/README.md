# Documentación del Proceso — Proyecto Siniestralidad Vial Colombia

**Minería de Datos · CIAF Educación Superior · X semestre**

**Integrantes:** Andrés Felipe Patiño, Bryan Bedoya Calderón, Juan Camilo Arenas Villa

**Repositorio:** https://github.com/JuanCamilo620/siniestralidad-vial-colombia

---

## Índice de entregables por unidad

| Unidad | Tema | Entregable | Enlace |
|---|---|---|---|
| 1 | Ficha del proyecto | Ficha con problema, pregunta e indicadores | `/ficha/` (documento aparte) |
| 2 | Fuentes de datos | Datasets descargados | `/data/` |
| 3 | Calidad de datos y EDA | Notebook 02 — Secciones 1-8 | `notebooks/02_calidad_y_eda.ipynb` |
| 4 | Ingeniería de características | Notebook 02 — Sección 9 | `notebooks/02_calidad_y_eda.ipynb` |
| 5 | Segmentación | Notebook 02 — Sección 1 | `notebooks/02_calidad_y_eda.ipynb` |
| 6 | Modelo predictivo | Notebook 02 — Sección 2 | `notebooks/02_calidad_y_eda.ipynb` |
| 7 | Técnica complementaria (anomalías) | Notebook 02 — Sección 3 | `notebooks/02_calidad_y_eda.ipynb` |
| — | Producto analítico final | Notebook 03 | `final/03_producto_analitico.ipynb` |
| — | Resumen ejecutivo | Documento | `final/resumen_ejecutivo.md` |
| — | Ficha de ética | Documento | `final/ficha_limitaciones_etica.md` |

## Estructura del repositorio

```text
siniestralidad-vial-colombia/
├── README.md                              ← Estado general del proyecto
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
│   └── 02_calidad_y_eda.ipynb             ← Unidades 3-7
└── final/
    ├── README.md                          ← Este archivo
    ├── 03_producto_analitico.ipynb        ← 6 gráficos finales
    ├── resumen_ejecutivo.md
    └── ficha_limitaciones_etica.md
```

## Cómo reproducir el proyecto

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/JuanCamilo620/siniestralidad-vial-colombia.git
   cd siniestralidad-vial-colombia
   ```

2. **Abrir el notebook de adquisición** (`notebooks/01_adquisicion_siniestralidad.ipynb`) en Colab y ejecutar todas las celdas. Requiere subir los CSV de `/data/`.

3. **Abrir el notebook de calidad y EDA** (`notebooks/02_calidad_y_eda.ipynb`) y ejecutar todas las celdas. Genera los datasets `dataset_cruzado_limpio.csv`, `dataset_con_features.csv`, `dataset_segmentado.csv` y `dataset_con_anomalias.csv`.

4. **Abrir el notebook de producto final** (`final/03_producto_analitico.ipynb`) y ejecutar todas las celdas. Genera los 6 gráficos finales.

5. **Leer los documentos:** `resumen_ejecutivo.md` y `ficha_limitaciones_etica.md`.

## Herramientas usadas

- Python 3
- pandas, numpy
- scikit-learn (K-means, Isolation Forest, DecisionTree)
- matplotlib, seaborn
- Google Colab
- GitHub

## Contacto

- Andrés Felipe Patiño
- Bryan Bedoya Calderón
- Juan Camilo Arenas Villa
