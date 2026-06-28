# 📈 Backtest d'une Stratégie Momentum sur AAPL

Analyse quantitative complète d'une stratégie de trading **Momentum** appliquée à l'action Apple (AAPL) sur la période 2018–2025, incluant le backtest, l'évaluation de la performance et la mesure du risque.

---

## 🎯 Objectif

Implémenter et évaluer une stratégie Momentum simple :
> *"Si l'action a progressé sur les 120 derniers jours de trading, on achète. Sinon, on reste en dehors du marché."*

La performance de la stratégie est ensuite comparée à un **Buy & Hold** passif, et soumise à une analyse de risque rigoureuse (VaR, Expected Shortfall, Drawdown, Sortino, Calmar).

> **Résultat principal** : sur la période 2018–2025, la stratégie Momentum sous-performe le Buy & Hold sur AAPL — y compris en risque-ajusté et y compris pendant le bear market 2022. Voir la section *Stratégie vs Buy & Hold* ci-dessous pour le détail et l'analyse par régime de marché.

---

## 🧠 Logique de la Stratégie

| Étape | Description |
|-------|-------------|
| **Signal** | Rendement sur 120 jours glissants (`pct_change(120)`) |
| **Position** | Long si signal > 0, neutre sinon |
| **Exécution** | Signal décalé d'un jour (`shift(1)`) pour éviter le look-ahead bias |
| **Benchmark** | Rendement cumulé Buy & Hold (AAPL) |

---

## 📊 Métriques de Performance

| Indicateur | Description |
|------------|-------------|
| **Rendement annualisé** | Performance moyenne annuelle de la stratégie |
| **Volatilité annualisée** | Écart-type des rendements × √252 |
| **Sharpe Ratio (annualisé)** | Rendement par unité de risque |
| **Max Drawdown** | Pire perte depuis un sommet sur toute la période |
| **VaR historique (95%)** | Perte journalière maximale 19 jours sur 20 |
| **VaR paramétrique (95%)** | VaR sous hypothèse de normalité |
| **Expected Shortfall (95%)** | Perte moyenne dans les 5% de pires jours |
| **Sortino Ratio** | Rendement par unité de risque, en ne pénalisant que la volatilité négative |
| **Calmar Ratio** | Rendement annualisé rapporté au Max Drawdown |

---

## ⚖️ Stratégie vs Buy & Hold : résultats risque-ajustés

La stratégie a été comparée au Buy & Hold sur l'ensemble des métriques risque-ajustées, et pas seulement sur le rendement cumulé brut. Résultat sur 2018–2025 :

| Stratégie | Rendement annualisé | Volatilité annualisée | Sharpe | Sortino | Max Drawdown | Calmar |
|---|---|---|---|---|---|---|
| **Buy & Hold** | 35.98% | 30.55% | 1.178 | 1.635 | -38.52% | 0.934 |
| **Momentum 120j** | 15.11% | 25.84% | 0.585 | 0.671 | -44.00% | 0.343 |

**Constat important : la stratégie sous-performe le Buy & Hold sur toutes les métriques, y compris risque-ajustées.** Elle a une volatilité plus faible, mais un Max Drawdown *plus élevé* et un Sharpe/Sortino/Calmar nettement inférieurs. L'hypothèse initiale "le momentum protège le capital en phase de stress, même s'il rapporte moins" n'est donc pas validée ici.

### Analyse par régime de marché

| Période | Buy & Hold (cumulé) | Momentum (cumulé) | Écart |
|---|---|---|---|
| Bull 2018-2019 (hors Q4 2018) | 32.55% | 24.35% | -8.20% |
| Correction Q4 2018 | -30.55% | -23.22% | +7.33% |
| Bull 2019-2021 (incl. COVID crash) | 365.49% | 219.17% | -146.32% |
| **Bear 2022** | **-26.40%** | **-38.54%** | **-12.13%** |
| Reprise 2023-2024 | 94.76% | 13.40% | -81.36% |

Seule la correction de fin 2018 montre une protection effective du momentum. Sur le bear market 2022 — le test le plus pertinent pour une stratégie défensive — la stratégie fait **pire** que le Buy & Hold (-38.54% vs -26.40%), probablement à cause de faux signaux de ré-entrée pendant une baisse en dents de scie. Sur les phases de forte tendance haussière (2019-2021, 2023-2024), rester hors marché pendant les phases de consolidation coûte l'essentiel de la sous-performance globale.

### Sensibilité au paramètre de fenêtre

| Fenêtre (jours) | Rendement annualisé | Sharpe |
|---|---|---|
| 40 | 19.12% | 0.918 |
| 60 | 21.12% | 0.934 |
| 90 | 18.26% | 0.729 |
| 120 | 15.11% | 0.585 |
| 150 | 22.34% | 0.864 |
| 200 | 17.69% | 0.685 |
| 252 | 21.59% | 0.816 |

Le choix de 120 jours est en fait l'un des **moins bons** points de la grille testée (Sharpe le plus faible). D'autres fenêtres (60 ou 150 jours) font sensiblement mieux, mais **aucune ne dépasse le Sharpe du Buy & Hold (1.178)**. La sous-performance n'est donc pas un simple problème de paramétrage : elle est structurelle sur un actif individuel en tendance haussière prononcée comme AAPL sur cette période.

---

## 📉 Visualisations

- **Rendement cumulé** — Stratégie Momentum vs Buy & Hold
- **Courbe de Drawdown** — Amplitude et durée des pertes depuis les sommets
- **Distribution des rendements** — Histogramme + loi normale ajustée avec seuils VaR et ES

---

## 🛠️ Stack Technique

```
Python 3.10
├── yfinance      — Collecte des données de marché
├── pandas        — Manipulation des séries temporelles
├── numpy         — Calculs numériques (VaR paramétrique, annualisation)
├── scipy.stats   — Loi normale, percentiles
├── matplotlib    — Visualisations
└── IPython       — Affichage HTML du tableau de synthèse
```

---

## 🚀 Lancer le Notebook

```bash
# 1. Cloner le dépôt
git clone https://github.com/<votre-username>/<votre-repo>.git
cd <votre-repo>

# 2. Installer les dépendances
pip install yfinance pandas numpy scipy matplotlib

# 3. Ouvrir le notebook
jupyter notebook "momentum_aapl_fixed.ipynb"
```

---

## 📁 Structure du Projet

```
.
├── momentum_aapl_fixed.ipynb
└── README.md
```

---

## 📚 Concepts Clés

**Momentum** — Effet bien documenté en finance empirique (Jegadeesh & Titman, 1993) : les actifs ayant bien performé récemment tendent à continuer à surperformer à court/moyen terme.

**Look-ahead bias** — Biais à éviter impérativement en backtesting : le signal est décalé d'un jour (`shift(1)`) pour simuler une décision prise *après* la clôture, exécutée à la clôture suivante.

**Expected Shortfall (CVaR)** — Mesure de risque cohérente au sens d'Artzner et al., adoptée progressivement par la réglementation **Bâle IV** en remplacement de la VaR classique.

---

## ⚠️ Limites & Pistes d'Amélioration

- **La sous-performance est confirmée structurelle, pas un artefact de paramétrage** : testée sur 7 fenêtres (40 à 252j), aucune ne dépasse le Sharpe du Buy & Hold
- Le signal momentum simple (rendement brut > 0 sur N jours) génère trop de faux signaux en marché baissier en dents de scie (cf. Bear 2022) — un filtre de tendance (ex. moyenne mobile longue, ADX) ou un seuil minimal de signal pourrait réduire le whipsaw
- Stratégie appliquée sur un **seul actif** (AAPL) — étendre à un portefeuille S&P 500 pour vérifier si le résultat se généralise ou s'il est spécifique à un titre très haussier sur cette période
- Pas de prise en compte des **coûts de transaction** ni du **slippage** — à intégrer, même si cela ne ferait qu'accentuer la sous-performance relative
- Comparer avec d'autres stratégies (Mean-Reversion, Moving Average Crossover) pour situer le momentum simple par rapport à des alternatives
- Décomposition par régime de marché actuellement bornée manuellement (dates en dur) — passer à une détection automatique des régimes (ex. via volatilité ou drawdown glissant) serait plus robuste

---
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/joshuacoureau12-creator/Backtest-d-une-Strat-gie-Momentum-sur-AAPL/blob/main/Backtest%20d'une%20strat%C3%A9gie%20Momentum%20sur%20les%20actions%20du%20S%26P%20500.ipynb)
---

*Projet réalisé dans le cadre d'une transition vers la finance quantitative (risk management / model validation).*
