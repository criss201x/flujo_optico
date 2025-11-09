# flujo_optico
Implementación del método de Lucas-Kanade para tracking de puntos en video

## 📄 Archivo Principal

**[lucas_kanade_video_tracking.ipynb](lucas_kanade_video_tracking.ipynb)** - Notebook completo con implementación y ejemplos

---

## 📋 Descripción

Este proyecto implementa el **algoritmo de Lucas-Kanade** para calcular flujo óptico y realizar tracking de puntos a través de múltiples frames de video. Incluye tanto implementación manual del algoritmo como integración con OpenCV.

### Características principales:

- ✅ Detección automática de puntos de interés (Shi-Tomasi)
- ✅ Tracking temporal frame a frame
- ✅ Gestión de puntos perdidos
- ✅ Visualización de trayectorias
- ✅ Generación de video con flechas de flujo óptico
- ✅ Comparación entre implementación manual y OpenCV
- ✅ Análisis de calidad del tracking

---

## 🚀 Instalación

### Dependencias

```bash
pip install numpy opencv-python matplotlib pillow ipywidgets jupyter
```

Si usas JupyterLab y tienes problemas con widgets:

```bash
pip install "numpy<2"  # Versión estable
pip install jupyterlab ipywidgets
```

---

## 📖 Uso

### 1. Cargar y procesar video

```python
from lucas_kanade_video_tracking import procesar_video_optical_flow

# Procesar video (devuelve trayectorias, frames y puntos por frame)
trayectorias, frames, puntos_por_frame = procesar_video_optical_flow(
    video_path='tu_video.mp4',
    max_puntos=50,           # Número de puntos a trackear
    tamaño_ventana=21,       # Tamaño de ventana Lucas-Kanade
    max_nivel_piramide=3,    # Niveles de pirámide (movimientos rápidos)
    visualizar=True          # Mostrar trayectorias
)
```

### 2. Generar video con tracking

```python
from lucas_kanade_video_tracking import crear_video_con_tracking

crear_video_con_tracking(
    frames,
    puntos_por_frame,
    trayectorias,
    output_path='tracking_output.mp4',
    fps=10
)
```

---

## 🔧 Parámetros principales

| Parámetro | Descripción | Valores recomendados |
|-----------|-------------|---------------------|
| `max_puntos` | Número máximo de puntos a detectar | 50-100 |
| `tamaño_ventana` | Tamaño de ventana para Lucas-Kanade | 15-25 (mayor si hay ruido) |
| `max_nivel_piramide` | Niveles de pirámide para movimientos grandes | 2-4 |
| `calidad` | Umbral de calidad Shi-Tomasi | 0.01-0.1 |
| `distancia_min` | Distancia mínima entre puntos | 10-20 px |

---

## 📊 Funciones principales

### `detectar_puntos_para_tracking(frame, max_puntos, calidad, distancia_min)`
Detecta esquinas usando el criterio de Shi-Tomasi.

**Returns:** Array de puntos `(N, 1, 2)` con coordenadas `[x, y]`

---

### `tracking_lucas_kanade(frames, puntos_iniciales, tamaño_ventana, max_nivel_piramide)`
Realiza tracking de puntos a través de todos los frames.

**Returns:**
- `trayectorias`: Dict `{id_punto: [(x,y), ...]}`
- `puntos_por_frame`: Lista de puntos válidos en cada frame

---

### `procesar_video_optical_flow(video_path, max_puntos, tamaño_ventana, max_nivel_piramide, visualizar)`
Pipeline completo: carga video, detecta puntos, ejecuta tracking y visualiza.

**Returns:**
- `trayectorias`: Dict con trayectorias completas
- `frames`: Lista de frames procesados
- `puntos_por_frame`: Puntos activos en cada frame

---

### `crear_video_con_tracking(frames, puntos_por_frame, trayectorias, output_path, fps)`
Genera un video mostrando:
- Puntos verdes (posición actual)
- Flechas azules (vector de movimiento)
- Contador de frames y puntos activos

---

## 🧪 Ejemplos incluidos en el notebook

1. **Video sintético** - Genera objetos en movimiento (círculo, cuadrado, triángulo)
2. **Detección de puntos** - Visualiza puntos Shi-Tomasi
3. **Tracking temporal** - Propaga puntos frame a frame
4. **Visualización de trayectorias** - Muestra caminos seguidos
5. **Análisis de calidad** - Estadísticas de supervivencia de puntos
6. **Comparación manual vs OpenCV** - Valida implementación
7. **Generación de video** - Crea video con flechas de flujo óptico

---

## 🎯 Casos de uso

- **Seguimiento de objetos** en videos
- **Análisis de movimiento** (deportes, biomecánica)
- **Detección de anomalías** (objetos que se mueven vs estáticos)
- **Estabilización de video** (compensación de movimiento de cámara)
- **Comprensión de escenas** dinámicas

---

## 📐 Fundamento matemático

El método de Lucas-Kanade busca las velocidades $(u, v)$ que minimizan:

$$\min_{u,v} \sum_{x,y \in W} [I_x(x,y)\,u + I_y(x,y)\,v + I_t(x,y)]^2$$

Solucionando el sistema lineal:

$$\begin{bmatrix}
\sum I_x^2 & \sum I_x I_y \\
\sum I_x I_y & \sum I_y^2
\end{bmatrix}
\begin{bmatrix}u \\ v\end{bmatrix}
=
-\begin{bmatrix}\sum I_x I_t \\ \sum I_y I_t\end{bmatrix}$$

**Criterio de Shi-Tomasi** (selección de puntos): Los eigenvalores de la matriz de estructura deben ser grandes para que el punto sea "bueno" para tracking.

---

## ⚠️ Consideraciones

### Preprocesamiento recomendado:
- Aplicar suavizado Gaussiano si hay mucho ruido
- Normalizar iluminación si varía entre frames

### Limitaciones del método:
- Asume movimiento pequeño entre frames
- Asume brillo constante (no cambios de iluminación)
- No maneja oclusiones

### Soluciones:
- Usar pirámides para movimientos grandes (`max_nivel_piramide`)
- Ventanas grandes para ruido (`tamaño_ventana`)
- Re-detectar puntos periódicamente

---

## 📂 Estructura del repositorio

```
lucas_kanade/
├── lucas_kanade_video_tracking.ipynb   # Notebook principal
├── Class1_Lucas_Kanade.ipynb           # Clase introductoria
├── 20251008_120434.mp4                 # Video de ejemplo
├── tracking_ejemplo.mp4                # Output de tracking
└── README.md                           # Este archivo
```

---

## 📚 Referencias

- [Lucas-Kanade method (Wikipedia)](https://en.wikipedia.org/wiki/Lucas%E2%80%93Kanade_method)
- [OpenCV Optical Flow Tutorial](https://docs.opencv.org/4.x/d4/dee/tutorial_optical_flow.html)
- [Shi-Tomasi Corner Detector](https://docs.opencv.org/4.x/d4/d8c/tutorial_py_shi_tomasi.html)

---

## 🤝 Contribuciones

Para reportar problemas o sugerencias, abre un issue en el repositorio.

---

## 📝 Licencia

Este proyecto es de uso educativo.