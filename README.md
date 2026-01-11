# Proyecto de Computer Vision - Piedra, Papel o Tijera

Sistema de reconocimiento de gestos manuales para jugar a Piedra, Papel o Tijera mediante visión por ordenador

## Características

- 🎮 **Dos modos de juego**: Jugador vs Jugador (PvP) y Jugador vs CPU (PvE)
- 🎨 **Selector de modo visual**: Detección de patrones de colores en tiempo real (HSV)
- 📷 **Calibración de cámara**: Corrección de distorsión de lente (Radial/Tangencial)
- ✋ **Detección de gestos**: Algoritmo robusto basado en defectos convexos y substracción de fondo (Chroma Key)
- 🔒 **Sistema de Seguridad**: Validación temporal de intenciones y filtros de circularidad

## Metodología y Funcionamiento

### Diagrama de Bloques
```
[WEBCAM] -> [CALIBRACIÓN] -> [CV2.UNDISTORT] -> [FLIP]
                                  |
            +---------------------+---------------------+
            v                     v                     v
     [ESTADO: MENÚ]        [ESTADO: JUEGO]      [VISIÓN ARTIFICIAL]
     (Selector Color)        (PvP / PvE)         (Detect. Gestos)
            |                     |                     |
            v                     v                     v
    [Validación HSV]      [Máquina Estados]    [Segmentación Piel]
            |                     |                     |
            v                     v                     v
    [Filtro Circular]     [Lógica Ganador]     [Convexity Defects]
```

### Pipeline de Procesamiento
1.  **Corrección**: Se aplica la matriz de calibración para eliminar distorsiones.
2.  **Segmentación**:
    *   **Piel**: Detección BGR->HSV (Tono piel adaptable).
    *   **Fondo**: Chroma Key (Verde) para eliminación robusta de fondo.
3.  **Filtrado**: Operaciones morfológicas (Erode/Dilate) para limpiar ruido.
4.  **Clasificación**: Conteo de defectos de convexidad (dedos levantados) para determinar el gesto.

## Requisitos

- Python 3.9+
- Cámara web (720p mínimo)
- Windows

## Instalación

```bash
# Crear entorno virtual
conda env create -f environment_win.yml
conda activate voi-lab

# Verificar instalación
python -c "import cv2; print(cv2.__version__)"
```

## Uso Rápido

```bash
# Ejecutar el juego
python final.py
```

## Calibración de Cámara (Opcional)

```bash
# 1. Capturar imágenes del patrón (15-20 fotos)
python capture_calibration_images.py

# 2. Calibrar
python calibrate.py
```

## Controles

### Menú
- Mostrar secuencia de bolas de colores:
  - **Rojo → Amarillo → Azul** = Modo PvP
  - **Azul → Amarillo → Rojo** = Modo PvE
- **ESPACIO**: Confirmar selección

### Juego
- **ESPACIO**: Iniciar cuenta regresiva
- **R**: Revancha
- **M**: Volver al menú
- **Q**: Salir

## Estructura del Proyecto

```
Proyecto/
├── final.py                          # Programa principal
├── calibrate.py                      # Calibración de cámara
├── capture_calibration_images.py     # Captura de imágenes
├── calibration_data.npz              # Datos de calibración
├── checkerboard_pattern.png          # Patrón de calibración
├── environment_win.yml               # Dependencias
└── captured_images/                  # Imágenes de calibración
```

## Autor

Proyecto desarrollado para la asignatura de Computer Vision - ICAI por Miguel Martín Vieira y Pablo Güell con la ayuda de la IA - Google Antigravity

