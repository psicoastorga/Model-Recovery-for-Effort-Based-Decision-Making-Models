# Model Recovery for Effort-Based Decision-Making Models

## Descripción

Este proyecto compara dos modelos computacionales de descuento del esfuerzo para analizar decisiones entre opciones que difieren en esfuerzo y recompensa.

Se utilizaron los datos del Experimento 1 de:

> Embrey et al. (2023). *Is all mental effort equal? The role of cognitive demand-type on effort avoidance.*

El objetivo fue comparar un **modelo de descuento lineal** y un **modelo de descuento cuadrático**, evaluando cuál presenta un mejor ajuste a las decisiones observadas.

## Modelos

### Modelo lineal

El valor subjetivo de cada opción se define como:

$$
SV = R - kE
$$

donde:

- `R` = recompensa
- `E` = esfuerzo
- `k` = costo del esfuerzo

### Modelo cuadrático

El valor subjetivo se define como:

$$
SV = R - kE^2
$$

Ambos modelos incorporan un parámetro `β`, que representa la sensibilidad a las diferencias de valor, mediante una función de elección tipo *softmax*.

## Datos

Los datos corresponden al Experimento 1 de Embrey et al. (2023).

En cada participante se identificaron 9 comparaciones, con 7 elecciones en cada comparación. A partir de estas elecciones se reconstruyeron las recompensas y los niveles de esfuerzo de las opciones fácil y difícil.

Las principales variables utilizadas para el modelado fueron:

| Variable | Descripción |
|---|---|
| `PID` | Identificador del participante |
| `E_easy` | Esfuerzo de la opción fácil |
| `E_hard` | Esfuerzo de la opción difícil |
| `R_easy` | Recompensa de la opción fácil |
| `R_hard` | Recompensa de la opción difícil |
| `choice` | Opción elegida |

## Ajuste de los modelos

Los dos modelos fueron ajustados mediante **máxima verosimilitud**, minimizando la *negative log-likelihood* mediante `optim()` en R.

Posteriormente, los modelos fueron comparados mediante el **Bayesian Information Criterion (BIC)**.

| Modelo | Log-likelihood | BIC |
|---|---:|---:|
| Modelo lineal | -1606.191 | 3821.305 |
| Modelo cuadrático | -1614.029 | 3836.981 |

El modelo lineal presentó el menor BIC y, por lo tanto, el mejor ajuste según este criterio.

## Model Recovery

Se realizó además un análisis de *model recovery* para evaluar si el diseño experimental permite distinguir entre los dos modelos.

El procedimiento consistió en:

1. Simular datos a partir del modelo lineal.
2. Simular datos a partir del modelo cuadrático.
3. Ajustar ambos modelos a los datos simulados.
4. Comparar los modelos mediante BIC.
5. Evaluar qué modelo era seleccionado como ganador.

Los resultados fueron:

| Modelo generativo | Modelo cuadrático seleccionado | Modelo lineal seleccionado |
|---|---:|---:|
| Modelo lineal | 0.1 | 0.9 |
| Modelo cuadrático | 0.1 | 0.9 |

El modelo lineal fue seleccionado en el **90% de las simulaciones**, tanto cuando los datos fueron generados por el modelo lineal como cuando fueron generados por el modelo cuadrático.

Este resultado indica que el diseño experimental presenta dificultades para distinguir adecuadamente entre los dos modelos, ya que el modelo lineal tiende a ser seleccionado incluso cuando los datos fueron generados por el modelo cuadrático.

## Conclusión

El modelo lineal presentó un mejor ajuste a los datos empíricos según el BIC. Sin embargo, los resultados del *model recovery* indican que este resultado debe interpretarse con cautela.

El procedimiento de recuperación favoreció al modelo lineal incluso cuando los datos fueron generados por el modelo cuadrático. Por lo tanto, si bien el modelo lineal fue el modelo con mejor ajuste según el criterio utilizado, el diseño experimental presenta limitaciones para discriminar entre las dos formas de descuento del esfuerzo.

## Tecnologías y métodos

- R
- Máxima verosimilitud
- `optim()`
- Bayesian Information Criterion (BIC)
- Model recovery
- Modelado computacional de decisiones

## Estructura del proyecto

```text
effort-based-decision-models/
│
├── README.md
├── data/
│   └── Exp1Data.csv
├── effort_model_comparison.Rmd
└── effort_model_comparison.pdf
