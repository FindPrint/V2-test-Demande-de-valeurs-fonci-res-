# Cellule 14 : Comparaison v1 vs v4

## Paramètres comparés

| version      |     a_T |     b |   m0 |   tau_m |   D |   m1 |      tau1 |   m2 |     tau2 |
|:-------------|--------:|------:|-----:|--------:|----:|-----:|----------:|-----:|---------:|
| v1 (2024)    | 18.7061 | 1e-06 |    0 |  49.999 |   0 |  nan | nan       |  nan | nan      |
| v4 (2020–23) |  5.5064 | 1e-06 |  nan | nan     |   0 |    0 |   5.77933 |    0 |  23.9995 |

## MSE comparés

| metric    |    value |
|:----------|---------:|
| v1_global | 0.440108 |
| v4_train  | 2.29458  |
| v4_val    | 2.64945  |
| v4_global | 2.40548  |

## Verdict

- v1 (2024) : parcimonieux, linéaire seul activé, MSE global ≈ 0.440.
- v4 (2020–23) : toujours linéaire, mémoire et bruit inactifs, MSE global ≈ 2.405, erreurs plus élevées (variance accrue du marché).
- Conclusion : l’élargissement temporel n’active pas la mémoire, mais révèle une dynamique plus volatile. Le terme linéaire reste dominant.

## Fichiers générés
- images/comp_v1_v4_trajectoires.png
- images/comp_v1_v4_residus.png
- images/comp_v1_v4_mse.png
- logs/summary_v1_vs_v4.md
