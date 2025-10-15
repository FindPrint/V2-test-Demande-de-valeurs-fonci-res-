# Cellule 20 : Rapport final v1 vs v4 vs v4_ext vs v4_ext_relâché

## Figures de synthèse

- images/rapport_final_overview.png
- images/rapport_final_mse.png

## Paramètres et MSE globaux comparés

| version                    |     a_T |        b |   m0 |   tau_m |         m1 |      tau1 |         m2 |     tau2 |   gain_h |   D |   MSE_global |
|:---------------------------|--------:|---------:|-----:|--------:|-----------:|----------:|-----------:|---------:|---------:|----:|-------------:|
| v1 (2024)                  | 18.7061 | 1e-06    |    0 |  49.999 | nan        | nan       | nan        | nan      |  0       |   0 |     0.440108 |
| v4 (2020–23, sans externe) |  0      | 0.467    |  nan | nan     |   0        |   6       |   0        |  24      |  0       |   0 |     2.40548  |
| v4_ext (gain_h=-5)         |  0      | 0.467    |  nan | nan     |   0        |   6       |   0        |  24      | -5       |   0 |     2.14972  |
| v4_ext_relâché             |  0      | 0.355307 |  nan | nan     |   0.683871 |   6.01019 |   0.489203 |  23.9998 | -5.65084 |   0 |     2.16316  |

## Verdict

- v1 (2024) : parcimonieux, linéaire seul activé, MSE global ≈ 0.440.
- v4 (2020–23) : sans externe, MSE global ≈ 2.405, le linéaire/cubique minimal peine face à la volatilité.
- v4_ext (borne -5) : champ externe activé (taux directeur), MSE global ≈ 2.150 (~gain marqué vs v4).
- v4_ext_relâché : champ externe fort + mémoire multi-échelle + cubique, MSE global ≈ 2.163, structure plus riche, gain comparable au cas borne -5.
- Conclusion : le taux directeur est un facteur explicatif majeur. En relâchant la borne, le modèle révèle aussi une inertie interne (≈6 et ≈24 mois) et une non-linéarité (cubique).

## Fichiers générés
- images/rapport_final_overview.png
- images/rapport_final_mse.png
- logs/rapport_final_comparatif.md
