# fLOR DE VIDA

#  Importación de librerías

```python
import bpy
import math
```

###  ¿Qué hace?

* `bpy`  Permite crear y modificar objetos dentro de Blender.
* `math`  Se usa para funciones matemáticas como seno, coseno y conversión a radianes.

---

# Limpiar la escena

```python
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```

### ¿Qué hace?

* Selecciona todos los objetos en la escena.
* Los elimina.

Esto asegura que la escena esté vacía antes de crear las nuevas figuras.

---

# Parámetros de la figura

```python
radio = 3
angulo_actual = 0
paso_angular = 10
```

### ¿Qué significa cada variable?

* `radio` tamaño de los círculos.
* `angulo_actual`  comienza en 0°, indica la posición angular.
* `paso_angular`  cuánto aumenta el ángulo en cada iteración (10°).

Esto controlará cuántos círculos se dibujan alrededor.

---

# Círculo central

```python
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0), vertices=64)
```

### ¿Qué hace?

Crea un círculo en el centro (0,0,0) con:

* Radio 3
* 64 vértices (más vértices = más suave)

Este será el círculo principal.

---

# Primer círculo alrededor (Manual)

```python
x1 = radio * math.cos(math.radians(angulo_actual))
y1 = radio * math.sin(math.radians(angulo_actual))
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x1, y1, 0), vertices=64)
```

###  ¿Qué hace?

* Convierte el ángulo a radianes.
* Usa coseno para calcular X.
* Usa seno para calcular Y.
* Crea un círculo en esa posición.

Como `angulo_actual = 0`, este círculo queda sobre el eje X positivo.

---

# Segundo círculo alrededor (Manual)

```python
angulo_actual += paso_angular
```

Aumenta el ángulo 10°.

```python
x2 = radio * math.cos(math.radians(angulo_actual))
y2 = radio * math.sin(math.radians(angulo_actual))
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x2, y2, 0), vertices=64)
```

### ¿Qué hace?

* Calcula nueva posición angular.
* Coloca otro círculo 10° más adelante.

---

# Inicio del patrón repetitivo (Automatización)

```python
while angulo_actual < 360:
```

### ¿Qué hace?

Repite el proceso hasta completar los 360° alrededor del círculo central.

---

# Cálculo dentro del ciclo

```python
x = radio * math.cos(math.radians(angulo_actual)) 
y = radio * math.sin(math.radians(angulo_actual))
```

### ¿Qué hace?

Calcula la posición del círculo usando coordenadas polares:

[
x = r \cos(\theta)
]
[
y = r \sin(\theta)
]

Esto distribuye los círculos uniformemente en forma circular.

---

# Creación del círculo en cada iteración

```python
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0), vertices=64)
```

Crea un nuevo círculo en la posición calculada.

---

# Incremento del ángulo

```python
angulo_actual += paso_angular
```

Aumenta 10° para generar el siguiente círculo.

---

# Lógica general del programa

1. Limpia la escena.
2. Crea un círculo central.
3. Calcula posiciones usando trigonometría.
4. Coloca círculos alrededor del centro.
5. Usa un `while` para repetir el patrón hasta completar la vuelta (360°).
