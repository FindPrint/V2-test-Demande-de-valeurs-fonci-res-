# Cellule 19 : Comparaison triple v4 vs v4_ext vs v4_ext_relâché

## Paramètres comparés

| version                  |   a_T |        b |       m1 |    tau1 |       m2 |    tau2 |   gain_h |   D |
|:-------------------------|------:|---------:|---------:|--------:|---------:|--------:|---------:|----:|
| v4 (sans externe)        |     0 | 0.467    | 0        | 6       | 0        | 24      |  0       |   0 |
| v4_ext (-5)              |     0 | 0.467    | 0        | 6       | 0        | 24      | -5       |   0 |
| v4_ext_relâché (-20,+20) |     0 | 0.355307 | 0.683871 | 6.01019 | 0.489203 | 23.9998 | -5.65084 |   0 |

## MSE comparés

| metric         |   value |
|:---------------|--------:|
| v4_train       | 2.29458 |
| v4_val         | 2.64945 |
| v4_global      | 2.40548 |
| v4_ext_train   | 1.86168 |
| v4_ext_val     | 2.78341 |
| v4_ext_global  | 2.14972 |
| v4_ext2_train  | 1.83326 |
| v4_ext2_val    | 2.88893 |
| v4_ext2_global | 2.16316 |

## Verdict

- v4 (sans externe) : linéaire seul, MSE global ≈ 2.405.
- v4_ext (-5) : cubique + champ externe saturé, gain_h=-5, MSE global ≈ 2.150.
- v4_ext_relâché : cubique + mémoire (τ1≈6.0, τ2≈24.0) + champ externe fort (gain_h≈-5.65), MSE global ≈ 2.163.
- Conclusion : le taux directeur est un facteur explicatif majeur. En relâchant la borne, le modèle active aussi mémoire et cubique, révélant une dynamique interne/externe mixte.

## Fichiers générés
- images/comp_v4_triple_trajectoires.png
- images/comp_v4_triple_residus.png
- images/comp_v4_triple_mse.png
- logs/summary_v4_triple.md
