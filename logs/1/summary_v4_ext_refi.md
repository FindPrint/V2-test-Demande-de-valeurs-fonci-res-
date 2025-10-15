# Cellule 22 : Fit v4 avec champ externe = taux refi BCE

## Paramètres optimisés

|   a_T |   b |   m1 |   tau1 |   m2 |   tau2 |   gain_h |   MSE_train |   MSE_val |   MSE_global |
|------:|----:|-----:|-------:|-----:|-------:|---------:|------------:|----------:|-------------:|
|     0 | 0.3 |  0.5 |      6 |  0.5 |     24 |       -1 |     171.953 |       nan |          nan |

## Verdict
- gain_h ≈ -1.000 → intensité du lien DVF ↔ taux refi.
- MSE global ≈ nan (train=171.953, val=nan).
- Si gain_h est significatif et négatif, cela confirme que la hausse du taux refi réduit la demande DVF.

## Fichiers générés
- images/v4_ext_refi_trajectoire.png
- images/v4_ext_refi_residus.png
- logs/summary_v4_ext_refi.md
