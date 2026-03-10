# Multi-Factor Stock Screener

Selection d'actions multi-factorielle sur un univers international de 3 000+ titres, 30+ bourses.

## Objectif

Systeme de scoring quantitatif qui classe l'ensemble des actions d'un univers international selon plusieurs facteurs de performance documentes par la recherche academique. Le systeme genere des signaux d'investissement a frequence mensuelle et construit un portefeuille sous contraintes de capital reel.

Ce n'est pas un screener discretionnaire. C'est un pipeline complet, de la collecte de donnees a l'envoi d'ordres chez le broker.

## Facteurs

Le scoring repose sur une combinaison ponderee de facteurs issus de la litterature academique :

| Famille | Fondement |
|---------|-----------|
| **Momentum** | Persistance des tendances de prix (Jegadeesh & Titman, 1993) |
| **Value** | Mean reversion des valorisations (Fama & French, 1992) |
| **Quality** | Entreprises rentables, peu endettees, stables (Novy-Marx, 2013) |
| **Size** | Prime historique des petites capitalisations (Banz, 1981) |

Les facteurs sont actives ou desactives individuellement. Les poids relatifs et les metriques exactes utilisees pour chaque facteur restent proprietaires.

## Pipeline

Le systeme s'execute en 5 etapes sequentielles a chaque rebalancement :

### 1. Collecte & nettoyage
- Donnees prix et fondamentales sur l'univers complet
- Detection des anomalies (splits non ajustes, trous de cotation, delistings)
- Conversion multi-devises vers la devise de reference

### 2. Scoring cross-sectionnel
- Calcul des metriques brutes par action sur l'univers entier
- Normalisation cross-sectionnelle (chaque action est evaluee relativement a l'univers, pas en absolu)
- Agregation ponderee des facteurs en un score composite unique

### 3. Filtres de robustesse
- Filtrage des titres illiquides en dessous d'un seuil de volume quotidien
- Verification de la couverture de donnees (fondamentales + prix)
- Tests de robustesse pour ecarter les titres dont le comportement historique est instable
- Filtrage des correlations excessives entre candidats (eviter la concentration involontaire)

### 4. Allocation capital-contrainte
- Construction du portefeuille sous contraintes reelles : lots minimums par marche, commissions, taille de position maximale
- Caps geographiques configurables (limiter l'exposition a une region)
- Deux modes d'allocation disponibles : equal weight et optimisation basee sur le risque

### 5. Execution
- Generation des ordres multi-marches, multi-devises
- Execution via Interactive Brokers API (TWS)
- Gestion des ordres partiels et des rejets
- Preflight automatique : verification de l'eligibilite de chaque titre avant envoi

## Couverture geographique

Le systeme couvre les marches suivants :

| Region | Bourses |
|--------|---------|
| Amerique du Nord | NYSE, NASDAQ, TSX |
| Europe | XETRA, LSE, Euronext, SIX, OMX, BME, Borsa Italiana |
| Asie-Pacifique | TSE, HKEX, ASX, SGX, KRX, TWSE |
| Amerique Latine | BMV |

Soit 30+ bourses, avec gestion automatique des fuseaux horaires et des jours feries.

## Metriques de suivi

| Metrique | Description |
|----------|-------------|
| CAGR | Rendement annualise compose |
| Sharpe Ratio | Rendement ajuste au risque (annualise) |
| Maximum Drawdown | Perte maximale depuis le plus haut |
| Hit Rate | % de positions cloturees en gain |
| Turnover | Rotation mensuelle du portefeuille |
| Factor Exposure | Decomposition des rendements par facteur |

## Stack

- **Python 3.11+**
- **pandas / numpy / scipy** — Manipulation de donnees, statistiques, optimisation
- **Interactive Brokers API** — Execution multi-marches
- **Matplotlib / Plotly** — Visualisation et reporting

---

Developpe par Jean-Jacques F. — [LinkedIn](https://linkedin.com/in/jjforier)
