
# Chocolate Sales Model Report

## Dataset Overview
- Total rows: 3282
- Train/Test split: 2625/657

## Metrics (Random Forest)
- MAE: 1757.55
- RMSE: 2318.42
- R²: 0.754
- Baseline MAE: 3749.19
- Improvement over baseline: 1991.64

## Top Errors
- MAE per country (top 3): {'India': 2141.909256363636, 'UK': 1828.8464235849058, 'USA': 1772.0626523809524}
- MAE per product (top 3): {'Manuka Honey Choco': 2552.884592592592, 'Milk Bars': 2241.3376448275862, 'Peanut Butter Cubes': 2131.1248419354843}

## Top Features (RF Built-in)
boxes_shipped                  0.197448
month                          0.084090
day_of_week                    0.040058
product_Organic Choco Syrup    0.023520
country_USA                    0.022204

## Top Features (Permutation)
boxes_shipped          1418.758304
month                  1007.129269
country_India           220.729677
country_USA             173.210839
country_New Zealand     171.393645

## Stretch Goals
- Weekend feature added: Yes
- Monthly sales MAE: 67334.58 (if run)

## Conclusions
Modelul Random Forest are MAE 1757.55, explicând 75.4% din variație.
Top feature: boxes_shipped.
Erorile sunt mari în țările cu puține date – recomandăm mai multe features sau date.

![Feature Importance](figures/feature_importance_rf.png)
![Errors by Country](figures/errors_by_country.png)
