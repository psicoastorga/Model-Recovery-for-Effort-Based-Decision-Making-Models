# Model Recovery for Effort-Based Decision-Making Models

## Descripción

Este proyecto compara dos modelos computacionales de descuento del esfuerzo para analizar decisiones entre opciones que difieren en esfuerzo y recompensa.

Se utilizaron los datos del experimento 1 de:

> Embrey et al. (2023). *Is all mental effort equal? The role of cognitive demand-type on effort avoidance.*

El objetivo fue evaluar si un modelo de descuento lineal o un modelo de descuento cuadrático describe mejor las decisiones observadas.

## Modelos

### Modelo lineal

El valor subjetivo de cada opción se define como:

$$
SV = R - kE
$$

donde:

- `R` = recompensa
- `E` = esfuerzo
- `k` = costo individual del esfuerzo

### Modelo cuadrático

El valor subjetivo se define como:

$$
SV = R - kE^2
$$

Ambos modelos incorporan un parámetro `β`, que representa la sensibilidad a las diferencias de valor, mediante una función de elección tipo softmax.

## Datos

Los datos corresponden al experimento 1 de Embrey et al. (2023).

Cada participante realizó 9 comparaciones, con 7 elecciones en cada comparación. A partir de estas elecciones se reconstruyeron las recompensas y niveles de esfuerzo de las opciones fácil y difícil.

Las principales variables utilizadas para el modelado fueron:

- `PID`: identificador del participante.
- `E_easy`: esfuerzo de la opción fácil.
- `E_hard`: esfuerzo de la opción difícil.
- `R_easy`: recompensa de la opción fácil.
- `R_hard`: recompensa de la opción difícil.
- `choice`: opción elegida.

## Ajuste de los modelos

Los dos modelos fueron ajustados mediante **máxima verosimilitud**, minimizando la *negative log-likelihood* mediante `optim()` en R.

Los modelos fueron comparados utilizando el **Bayesian Information Criterion (BIC)**.

| Modelo | Log-likelihood | BIC |
|---|---:|---:|
| Lineal | -1606.191 | 3821.305 |
| Cuadrático | -1614.029 | 3836.981 |

El **modelo lineal presentó el menor BIC**, por lo que obtuvo el mejor ajuste según este criterio.

## Model Recovery

También se realizó un análisis de *model recovery* para evaluar si el diseño experimental permite distinguir adecuadamente entre los dos modelos.

El procedimiento consistió en:

1. Simular datos utilizando el modelo lineal.
2. Simular datos utilizando el modelo cuadrático.
3. Ajustar ambos modelos a los datos simulados.
4. Comparar los modelos mediante BIC.
5. Evaluar qué modelo era seleccionado como ganador.

Los resultados mostraron que el modelo lineal fue seleccionado en el **90% de las simulaciones**, tanto cuando los datos fueron generados por el modelo lineal como cuando fueron generados por el modelo cuadrático.

| Modelo generativo | Cuadrático seleccionado | Lineal seleccionado |
|---|---:|---:|
| Lineal | 0.1 | 0.9 |
| Cuadrático | 0.1 | 0.9 |

Este resultado indica que el diseño experimental presenta **dificultades para distinguir entre los modelos**, ya que el modelo lineal tiende a ser seleccionado incluso cuando los datos son generados por el modelo cuadrático.

## Conclusión

El modelo lineal presentó un mejor ajuste a los datos empíricos según el BIC. Sin embargo, el análisis de *model recovery* muestra que este resultado debe interpretarse con cautela, dado que el procedimiento de recuperación favoreció al modelo lineal incluso cuando los datos fueron generados por el modelo cuadrático.

Por lo tanto, los resultados permiten identificar al modelo lineal como el modelo con mejor ajuste según el criterio utilizado, pero también muestran una **limitación del diseño para discriminar entre las dos formas de descuento del esfuerzo**.

## Tecnologías

- R
- Máxima verosimilitud
- `optim()`
- Bayesian Information Criterion (BIC)
- Model recovery
- Modelado computacional de decisiones

## Estructura del repositorio

```text
.
├── TRABAJO-FINAL-MODELADO.Rmd
├── TRABAJO-FINAL-MODELADO.pdf
├── Exp1Data.csv
└── README.md
