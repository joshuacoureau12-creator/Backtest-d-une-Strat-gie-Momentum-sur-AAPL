# 📈 Backtest d'une Stratégie Momentum sur AAPL

Analyse quantitative complète d'une stratégie de trading **Momentum** appliquée à l'action Apple (AAPL) sur la période 2018–2025, incluant le backtest, l'évaluation de la performance et la mesure du risque.

---

## 🎯 Objectif

Implémenter et évaluer une stratégie Momentum simple :
> *"Si l'action a progressé sur les 120 derniers jours de trading, on achète. Sinon, on reste en dehors du marché."*

La performance de la stratégie est ensuite comparée à un **Buy & Hold** passif, et soumise à une analyse de risque rigoureuse (VaR, Expected Shortfall, Drawdown).

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
jupyter notebook "Backtest_d_une_stratégie_Momentum_sur_les_actions_du_S_P_500.ipynb"
```

---

## 📁 Structure du Projet

```
.
├── Backtest_d_une_stratégie_Momentum_sur_les_actions_du_S_P_500.ipynb
└── README.md
```

---

## 📚 Concepts Clés

**Momentum** — Effet bien documenté en finance empirique (Jegadeesh & Titman, 1993) : les actifs ayant bien performé récemment tendent à continuer à surperformer à court/moyen terme.

**Look-ahead bias** — Biais à éviter impérativement en backtesting : le signal est décalé d'un jour (`shift(1)`) pour simuler une décision prise *après* la clôture, exécutée à la clôture suivante.

**Expected Shortfall (CVaR)** — Mesure de risque cohérente au sens d'Artzner et al., adoptée progressivement par la réglementation **Bâle IV** en remplacement de la VaR classique.

---

## ⚠️ Limites & Pistes d'Amélioration

- Stratégie appliquée sur un **seul actif** (AAPL) — étendre à un portefeuille S&P 500
- Pas de prise en compte des **coûts de transaction** ni du **slippage**
- Paramètre de fenêtre Momentum (120 jours) non optimisé — envisager une **recherche sur grille**
- Comparer avec d'autres stratégies (Mean-Reversion, Moving Average Crossover)

---

*Projet réalisé dans le cadre d'une transition vers la finance quantitative (risk management / model validation).*
