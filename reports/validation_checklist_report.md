# Checklist de validation — Fit, κ rolling, Bootstrap
_Généré le_: 2025-10-15 21:47

## Configuration exécutée
- Fichier phi: **phi_matrix_prepared_resampled.csv**; T=1824, K=3
- Split OOS: last **12** months
- Rolling κ window: **24** months; κ grid: -1.500..1.500
- Bootstrap: n_boot=500; block_size=6

## Résultats OOS
- Fit trouvé: **False**
- Aucun fit convergent trouvé sur la fenêtre d'entraînement (voir logs/validation_oos_sweep.csv).

## Kappa rolling
- Kappa rolling non produit (erreur ou fichier manquant).

## Bootstrap final (résumé)
- Source bootstrap: **existing_26_final**
- **n_draws**: 500
- **a_median**: 0.2321717653716287
- **a_std**: 0.46891042700730456
- **b_median**: 1.3281604644039424e-06
- **b_std**: 0.0081401979173907
- **m_median**: 0.0031316739013805
- **m_std**: 0.12050585562905072
- **kappa_median**: -0.0005775641256167
- **kappa_std**: 0.031078602496268003

## Fichiers produits / à vérifier
- reports/oos_results.json
- reports/bootstrap_summary.json
- logs/validation_oos_sweep.csv
- logs/validation_bootstrap_params.csv (ou logs/26_final_bootstrap_params.csv si existant)
- logs/validation_kappa_rolling.csv (si créé)
- images/validation_obs_vs_pred.png (si créé)
- images/validation_kappa_rolling.png (si créé)

## Interprétation synthétique (FR)
- Si MSE_val ≫ MSE_train, cela signale un risque de sur‑ajustement; inspecter résidus et politique de régularisation.
- Si κ rolling varie fortement, documenter κ(τ) et tester corrélation avec chocs externes.
- Si bootstrap montre distribution large pour a ou m, rapporter médianes et IC dans le rapport final.

## Short interpretation (EN)
- Check train vs val MSE for overfitting; inspect residuals and regularization choices.
- Large variability in rolling κ suggests time-varying spatial coupling; test correlation with external shocks.
- Wide bootstrap spreads imply parameter uncertainty; report medians and confidence intervals.
