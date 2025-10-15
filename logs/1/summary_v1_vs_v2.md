# Cellule 10 : Comparaison v1 vs v2

## Paramètres comparés

| version   |     a_T |     b |   m0 |   tau_m |   D |
|:----------|--------:|------:|-----:|--------:|----:|
| v1        | 18.7061 | 1e-06 |    0 | 49.999  | 0   |
| v2        | 13.2465 | 1e-06 |    0 | 49.9985 | 0.5 |

## MSE comparés

| metric    |    value |
|:----------|---------:|
| v1_global | 0.440108 |
| v2_train  | 0.246806 |
| v2_val    | 0.888905 |
| v2_global | 0.439436 |

## Verdict

- - v1 parcimonieux : MSE global ≈ 0.440, un seul terme activé (linéaire).
- - v2 sur-ajusté : MSE_train ≈ 0.247 mais MSE_val ≈ 0.889 (dégradation), D activé.
- - Conclusion : en DVF 2024, v1 est plus robuste et parcimonieux. Le bruit de v2 n’améliore pas la généralisation.

## Fichiers générés
- images/comp_v1_v2_trajectoires.png
- images/comp_v1_v2_residus.png
- images/comp_v1_v2_mse.png
- logs/summary_v1_vs_v2.md
