# Cellule 22bis : Fit v4 avec champ externe = Euribor 3M

## Paramètres optimisés

|      a_T |          b |   m1 |   tau1 |   m2 |   tau2 |    gain_h |   MSE_train |   MSE_val |   MSE_global |
|---------:|-----------:|-----:|-------:|-----:|-------:|----------:|------------:|----------:|-------------:|
| 0.924829 | -0.0344404 |    0 |      6 |    0 |     24 | 0.0330383 |     1.40411 |   2.15418 |      1.63851 |

## Verdict
- gain_h ≈ 0.033 → intensité du lien DVF ↔ Euribor 3M.
- MSE global ≈ 1.639 (train=1.404, val=2.154).
- Si gain_h est significatif et négatif, cela confirme que la hausse de l’Euribor réduit la demande DVF.

## Fichiers générés
- images/v4_ext_euribor_trajectoire.png
- images/v4_ext_euribor_residus.png
- logs/summary_v4_ext_euribor.md
