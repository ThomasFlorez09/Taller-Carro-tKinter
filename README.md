# Seguidor de Línea con Tkinter

Este repositorio contiene una simulación de un **carro seguidor de línea** implementada en Python utilizando la librería **Tkinter**. El proyecto emplea un controlador **PID** para ajustar la dirección del carro y mantenerlo siguiendo una pista definida.

---

## Contenido del repositorio

- `Carito.py`: Archivo principal que contiene la definición de clases y la lógica de la simulación.
- `README.md`: Este documento explicativo.

---

## Requisitos

- Python 3.7 o superior
- Tkinter 

---

## Instalación

1. Clona este repositorio:
   ```bash
   git clone https://github.com/tu-usuario/seguidor-linea-tkinter.git
   cd seguidor-linea-tkinter
   ```
2. (Opcional) Crea y activa un entorno virtual:
   ```bash
   python -m venv venv
   source venv/bin/activate   # Linux/macOS
   venv\\Scripts\\activate  # Windows
   ```
3. Ejecuta la simulación:
   ```bash
   python main.py
   ```

---

## Uso

Al ejecutar el script:

1. Se abrirá una ventana de **800×600 px** con la pista (línea gris) y la línea guía (línea negra).
2. El carro (rectángulo azul con ruedas y sensores rojos) intentará seguir la línea negra automáticamente.
3. Al llegar a la "parada final" (rectángulo negro/rojo en la S5), el carro se detendrá y mostrará el mensaje "¡DETENIDO!".

---

## Estructura de código

### Clase `PIDController`

- `__init__(self, Kp, Ki, Kd)`: Inicializa ganancias proporcional, integral y derivativa.
- `compute(self, error, dt)`: Calcula la salida PID usando el error actual y el tiempo transcurrido.
  - Integra el error con límite para evitar "integral windup".
  - Suaviza la señal con un filtro recursivo.

### Clase `LineFollowerCar`

- `__init__(self, canvas)`: Inicializa posición, ángulo, velocidad, PID y dibuja el carro.
- `update_car(self)`: Actualiza las coordenadas de carro, ruedas y sensores en el canvas.
- `get_sensor_positions(self)`: Calcula la posición de los dos sensores delanteros.
- `check_sensor(self, x, y)`: Detecta si un sensor está sobre la línea guía.
- `check_stop_bar(self, sl, sr)`: Comprueba si ambos sensores están sobre la barra de parada.
- `move(self)`: Lógica principal de movimiento:
  1. Leer posición de sensores izquierda, derecha y centro.
  2. Determinar estado (`none`, `left`, `right`, `center`, `both`).
  3. Ajustar velocidad y ángulo con el controlador PID.
  4. Mover carro y actualizar su traza.

### Funciones globales

- `create_track_path(stops)`: Construye la ruta suave cerrada uniendo los puntos de parada.
- Bucle principal con `game_loop()`: Llama a `car.move()` y refresca la ventana cada 30 ms.

---

## Configuración de la pista

El recorrido está definido por la lista de puntos `stops`:

```python
stops = [
    (150, 100),  # S1: esquina superior izquierda
    (650, 100),  # S2: esquina superior derecha
    (650, 300),  # S3: punto medio-derecha
    (150, 300),  # S4: punto medio-izquierda
    (150, 500)   # S5: esquina inferior izquierda (barra de parada)
]
```

Puedes modificar estos puntos para cambiar la forma de la pista.

---

## Control PID

El controlador PID se utiliza para ajustar la dirección del carro en función del error de posición:

- **Error**: Valor asignado según qué sensor detecta la línea.
- **Ganancias** `Kp`, `Ki`, `Kd`: Configurables en `PIDController(3, 0.00001, 0.00001)`.
- **Limitación de integral**: Se evita que la parte integral crezca sin control.
- **Suavizado**: Filtrado recursivo para disminuir oscilaciones.

Puedes ajustar `Kp`, `Ki` y `Kd` para afinar la respuesta.


