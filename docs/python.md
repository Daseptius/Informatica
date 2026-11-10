# 🐍 Guía Completa de Python — De Cero a Intermedio

Guía estructurada en 8 módulos, de menor a mayor dificultad. Cada concepto incluye descripción, sintaxis y ejemplos.

---

## 🟢 MÓDULO 1 — Tipos de datos primitivos simples

### 1.1 Números (`int` y `float`)

**Descripción:** Secuencia de dígitos que representan números. Pueden ser enteros (`int`) o reales (`float`, con decimales). Se permite el `-` para negativos y el `.` para decimales.

**Sintaxis / Ejemplos:**

```python
0, -1, 3.1415

type(1)      # <class 'int'>
type(-2)     # <class 'int'>
type(2.3)    # <class 'float'>
```

### 1.2 Cadenas (`str`)

**Descripción:** Secuencia de caracteres alfanuméricos que representan texto. Se escriben entre comillas simples o dobles.

**Ejemplos:**

```python
'Hola'
"Adiós"
'Python'
"123"
'True'
''        # cadena vacía
' '       # cadena con un espacio en blanco
'\n'      # salto de línea
'\t'      # tabulador
```

### 1.3 Booleanos (`bool`)

**Descripción:** Solo contiene dos valores: `True` y `False`, que representan verdadero y falso. Son inmutables.

```python
True
False
```

> 💡 **Concepto clave: inmutabilidad.** Los datos primitivos simples (números, cadenas, booleanos) son **inmutables**: su valor no puede cambiar durante la ejecución.

---

## 🟢 MÓDULO 2 — Operadores y conversión de datos

### 2.1 Operadores aritméticos

| Operador | Significado | Ejemplo | Resultado |
|----------|-------------|---------|-----------|
| `+` | suma | `2 + 3` | `5` |
| `-` | resta | `5 - 2` | `3` |
| `*` | producto | `5 * -2` | `-10` |
| `/` | cociente (real) | `5 / 2` | `2.5` |
| `//` | cociente entero | `5 // 2` | `2` |
| `%` | resto | `5 % 2` | `1` |
| `**` | potencia | `2 ** 3` | `8` |

**Orden de prioridad de evaluación:**

1. Funciones predefinidas
2. Potencias
3. Productos y cocientes
4. Sumas y restas

Se puede saltar el orden usando **paréntesis**:

```python
(2 + 3) ** 2   # 25
```

### 2.2 Operadores lógicos con números (comparación)

Devuelven un valor booleano:

| Operador | Significado |
|----------|-------------|
| `==` | igual que |
| `>` | mayor que |
| `<` | menor que |
| `>=` | mayor o igual que |
| `<=` | menor o igual que |
| `!=` | distinto de |

```python
3 == 3        # True
3.1 <= 3      # False
-1 != 1       # True
```

### 2.3 Conversión de datos primitivos (casting)

**Descripción:** Convierten un dato de un tipo a otro, siempre que sea posible.

| Función | Convierte a | Ejemplo | Resultado |
|---------|-------------|---------|-----------|
| `int()` | entero | `int('12')` | `12` |
| | | `int(True)` | `1` |
| | | `int('c')` | ❌ Error |
| `float()` | real | `float('3.14')` | `3.14` |
| | | `float(True)` | `1.0` |
| | | `float('III')` | ❌ Error |
| `str()` | cadena | `str(3.14)` | `'3.14'` |
| | | `str(True)` | `'True'` |
| `bool()` | lógico | `bool('0')` | `False` |
| | | `bool('3.14')` | `True` |
| | | `bool('')` | `False` |
| | | `bool('Hola')` | `True` |

> ⚠️ **Ojo:** `bool('0')` es `True` porque la cadena `'0'` no está vacía. Solo `bool('')` (cadena vacía) es `False`.

### 2.4 Operadores lógicos booleanos

| Operador | Descripción |
|----------|-------------|
| `not b` | Devuelve `True` si `b` es `False`, y viceversa |
| `b1 and b2` | `True` solo si ambos son `True` |
| `b1 or b2` | `True` si al menos uno es `True` |

**Tabla de verdad:**

| x | y | not x | x and y | x or y |
|---|---|-------|---------|--------|
| False | False | True | False | False |
| False | True | True | False | True |
| True | False | False | False | True |
| True | True | False | True | True |

```python
not True          # False
False or True     # True
True and False    # False
True and True     # True
```

---

## 🟢 MÓDULO 3 — Variables

**Descripción:** Una variable es un identificador ligado a un valor. Python tiene **tipado dinámico**: no se declara el tipo ni es necesario declararlas antes de usarlas.

**Reglas para nombrarlas:**

- Comienzan siempre por una letra, seguida de otras letras o números.
- No se pueden usar palabras reservadas del lenguaje.

**Sintaxis y ejemplos:**

```python
# Asignación simple
lenguaje = 'Python'
x = 3.14
y = 3 + 2

# Asignación múltiple
a1, a2 = 1, 2

# Intercambio de valores
a, b = b, a

# Incremento (equivale a x = x + 2)
x += 2

# Decremento (equivale a x = x - 1)
x -= 1

# Valor no definido
x = None

# Borrar una variable
del x
```

---

## 🟡 MÓDULO 4 — Entrada y salida por terminal

### 4.1 Entrada: `input()`

**Descripción:** Muestra un mensaje por la terminal y devuelve una cadena con la entrada del usuario.

**Sintaxis:**

```python
input(mensaje)
```

**Ejemplos:**

```python
language = input('¿Cuál es tu lenguaje favorito? ')
# Usuario escribe: Python
# language -> 'Python'   (siempre es str)

age = input('¿Cuál es tu edad? ')
# Usuario escribe: 20
# age -> '20'   (¡es cadena, no número!)
```

> ⚠️ **Advertencia clave:** El valor devuelto por `input()` **siempre es una cadena**, incluso si el usuario introduce un número. Si necesitas un número, conviértelo: `int(input(...))`.

### 4.2 Salida: `print()`

**Sintaxis:**

```python
print(dato1, ..., sep=' ', end='\n', file=sys.stdout)
```

- `dato1, ...`: datos a imprimir (tantos como quieras, separados por comas).
- `sep`: separador entre datos (por defecto un espacio `' '`).
- `end`: cadena final de la impresión (por defecto `\n`, salto de línea).
- `file`: flujo de salida (por defecto la salida estándar).

**Ejemplos:**

```python
print('Hola')                       # Hola
name = 'Alf'
print('Hola', name)                 # Hola Alf
print('El valor de pi es', 3.1415)  # El valor de pi es 3.1415
print('Hola', name, sep='')         # HolaAlf
print('Hola', name, end='\n')       # Hola Alf
```

### 4.3 Salida formateada con f-strings

**Descripción:** Construyen cadenas combinando texto con variables o expresiones. Se antepone `f` a la cadena y se incluyen las variables entre `{}`.

**Sintaxis:**

```python
f'texto {variable} más texto {expresión}'
```

**Ejemplos básicos:**

```python
name = 'Alf'
age = 20
print(f'Me llamo {name} y tengo {age} años')
# Me llamo Alf y tengo 20 años

print(f'El año que viene tendré {age + 1} años')
# El año que viene tendré 21 años
```

**Formatos dentro de las llaves** (tras `:`):

| Formato | Descripción |
|---------|-------------|
| `{valor:n}` | Alinea a la izquierda, rellena con espacios a la derecha hasta n caracteres |
| `{valor:>n}` | Alinea a la derecha, rellena con espacios a la izquierda hasta n caracteres |
| `{valor:^n}` | Centra, rellena con espacios por ambos lados hasta n caracteres |
| `{valor:nd}` | Formatea como entero con n caracteres (rellena con espacios a la izquierda) |
| `{valor:n.mf}` | Formatea como real con n caracteres totales y m decimales |

```python
pi = 3.141592
print(f'Pi vale {pi:8.2f}')   # Pi vale 3.14

dia = 'Lunes'
temperatura = 21.4
print(f'{dia:<10}{temperatura:>6.1f} °C')   # Lunes       21.4 °C
```

> ⚠️ Las f-strings solo están disponibles a partir de **Python 3.6**.

### 4.4 Alternativa: `format()`

**Sintaxis:**

```python
c.format(valores)
```

Sustituye los marcadores `{}` de la cadena `c` por los valores, por posición o por nombre.

```python
'Un {} vale {} {}'.format('€', 1.12, '$')        # 'Un € vale 1.12 $'
'Un {2} vale {1} {0}'.format('€', 1.12, '$')     # 'Un $ vale 1.12 €'
'Un {moneda1} vale {cambio} {moneda2}'.format(
    moneda1='€', cambio=1.12, moneda2='$')       # 'Un € vale 1.12 $'
```

---

## 🟡 MÓDULO 5 — Cadenas en profundidad (`str`)

### 5.1 Acceso a elementos por índice

**Descripción:** Cada carácter tiene un índice. El primer carácter es el índice `0`. Se pueden usar índices negativos para recorrer del final al principio (el último es `-1`).

**Sintaxis:**

```python
c[i]
```

**Índices de `'Python'`:**

| Carácter | P | y | t | h | o | n |
|----------|---|---|---|---|---|---|
| Índice positivo | 0 | 1 | 2 | 3 | 4 | 5 |
| Índice negativo | -6 | -5 | -4 | -3 | -2 | -1 |

**Ejemplos:**

```python
'Python'[0]    # 'P'
'Python'[1]    # 'y'
'Python'[-1]   # 'n'
'Python'[6]    # ❌ IndexError: string index out of range
```

### 5.2 Subcadenas (slicing)

**Sintaxis:**

```python
c[i:j:k]
```

Devuelve la subcadena desde el índice `i` hasta el carácter **anterior** al índice `j`, tomando caracteres cada `k`.

```python
'Python'[1:4]   # 'yth'
'Python'[1:1]   # ''  (vacía)
'Python'[2:]    # 'thon'
```

### 5.3 Operaciones con cadenas

| Operación | Descripción | Ejemplo |
|-----------|-------------|---------|
| `c1 + c2` | Concatenación | `'Me gusta ' + 'Python'` → `'Me gusta Python'` |
| `c * n` | Repetición n veces | `'Python' * 3` → `'PythonPythonPython'` |
| `c1 in c2` | ¿Está contenida? | `'y' in 'Python'` → `True` |
| `c1 not in c2` | ¿No está contenida? | `'to' not in 'Python'` → `True` |

### 5.4 Comparación de cadenas

| Operación | Descripción |
|-----------|-------------|
| `c1 == c2` | Igualdad |
| `c1 > c2` | `c1` sucede a `c2` |
| `c1 < c2` | `c1` antecede a `c2` |
| `c1 >= c2` | Sucede o es igual |
| `c1 <= c2` | Antecede o es igual |
| `c1 != c2` | Distintas |

### 5.5 Funciones de cadenas

| Función | Descripción |
|---------|-------------|
| `len(c)` | Número de caracteres |
| `min(c)` | Carácter menor |
| `max(c)` | Carácter mayor |
| `c.upper()` | Devuelve la cadena en mayúsculas |
| `c.lower()` | Devuelve la cadena en minúsculas |
| `c.title()` | Primer carácter en mayúsculas, resto en minúsculas |
| `c.split(delimitador)` | Lista de subcadenas separadas por el delimitador (por defecto, espacio) |

```python
len('Python')            # 6
min('Python')            # 'P'
max('Python')            # 'y'
'Python'.upper()         # 'PYTHON'
'A,B,C'.split(',')       # ['A', 'B', 'C']
'I love Python'.split()  # ['I', 'love', 'Python']
```

---

## 🟡 MÓDULO 6 — Condicionales y bucles

### 6.1 Condicional `if` / `elif` / `else`

**Descripción:** Evalúa una condición lógica y ejecuta el bloque correspondiente. Si `condicion1` es `True`, ejecuta su bloque; si no, evalúa las siguientes hasta llegar a la primera `True`. Si ninguna lo es, ejecuta `else`.

**Sintaxis:**

```python
if condicion1:
    bloque_codigo
elif condicion2:
    bloque_codigo
...
else:
    bloque_codigo
```

- Pueden aparecer varios `elif`, pero **solo un `else`** al final.
- ⚠️ **Los bloques deben estar indentados con 4 espacios.**

**Ejemplo:**

```python
edad = 14
if edad <= 18:
    print('Menor')
elif edad > 65:
    print('Jubilado')
else:
    print('Activo')
# Menor

edad = 20
if edad <= 18:
    print('Menor')
elif edad > 65:
    print('Jubilado')
else:
    print('Activo')
# Activo
```

### 6.2 Bucle `while`

**Descripción:** Repite la ejecución del bloque mientras la condición sea cierta.

**Sintaxis:**

```python
while condicion:
    bloque_codigo
```

Se puede interrumpir con `break`. ⚠️ Bloque indentado con 4 espacios.

**Ejemplo 1 (condición en el while):**

```python
num = None
while num != 0:
    num = int(input('Introduce un número: '))
```

**Ejemplo 2 (alternativa con `break`):**

```python
while True:
    num = int(input('Introduce un número: '))
    if num == 0:
        break
```

### 6.3 Bucle `for`

**Descripción:** Repite el bloque para cada elemento de una secuencia, asignando dicho elemento a la variable en cada repetición. Se usa para recorrer cadenas, listas, tuplas o diccionarios.

**Sintaxis:**

```python
for i in secuencia:
    bloque_codigo
```

- `break`: interrumpe el bucle.
- `continue`: salta a la siguiente iteración.
- ⚠️ Bloque indentado con 4 espacios.

**Ejemplos:**

```python
palabra = 'Python'
for letra in palabra:
    print(letra)
# P y t h o n (uno por línea)
```

**La función `range`:**

- `range(fin)`: enteros desde `0` hasta `fin-1`.
- `range(inicio, fin, salto)`: enteros desde `inicio` hasta `fin-1` con incremento `salto`.

```python
for i in range(1, 10, 2):
    print(i, end=", ")
# 1, 3, 5, 7, 9,
```

---

## 🟠 MÓDULO 7 — Listas, tuplas y diccionarios

### 7.1 Listas (`list`)

**Descripción:** Secuencias **ordenadas** de objetos de distintos tipos. Son **mutables** (pueden alterarse durante la ejecución).

**Creación con corchetes:**

```python
[]                          # lista vacía
[1, "dos", True]            # elementos de distintos tipos
[1, [2, 3], 4]              # listas anidadas
```

**Creación con `list()`:**

```python
list(c)   # crea una lista con los elementos de la secuencia/colección c
```

**Acceso a elementos** (igual que en cadenas):

```python
a = ['P', 'y', 't', 'h', 'o', 'n']
a[0]     # 'P'
a[5]     # 'n'
a[6]     # ❌ IndexError: list index out of range
a[-1]    # 'n'
```

**Sublistas** (slicing):

```python
a[1:4]   # ['y', 't', 'h']
a[1:1]   # []
```

**Operaciones que NO modifican la lista:**

| Operación | Descripción |
|-----------|-------------|
| `len(l)` | Número de elementos |
| `min(l)` | Mínimo (si son comparables) |
| `max(l)` | Máximo (si son comparables) |
| `sum(l)` | Suma de elementos (si se pueden sumar) |
| `dato in l` | ¿Pertenece? |
| `l.index(dato)` | Posición del primer elemento con ese valor |
| `l.count(dato)` | Nº de veces que aparece el valor |
| `all(l)` | `True` si todos los elementos son `True` |
| `any(l)` | `True` si algún elemento es `True` |

```python
a = [1, 2, 2, 3]
len(a)        # 4
min(a)        # 1
max(a)        # 3
sum(a)        # 8
3 in a        # True
a.index(2)    # 1
a.count(2)    # 2
```

**Operaciones que SÍ modifican la lista:**

| Operación | Descripción |
|-----------|-------------|
| `l1 + l2` | Nueva lista concatenando ambas |
| `l.append(dato)` | Añade `dato` al final |
| `l.extend(secuencia)` | Añade los datos de `secuencia` al final |
| `l.insert(indice, dato)` | Inserta `dato` en `indice`, desplazando el resto |
| `l.remove(dato)` | Elimina el primer elemento con ese valor |
| `l.pop([indice])` | Devuelve y elimina el elemento en `indice` |
| `l.sort()` | Ordena (si son comparables) |
| `l.reverse()` | Invierte el orden |

```python
a = [1, 3]
b = [2, 4, 6]
a.append(5)      # a = [1, 3, 5]
a.remove(3)      # a = [1, 5]
a.insert(1, 3)   # a = [1, 3, 5]
b.pop()          # devuelve 6
c = a + b        # c = [1, 3, 5, 2, 4]
c.sort()         # c = [1, 2, 3, 4, 5]
c.reverse()      # c = [5, 4, 3, 2, 1]
```

**Copia de listas — ¡concepto clave!**

- **Copia por referencia** `l1 = l2`: ambas variables apuntan a la **misma** lista. Cualquier cambio afecta a ambas.
- **Copia por valor** `l1 = list(l2)`: crea una **nueva** lista en otra dirección de memoria. Los cambios no se afectan mutuamente.

```python
# Por referencia
a = [1, 2, 3]
b = a
b.remove(2)
a          # [1, 3]  ¡a también cambió!

# Por valor
a = [1, 2, 3]
b = list(a)
b.remove(2)
a          # [1, 2, 3]  a no cambió
```

### 7.2 Tuplas (`tuple`)

**Descripción:** Secuencias **ordenadas** de objetos de distintos tipos. Son **inmutables** (no cambian durante la ejecución). Se usan para colecciones con estructura semántica (vectores, matrices).

**Creación con paréntesis:**

```python
()                          # tupla vacía
(1, "dos", True)
(1, 2, 3)                   # vector
((1, 2, 3), (4, 5, 6))      # matriz
```

**Creación con `tuple()`:**

```python
tuple()            # ()
tuple(1, 2, 3)     # (1, 2, 3)  ⚠️ (ver nota)
tuple("Python")    # ('P', 'y', 't', 'h', 'o', 'n')
tuple([1, 2, 3])   # (1, 2, 3)
```

**Operaciones:** El acceso y el slicing funcionan igual que en listas. Las operaciones de listas que **no modifican** también aplican a tuplas.

```python
a = (1, 2, 3)
a[1]         # 2
len(a)       # 3
a.index(3)   # 2
0 in a       # False

b = ((1, 2, 3), (4, 5, 6))
b[1]         # (4, 5, 6)
b[1][2]      # 6
```

### 7.3 Diccionarios (`dict`)

**Descripción:** Colección de pares **clave → valor**. Características:

- **No tienen orden.**
- Pueden contener elementos de distintos tipos.
- Son **mutables**.
- Las claves son **únicas** (no se repiten) y deben ser de tipo **inmutable**.

**Creación con llaves:**

```python
{}   # diccionario vacío
{'nombre': 'Alfredo', 'despacho': 218, 'email': 'asalber@ceu.es'}
{'nombre_completo': {'nombre': 'Alfredo', 'Apellidos': 'Sánchez Alberca'}}  # anidados
```

**Acceso a elementos:**

- `d[clave]`: devuelve el valor asociado. Si la clave no existe, **error**.
- `d.get(clave, valor)`: devuelve el valor asociado; si no existe, devuelve `valor` (o `None` si no se especifica).

```python
a = {'nombre': 'Alfredo', 'despacho': 218, 'email': 'asalber@ceu.es'}
a['nombre']                    # 'Alfredo'
a.get('email')                 # 'asalber@ceu.es'
a.get('universidad', 'CEU')    # 'CEU'
```

**Operaciones que NO modifican el diccionario:**

| Operación | Descripción |
|-----------|-------------|
| `len(d)` | Nº de elementos |
| `min(d)` / `max(d)` | Mínima/máxima clave (si son comparables) |
| `sum(d)` | Suma de las claves (si se pueden sumar) |
| `clave in d` | ¿Pertenece la clave? |
| `d.keys()` | Iterador sobre claves |
| `d.values()` | Iterador sobre valores |
| `d.items()` | Iterador sobre pares clave-valor |

```python
a = {'nombre': 'Alfredo', 'despacho': 218, 'email': 'asalber@ceu.es'}
len(a)          # 3
min(a)          # 'despacho'
'email' in a    # True
a.keys()        # dict_keys(['nombre', 'despacho', 'email'])
a.values()      # dict_values(['Alfredo', 218, 'asalber@ceu.es'])
a.items()       # dict_items([('nombre', 'Alfredo'), ('despacho', 218), ('email', 'asalber@ceu.es')])
```

**Operaciones que SÍ modifican el diccionario:**

| Operación | Descripción |
|-----------|-------------|
| `d[clave] = valor` | Añade/actualiza el par |
| `d.update(d2)` | Añade los pares de `d2` |
| `d.pop(clave, alternativo)` | Devuelve y elimina el valor; si no existe, devuelve `alternativo` |
| `d.popitem()` | Devuelve y elimina el último par añadido (como tupla) |
| `del d[clave]` | Elimina el par con esa clave |
| `d.clear()` | Vacía el diccionario |

```python
a = {'nombre': 'Alfredo', 'despacho': 218, 'email': 'asalber@ceu.es'}
a['universidad'] = 'CEU'
a.pop('despacho')      # 218
a.popitem()            # ('universidad', 'CEU')
del a['email']
a.clear()              # {}
```

**Copia de diccionarios** (mismo concepto que listas):

- Por referencia: `d1 = d2`
- Por valor: `d1 = dict(d2)`

---

## 🔴 MÓDULO 8 — Comprensión de colecciones

**Descripción:** Mecanismo de Python para generar colecciones nuevas a partir de colecciones existentes aplicando una expresión y/o condición, de forma más compacta que con un bucle.

### 8.1 Comprensión de listas

**Sintaxis:**

```python
[expresion for variable in lista if condicion]
```

Genera la lista con el resultado de evaluar `expresion` para cada `variable` que toma los valores de `lista` que cumplen `condicion`.

**Ejemplos:**

```python
[x ** 2 for x in range(10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

[x for x in range(10) if x % 2 == 0]
# [0, 2, 4, 6, 8]

[x ** 2 for x in range(10) if x % 2 == 0]
# [0, 4, 16, 36, 64]

notas = {'Carmen': 5, 'Antonio': 4, 'Juan': 8, 'Mónica': 9, 'María': 6, 'Pablo': 3}
[nombre for (nombre, nota) in notas.items() if nota >= 5]
# ['Carmen', 'Juan', 'Mónica', 'María']
```

### 8.2 Comprensión de diccionarios

**Sintaxis:**

```python
{expresion_clave: expresion_valor for variables in lista if condicion}
```

Genera un diccionario cuyas claves son `expresion_clave` y cuyos valores son `expresion_valor`, para cada `variable` de `lista` que cumpla `condicion`.

**Ejemplo:**

```python
notas = {'Carmen': 5, 'Antonio': 4, 'Juan': 8, 'Mónica': 9, 'María': 6, 'Pablo': 3}
{nombre: nota + 1 for (nombre, nota) in notas.items() if nota >= 5}
# {'Carmen': 6, 'Juan': 9, 'Mónica': 10, 'María': 7}
```

---

## 📚 Resumen del orden de aprendizaje

| Módulo | Tema | Dificultad |
|--------|------|-----------|
| 1 | Tipos primitivos simples (números, cadenas, booleanos) | 🟢 Básico |
| 2 | Operadores y conversión de datos | 🟢 Básico |
| 3 | Variables | 🟢 Básico |
| 4 | Entrada/salida por terminal (`input`, `print`, f-strings) | 🟡 Intermedio |
| 5 | Cadenas en profundidad (índices, slicing, funciones) | 🟡 Intermedio |
| 6 | Condicionales y bucles (`if`, `while`, `for`, `range`) | 🟡 Intermedio |
| 7 | Listas, tuplas y diccionarios | 🟠 Intermedio-alto |
| 8 | Comprensión de colecciones | 🔴 Avanzado |

### 💡 Consejos clave para recordar

1. **Indentación de 4 espacios** en todos los bloques (`if`, `while`, `for`).
2. **`input()` siempre devuelve `str`** — convierte con `int()` o `float()` si necesitas números.
3. **Índices empiezan en 0** y el último es `-1`.
4. **Copia por referencia vs. por valor** es una de las trampas más comunes con listas y diccionarios: usa `list()` o `dict()` para copiar de verdad.
5. **`bool('0')` es `True`** — solo la cadena vacía `''` es `False`.
6. **Las tuplas son inmutables**, las listas y diccionarios son mutables.
7. **f-strings requieren Python 3.6+**.
8. **Las claves de un diccionario son únicas y deben ser inmutables.**
