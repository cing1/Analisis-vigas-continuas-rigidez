# Análisis Estructural de Vigas Contínuas
### Método de Rigidez (Elementos Finitos)

> **Autor:** Mario Cesgo S.  
> **Tema:** Análisis Estructural Matricial  
> **Método:** Rigidez Directa / Elementos Finitos (FEM)

---

## Descripción

Herramienta de análisis estructural para **vigas contínuas** basada en el **Método de Rigidez** (o Método de Desplazamientos). El programa resuelve el sistema matricial:

$$[K]\{U\} = \{F\}$$

y calcula automáticamente las reacciones en apoyos, diagramas de fuerza cortante V(x), momento flector M(x) y deflexión y(x) para cada tramo.

---

## Características

- Modelado con **elementos viga de Euler-Bernoulli** (4 GDL por elemento)
- Soporte para vigas con **n tramos** y **cargas distribuidas uniformes** por tramo
- Dos tipos de apoyo: **móvil** (restringe desplazamiento vertical) y **fijo** (restringe desplazamiento y giro)
- **Post-procesamiento completo**: V(x), M(x), y(x) usando funciones de forma de Hermite
- **Verificación de equilibrio** global automática
- **Visualización** en 5 subplots: esquema de viga, reacciones, cortante, momento y deflexión

---

## Requisitos

```bash
pip install numpy matplotlib
```

---

## Uso

Abrir el notebook `Análisis_Vigas_Contínuas_Rigidez.ipynb` y modificar únicamente la sección de **Datos de Entrada** (Sección 7):

```python
L_tramos          = [2, 3.4, 5.89, 4.345, 6.5]         # Longitudes de tramo (m)
W_tramos          = [12, 15, 12, 20, 15]                 # Cargas distribuidas (kN/m)
E_material        = 200e9                                 # Módulo de elasticidad (Pa)
I_seccion         = 0.0005                                # Momento de inercia (m⁴)
tipos_apoyo_nudos = ['fijo', 'movil', 'movil', 'movil', 'movil', 'movil']
```

Luego ejecutar todas las celdas. El análisis genera los resultados numéricos y guarda el gráfico como `viga_anotada.png`.

### Ejemplo rápido — viga de 3 tramos

```python
L_tramos          = [4.0, 6.0, 5.0]
W_tramos          = [20, 15, 25]              # kN/m
E_material        = 200e9                     # Acero, 200 GPa
I_seccion         = 8.356e-5                  # Perfil IPE300 (m⁴)
tipos_apoyo_nudos = ['movil', 'movil', 'movil', 'movil']
```

---

## Reglas de los datos de entrada

| Regla | Descripción |
|-------|-------------|
| `len(L_tramos) == len(W_tramos)` | Una carga por cada tramo |
| `len(tipos_apoyo_nudos) == len(L_tramos) + 1` | Un tipo de apoyo por nudo |
| Unidades consistentes | `E` en Pa, `I` en m⁴; las cargas se ingresan en kN/m (el código convierte a N/m) |
| Al menos un apoyo fijo | Necesario para que la matriz reducida sea invertible |

---

## Fundamentos Teóricos

### Elemento viga de Euler-Bernoulli

Cada tramo se modela con **4 grados de libertad**: desplazamiento vertical y giro en cada extremo.

La matriz de rigidez local es:

$$[k_e] = \frac{EI}{L^3}
\begin{bmatrix}
12 & 6L & -12 & 6L \\
6L & 4L^2 & -6L & 2L^2 \\
-12 & -6L & 12 & -6L \\
6L & 2L^2 & -6L & 4L^2
\end{bmatrix}$$

### Cargas de empotramiento equivalentes

Para carga distribuida uniforme $w$ (N/m):

$$\{f_{emp}\} = \left\{ -\frac{wL}{2},\ -\frac{wL^2}{12},\ -\frac{wL}{2},\ +\frac{wL^2}{12} \right\}^T$$

### Post-procesamiento

Una vez obtenidos los desplazamientos nodales {U}, las fuerzas internas se calculan como:

$$\{f_{int}\} = [k_e]\{u_e\} + \{f_{emp}\}$$

Y los diagramas en cada punto x del tramo:

| Cantidad | Fórmula |
|----------|---------|
| Cortante | $V(x) = V_0 - w \cdot x$ |
| Momento | $M(x) = M_0 + V_0 x - \frac{w x^2}{2}$ |
| Deflexión | $y(x) = \{N(x)\}^T \{u_e\} - \frac{w x^2 (L-x)^2}{24EI}$ |
| Reacciones | $\{R\} = [K]\{U\} - \{F\}$ |

---

## Supuestos y limitaciones

| # | Supuesto | Detalle |
|---|----------|---------|
| 1 | Euler-Bernoulli | Secciones planas permanecen planas; se desprecian deformaciones por cortante. Válido para $L/h > 10$. |
| 2 | Material lineal elástico | $E$ e $I$ constantes en toda la viga |
| 3 | Pequeñas deformaciones | Desplazamientos pequeños frente a las dimensiones |
| 4 | Solo cargas distribuidas uniformes | No se modelan cargas concentradas ni momentos intermedios |
| 5 | Sección constante | El mismo $E$ e $I$ para todos los tramos |
| 6 | Sin deformación axial | Viga horizontal, sin carga longitudinal |
| 7 | Apoyos rígidos | Sin deformación ni asentamiento en los apoyos |

**Funcionalidades no implementadas actualmente:** cargas concentradas intermedias, apoyos con resorte (spring supports), análisis de segundo orden (P-Δ).

---

## Estructura del notebook

| Sección | Contenido |
|---------|-----------|
| 1 | Fundamentos teóricos (matriz de rigidez, CEE, funciones de forma) |
| 2 | Procedimiento general del análisis (6 pasos) |
| 3 | Tipos de apoyos y condiciones de frontera |
| 4 | Fórmulas de post-procesamiento |
| 5 | Supuestos y restricciones del modelo |
| 6 | Implementación en Python (`analizar_viga_final`, `plot_beam_results`) |
| 7 | **Datos de entrada** ← modificar aquí |
| 8 | Ejecución del análisis |
| 9 | Resultados numéricos |
| 10 | Diagramas estructurales |
| 11 | Verificación de resultados |
| 12 | Guía para modificar el análisis |

---

## Salida del programa

- **Consola:** reacciones en cada apoyo, valores de V y M por tramo, verificación de equilibrio global (ΣR vs carga total).
- **Figura:** `viga_anotada.png` con 5 subplots anotados (esquema, reacciones, V(x), M(x), y(x)).
