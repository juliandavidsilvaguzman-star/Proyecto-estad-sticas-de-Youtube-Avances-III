# Proyecto-estad-sticas-de-Youtube-Avances-III
readme_content = f"""# Proyecto Estadísticas de YouTube - Avances III

Este repositorio contiene el desarrollo de un **modelo NLP en dos capas** diseñado para predecir el desempeño de un video de YouTube a partir de la información y el análisis de sentimiento de sus comentarios.

## 📋 Resumen del Proyecto
- **Dataset:** [advaypatil/youtube-statistics](https://www.kaggle.com/datasets/advaypatil/youtube-statistics)
- **Autores:** Jhon Eduard Tinjaca Cruz y Julián David Silva Guzmán
- **Enfoque:** Arquitectura híbrida en dos fases:
  - **Capa A (Sentimiento de comentarios):** Clasificación del texto de los comentarios (negativo, neutro, positivo).
  - **Capa B (Desempeño del video):** Agregación de características sentimentales y volumétricas a nivel de video para predecir visualizaciones (`Views` en escala logarítmica y clasificación de impacto alto/bajo).

---

## 📊 Resultados y Respuestas Clave

### 1. ¿Cuál fue el modelo seleccionado?
- **Capa A (Sentimiento):** **TF-IDF + Regresión Logística** optimizada con búsqueda en cuadrícula (hiperparámetros óptimos: `C: {gs.best_params_.get('clf__C')}`, `min_df: {gs.best_params_.get('tfidf__min_df')}`, `ngram_range: {gs.best_params_.get('tfidf__ngram_range')}`). Se prefiere sobre modelos Transformer (como RoBERTa) por su balance de costo computacional y alta interpretabilidad.
- **Capa B (Desempeño):** 
  - **Regresión:** **Gradient Boosting** (R² = {rg['R2']:.3f})
  - **Clasificación Alto/Bajo:** **XGBoost** (AUC = {cl['AUC']:.3f})

### 2. ¿Cuál fue su desempeño y bajo qué métricas?
Las evaluaciones fueron realizadas usando **Validación Cruzada Agrupada (GroupKFold)** por Video ID para evitar cualquier tipo de fuga de información:
- **Capa A (Sentimiento):** 
  - F1 Macro: `{SENT_F1:.3f}`
  - Exactitud (Accuracy): `{SENT_ACC:.3f}`
- **Capa B (Predicción de Visualizaciones):**
  - Regresión: R² = `{rg['R2']:.3f}`, RMSE = `{rg['RMSE']:.3f}` (log Views), MAE = `{rg['MAE']:.3f}`
  - Clasificación (Alto/Bajo): F1-Score = `{cl['F1']:.3f}`, AUC = `{cl['AUC']:.3f}`

### 3. ¿Cuántos modelos se probaron?
Se probaron un total de **{n_total} configuraciones/ajustes** en la experimentación:
- **Capa A:** {n_arq_A} arquitecturas base y variantes de parámetros.
- **Capa B:** {n_arq_B} modelos evaluados a lo largo de dos escenarios experimentales diferentes (A: Solo Comentarios, B: Comentarios + Antigüedad).

### 4. ¿Qué herramientas se usaron para determinar el modelo?
- **Procesamiento de datos:** `pandas`, `NumPy`.
- **Aprendizaje automático (ML):** `scikit-learn` (`TF-IDF`, `Pipeline`, `GroupKFold`, `GridSearchCV`, `cross_validate`), `XGBoost`.
- **NLP:** `vaderSentiment`, `transformers` (`RoBERTa`).
- **Visualización:** `matplotlib`, `seaborn`.

### 5. ¿Es suficiente un solo modelo?
**No.** Un único modelo no puede capturar la estructura de datos jerárquica (comentarios anidados en videos). Se necesita un flujo desacoplado en dos niveles para poder agregar y transformar el texto crudo en señales agrupadas representativas a nivel de canal/video.

### 6. ¿Qué variables se seleccionaron para predecir?
- **Variables del Comentario (Escenario A):** `n_comentarios`, `pct_positivo`, `pct_neutro`, `pct_negativo`, `sentimiento_medio`, `longitud_media_comentario`, `pct_con_signos_!?`, `likes_medios_comentario`.
- **Variables Adicionales (Escenario B):** `antiguedad_dias`.
- **Variables Objetivo:** `log(1+Views)` para regresión y `Views >= mediana` para clasificación.

---

## 🛠️ Instrucciones de Uso

1. Clonar el repositorio.
2. Asegurarse de tener instaladas las dependencias necesarias:
   ```bash
   pip install kagglehub vaderSentiment xgboost transformers torch pandas scikit-learn matplotlib
   ```
3. Ejecutar el notebook principal en Google Colab para replicar los resultados de punta a punta.
"""

with open("README.md", "w", encoding="utf-8") as f:
    f.write(readme_content)

print("¡Archivo README.md generado con éxito!")
