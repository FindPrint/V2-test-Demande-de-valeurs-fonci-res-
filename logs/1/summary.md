# Test empirique — Équation universelle

- Initialisation : 2025-10-15T10:35:43.796287Z
- Dossiers créés : images/, logs/, models/, data/, data_copies/

## Cellule 1 : Setup
- Logger niveau DEBUG configuré (logs/simulation_log.txt)
- Logs structurés (logs/logs.csv)
- Résumé lisible (logs/summary.md)

## Cellule 2 : Chargement et prétraitement des données DVF 2024

- Archive téléchargée : data/valeurs_foncieres_2024.zip
- Extraite dans : data/valeurs_foncieres_2024
- Fichier principal : data/valeurs_foncieres_2024/ValeursFoncieres-2024.txt
- Copie brute sauvegardée : data_copies/raw_data.csv
- Colonnes clés disponibles : ['date_mutation', 'valeur_fonciere', 'code_departement', 'surface_reelle_bati']
- Aperçu :

|    | date_mutation       |   valeur_fonciere |   code_departement |   surface_reelle_bati |
|---:|:--------------------|------------------:|-------------------:|----------------------:|
|  0 | 2024-02-01 00:00:00 |             346.5 |                 01 |                   nan |
|  1 | 2024-03-01 00:00:00 |           10000   |                 01 |                   nan |
|  2 | 2024-08-01 00:00:00 |          249000   |                 01 |                   nan |
|  3 | 2024-03-01 00:00:00 |          329500   |                 01 |                     0 |
|  4 | 2024-03-01 00:00:00 |          329500   |                 01 |                     0 |

## Cellule 3 : Paramètres universels

- Paramètres initiaux : {
  "kappa": 0.1,
  "alpha": 1.0,
  "b": 0.01,
  "D": 0.05,
  "T_c": 1.0,
  "delta_T": 1.0,
  "n": 1000,
  "d": 6,
  "T_log": 13.815510557964274,
  "T_star": 12.815510557964274,
  "a_T": 12.815510557964274
}

## Cellule 4 : Discrétisation et grilles

- Nt = 1000
- Normalisation robuste : median=105531.750, mad=9475.000, scale=14047.635
- Grilles : data/grids.npy, data/grids.csv
- Noyau mémoire : data/memory_kernel.csv (m0=0.1, tau_m=50.0)
- Figure : images/phi_on_grid.png

## Cellule 5 : Fitting des paramètres

- Méthode d’optimisation : L-BFGS-B
- Paramètres fittés : |     a_T |     b |   m0 |   tau_m |   gain_h |      mse |   nit | success   | message                                          |
|--------:|------:|-----:|--------:|---------:|---------:|------:|:----------|:-------------------------------------------------|
| 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.440108 |     5 | True      | CONVERGENCE: NORM OF PROJECTED GRADIENT <= PGTOL |

- Fichiers :
  - models/fitted_params.csv
  - copiemodels/fitted_params.npy
  - data/simulation.csv
  - images/fit_phi_vs_data.png

## Cellule 6 : Simulation complète et ablation

- Fichier ablation : data/simulation_ablation.csv
- Fichier métriques : data/metrics.csv
- Figures : images/sim_full_vs_data.png, images/ablation_compare.png, images/ablation_mse_bar.png
- Paramètres fittés (réutilisés) :
|     a_T |     b |   m0 |   tau_m |   gain_h |      mse |   nit | success   | message                                          |
|--------:|------:|-----:|--------:|---------:|---------:|------:|:----------|:-------------------------------------------------|
| 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.440108 |     5 | True      | CONVERGENCE: NORM OF PROJECTED GRADIENT <= PGTOL |

- Metrics:
| scenario   |      MSE |     a_T |     b |   m0 |   tau_m |   gain_h |    D | include_Tlog   |
|:-----------|---------:|--------:|------:|-----:|--------:|---------:|-----:|:---------------|
| full       | 0.439844 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_linear  | 0.985186 |  0      | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_cubic   | 0.439844 | 18.7061 | 0     |    0 |  49.999 |        0 | 0.02 | False          |
| no_memory  | 0.439844 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_field   | 0.439844 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_noise   | 0.440108 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0    | False          |
| with_Tlog  | 1.20432  | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | True           |

## Cellule 7 : Analyse des termes et résidus

- MSE (full) : 0.439844
- Deltas de MSE vs full par scénario :
| scenario   | terme                   |      MSE |   delta_MSE_vs_full |
|:-----------|:------------------------|---------:|--------------------:|
| full       | Référence (full)        | 0.439844 |         0           |
| no_linear  | Terme linéaire -a(T*)Φ* | 0.985186 |         0.545342    |
| no_cubic   | Terme cubique -bΦ*³     | 0.439844 |        -1.51218e-11 |
| no_memory  | Mémoire ∫MΦ             | 0.439844 |         0           |
| no_field   | Champ externe h(τ)      | 0.439844 |         0           |
| no_noise   | Bruit √(2D*)ξ(τ)        | 0.440108 |         0.000263775 |
| with_Tlog  | T_log explicite         | 1.20432  |         0.764473    |

- Fichiers générés :
  - data/metrics_analysis.csv
  - images/residuals_full.png
  - images/residuals_hist.png
  - images/delta_mse_terms.png

## Cellule 8 : Reproductibilité et synthèse finale

- Seed fixé : 42
- Versions :
  - python: 3.12.12 (main, Oct 10 2025, 08:52:57) [GCC 11.4.0]
  - platform: Linux-6.6.105+-x86_64-with-glibc2.35
  - numpy: 2.0.2
  - pandas: 2.2.2
  - matplotlib: 3.10.0
  - scipy: 1.16.2
  - sklearn: 1.6.1

- Paramètres fittés :
|     a_T |     b |   m0 |   tau_m |   gain_h |      mse |   nit | success   | message                                          |
|--------:|------:|-----:|--------:|---------:|---------:|------:|:----------|:-------------------------------------------------|
| 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.440108 |     5 | True      | CONVERGENCE: NORM OF PROJECTED GRADIENT <= PGTOL |

- Résultats d’ablation (ΔMSE vs full) :
| scenario   |      MSE |   delta_MSE_vs_full |
|:-----------|---------:|--------------------:|
| full       | 0.439844 |         0           |
| no_linear  | 0.985186 |         0.545342    |
| no_cubic   | 0.439844 |        -1.51218e-11 |
| no_memory  | 0.439844 |         0           |
| no_field   | 0.439844 |         0           |
| no_noise   | 0.440108 |         0.000263775 |
| with_Tlog  | 1.20432  |         0.764473    |

- Manifeste complet sauvegardé : logs/manifest.json

### Check‑list cross‑domaines
1. Fournir un dataset réel (Φ*, τ) adapté au domaine.
2. Relancer Cellule 2 pour charger et normaliser.
3. Ajuster dictionnaire de paramètres (Cellule 3).
4. Re‑discrétiser (Cellule 4).
5. Fitter (Cellule 5).
6. Simuler et ablater (Cellule 6).
7. Analyser (Cellule 7).
8. Sauvegarder manifeste (Cellule 8).

## Cellule 1 : Setup
- Logger niveau DEBUG configuré (logs/simulation_log.txt)
- Logs structurés (logs/logs.csv)
- Résumé lisible (logs/summary.md)

## Cellule 2 : Chargement et prétraitement des données DVF 2024

- Archive téléchargée : data/valeurs_foncieres_2024.zip
- Extraite dans : data/valeurs_foncieres_2024
- Fichier principal : data/valeurs_foncieres_2024/ValeursFoncieres-2024.txt
- Copie brute sauvegardée : data_copies/raw_data.csv
- Colonnes clés disponibles : ['date_mutation', 'valeur_fonciere', 'code_departement', 'surface_reelle_bati']
- Aperçu :

|    | date_mutation       |   valeur_fonciere |   code_departement |   surface_reelle_bati |
|---:|:--------------------|------------------:|-------------------:|----------------------:|
|  0 | 2024-02-01 00:00:00 |             346.5 |                 01 |                   nan |
|  1 | 2024-03-01 00:00:00 |           10000   |                 01 |                   nan |
|  2 | 2024-08-01 00:00:00 |          249000   |                 01 |                   nan |
|  3 | 2024-03-01 00:00:00 |          329500   |                 01 |                     0 |
|  4 | 2024-03-01 00:00:00 |          329500   |                 01 |                     0 |

## Cellule 3 : Paramètres universels

- Paramètres initiaux : {
  "kappa": 0.1,
  "alpha": 1.0,
  "b": 0.01,
  "D": 0.05,
  "T_c": 1.0,
  "delta_T": 1.0,
  "n": 1000,
  "d": 6,
  "T_log": 13.815510557964274,
  "T_star": 12.815510557964274,
  "a_T": 12.815510557964274
}

## Cellule 4 : Discrétisation et grilles

- Nt = 1000
- Normalisation robuste : median=105531.750, mad=9475.000, scale=14047.635
- Grilles : data/grids.npy, data/grids.csv
- Noyau mémoire : data/memory_kernel.csv (m0=0.1, tau_m=50.0)
- Figure : images/phi_on_grid.png

## Cellule 5 : Fitting des paramètres

- Méthode d’optimisation : L-BFGS-B
- Paramètres fittés : |     a_T |     b |   m0 |   tau_m |   gain_h |      mse |   nit | success   | message                                          |
|--------:|------:|-----:|--------:|---------:|---------:|------:|:----------|:-------------------------------------------------|
| 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.440108 |     5 | True      | CONVERGENCE: NORM OF PROJECTED GRADIENT <= PGTOL |

- Fichiers :
  - models/fitted_params.csv
  - copiemodels/fitted_params.npy
  - data/simulation.csv
  - images/fit_phi_vs_data.png

## Cellule 6 : Simulation complète et ablation

- Fichier ablation : data/simulation_ablation.csv
- Fichier métriques : data/metrics.csv
- Figures : images/sim_full_vs_data.png, images/ablation_compare.png, images/ablation_mse_bar.png
- Paramètres fittés (réutilisés) :
|     a_T |     b |   m0 |   tau_m |   gain_h |      mse |   nit | success   | message                                          |
|--------:|------:|-----:|--------:|---------:|---------:|------:|:----------|:-------------------------------------------------|
| 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.440108 |     5 | True      | CONVERGENCE: NORM OF PROJECTED GRADIENT <= PGTOL |

- Metrics:
| scenario   |      MSE |     a_T |     b |   m0 |   tau_m |   gain_h |    D | include_Tlog   |
|:-----------|---------:|--------:|------:|-----:|--------:|---------:|-----:|:---------------|
| full       | 0.439844 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_linear  | 0.985186 |  0      | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_cubic   | 0.439844 | 18.7061 | 0     |    0 |  49.999 |        0 | 0.02 | False          |
| no_memory  | 0.439844 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_field   | 0.439844 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | False          |
| no_noise   | 0.440108 | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0    | False          |
| with_Tlog  | 1.20432  | 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.02 | True           |

## Cellule 7 : Analyse des termes et résidus

- MSE (full) : 0.439844
- Deltas de MSE vs full par scénario :
| scenario   | terme                   |      MSE |   delta_MSE_vs_full |
|:-----------|:------------------------|---------:|--------------------:|
| full       | Référence (full)        | 0.439844 |         0           |
| no_linear  | Terme linéaire -a(T*)Φ* | 0.985186 |         0.545342    |
| no_cubic   | Terme cubique -bΦ*³     | 0.439844 |        -1.51218e-11 |
| no_memory  | Mémoire ∫MΦ             | 0.439844 |         0           |
| no_field   | Champ externe h(τ)      | 0.439844 |         0           |
| no_noise   | Bruit √(2D*)ξ(τ)        | 0.440108 |         0.000263775 |
| with_Tlog  | T_log explicite         | 1.20432  |         0.764473    |

- Fichiers générés :
  - data/metrics_analysis.csv
  - images/residuals_full.png
  - images/residuals_hist.png
  - images/delta_mse_terms.png

## Cellule 8 : Reproductibilité et synthèse finale

- Seed fixé : 42
- Versions :
  - python: 3.12.12 (main, Oct 10 2025, 08:52:57) [GCC 11.4.0]
  - platform: Linux-6.6.105+-x86_64-with-glibc2.35
  - numpy: 2.0.2
  - pandas: 2.2.2
  - matplotlib: 3.10.0
  - scipy: 1.16.2
  - sklearn: 1.6.1

- Paramètres fittés :
|     a_T |     b |   m0 |   tau_m |   gain_h |      mse |   nit | success   | message                                          |
|--------:|------:|-----:|--------:|---------:|---------:|------:|:----------|:-------------------------------------------------|
| 18.7061 | 1e-06 |    0 |  49.999 |        0 | 0.440108 |     5 | True      | CONVERGENCE: NORM OF PROJECTED GRADIENT <= PGTOL |

- Résultats d’ablation (ΔMSE vs full) :
| scenario   |      MSE |   delta_MSE_vs_full |
|:-----------|---------:|--------------------:|
| full       | 0.439844 |         0           |
| no_linear  | 0.985186 |         0.545342    |
| no_cubic   | 0.439844 |        -1.51218e-11 |
| no_memory  | 0.439844 |         0           |
| no_field   | 0.439844 |         0           |
| no_noise   | 0.440108 |         0.000263775 |
| with_Tlog  | 1.20432  |         0.764473    |

- Manifeste complet sauvegardé : logs/manifest.json

### Check‑list cross‑domaines
1. Fournir un dataset réel (Φ*, τ) adapté au domaine.
2. Relancer Cellule 2 pour charger et normaliser.
3. Ajuster dictionnaire de paramètres (Cellule 3).
4. Re‑discrétiser (Cellule 4).
5. Fitter (Cellule 5).
6. Simuler et ablater (Cellule 6).
7. Analyser (Cellule 7).
8. Sauvegarder manifeste (Cellule 8).

