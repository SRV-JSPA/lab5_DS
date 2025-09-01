# Laboratorio 5 — Uso de RNN LSTM para Series de Tiempo

## Integrantes
- Santiago Pereira — 22318  
- Nancy Mazariegos — 22513  

---

## Introducción
El objetivo de este laboratorio fue aplicar redes neuronales recurrentes (RNN), específicamente con arquitectura LSTM, para modelar y predecir una serie temporal de producción industrial de helados y postres congelados (serie FRED: *IPN31152N*).  

El proceso incluyó la carga y exploración de los datos, su preparación mediante normalización y generación de secuencias, la construcción de un modelo LSTM con regularización, el entrenamiento con callbacks de optimización y la posterior evaluación de su desempeño en un conjunto de prueba.

---

## Metodología

1. **Carga y exploración de datos**  
   Se descargó la serie histórica (1992–2024) desde FRED. Se analizaron estadísticas descriptivas, valores faltantes y patrones estacionales.  

2. **División de conjuntos**  
   Se utilizó el 94% de los datos para entrenamiento y los últimos 24 meses (6%) como conjunto de prueba.  

3. **Normalización y preparación**  
   Se aplicó MinMaxScaler para escalar los datos a [0,1]. Se configuró un `TimeSeriesGenerator` con ventanas de 12 meses para que el modelo aprenda patrones anuales.  

4. **Construcción del modelo**  
   - Dos capas LSTM (100 y 50 neuronas).  
   - Capas de Dropout para regularización.  
   - Capa densa intermedia con activación ReLU.  
   - Capa de salida lineal para regresión.  
   El modelo totalizó 72,301 parámetros entrenables.  

5. **Entrenamiento y validación**  
   Se entrenó con callbacks (`EarlyStopping` y `ReduceLROnPlateau`) que optimizaron el proceso y evitaron sobreajuste. El entrenamiento se detuvo en la época 64 con una pérdida de validación mínima de 0.0055.  

6. **Evaluación y visualización**  
   Se calcularon métricas en el conjunto de prueba:  
   - RMSE y MAE bajos en escala original.  
   - R² alto, indicando buena explicación de la varianza.  
   - MAPE bajo, mostrando precisión aceptable en contexto económico.  
   Además, se analizaron residuos y gráficas comparativas (serie completa, zoom en prueba, residuos y dispersión predicción vs realidad).  

---

## Resultados Principales

- La exploración inicial confirmó estacionalidad clara: máximos en junio y mínimos en diciembre.  
- El modelo LSTM logró capturar tendencia y estacionalidad, ajustándose a los valores recientes de prueba.  
- Las métricas reportaron bajo error absoluto medio y un R² elevado, lo que refleja buena capacidad predictiva.  
- Los residuos se distribuyeron de manera balanceada alrededor de cero, sin evidencias de sesgo sistemático.  

---

## Discusión

Los resultados muestran que las LSTM son adecuadas para modelar series temporales con fuerte estacionalidad y tendencias suaves. La estrategia de dividir los últimos 24 meses como prueba permitió validar la capacidad del modelo para generalizar en un periodo reciente.  

El uso de normalización y ventanas de 12 meses fue clave para que la red aprendiera patrones anuales. Los callbacks ayudaron a optimizar el entrenamiento, evitando sobreajuste y ajustando dinámicamente el aprendizaje.  

Entre las limitaciones se encuentra que solo se utilizó una variable univariada, sin factores externos como clima, consumo u otras variables macroeconómicas. Esto puede limitar la robustez del modelo en escenarios extraordinarios.  

Como posibles mejoras se plantea:  
- Incluir variables exógenas.  
- Probar arquitecturas más avanzadas (Bidirectional LSTM, CNN-LSTM, Transformers).  
- Comparar con modelos tradicionales (ARIMA, Prophet) para validar la superioridad del enfoque.  

---

## Conclusión
El laboratorio permitió implementar con éxito un modelo LSTM que predice de forma precisa la producción mensual de helados y postres congelados. Se comprobó la importancia de la preparación de datos, la regularización del modelo y la correcta evaluación de métricas.  

El ejercicio demostró la utilidad práctica de las redes recurrentes para análisis de series de tiempo y brindó una base sólida para aplicaciones futuras con datos más complejos y multivariados.  
