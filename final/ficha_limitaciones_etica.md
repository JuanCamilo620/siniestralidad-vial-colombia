# Ficha de Limitaciones, Riesgos y Ética

**Proyecto:** Siniestralidad Vial Colombia
**Integrantes:** Andrés Felipe Patiño, Bryan Bedoya Calderón, Juan Camilo Arenas Villa

---

## Limitaciones del dataset

### 1. Desfase temporal entre fuentes
- **F1 (Sectores Críticos)** cubre 2015-2019.
- **F2 (Víctimas fatales)** cubre 2021-2024.
- **Implicación:** no podemos comparar directamente los fallecidos históricos de un tramo con los recientes del mismo tramo. Usamos F1 como capa geográfica y F2 como base temporal principal.

### 2. Cobertura incompleta del cruce
- **25 de 316 tramos (7.9%)** no cruzaron entre fuentes porque el nombre del municipio está escrito distinto (ej. `CARTAGENA DE INDIAS` vs `CARTAGENA`).
- **Implicación:** el análisis de fallecidos recientes (2021-2024) no cubre el 100% de los tramos.
- **Mitigación futura:** usar el código `divipola` del DANE como clave de cruce.

### 3. Datos agregados por municipio
- F2 se agregó por Departamento + Municipio, no por tramo. Eso significa que el `total_fallecidos` de un tramo es el total del municipio, no del tramo específico.
- **Implicación:** un tramo con pocos fallecidos históricos puede estar en un municipio con muchos fallecidos recientes, y eso no significa que ese tramo específico sea el peligroso.

### 4. Outliers reales vs errores
- Los tramos con más fallecidos (Tumaco–Pedregal, Cereté–San Pelayo) son **casos reales**, no errores de digitación.
- **Implicación:** no se deben eliminar automáticamente. Son precisamente los tramos que más atención requieren.

## Riesgos del análisis

### 1. Fuga de datos en el modelo predictivo
- El modelo con `Fallecidos` da accuracy perfecto (1.000) porque `nivel_riesgo` se construyó a partir de `Fallecidos`.
- **Riesgo:** usar ese modelo para predecir tramos nuevos daría resultados engañosos.
- **Mitigación:** usar el modelo sin `Fallecidos` (accuracy 0.747) como métrica honesta.

### 2. Anomalías estadísticas != errores
- Isolation Forest detecta tramos que se salen del patrón, pero **no necesariamente son errores**.
- **Riesgo:** descartar un tramo anómalo que en realidad es un caso crítico real.
- **Mitigación:** revisar manualmente los 15 tramos anómalos antes de cualquier decisión.

### 3. Sesgo hacia los valores extremos
- El algoritmo tiende a marcar como anómalos los valores extremos, que son los tramos más letales.
- **Riesgo:** priorizar los tramos extremos y dejar de lado los de riesgo medio que también requieren atención.

## Consideraciones éticas

### 1. ¿Qué decisiones NO deberían tomarse solo con este análisis?
- **NO** cerrar tramos viales basándose solo en el cluster de riesgo.
- **NO** descartar tramos anómalos sin revisión humana.
- **NO** usar el modelo con fuga de datos para predecir tramos nuevos.
- **NO** asumir que los 25 tramos sin pareja no tienen siniestralidad (solo no cruzaron).

### 2. ¿Qué sesgos tiene el dataset?
- **Sesgo de registro:** los datos dependen de que las autoridades reporten correctamente los siniestros. Municipios con menos capacidad institucional pueden tener subregistro.
- **Sesgo temporal:** F1 (2015-2019) y F2 (2021-2024) no cubren los mismos períodos.
- **Sesgo geográfico:** la cobertura de F1 se limita a los 316 tramos priorizados por la ANSV, no a toda la red vial del país.

### 3. ¿Qué pasaría si alguien usara estos resultados sin revisión humana?
- **Falsos positivos:** se podrían descartar tramos que sí requieren intervención.
- **Falsos negativos:** se podrían intervenir tramos que no son los más críticos.
- **Decisiones sesgadas:** se podrían priorizar departamentos con mejor registro de datos, no necesariamente los más peligrosos.

### 4. Recomendación de uso
Este análisis debe usarse como **herramienta de priorización para revisión humana**, no como filtro automático. Los tramos marcados como de alto riesgo o anómalos deben ser revisados por un analista antes de tomar cualquier decisión de intervención.

## Declaración de uso responsable

Los resultados de este proyecto son un insumo para la toma de decisiones, no un reemplazo del juicio humano. Cualquier decisión de intervención vial debe ser validada por las autoridades competentes (ANSV, INVIAS, alcaldías) y complementada con análisis de campo.
