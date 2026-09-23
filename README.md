# Práctica 2 - Funciones básicas de OpenCV

Este repositorio contiene la segunda práctica de la asignatura Visión por Computador, llamada **Funciones básicas de OpenCV**. El trabajo realizado hasta ahora se encuentra en el cuaderno `VC_P2.ipynb`, usando Python, OpenCV, NumPy y Matplotlib para cargar imágenes, convertir espacios de color, detectar bordes, aplicar umbralizados y analizar la distribución de píxeles en imágenes binarias.

## Autores

- Pablo Llopis Parrilla
- David González Espino

## Contenido del repositorio

- `VC_P2.ipynb`: cuaderno principal de la práctica.
- `mandril.jpg`: imagen utilizada para las pruebas de detección de bordes, umbralizado y conteo de píxeles.

## Instalación y ejecución

No se necesita instalar nada adicional respecto al entorno indicado en el [README de la práctica original proporcionada por el profesor](https://github.com/otsedom/otsedom.github.io/blob/main/VC/P2/README.md). Con ese entorno es suficiente para ejecutar el cuaderno.

El cuaderno trabaja con la imagen `mandril.jpg`, que debe estar en el mismo directorio que `VC_P2.ipynb`. Para las partes que usan la cámara, el equipo debe tener una webcam disponible y permisos para acceder a ella. La salida de cámara se cierra pulsando ESC.

## TAREA: cuenta de píxeles blancos por filas en Canny

Se parte de la imagen `mandril.jpg`, que se carga con `cv2.imread()`. Después se convierte a escala de grises con `cv2.cvtColor()` y se aplica el detector de bordes Canny mediante `cv2.Canny(gris, 100, 200)`.

La tarea consiste en contar los píxeles blancos por filas, en lugar de hacerlo por columnas. Para ello se utiliza `cv2.reduce()` sobre la imagen de Canny, sumando los valores de cada fila. Como la salida de Canny contiene píxeles con valores 0 o 255, el resultado se divide entre 255 para obtener el número real de píxeles blancos.

A partir del conteo se calcula `maxfil`, que representa el máximo número de píxeles blancos encontrado en una misma fila. En la ejecución realizada se obtiene:

- Valor máximo de píxeles blancos en una fila: 220.
- Filas con un número de píxeles blancos mayor o igual que el 90 % de `maxfil`: 6, 12, 15, 20, 21, 88 y 100.
- Número total de filas que cumplen la condición: 7.

Para visualizar el resultado, se convierte la imagen de Canny a BGR y se dibujan líneas horizontales rojas sobre las filas seleccionadas mediante `cv2.line()`. Además, se genera una gráfica con Matplotlib que representa la proporción de píxeles blancos por fila.

Las filas resaltadas indican las zonas de la imagen donde Canny ha detectado una mayor concentración de bordes.


## Ampliación


## TAREA: umbralizado de Sobel y conteo por filas y columnas

Antes de resolver la tarea se calcula la imagen de bordes mediante Sobel. Primero se suaviza la imagen en escala de grises con una gaussiana usando `cv2.GaussianBlur()`. Después se obtienen las derivadas horizontal y vertical con `cv2.Sobel()`, y ambas se combinan con `cv2.add()`.

Como el resultado de Sobel queda en tipo `float64` y contiene valores positivos y negativos, se convierte a 8 bits mediante `cv2.convertScaleAbs()`. Sobre esta imagen se aplica un umbralizado binario con valor de umbral 100:

```python
_, sobelUmbralizado = cv2.threshold(sobel8, 100, 255, cv2.THRESH_BINARY)
```

Después se cuentan los píxeles no nulos por filas y por columnas usando `np.count_nonzero()`. Con estos conteos se calculan los valores máximos y se seleccionan las filas y columnas cuyo número de píxeles no nulos es mayor o igual que el 90 % del máximo correspondiente.

En la ejecución realizada se obtiene:

- Máximo de píxeles por fila: 216.
- Máximo de píxeles por columna: 219.
- Filas por encima del 90 % del máximo: 2, 3, 4, 5, 8, 11, 12, 19, 20, 24, 51, 80, 81, 82, 83, 84, 85, 87 y 100.
- Columnas por encima del 90 % del máximo: 104, 105, 127 y 288.

Para remarcar el resultado, se crea una copia en color de la imagen de Sobel umbralizada. Sobre ella se dibujan líneas horizontales rojas para las filas seleccionadas y líneas verticales azules para las columnas seleccionadas. También se generan dos gráficas: una para el conteo por filas y otra para el conteo por columnas, incluyendo una línea horizontal que marca el umbral del 90 %.

Finalmente se comparan visualmente los resultados de Sobel umbralizado y Canny. Sobel, tras el umbralizado, produce bordes más gruesos y con mayor cantidad de píxeles activados, ya que responde directamente a los cambios de intensidad de la imagen. Canny genera una detección más fina y selectiva, porque incluye pasos adicionales como suavizado, supresión de no máximos y doble umbral con histéresis. Por ello, en esta imagen Canny ofrece bordes más definidos, mientras que Sobel resalta más zonas y resulta más dependiente del valor de umbral elegido.

## Ampliación

## TAREA: propuesta propia

## Ampliación

## Fuentes y herramientas utilizadas

- Repositorio completo de la práctica proporcionado por el profesor, usado como enunciado y referencia principal: https://github.com/otsedom/otsedom.github.io/tree/main/VC/P2
