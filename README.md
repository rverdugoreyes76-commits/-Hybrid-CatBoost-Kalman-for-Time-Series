# -Hybrid-CatBoost-Kalman-for-Time-Series
CatBoost Regressor para capturar tendencias principales y relaciones no lineales., más Filtro de Kalman para modelar la dinámica temporal de los residuos y estimar incertidumbre.
## Modelo matemático

Sea ![y_t](https://latex.codecogs.com/png.latex?y_t) la serie temporal a predecir y ![\mathbf{x}_t](https://latex.codecogs.com/png.latex?%5Cmathbf%7Bx%7D_t) el vector de características asociadas:

1. **Predicción inicial con CatBoost:**

   ![\hat{y}^{\text{boost}}_t = f_{\text{CatBoost}}(\mathbf{x}_t)](https://latex.codecogs.com/png.latex?%5Chat%7By%7D%5E%7B%5Ctext%7Bboost%7D%7D_t%20%3D%20f_%7B%5Ctext%7BCatBoost%7D%7D%28%5Cmathbf%7Bx%7D_t%29)

   donde ![f_{\text{CatBoost}}](https://latex.codecogs.com/png.latex?f_%7B%5Ctext%7BCatBoost%7D%7D) es la función estimada por el booster.

2. **Residuos:**

   ![r_t = y_t - \hat{y}^{\text{boost}}_t](https://latex.codecogs.com/png.latex?r_t%20%3D%20y_t%20-%20%5Chat%7By%7D%5E%7B%5Ctext%7Bboost%7D%7D_t)

3. **Modelo lineal estocástico para residuos (Kalman):**

   ![z_t = \phi z_{t-1} + \epsilon_t, \;\;\; \epsilon_t \sim \mathcal{N}(0, \sigma^2_{\text{trans}})](https://latex.codecogs.com/png.latex?z_t%20%3D%20%5Cphi%20z_%7Bt-1%7D%20%2B%20%5Cepsilon_t%2C%20%5C%3B%5C%3B%5C%3B%20%5Cepsilon_t%20%5Csim%20%5Cmathcal%7BN%7D%280%2C%20%5Csigma%5E2_%7B%5Ctext%7Btrans%7D%7D%29)

   ![r_t = z_t + \eta_t, \;\;\; \eta_t \sim \mathcal{N}(0, \sigma^2_{\text{obs}})](https://latex.codecogs.com/png.latex?r_t%20%3D%20z_t%20%2B%20%5Ceta_t%2C%20%5C%3B%5C%3B%5C%3B%20%5Ceta_t%20%5Csim%20%5Cmathcal%7BN%7D%280%2C%20%5Csigma%5E2_%7B%5Ctext%7Bobs%7D%7D%29)

4. **Predicción híbrida:**

   ![\hat{y}_t^{\text{hybrid}} = \hat{y}^{\text{boost}}_t + \hat{z}_t](https://latex.codecogs.com/png.latex?%5Chat%7By%7D_t%5E%7B%5Ctext%7Bhybrid%7D%7D%20%3D%20%5Chat%7By%7D%5E%7B%5Ctext%7Bboost%7D%7D_t%20%2B%20%5Chat%7Bz%7D_t)

   donde ![\hat{z}_t](https://latex.codecogs.com/png.latex?%5Chat%7Bz%7D_t) es la estimación del estado latente del Filtro de Kalman.

5. **Intervalos de predicción (aproximación 95%):**

   ![\hat{y}_t^{\text{hybrid}} \pm 2 \sqrt{\operatorname{Var}(\hat{z}_t)}](https://latex.codecogs.com/png.latex?%5Chat%7By%7D_t%5E%7B%5Ctext%7Bhybrid%7D%7D%20%5Cpm%202%20%5Csqrt%7B%5Coperatorname%7BVar%7D%28%5Chat%7Bz%7D_t%29%7D)
