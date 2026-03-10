# ETF Portfolio Optimizer

Allocation de portefeuille ETF dynamique pour investisseurs patrimoniaux.

## Objectif

Systeme complet de selection et d'allocation d'ETFs, concu pour un investisseur qui veut construire un portefeuille diversifie sans stock-picking. Le systeme identifie les meilleurs ETFs d'un univers configurable, les pondere selon une methode d'optimisation, et genere un portefeuille rebalancable periodiquement.

L'objectif n'est pas de battre le marche a tout prix, mais de construire un portefeuille coherent, diversifie et adapte au profil de risque — avec un processus reproductible et sans intervention discretionnaire.

## Univers

Trois perimetres d'investissement configurables :

| Univers | Perimetre | Enveloppe |
|---------|-----------|-----------|
| **EU UCITS Strict** | ETFs conformes UCITS cotes en Europe | PEA + CTO |
| **EU elargi** | Univers europeen complet (XETRA, LSE, Euronext, SIX) | CTO |
| **International** | Inclusion des ETFs americains | CTO uniquement |

### Filtres automatiques
- Exclusion des produits a effet de levier (x2, x3)
- Exclusion des ETFs inverses (short)
- Filtrage par encours minimum (AUM)
- Filtrage par anciennete (historique minimum requis)

## Pipeline

### 1. Collecte
- Donnees prix et fondamentales sur l'univers ETF complet
- Metadonnees : TER (frais de gestion), encours, domicile, indice replique, methode de replication

### 2. Scoring multi-criteres
Chaque ETF recoit un score composite qui integre plusieurs dimensions. Le scoring ne repose pas sur un seul critere (comme la performance passee) mais sur une evaluation croisee qui penalise les extremes et favorise la regularite.

Les dimensions evaluees incluent :
- Tendance recente et persistance
- Cout de detention (TER)
- Comportement relatif a son indice de reference
- Contribution a la diversification du portefeuille

Les poids exacts et les formules de scoring restent proprietaires.

### 3. Allocation
Trois methodes disponibles :

| Methode | Principe |
|---------|----------|
| **Equal Weight** | Repartition uniforme entre les ETFs selectionnes |
| **Score-Weighted** | Ponderation proportionnelle au score composite |
| **HRP** | Hierarchical Risk Parity (Lopez de Prado, 2016) — allocation basee sur la structure de correlation et le risque |

La methode HRP est la methode par defaut. Elle equilibre le risque entre les positions sans reposer sur l'estimation instable des rendements esperes (contrairement a Markowitz).

### 4. Contraintes
- Exposition geographique maximale par region (Amerique du Nord, Europe, Asie, Emergents)
- Exposition sectorielle maximale (Technologie, Sante, Finance, Energie, etc.)
- Correlation maximale entre positions (eviter la redondance)
- Nombre minimum et maximum de lignes

### 5. Rebalancement
Frequences configurables : mensuel, trimestriel, semestriel ou annuel. Le systeme calcule les ajustements necessaires et genere les ordres.

## Profils de risque

Le systeme peut etre calibre pour differents profils :

| Profil | Volatilite cible | Composition type |
|--------|------------------|------------------|
| Prudent | 5-8% | Obligations + actions defensives |
| Equilibre | 8-12% | Mix diversifie |
| Dynamique | 12-18% | Actions monde + thematiques |
| Offensif | 18%+ | Actions concentrees + emergents |

## Stack

- **Python 3.11+**
- **pandas / numpy / scipy** — Manipulation de donnees et optimisation
- **Plotly** — Visualisation interactive
- **EODHD API** — Donnees ETF

---

Developpe par Jean-Jacques F. — [LinkedIn](https://linkedin.com/in/jjforier)
