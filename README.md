# Sistema de Recomendación Book-Crossing y Evaluación de Equidad (Fairness)

Este proyecto implementa un sistema de recomendación basado en filtrado colaborativo (**SVD**) utilizando el dataset de **Book-Crossing**, con un enfoque central en auditoría de equidad algorítmica (*Fairness*). El objetivo principal es evaluar si el modelo presenta sesgos o disparidades de rendimiento sistemáticas al ser evaluado en distintos subgrupos demográficos y de comportamiento.

---

## 📋 Requisitos del Sistema y Dependencias

El entorno requiere **Python 3.8+** y las siguientes librerías:

* **Pandas** (manipulación de datos)
* **NumPy** (operaciones numéricas)
* **Scipy** (pruebas de hipótesis estadísticas)
* **Scikit-Surprise** (algoritmos de recomendación)

---

## 📁 Estructura y Archivos de Datos

Para ejecutar el cuaderno, se requieren los siguientes datasets en formato CSV (del dataset público *Book-Crossing* ubicados en el directorio `./datasets/`):

* `BX-Book-Ratings.csv`: Registro de valoraciones de los usuarios a los libros.
* `BX-Users.csv`: Información demográfica de los usuarios (Edad, Ubicación).

---

## ⚙️ Estructura y Flujo del Cuaderno (Refactorizado)

El código está organizado en bloques modulares, cada uno con su respectiva documentación en Markdown y salidas limpias:

1. **Importación y Carga de Datos**: Importación de librerías y lectura inicial de los archivos CSV.
2. **Segmentación Demográfica**: Creación de variables categóricas (Edad: Jóvenes vs. Adultos; Geografía: USA/Canadá vs. Resto del Mundo).
3. **Filtrado de Interacciones y Configuración del Modelo**: Depuración de usuarios con interacciones insuficientes (mínimo 8) y partición Train/Test (80/20).
4. **Segmentación por Comportamiento**: Clasificación de perfiles de usuario (*Mainstream* vs. *Nicho*) computada exclusivamente sobre el trainset para evitar *data leakage*.
5. **Entrenamiento del Modelo Predictivo**: Ajuste del algoritmo SVD y consolidación del dataset de errores.
6. **Evaluación Predictiva (RMSE y MAE)**: Medición de desviaciones predictivas por grupo y ejecución del test U de Mann-Whitney para validación de significancia en el MAE.
7. **Preparación para Métricas de Ranking**: Simulación de catálogo inyectando 100 ítems aleatorios negativos (no interactuados) por cada usuario.
8. **Cálculo de Precision, Recall y Cobertura**: Evaluación de calidad del recomendador en los primeros 5 resultados (Top-5) considerando un umbral de relevancia $\ge 7$.
9. **Test de Significancia para Ranking**: Aplicación del test de Mann-Whitney sobre las métricas P@5 y R@5 para detectar variaciones estadísticamente significativas.
10. **Experimento de Impacto 50/50 (Entrenamiento)**: Reducción del conjunto de entrenamiento al 50% para forzar un escenario de escasez de datos.
11. **Experimento de Impacto 50/50 (Evaluación de Deltas)**: Verificación de la ampliación de brechas de error y empeoramiento de la equidad entre subgrupos.
12. **Análisis y Resumen de Resultados**: Conclusión final con la interpretación de las métricas predictivas y de ranking, confirmando los sesgos detectados.
13. **Declaración de uso de herramientas de IA generativa**: Sección de transparencia con los prompts y herramientas empleadas como asistencia técnica durante el desarrollo.

---

## 📊 Métricas e Indicadores Clave

| Criterio | Subgrupos Evaluados | Métricas Medidas |
| --- | --- | --- |
| **Edad** | Jóvenes ($\le 35$) / Adultos ($> 35$) | RMSE, Precision@5, Recall@5, Cobertura, Mann-Whitney U |
| **Geografía** | USA/Canadá / Resto del Mundo | RMSE, Precision@5, Recall@5, Cobertura, Mann-Whitney U |
| **Gustos** | Mainstream / Nicho | RMSE, Precision@5, Recall@5, Cobertura, Mann-Whitney U |

---

## 🚀 Instrucciones de Ejecución

1. Verifica que los archivos `BX-Book-Ratings.csv` y `BX-Users.csv` se encuentren en la carpeta `./datasets/`.
2. Instala las dependencias necesarias mediante `pip install pandas numpy scipy scikit-surprise`.
3. Ejecuta las celdas en orden secuencial en un entorno de **Jupyter Notebook**, observando el análisis de resultados al final del documento.
