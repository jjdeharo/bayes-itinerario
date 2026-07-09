# Itinerario de aprendizaje bayesiano · Despejar x · 1.º ESO

Recurso educativo interactivo para aprender a despejar la incógnita x en ecuaciones de primer grado. Combina un **itinerario de aprendizaje progresivo** con un **motor de evaluación adaptativa bayesiana** que ajusta la práctica a las necesidades de cada alumno.

🔗 **Demo:** https://jjdeharo.github.io/bayes-itinerario/

---

## ¿Qué hace?

El programa guía al alumno a través de tres etapas progresivas:

1. **Ecuaciones de una operación** — suma, resta, multiplicación y división
2. **Ecuaciones de dos pasos** — dos operaciones combinadas
3. **x en ambos lados** — ecuaciones completas

En cada etapa el alumno recibe una lección con explicación y ejemplo, responde preguntas adaptativas y obtiene refuerzo puntual si comete errores repetidos. Al terminar recibe un informe detallado con su nivel estimado y recomendaciones personalizadas.

---

## Funcionamiento técnico

### Motor bayesiano

El sistema mantiene una distribución de probabilidad sobre tres hipótesis de nivel (*Iniciando*, *Avanzando*, *Dominando*) que se actualiza tras cada respuesta mediante el teorema de Bayes:

$$P(H_i \mid r) = \frac{P(r \mid H_i) \cdot P(H_i)}{\sum_j P(r \mid H_j) \cdot P(H_j)}$$

### IRT 3PL

La verosimilitud de cada respuesta se calcula con el modelo logístico de Teoría de Respuesta al Ítem de 3 parámetros:

$$P(\text{acierto} \mid H_i, q) = c_q + \frac{1 - c_q}{1 + e^{-a(\theta_i - b_q)}}$$

| Parámetro | Descripción | Valores |
|-----------|-------------|---------|
| θ | Habilidad por hipótesis | −2, 0, +2 |
| b | Dificultad del ítem | −1, 0, +1 |
| a | Discriminación | derivada por pregunta: `a = a_ef / (1 − c)`, con `a_ef = 1.25` |
| c | Pseudo-azar | 0 / 0.25 / 0.5 según el tipo de pregunta |

Como el banco mezcla preguntas abiertas y de opción múltiple con distinto número de opciones, `a` no se fija: se deriva de la discriminación efectiva objetivo `a_ef` y del `c` de cada pregunta, para que todas discriminen igual en su punto de inflexión con independencia del formato (`c=0` → `a=1.25`; `c=0.25` → `a≈1.667`; `c=0.5` → `a=2.5`).

### Selección adaptativa

En cada turno se selecciona la pregunta que maximiza la ganancia de información (reducción de entropía de Shannon), con peso de diversidad para evitar repetir categorías.

### Condición de parada por etapa

Una etapa termina cuando el sistema converge (entropía < H_stop y nivel estimado > *Iniciando*) tras un mínimo de 4 preguntas, o al alcanzar el máximo de 10. Para darla por **superada** no basta con terminarla: además de la confianza mínima (≥ 80 % en un nivel distinto de *Iniciando*), el patrón de aciertos debe ser coherente con ese dominio —los aciertos observados no deben quedar muy por debajo de los que predice el nivel estimado—. No se usa un umbral fijo de porcentaje de aciertos, porque al seleccionar las preguntas por máxima información la tasa de acierto tiende hacia *(1+c)/2* y penalizaría a quien sí domina. Si el patrón no es coherente, se recomienda repasar la lección antes de continuar.

### Preguntas generadas aleatoriamente

Las ecuaciones se generan en tiempo real con parámetros aleatorios y soluciones siempre enteras, por lo que nunca se repiten entre sesiones.

---

## Tecnologías

- HTML + CSS + JavaScript (sin dependencias de framework)
- [KaTeX](https://katex.org/) para renderizado de fórmulas matemáticas
- Alojado en GitHub Pages

---

## Estructura del proyecto

```
bayes-itinerario/
├── index.html       Interfaz y lógica del itinerario adaptativo
├── ayuda.html       Guía de uso y fundamentos técnicos
└── README.md        Este documento
```

---

## Licencia

- Contenido educativo: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- Código fuente: [AGPL-3.0](https://www.gnu.org/licenses/agpl-3.0.html)

© [Juan José de Haro](https://bilateria.org)
