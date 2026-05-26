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
| a | Discriminación | 1.5 (fijo) |
| c | Pseudo-azar | 0 / 0.25 / 0.5 |

### Selección adaptativa

En cada turno se selecciona la pregunta que maximiza la ganancia de información (reducción de entropía de Shannon), con peso de diversidad para evitar repetir categorías.

### Condición de parada por etapa

Una etapa termina cuando el sistema converge (entropía < H_stop y nivel estimado > *Iniciando*) tras un mínimo de 4 preguntas, o al alcanzar el máximo de 10. Si el alumno no supera el umbral se le recomienda repasar la lección antes de continuar.

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
