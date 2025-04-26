# Clasificación de Instrumentos Musicales con Filtros de Borde y CNN

## Objetivo

Desarrollar un modelo de red neuronal convolucional (CNN) que clasifique imágenes de instrumentos musicales. Para enriquecer el preprocesamiento de los datos, se implementaron filtros de detección de bordes (Canny, Laplaciano, Sobel y Prewitt), aplicados a las imágenes antes del entrenamiento. Esto permite explorar el impacto visual y funcional de estos filtros en el rendimiento del modelo.

## Dataset

Se utilizó el dataset **Musical-Instrument-Image-Dataset**, disponible en Kaggle. Contiene imágenes de diferentes instrumentos musicales organizadas en carpetas por clase.

Las imágenes fueron redimensionadas a **128x128 píxeles** (en algunas pruebas a 64x64 solo para observar el efecto en el entrenamiento) para facilitar el procesamiento y reducir el tiempo de entrenamiento.

## Preprocesamiento

- **Escalado:** Se normalizaron los valores de píxeles a un rango de [0, 1].
- **División de datos:** Se separaron los datos en entrenamiento y validación con una proporción de 80%-20%.
- **Filtros de borde:** Se aplicaron filtros a un conjunto de imágenes de muestra para visualizar su efecto.

### Filtros Utilizados

1. **Filtro Canny:** Utiliza dos umbrales para detectar bordes fuertes y conectarlos con bordes débiles si son relevantes. Es efectivo para detectar contornos precisos.
2. **Filtro Laplaciano:** Calcula la segunda derivada de la imagen, lo que ayuda a detectar cambios abruptos de intensidad. Es muy sensible al ruido.
3. **Filtro Sobel:** Estima las derivadas en direcciones horizontal y vertical. Resalta bordes suaves y graduales.
4. **Filtro Prewitt:** Similar al Sobel pero con diferente máscara, ofrece un resultado ligeramente distinto para bordes. Es más simple pero menos robusto al ruido que Sobel.

## Arquitectura del Modelo

Se construyó una CNN con la siguiente arquitectura:

- **Entrada:** Imágenes de 128x128x3.
- **Capas convolucionales:**
  - Conv2D con 32 filtros, kernel 3x3, activación ReLU
  - MaxPooling2D para reducir dimensionalidad
  - Conv2D con 64 filtros, kernel 3x3, activación ReLU
  - MaxPooling2D
- **Capa densa:**
  - Flatten para aplanar las características
  - Dense con 128 neuronas y activación ReLU
  - Dropout para evitar sobreajuste (rate=0.3)
  - Capa de salida con activación softmax (una neurona por clase)

## Entrenamiento

- Optimizador: **Adam** (aprendizaje adaptativo)
- Pérdida: **Categorical Crossentropy** (para clasificación multiclase)
- Métrica: **Precisión (accuracy)**
- Épocas: 10
- Tamaño de lote: 32

## Resultados

Se obtuvieron gráficas que muestran:

- Evolución de la **precisión** en entrenamiento y validación por época.
- Comportamiento de la **función de pérdida** durante el entrenamiento.

Esto permite identificar si hubo **overfitting** o buen ajuste.

## Conclusiones

- El uso de filtros de borde antes de alimentar las imágenes a una CNN puede ayudar a destacar las características relevantes visuales, como los contornos del instrumento.
- Cada filtro aporta una forma distinta de resaltar información:
  - El **Canny** y **Laplaciano** son más sensibles a cambios abruptos.
  - **Sobel** y **Prewitt** ofrecen suavidad y son útiles para bordes orientados.
- En el Notebook podremos observan en la graficas con cual filtro obtuvimos el valor más elto de exactitud/presición y con cual se obtuvo el valor más bajo.
