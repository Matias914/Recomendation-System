# Sistema de Recomendación Book-Crossing y Evaluación de Equidad (Fairness)

Este proyecto implementa un sistema de recomendación basado en filtrado colaborativo (**SVD**) utilizando el dataset de **Book-Crossing**, con un enfoque central en auditoría de equidad algorítmica (*Fairness*). El objetivo principal es evaluar si el modelo presenta sesgos o disparidades de rendimiento sistemáticas al ser evaluado en distintos subgrupos demográficos y de comportamiento.

---

## 📋 Requisitos del Sistema y Dependencias

El entorno requiere **Python 3.8+** y las siguientes librerías:

* **Pandas** (manipulación de datos)
* **NumPy** (operaciones numéricas)
* **Scipy** (pruebas de hipótesis estadísticas)
* **Scikit-Surprise** (algoritmos de recomendación)

> *Nota: El cuaderno incluye un script automático al inicio que verifica e instala `scikit-surprise` mediante `pip` si no se encuentra en el entorno.*

---

## 📁 Estructura y Archivos de Datos

Para ejecutar el cuaderno, se requieren los siguientes datasets en formato CSV (del dataset público *Book-Crossing*):

* `BX-Book-Ratings.csv`: Registro de valoraciones de los usuarios a los libros.
* `BX-Users.csv`: Información demográfica de los usuarios (Edad, Ubicación).

---

## ⚙️ Flujo del Proyecto

1. **Preprocesamiento y Limpieza**: Normalización de nombres de columnas, filtrado de usuarios en un rango de edad válido ($10 \le \text{Edad} \le 90$) y selección de usuarios con al menos 8 interacciones pasadas.
2. **Segmentación de Subgrupos (Atributos Protegidos)**:
* **Edad**: Jóvenes ($\le 35$ años) vs. Adultos ($> 35$ años).
* **Geografía**: USA/Canadá vs. Resto del Mundo.
* **Gustos**: Usuarios *Mainstream* (consumidores del Top 20% de libros más populares) vs. Usuarios de *Nicho*.


3. **Entrenamiento SVD**: Ajuste del modelo de factorización de matrices (SVD) con escala de calificación de $0$ a $10$ y una partición $80/20$ (train/test).
4. **Evaluación de Métrica de Error y Calidad de Recomendación**:
* **RMSE** (Root Mean Squared Error) global y por subgrupo.
* **Precision@5 y Recall@5** (umbral de relevancia $\ge 7$).
* **Cobertura del Catálogo** (%) recomendada para cada perfil.


5. **Pruebas de Significancia Estadística**:
* Test de **Mann-Whitney U** sobre los errores absolutos de predicción ($\vert{}y - \hat{y}\vert{}$) para validar si el sesgo detectado entre grupos es estadísticamente significativo ($p < 0.05$).


6. **Experimentos de Sensibilidad**:
* Re-evaluación del comportamiento y sesgos del modelo aplicando una partición ajustada de $50/50$ (train/test).



---

## 📊 Métricas e Indicadores Clave

| Criterio | Subgrupos Evaluados | Métricas Medidas |
| --- | --- | --- |
| **Edad** | Jóvenes ($\le 35$) / Adultos ($> 35$) | RMSE, Precision@5, Recall@5, Cobertura, Mann-Whitney U |
| **Geografía** | USA/Canadá / Resto del Mundo | RMSE, Precision@5, Recall@5, Cobertura, Mann-Whitney U |
| **Gustos** | Mainstream / Nicho | RMSE, Precision@5, Recall@5, Cobertura, Mann-Whitney U |

---

## 🚀 Instrucciones de Ejecución

1. Actualiza las rutas locales de los archivos CSV en la Celda 2 (`ruta_ratings` y `ruta_users`).
2. Ejecuta las celdas en orden secuencial en un entorno de **Jupyter Notebook** o **Google Colab**.
