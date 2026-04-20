# 🏗️ Análisis Estructural de Vigas Continuas
## Método de Rigidez (Elementos Finitos)

> **Autor:** Mario Cesgo S.  
> **Tema:** Análisis Estructural Matricial  
> **Método:** Rigidez Directa / Elementos Finitos (FEM)  
> **Lenguaje:** Python 3

---

## 📖 Descripción
Este proyecto implementa el **Método de Rigidez Directa** (o Método de Desplazamientos) para el análisis estructural de vigas continuas. Mediante la discretización en elementos de viga de Euler-Bernoulli, el código ensambla la matriz de rigidez global, aplica condiciones de frontera, resuelve el sistema lineal y calcula de forma automática los desplazamientos nodales, reacciones, fuerzas internas y deflexiones.

Incluye un módulo de visualización que genera diagramas estructurales anotados y listos para informes técnicos.

https://colab.research.google.com/drive/1-px_bK0mq-Aa4CTiyuXWK2WvHmIVHXZk?usp=sharing
---

## ✨ Características Principales
- ✅ Análisis matricial completo: discretización, ensamblaje, aplicación de apoyos y resolución del sistema `[K]{U}={F}`.
- ✅ Cálculo exacto de reacciones, fuerza cortante $V(x)$, momento flector $M(x)$ y deflexión $y(x)$.
- ✅ Soporte para **cargas distribuidas uniformes** por tramo.
- ✅ Tipos de apoyo: **Móvil** (restringe $v$) y **Fijo** (restringe $v$ y $\theta$).
- ✅ Generación automática de diagramas estructurales anotados (se guardan como `viga_anotada.png`).
- ✅ Verificación automática de equilibrio global ($\sum R = \sum wL$).
- ✅ Código modular, comentado y fácil de adaptar a nuevas geometrías.

---

## 📦 Requisitos
- Python 3.8+
- Librerías: `numpy`, `matplotlib`

```bash
pip install numpy matplotlib