# Quantitative Finance

Projets et travaux en finance quantitative — selection d'actifs, allocation de portefeuille, trading systematique.

Trois systemes developpes en Python, concus pour fonctionner sur des donnees reelles et s'executer sur des comptes de trading en production. Chaque projet a ete construit, teste et itere sur plusieurs annees de donnees historiques avant toute mise en live.

## Projets

| Projet | Perimetre | Univers |
|--------|-----------|---------|
| [**multi-factor-stock-screener**](multi-factor-stock-screener/) | Selection d'actions individuelles par scoring multi-factoriel | 3 000+ titres, 30+ bourses |
| [**etf-portfolio-optimizer**](etf-portfolio-optimizer/) | Allocation ETF optimisee sous contraintes patrimoniales | 500+ ETFs europeens et internationaux |
| [**systematic-trading-framework**](systematic-trading-framework/) | Backtesting rigoureux et execution automatisee multi-actifs | Forex, indices, crypto |

## Papers

Travaux publies — retours d'experience, analyses, etudes.

| # | Titre |
|---|-------|
| [WP-001](papers/) | "Je bats le MSCI World." So what. Le hasard aussi. |

## Principes

Chaque projet suit la meme discipline :

- **Donnees reelles** — Prix, fondamentales et volumes de marche. Pas de simulations synthetiques.
- **Validation hors echantillon** — Aucune decision de design n'est validee sur les donnees d'entrainement. Walk-Forward, split temporel, ou validation croisee selon le contexte.
- **Execution sous contraintes reelles** — Frais de transaction, spread, slippage, lots minimums, commissions multi-devises, horaires de marche. Le backtest integre les memes contraintes que le live.
- **Modularite** — Chaque composant (scoring, allocation, execution, reporting) est independant et remplacable.
- **Code proprietaire** — La logique interne, les parametres et les seuils ne sont pas publies. Seuls les README decrivent l'architecture et la methodologie.

## Stack

| Composant | Usage |
|-----------|-------|
| Python 3.11+ | Langage principal |
| pandas / numpy / scipy | Manipulation de donnees, optimisation, statistiques |
| matplotlib / plotly | Visualisation et reporting |
| Interactive Brokers API | Execution actions et ETF |
| MetaTrader 5 | Execution forex et indices |

---

Jean-Jacques F. — Analyste Financier
[LinkedIn](https://www.linkedin.com/in/jean-jacques-forier-invest000/) | [GitHub](https://github.com/Jayovernight)
