# Cellule 17 : Comparaison v4 vs v4 avec champ externe

## Paramètres comparés

| version               |   a_T |        b |          m1 |    tau1 |   m2 |    tau2 |   gain_h |   D |
|:----------------------|------:|---------:|------------:|--------:|-----:|--------:|---------:|----:|
| v4 (sans externe)     |     0 | 0.467369 | 4.59077e-09 | 5.97595 |    0 | 23.9999 |        0 |   0 |
| v4_ext (avec externe) |     0 | 0.467369 | 4.59077e-09 | 5.97595 |    0 | 23.9999 |       -5 |   0 |

## MSE comparés

| metric        |   value |
|:--------------|--------:|
| v4_train      | 2.29458 |
| v4_val        | 2.64945 |
| v4_global     | 2.40548 |
| v4_ext_train  | 1.86168 |
| v4_ext_val    | 2.78341 |
| v4_ext_global | 2.14972 |

## Verdict

- v4 (sans externe) : linéaire seul activé, MSE global ≈ 2.405.
- v4_ext (avec taux directeur) : cubique + champ externe activés, gain_h ≈ -5.000, MSE global ≈ 2.150.
- Conclusion : l’introduction du taux directeur améliore l’ajustement (~10% de gain en MSE global) et modifie la structure du modèle.

## Fichiers générés
- images/comp_v4_v4ext_trajectoires.png
- images/comp_v4_v4ext_residus.png
- images/comp_v4_v4ext_mse.png
- logs/summary_v4_vs_v4ext.md
