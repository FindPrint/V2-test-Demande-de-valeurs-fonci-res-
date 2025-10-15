# Cellule 12 : Clôture v3 — Comparaison finale et rapport

## Paramètres comparés (v1, v2, v3)

| version   |     a_T |     b |   m0 |    tau_m |   gain_h |         D |   m1 |     tau1 |   m2 |   tau2 |
|:----------|--------:|------:|-----:|---------:|---------:|----------:|-----:|---------:|-----:|-------:|
| v1        | 18.7061 | 1e-06 |    0 |  49.999  |        0 | 0         |  nan | nan      |  nan |    nan |
| v2        | 13.2465 | 1e-06 |    0 |  49.9985 |        0 | 0.5       |  nan | nan      |  nan |    nan |
| v3        | 12.0466 | 1e-06 |  nan | nan      |        0 | 0.0834645 |    0 |  29.9997 |    0 |    250 |

## MSE comparés

| metric    |    value |
|:----------|---------:|
| v1_global | 0.440108 |
| v2_train  | 0.246806 |
| v2_val    | 0.888905 |
| v2_global | 0.439436 |
| v3_train  | 0.245648 |
| v3_val    | 0.891269 |
| v3_global | 0.439335 |

## Verdict

- v1 parcimonieux et robuste: MSE global ≈ 0.440, terme linéaire seul activé.
- v2 sur-ajusté par le bruit: MSE_train ≈ 0.247, MSE_val ≈ 0.889 (dégradation).
- v3 régularisé: bruit modéré, mémoire non activée; MSE_val ≈ 0.891, MSE_global ≈ 0.439 (proche v1).
- Conclusion: sur DVF 2024 agrégé, la dynamique est quasi-linéaire; v1 reste le choix parcimonieux de référence.

## Fichiers générés
- images/comp_v1_v2_v3_trajectoires.png
- images/comp_v1_v2_v3_residus.png
- images/comp_v1_v2_v3_mse.png
- logs/summary_v3_final.md
