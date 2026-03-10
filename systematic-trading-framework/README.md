# Systematic Trading Framework

Backtesting et execution pour strategies de trading systematiques multi-actifs.

## Objectif

Framework complet pour developper, valider et executer des strategies de trading a court terme. Le systeme couvre l'ensemble du cycle de vie d'une strategie : generation de signaux, validation statistique rigoureuse, gestion du risque, et execution automatisee en production.

L'enjeu principal n'est pas de trouver des signaux — c'est de s'assurer qu'ils tiennent hors echantillon. La majorite du framework est consacree a la validation et au filtrage des faux positifs.

## Walk-Forward Optimization

Le coeur du systeme est un moteur de validation Walk-Forward (WFO), concu pour eviter l'overfitting :

### Principe
Le WFO decoupe l'historique en fenetres successives (train / test). Chaque fenetre d'entrainement produit des parametres qui sont immediatement testes sur la fenetre suivante, jamais vue pendant l'optimisation. Le processus se repete sur tout l'historique.

### Configuration
| Parametre | Description |
|-----------|-------------|
| Mode | Rolling (fenetre fixe) ou Expanding (fenetre croissante) |
| Train / Test split | Ratio configurable par strategie |
| Boundary timezone | Alignement sur les sessions de marche (eviter les biais de timezone) |

### Metriques OOS
La performance est evaluee exclusivement sur les fenetres hors echantillon :

| Metrique | Usage |
|----------|-------|
| Sharpe Ratio | Performance ajustee au risque |
| Sortino Ratio | Penalisation asymetrique (downside seul) |
| Calmar Ratio | Performance / drawdown maximum |
| Profit Factor | Gains bruts / pertes brutes |
| OOS Ratio | % de performance conservee entre train et test |
| Stabilite inter-folds | Variance des parametres entre les fenetres |

Le OOS Ratio est la metrique la plus surveillee. Un signal qui perd plus de 50% de sa performance hors echantillon est ecarte, quelle que soit sa performance absolue.

## Gestion du risque

### Correlation dynamique
- Calcul continu de la matrice de correlation entre actifs
- Regroupement en clusters d'actifs correles
- Limite d'exposition par cluster (eviter la concentration involontaire sur un meme mouvement de marche)

### Position sizing
- Taille de position adaptative basee sur la volatilite recente de chaque actif
- Coefficients de volume par actif (certains actifs sont naturellement plus volatils)
- Multiplicateur global configurable

### Limites de risque
- Drawdown intraday : arret automatique si un seuil est atteint en seance
- Drawdown cumule : reduction progressive de l'exposition
- Nombre maximum de positions simultanees

## Classes d'actifs

| Classe | Perimetre | Timeframes |
|--------|-----------|------------|
| **Forex** | Paires majeures et mineures (27+) | H1, H4, D1 |
| **Indices** | US, Europe, Asie (10+) | H1, H4, D1 |
| **Crypto** | BTC, ETH | H4, D1 |

Le systeme gere les specificites de chaque classe : sessions de marche, spread variable, swap overnight, jours feries, rollover des contrats.

## Execution

- Connexion directe MetaTrader 5
- Signaux automatises a chaque cloture de bougie
- Gestion des ordres multi-actifs en parallele
- Hard close programme (cloture forcee en fin de session si necessaire)
- Monitoring temps reel des positions ouvertes

## Architecture

```
Signal Generation → WFO Validation → Risk Filter → Position Sizing → Execution → Monitoring
```

Chaque module est independant. On peut remplacer le generateur de signaux sans toucher a la validation, ou changer le broker sans modifier la logique de trading.

## Stack

- **Python 3.11+**
- **MetaTrader 5** — Connexion broker et execution
- **pandas / numpy / scipy** — Calculs et statistiques
- **Matplotlib** — Reporting et visualisation

---

Developpe par Jean-Jacques F. — [LinkedIn](https://linkedin.com/in/jjforier)
