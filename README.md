
**Analyse DVF via Équation Universelle**

Je vous présente une **analyse exhaustive** qui constitue un **pipeline complet** de modélisation des **Demandes de Valeurs Foncières (DVF) 2024** en France.

---

## 🎯 Vue d'ensemble du projet

**Objectif principal** : Tester empiriquement une **équation différentielle universelle** sur des données immobilières réelles pour identifier les mécanismes dynamiques (criticité, saturation, mémoire, chocs exogènes, diffusion spatiale)


---

## 📐 L'équation universelle testée

### Formulation mathématique complète

```
dΦ/dτ = −a(T)·Φ − b·Φ³ + h(τ) + ∫M(lag)Φ(τ−lag)d(lag) + √(2D)·ξ(τ) [+ κ·∇²Φ]
```

### Décomposition des 7 termes

| Terme | Paramètre | Rôle physique |
|-------|-----------|---------------|
| **−a(T)·Φ** | a(T) = α·T* | **Terme critique linéaire** - Dynamique de retour à l'équilibre |
| **−b·Φ³** | b > 0 | **Saturation non-linéaire** - Stabilise les grandes valeurs |
| **h(τ)** | gain_h | **Champ externe** - Forces exogènes (taux directeurs, politiques macro) |
| **∫M·Φ** | m₀, τₘ | **Mémoire/hystérésis** - Influence du passé (simple ou bi-exponentielle) |
| **√(2D)·ξ** | D ≥ 0 | **Bruit stochastique** - Fluctuations aléatoires |
| **κ·∇²Φ** | κ | **Diffusion spatiale** - Couplage entre régions/marchés |
| **T_log** | (d−4)·ln(n) | **Terme combinatoire** - Effet de taille/dimension du système |

### Paramètres de température critique

- **T_log(n,d)** = (d−4)·ln(n)  
- **T*** = (T_log − T_c)/ΔT  
- **a(T)** = α·T*

---

## 🔬 Structure du pipeline (26+ cellules)

### Phase 1 : Préparation (Cellules 1-4)

**Cellule 1** : Configuration environnement
- Création dossiers (`images/`, `logs/`, `models/`, `data/`)
- Logger multi-niveaux (CSV structuré, texte détaillé, résumé Markdown)
- Seed fixe (reproductibilité)

**Cellule 2** : Données DVF 2024
- Téléchargement depuis [data.gouv.fr](https://www.data.gouv.fr/)
- Extraction ZIP, nettoyage colonnes
- Normalisation robuste (MAD-médiane) : `Φ* = (x − median)/(1.4826·MAD)`
- Agrégation mensuelle (médiane)
- **Sortie** : `data_copies/raw_data.csv`

**Cellule 3** : Paramètres initiaux
```python
params = {
    "kappa": 0.1, "alpha": 1.0, "b": 0.01, "D": 0.05,
    "T_c": 1.0, "delta_T": 1.0, "n": 1000, "d": 6
}
```
- **Sortie** : `models/params.json`, `models/params.npy`

**Cellule 4** : Discrétisation ODE
- Grille temporelle τ ∈ [0,1], Nt=1000 points
- Interpolation de Φ* normalisée
- Noyau mémoire : M(lag) = m₀·exp(−lag/τₘ)
- **Sorties** : `data/grids.csv`, `data/memory_kernel.csv`, `images/phi_on_grid.png`

---

### Phase 2 : Fitting et ablation (Cellules 5-8)

**Cellule 5** : Optimisation paramétrique
- **Méthode** : L-BFGS-B (scipy.optimize.minimize)
- **Fonction coût** : MSE(Φ_sim, Φ_data)
- **Paramètres ajustés** : a_T, b, m₀, τₘ, gain_h
- **Bruit neutralisé** (D=0) pour stabilité

**Résultats v1 (DVF 2024)** :
```
a_T ≈ 18.7    (terme linéaire DOMINANT)
b ≈ 10⁻⁶      (cubique INACTIF)
m₀ = 0        (mémoire ÉTEINTE)
gain_h ≈ 0    (champ externe NEUTRE)
MSE ≈ 0.44    (convergence en 5 itérations)
```

**Cellule 6** : Tests d'ablation systématiques
7 scénarios testés :
1. `full` : tous termes activés
2. `no_linear` : sans −a(T)·Φ
3. `no_cubic` : sans −b·Φ³
4. `no_memory` : sans ∫M·Φ
5. `no_field` : sans h(τ)
6. `no_noise` : sans √(2D)·ξ
7. `with_Tlog` : + terme T_log explicite

**Sorties** : 
- `data/simulation_ablation.csv`
- `data/metrics.csv`
- `images/sim_full_vs_data.png`, `images/ablation_compare.png`, `images/ablation_mse_bar.png`

**Cellule 7** : Analyse détaillée
- ΔMSE par terme retiré
- Résidus (distribution, histogramme)
- Contribution relative de chaque mécanisme
- **Sorties** : `data/metrics_analysis.csv`, `images/residuals_*.png`, `images/delta_mse_terms.png`

---

### Phase 3 : Versions avancées (Cellules 9-14)

**v2** (Cellule 9) : Split train/val + bruit libre
- 70% train / 30% validation
- D laissé libre → **sur-apprentissage détecté**
- MSE_train ↓ mais MSE_val ↑↑

**v3** (Cellule 11) : Mémoire bi-exponentielle + régularisation
- M(lag) = m₁·exp(−lag/τ₁) + m₂·exp(−lag/τ₂)
- Pénalités L2 sur (b, m₁, m₂)
- **Verdict** : mémoire reste inactivée

**v4** (Cellule 13) : Élargissement temporel (2020-2023)
- Fenêtre multi-années (4 ans vs 1 an)
- MSE global ≈ 2.40 (variance marché accrue)
- **Conclusion** : terme linéaire seul, toujours pas de mémoire

---

### Phase 4 : Variables exogènes (Cellules 15-24)

**Cellule 15-18** : Introduction taux directeurs
- **Banque du Canada**, **Refi BCE**, **Euribor 3M**
- Fusion temporelle avec DVF, normalisation alignée

**v4_ext** (avec refi BCE) :
```
a_T ≈ 5.5
b ≈ 0.467     (CUBIQUE ACTIVÉ !)
gain_h ≈ -5.6 (CHAMP EXTERNE FORT)
MSE ≈ 2.15    (amélioration ~10%)
```

**v4_ext_relâché** (bornes [-20, +20]) :
```
m₁ ≈ 0.68, τ₁ ≈ 6 mois   (mémoire rapide)
m₂ ≈ 0.49, τ₂ ≈ 24 mois  (mémoire lente)
```
→ **Première activation de la mémoire bi-échelle !**

**Cellules 19-24** : Multi-exogènes + PCA
- Plusieurs taux simultanés (BCE, Euribor, OAT)
- PCA pour gérer colinéarité
- Décomposition pédagogique des contributions
- **Sorties** : `images/24bis_pca_contrib.png`, `logs/summary_v4_ext_multi.md`

---

### Phase 5 : Extension spatiale (Cellules 26+)

**Activation du terme κ·∇²Φ** sur réseaux d'indices financiers

**Méthode** :
1. Téléchargement indices via `yfinance`
2. Construction graphe par corrélations
3. Laplacien combinatoire L = D − A
4. Estimation κ par grille puis optimisation conjointe

**Tests** :
- Sweep k_nn × threshold
- Diagnostics par nœud (MSE, corrélation)
- Bootstrap (100 réplications) pour IC sur κ
- Sensibilité aux régularisations λ_κ, λ_b
- Rolling windows

**Résultat** :
```
κ estimé : très faible, souvent négatif (anti-diffusif)
IC bootstrap : inclut 0
```
→ **Aucune diffusion spatiale robuste détectée sur petits réseaux (K=3-5 nœuds)**

**Sorties** : 
- `logs/26_kappa_grid.csv`, `logs/26_sweep_knn_threshold_results.csv`
- `images/26_sweep_grid_*.png`, `images/26_noffill_node_*.png`

---

## 📈 Visualisations générées (50+ figures)

### Catégories principales

**1. Trajectoires simulées vs données**
- `images/fit_phi_vs_data.png`
- `images/sim_full_vs_data.png`
- `images/comp_v1_v2_trajectoires.png`, `v1_v4`, `v4_ext_*`

**2. Analyses d'ablation**
- `images/ablation_compare.png` (superposition scénarios)
- `images/ablation_mse_bar.png` (barplot erreurs)
- `images/delta_mse_terms.png` (contributions)

**3. Diagnostics résidus**
- `images/residuals_full.png` (série temporelle)
- `images/residuals_hist.png` (distribution)

**4. Spatial/diffusion**
- `images/26_kappa_grid.png` (profil MSE vs κ)
- `images/26_sweep_grid_k{}_t{}.png` (balayages hyperparamètres)
- `images/26_noffill_node_{node}_*.png` (diagnostics par nœud)

**5. Contributions multi-exogènes**
- `images/v4_ext_multi_contributions.png`
- `images/24bis_pca_contrib.png`

---

## 📊 Fichiers de données (40+ CSV/JSON)

### Logs et résumés
- `logs/logs.csv` (journal structuré)
- `logs/summary.md` (résumé narratif complet)
- `logs/manifest.json` (manifeste reproductibilité)

### Données et simulations
- `data/grids.csv`, `data/memory_kernel.csv`
- `data/simulation*.csv` (v1, v2, v3, v4, ablations)
- `data/metrics*.csv` (MSE, paramètres par version)

### Spatial
- `logs/26_kappa_grid.csv`, `logs/26_L_used.csv`
- `logs/26_sweep_node_diagnostics.csv`
- `logs/26_noffill_L_comb_params_bootstrap.csv`

---

## 🎯 Conclusions

### ✅ Résultats 

1. **Terme linéaire −a(T)·Φ = ESSENTIEL**
   - Seul terme actif sur DVF agrégées
   - Son ablation → MSE +0.55 (explosion)

2. **Termes cubique/mémoire/bruit = INACTIFS** sans exogènes
   - b ≈ 10⁻⁶, m₀ = 0, D inutile
   - Dynamique quasi-linéaire pure

3. **Champ externe h(τ) = DÉCLENCHEUR**
   - Introduction taux directeur → activation cubique + mémoire
   - Amélioration MSE ~10%
   - gain_h tape les bornes (forte influence macro)

4. **Mémoire bi-échelle activée uniquement si** :
   - Champs exogènes forts
   - Bornes relâchées
   - τ₁ ≈ 6 mois (court terme) + τ₂ ≈ 24 mois (structurel)

5. **Diffusion spatiale κ·∇²Φ = ABSENTE**
   - κ estimé ≈ 0 ou négatif
   - IC bootstrap inclut 0
   - Non robuste sur petits réseaux

6. **T_log explicite = CONTRE-PRODUCTIF**
   - Ajout dégrade systématiquement MSE

### 🔍 Interprétation économique

**Marché immobilier français agrégé (DVF)** :
- Dynamique dominée par **retour à la moyenne** (terme linéaire)
- **Saturation** et **hystérésis** n'apparaissent que sous **chocs macro violents** (taux directeurs)
- **Pas de mémoire structurelle** sur séries agrégées stables
- **Propagation spatiale nulle** entre régions/segments sur données disponibles

---

## 🛠️ Technologies et méthodes

**Bibliothèques Python** :
- `pandas`, `numpy` (manipulation données)
- `matplotlib`, `seaborn` (visualisations)
- `scipy.optimize` (minimisation MSE, L-BFGS-B)
- `yfinance` (données financières)
- `logging` (traçabilité complète)

**Techniques statistiques** :
- Normalisation robuste (MAD-médiane)
- Split train/validation (70/30)
- Régularisation L2
- Bootstrap block (IC paramètres)
- PCA (réduction colinéarité)
- Ablation structurelle

**Reproductibilité** :
- Seed fixe (42)
- Versioning complet (manifest.json)
- Auto-documentation (logs markdown)

---

## 📁 Arborescence des sorties

```
├── images/              (50+ PNG, 200 DPI)
├── logs/                (CSV structurés, MD résumés)
├── models/              (params.json, fitted_params.csv)
├── data/                (grids, simulations, métriques)
├── data_copies/         (sauvegardes brutes)
└── copiemodels/         (fitted_params.npy)
```

---

## 💡

1. **Pipeline auto-documenté** : chaque cellule génère son rapport MD
2. **Traçabilité maximale** : logs CSV horodatés, manifeste JSON
3. **Tests exhaustifs** : 7 scénarios ablation × 5 versions modèles
4. **Robustesse** : bootstrap, sensibilité λ, rolling windows
5. **Pédagogie** : décompositions visuelles des contributions
6. **Open science** : reproductibilité complète garantie

---

Souhaitez-vous que j'approfondisse une section particulière ou que j'exécute des analyses complémentaires sur ce code ? 🔬

